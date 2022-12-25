---
layout: post
title: 'Fixing Common Errors PowerShell Remoting From Kali'
date: '2022-02-25'
description: 'In this article, I will briefly explain how to fix common issues when trying to PowerShell Remote From Kalil.'
coverimage: Fixing_Common_Errors_PowerShell_Remoting_From_Kali.jpg
tags: error-message kali-linux powershell
published: true
posttype: article
categories: article
---

Assuming you have powershell installed on Kali. 

// Insert link or brief description of installing PowerShell on Kali. 



## Problem 1

When you try and perform PowerShell remoting the first time you will probably see the following error. 

"Enter-PSSession: This parameter set requires WSMan, and no supported WSMan client library was found. WSMan is either not installed or unavailable for this system." 

```
┌──(consultant㉿pentest)-[~]
└─$ pwsh 
PowerShell 7.1.4
Copyright (c) Microsoft Corporation.

https://aka.ms/powershell
Type 'help' to get help.

PS /home/consultant> $cred = Get-Credential

PowerShell credential request
Enter your credentials.
User: Administrator
Password for user Administrator: *********

PS /home/consultant> Enter-PSSession -ComputerName 192.168.1.235 -Credential $cred -Authentication Negotiate
Enter-PSSession: This parameter set requires WSMan, and no supported WSMan client library was found. WSMan is either not installed or unavailable for this system.
PS /home/consultant> 
```
<img src="/static/86717c1c-326e-405e-a56f-4bcb1d38a049.png">


The problem here is that the library is looking for packages that do not exist. 


In this case, our PowerShell is located in the following path `/opt/microsoft/powershell/7`

if we do
```
ldd /opt/microsoft/powershell/7/libmi.so 
	linux-vdso.so.1 (0x00007ffc7bbe9000)
	libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007ff9e2e02000)
	libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007ff9e2dfb000)
	libpam.so.0 => /lib/x86_64-linux-gnu/libpam.so.0 (0x00007ff9e2de9000)
	libssl.so.1.0.0 => not found
	libcrypto.so.1.0.0 => not found
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007ff9e2c0f000)
	/lib64/ld-linux-x86-64.so.2 (0x00007ff9e3010000)
	libaudit.so.1 => /lib/x86_64-linux-gnu/libaudit.so.1 (0x00007ff9e2bdb000)
	libcap-ng.so.0 => /lib/x86_64-linux-gnu/libcap-ng.so.0 (0x00007ff9e2bd3000)

```

<img src="/static/adfdb756-450d-4869-a4d9-f6862bb0ba16.png">


In that directory, we need to add our symlinks:


```
cd /usr/lib/x86_64-linux-gnu 
sudo ln -s /usr/lib/x86_64-linux-gnu/libssl.so.1.0.2 libssl.so.1.0.0
sudo ln -s /usr/lib/x86_64-linux-gnu/libcrypto.so.1.0.2 libcrypto.so.1.0.0
```
<img src="/static/0743039e-e8ad-49d5-9d64-91588530b5f6.png">


Now fixed!


## Problem 2

You get a `MI_RESULT_ACCESS_DENIED` error message.
```
Enter-PSSession : MI_RESULT_ACCESS_DENIED
```

This is probably because you missed off the `-Authentication` flag on your command

Check if you are typing in something like this
```
Enter-PSSession -ComputerName 192.168.1.235 -Credential $cred 
```

The solution is to use `-Authentication Negotiate`
```
Enter-PSSession -ComputerName 192.168.1.235 -Credential $cred -Authentication Negotiate
```


## Problem 3

Errors with non-Kerberos authentication i.e. because you are not domain-joined

If you have seen this error when trying to connect with Negotiate because you are not domain-joined. 

"Enter-PSSession: Connecting to remote server 192.168.1.235 failed with the following error message : acquiring creds with username only failed An invalid name was supplied SPNEGO cannot find mechanisms to negotiate For more information, see the about_Remote_Troubleshooting Help topic."

From trying to type in a command such as 
```
PS /home/consultant> Enter-PSSession -ComputerName 192.168.1.235 -Credential $cred -Authentication Negotiate
Enter-PSSession: Connecting to remote server 192.168.1.235 failed with the following error message : acquiring creds with username only failed An invalid name was supplied SPNEGO cannot find mechanisms to negotiate For more information, see the about_Remote_Troubleshooting Help topic.
```
<img src="/static/7a048cd7-7167-4242-9c30-710c29d5b01b.png">


This is because you need the following package installed on your kali Linux
```
sudo apt-get install gss-ntlmssp
```

After this, you should be able to connect.