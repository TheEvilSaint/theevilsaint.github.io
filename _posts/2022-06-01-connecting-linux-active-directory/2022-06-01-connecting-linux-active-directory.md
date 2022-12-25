---
layout: post
title: 'Connecting Linux To Active Directory'
date: '2022-06-01'
description: 'There are several benefits to connecting a Linux OS to Active Directory instead of leaving it as a stand alone. Because of this, you do sometimes see this configuration on client engagements and it is useful to test out how a Linux box shows up in your enumeration tools. This article details how you can connect both Debian and Centos based systems to Active Directory for setting up a home lab.'
coverimage: Connecting_Linux_To_Active_Directory.jpg
tags: active-direction linux
published: true
posttype: article
categories: article
---
# Connecting Linux To Active Directory

### Change Password of AD User from Linux

Change the password of a regular user with `net rpc`

```
root@kali:~# net rpc password adminuser -U helpdesk -S 192.168.80.10
Enter new password for adminuser:
Enter helpdesk's password:
root@kali:~#
```

Change the password of a regular user with `rpcclient`

```
root@kali:~# rpcclient -U helpdesk \\192.168.80.10
Enter helpdesk's password:
rpcclient $> setuserinfo2 normaluser 23 'Passw0rd!'
```

### Connect CentOS to Active Directoy

Install the following packages

```
yum install sssd realmd oddjob oddjob-mkhomedir adcli samba-common samba-common-tools krb5-workstation openldap-clients policycoreutils-python
```

Update the `/etc/hosts` file

```
192.168.0.151    adserver.example.com  adserver
```

Contents of `resolv.conf` should be something like below

```
cat /etc/resolv.conf

search example.com
nameserver 192.168.0.151
```

Join the domain

```
realm join --user=tech adserver.example.com
Password for tech:
```

Verify we are now joined.

```
realm list
```

Check an verify users with full domain

```
id linuxtechi@example.com
```

To make this work without full domain

```
nano /etc/sssd/sssd.conf

use_fully_qualified_names = False
fallback_homedir = /home/%u
```

### Connect Debian to Active Directoy

Configure the network interfaces

```
nano  /etc/network/interfaces
```

Add the ip address of the domain controller like follows

```
auto ens192
iface ens192 inet static
address 10.100.1.250
gateway 10.100.1.1
network 10.100.1.0
broadcast 10.100.1.255
dns-nameservers 10.100.1.254
```

Restart the interface config

```
/etc/init.d/networking restart
```

Install the following packages

```
apt -y install realmd sssd sssd-tools adcli krb5-user packagekit samba-common samba-common-bin samba-libs resolvconf
```

nano 5.conf

```
[logging]
        Default = FILE:/var/log/krb5.log

[libdefaults]
        ticket_lifetime = 24000
        clock-skew = 300
        default_realm = MARVEL.LAB
#       dns_lookup_realm = false
#       dns_lookup_kdc = true

[realms]
        MARVEL.LAB = {
                kdc = DC01.MARVEL.LAB:88
                admin_server = DC01.MARVEL.LAB:464
                default_domain = MARVEL.LAB

}

[domain_realm]
        .server.com = MARVEL.LAB
        server.com = MARVEL.LAB
```

Verify connection

```
kinit Administrator@MARVEL.LAB
klist
```

**Extra config to come back and do**

https:.debian.org

Install the following tools `apt-get install winbind samba`

nano .conf

```
[global]
        security = ads
        realm = MARVEL.LAB
        password server = 10.100.1.254
        workgroup = test
#       winbind separator = +
        idmap uid = 10000-20000
        idmap gid = 10000-20000
        winbind enum users = yes
        winbind enum groups = yes
        template homedir = \home\%D\%U
        template shell = \bin\bash
        client use spnego = yes
        client ntlmv2 auth = yes
        encrypt passwords = yes
        winbind use default domain = yes
        restrict anonymous = 2
        domain master = no
        local master = no
        preferred master = no
        os level = 0
```

Add the following line to the bottom of the common-session pam module

```
nano /etc/pam.d/common-session
# add to the end if need (create home directory automatically at initial login)
session optional        pam_mkhomedir.so skel=/etc/skel umask=077
```

Join to domain

```
realm join marvel.lab
```