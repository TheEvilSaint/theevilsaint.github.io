---
layout: post
title: 'SSH - Connecting To Legacy Servers'
date: '2022-03-06'
description: ''
coverimage: SSH__Connecting_To_Legacy_Servers_nc8XLGd.jpg
tags: 
published: true
posttype: article
categories: article
---
Accept more legacy ciphers. Lowers hardening for support for older clients. 
https://www.kali.org/docs/general-use/ssh-configuration/



Ciphers: ssh -Q cipher
MACs: ssh -Q mac
KexAlgorithms: ssh -Q kex
PubkeyAcceptedKeyTypes: ssh -Q key


Use the `-G` option to see negotiation 

Use the `-vv` for verbose.


nmap --script ssh2-enum-algos -sV -p <port> <host>


```
sudo apt install libssl1.0-dev
wget -c https://mirror.ox.ac.uk/pub/OpenBSD/OpenSSH/portable/openssh-6.0p1.tar.gz
tar -xzf openssh-6.0p1.tar.gz
cd openssh-6.0p1/
./configure --prefix=/opt/openssh-6.0p1/
./configure --prefix=/opt/openssh-6.0p1/
make 
make install 
```
<img src="/static/c38f2220-e15a-46c4-a306-9bd78f59f012.png">


/etc/ssh/ssh_config.d/kali-wide-compat.conf
<img src="/static/77ebcb2a-7c9d-46a4-bdc2-492183739299.png">
