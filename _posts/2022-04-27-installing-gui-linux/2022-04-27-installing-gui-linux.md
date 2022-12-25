---
layout: post
title: "Installing A GUI On Linux"
date: "2022-04-27"
description: "Some server distros do not include a Graphical User Interface (GUI) by default. A GUI can take up system resources such as memory and processor, and often for a server-orientated operating system, this is not ideal. However, specific tasks and applications are more manageable and better run in a GUI environment."
coverimage: Installing_A_GUI_On_Linux.jpg
tags: linux
published: true
posttype: article
categories: tutorial
---
# Installing a GUI on Debian or Centos

## On Debian

First, update your system.
```
apt-get update
```

Select your choice of package and install
```
apt-get install <package name>
# i.e.
apt-get install gnome
apt-get install gnome-core
apt-get install kde-full
apt-get install kde-standard
apt-get install mate-desktop-environment
```

Once installed, you can start
```
startx
```

To enable it on boot.
```
systemctl set-default graphical.target
```

Either add a non-root user to login with
```
adduser evilsaint
```

### Login As Root

if you need to login to the GUI by root for whatever reason (not recommended), then the below will work on Gnome

Edit the following file

```
nano /etc/gdm3/daemon.conf
```

Underneath `[security]` you need to add an `AllowRoot` parameter like the following.

```
[security]
AllowRoot=true
```

Edit the following file
```
nano /etc/pam.d/gdm-password
```

Make sure this line is commented out.
```
auth required pam_succeed_if.so user != root quiet_success
```

On systems with multiple desktops, you can choose
```
echo 'exec gnome-session' > /home/gnomeuser/.xinitrc
echo 'startkde' > /home/kdeuser/.xinitrc
```

Usually, if there is a reason for a GUI environment that isn't Kali Linux then you might need security tools. Add the kali sources using Katoolin.

```
apt-get install git
git clone https://github.com/LionSec/katoolin.git  && cp katoolin/katoolin.py /usr/bin/katoolin
chmod +x  /usr/bin/katoolin
katoolin (Option 1 to add repos)
apt-get update
apt-get upgrade
apt-get
apt-get install kali-linux-full
```

## On Centos

Decide on Gnome or KDE and install using the yum package manager
```
yum -y groups install "GNOME Desktop"
yum -y groups install "KDE Plasma Workspaces"
```

Once installed, you can start
```
startx
```

To enable it on boot.
```
systemctl set-default graphical.target
```

Add a non-root user to login with
```
adduser evilsaint
```