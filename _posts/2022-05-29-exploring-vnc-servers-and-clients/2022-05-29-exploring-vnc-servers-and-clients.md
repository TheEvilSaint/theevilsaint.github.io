---
layout: post
title: 'Exploring VNC Servers And Clients'
date: '2022-05-29'
description: 'This article looks at several different VNC Server technologies, and we perform a number of actions on them including installing and configuring them. We then look at interacting with them via connection with a VNC Client.'
coverimage: Exploring_VNC_Servers_And_Clients.jpg
tags: vnc
published: true
posttype: article
categories: article
---
# VNC Servers & Clients

VNC works on a client and server model. A server component is installed on the remote computer (the one you wish to control), and a VNC viewer (the client) is installed on the computer from where you want to take control. Over the VNC protocol, the remote computer with the agent installed will transmit a copy of the remote computer’s screen to the viewer. The most common VNC Server I found myself using was TigerVNC which is forked off of TightVNC. It allows users to create an independent session. However, it can not do shared screens or allow requesting assistance so its mileage on pentests is limited to just the protocol. If however, you have a choice between the two then x11vnc can turn out a little better on pentests.

## Tiger VNC

Website: http://tigervnc.org

Tight VNC and Tiger VNC originated from the same project. Tiger VNC branched off from the never released VNC 4 branch of TightVNC. It has a GPL license and is free for personal and commercial use. It can operate as a server and client on MS Windows, Linux and FreeBSD. On Mac, it can just act as a client. It supports SSL/TLS Encryption.

Unfortunately, TigerVNC doesn't support File Transfer.

### Installing

If you install the `vnc4server` package it will also install `tigervnc-standalone-server`, which has replaced `tigervnc-server`

```
apt-get install vnc4server
```

Made up of the following commands

