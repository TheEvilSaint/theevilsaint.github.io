---
layout: post
title: 'Windows Query Logged On Users'
date: '2022-06-10'
description: 'As a pentester, there are plenty of situations where we need to view who is logged on to a given computer remotely. Often we not only need to check who is logged on interactively at the console but also who is connected remotely via a Remote Desktop Connection. This article will discuss server non-invasive ways of doing this.'
coverimage: Windows_Query_Logged_On_Users.jpg
tags: wmic
published: true
posttype: article
categories: article
---

# Query Logged On Users

Logon Types

| Type | Meaning |
| --- | --- |
| 0 | SYSTEM account logon, typically used by the computer itself. |
| 2 | Interactive Logon - terminal services, console, directly interact with the system |
| 3 | Network Logon - For things like WMI, SMB, and other remote protocols (non-interactive) |
| 5 | Service Logon |
| 10 | Remote Interactive Logon |

On some machines, you may need to enable remoting.

```
netsh firewall set service remoteadmin enable
```

Manually with wmic command line

```
wmic logon where LogonType=2 GET LoginID, LogonType
wmic path Win32_LoggedOnUser | find "273995"
wmic \node:10.50.1.254 \user:marvel.lab\troy.wood \password:"password123" path Win32_LoggedOnUser
```

Using WMiQuery

```
wmiquery.py 'Administrator:Passw0rd!@10.50.1.254'
select LogonType, LogonID from Win32_logonsession where LogonType=2
```

Using PTH-Wmic

```
pth-wmic -U "marvel.lab\Administrator%Passw0rd!" \\10.50.1.254 'select LogonType, LogonID from Win32_logonsession where LogonType=2'
pth-wmic -U "marvel.lab\Administrator%Passw0rd!" \\10.50.1.254 'select * Win32_loggedonuser'
```

Using Sysinternals PsLoggedOn64

```
psexec.py -c \pentesting\PsLoggedon64.exe 'marvel.lab\Administrator:Passw0rd!@10.50.1.15' "-l -accepteula"
```

```
nbtscan -r 10.50.1.0\24 | grep ^10 | awk '{ print $1 }' > \tmp\nbtscan.txt
for ip in $(cat \tmp\nbtscan.txt); do psexec.py -c \pentesting\PsLoggedon64.exe marvel.lab\Administrator:Passw0rd\!@${ip} "-l -accepteula"; done >> \tmp\loggedon.txt
```