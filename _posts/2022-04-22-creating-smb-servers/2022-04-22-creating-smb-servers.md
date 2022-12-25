---
layout: post
title: 'Creating SMB Servers'
date: '2022-04-22'
description: 'This article looks at two ways of creating an SMB Server. First, we use the impacket library to spin up an SMB server from the command line and then we look at creating an SMB share manually on our Kali Linux VM setting up the SAMBA service.'
coverimage: Creating_SMB_Servers.jpg
tags: smb
published: true
posttype: article
categories: article
---
# SMB

## Create SMB Servers

Impacket

```
python smbserver.py -username evilsmb -password evilpass -ip 10.1.1.99 evilshare /root/payloads/
```

> Leaving -username and -password off of the command will create an anonymous share.

Manual

Backing up the original `smb.conf`

```
cp /etc/samba/smb.conf /etc/samba/smb.conf.bk
```

Emptying our config file and then edit it so we can add only the lines we need

```
root@evilsaint:/# >/etc/samba/smb.conf
root@evilsaint:/# nano /etc/samba/smb.conf
```

We make the following changes to our smb.conf

```
cat /etc/samba/smb.conf
[global]
   workgroup = MARVEL
[myshare]
   comment = My Share For Sharing Files
   read only = no
   locking = no
   path = /root/Documents
   guest ok = no
```

> To create an smbuser, a normal user with the same name needs to be present on the system.
> 

We now need to add a user to access the smbserver

```
root@evilsaint:/# smbpasswd -a evilsaint
New SMB password:
Retype new SMB password:
Added user evilsaint.
```

We can verify the user was created with pdbedit

```
pdbedit -w -L
evilsaint:1001:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX:E9298AEE730720E27F879300509D911A:[U          ]:LCT-5A89C279:
```

We now restart the service.

```
service smbd restart
```