- [vncconfig](http://tigervnc.org/doc/vncconfig.html)
- [vncpasswd](http://tigervnc.org/doc/vncpasswd.html)
- [vncserver](http://tigervnc.org/doc/vncserver.html)
- [vncviewer](http://tigervnc.org/doc/vncviewer.html)
- [x0vncserver](http://tigervnc.org/doc/x0vncserver.html)
- [Xvnc](http://tigervnc.org/doc/Xvnc.html)

### Running a VNC Server

The following line runs `vncserver` . The `-name` parameter allows us to specify a display name for the session window. We can use the `-geometry` to specify dimensions (WxH) for the connecting session. We can specify the colour depth (16/24/32) using the `-depth` parmeter. The `-localhost` parameter can be yes or no as whether or not to listen locally. Adding `-cleanstale` tells TigerVNC Server to clean up any stale files before trying to work out what x display number is available. Lastly `-autokill` kills the TigerVNC

```
vncserver -name "My Window Title" -geometry "1600x1000" -depth 24 -localhost no  -cleanstale -autokill
```

If you have been working on any previous connections and you don't want to worry about closing them down before starting a new session then you can append on the following command to kill all previously existing server sessions.

```
-kill :*
```

Depending on your target audience, you can also specify

Never treat incoming connections as shared. If a second person connects it disconnects. Good option as you at least know if someone is connected.

```
--NeverShared
```

Always treat the session as shared. If you share the connection and use the ‘view only’ password, you can perform a presentation to many.

```
-AlwaysShared
```

Specify the allowed users

```
-AllowedUsers users
```

### Manage VNC Sessions

To list any VNC Sessions that have been setup.

```
vncserver -list
```

Kill a display (or all displays)

```
vncserver -kill :1
vncserver -kill :*
```

### Authentication

For your session, you will need to authenticate

The VNC Password is stored in `/.vnc.passwd` of the person who started the service. Type the following to be prompted for a password.

```
vncpasswd
```

A nice way to connect is over SSH

```
vncviewer -via root@46.101.90.247 :1
```

### Configuration

The `~/.vnc/Xvnc-session` file used to be `~/.vnc/xstartup` and because of such, TigerVNC will check both. The file `/etc/vnc.conf` is the global configuration file. Passwords are stored in `~/.vnc/passwd`

## x11vnc

http://www.karlrunge.com/x11vnc/

Has TightVNC and UltraVNC file transfer built-in along with built-in SSL encryption and authentication.

- Runs of the command line
- Supports lots of graphical environments

### Installing

On Debian Linux, it is normally an easy install with the package manager

```
apt-get install x11vnc
```

With centos 9 you need to pull down the binary

```
wget http://dl.fedoraproject.org/pub/epel/7/x86_64/x/x11vnc-0.9.13-11.el7.x86_64.rpm
yum install Xvfb
yum install libvncserver
yum install tk
rpm -i --allfiles -v x11vnc-0.9.13-11.el7.x86_64.rpm
```

### Running a VNC Server

To set a password file for x11vnc, use the `-storepasswd` parameter. By default, it will save it in `~/.vnc/passwd` unless you specify it elsewhere

```
x11vnc -storepasswd
```

Run a session specify a clear-text password credential of ‘truffle’

```
x11vnc -passwd truffle
```

The `-forever` flag will keep x11vnc listening for further connections rather than exiting.

```
x11vnc -passwd truffle -forever
```

We can specify the window geometry of connections along with a name

```
x11vnc -passwd truffle -forever -desktop "Desktop Name"
```

Instead of specify a plain text password on the command line you can include your file. If you want to put your password in plain text in a file you can simply use `-passwdfile`. However more commonly you will want to use the password you created with the `-storepasswd` flag and for that you specify `-rfbauth`

```
x11vnc -forever -desktop "Desktop Name" -passwdfile /root/unencrypted-pass
x11vnc -forever -desktop "Desktop Name" -rfbauth /root/.vnc/passwd
```

A good idea is to also restrict access based on ip address if possible.

```
x11vnc -forever -desktop "Desktop Name" -rfbauth /root/.vnc/passwd -allow xx.xxx.xx.xxx
```

If you need to run x11vnc on a non-standard port you can specify it with the `-rfbport`

```
x11vnc -forever -desktop "Desktop Name" -passwdfile /root/.vnc/cleartext -rfbport 7777
```

A secondary password can be supplied for those only allowed to view the session

```
x11vnc -forever -desktop "Desktop Name" -rfbauth /root/.vnc/passwd -viewpasswd "no-touching!"
```

> -tightfilexfer Enable the TightVNC file transfer extension.
>  -ultrafilexfer Enable UltraVNC filetransfer

| Option | Description |
| --- | --- |
| -shared | When display is shared more than one person can connect at once. (defaults to off) |
| -geometry | WxH |
| -forever | Keep listening for more connections rather than exiting. |
| -once | Exit after the first successfully connected viewer disconnects |
| -allow | Specify the IP addresses (separated by commas) of the allowed hosts |
| -unixpw | List of comma-separated user names of system users that can login |
| -ssl | Use the openssl library to provide a built-in encrypted SSL/TLS tunnel between server and viewers |
| -passwd | Add a plain password to the connection |
| -desktop | Set Desktop name for clients |
| -chatwindow | Place a local UltraVNC chat window on the X11 display |

## Viewing VNC

On client install the VNC viewer

```
apt-get install xtightvncviewer
```

The basic syntax for viewing the server.
```
vncviewer 54.244.102.175
```

If the server is running on a different display (n = display number)
```
vncviewer 54.244.102.175:n
```

If we get tired of typing out a password on the terminal, we can reference it via a file
```
vncviewer 45.76.134.217 -passwd <path to my password file>
```

An alternative to the Tight VNC Viewer is SSVNC, an enhanced version of Tight VNC with support for more encodings, colour modes, and extensions.
```
apt-get install ssvnc
ssvncviewer 46.101.74.11:1
```