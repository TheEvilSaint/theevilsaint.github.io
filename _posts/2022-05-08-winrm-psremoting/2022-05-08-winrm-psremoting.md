---
layout: post
title: 'WinRM PSRemoting'
date: '2022-05-08'
description: 'PowerShell Remoting is the recommended way to manage Windows systems. PowerShell Remoting uses Windows Remote Management (WinRM), the Microsoft implementation of the Web Services for Management (WS-Management) protocol, to allow users to run PowerShell commands on remote computers. By default, PowerShell Remoting only allows connections from members of the Administrators group. This article explores using WinRm PSRemoting.'
coverimage: WinRM_PSRemoting.jpg
tags: powershell winrm
published: true
posttype: article
categories: article
---

# WinRM  PSRemoting

The system needs to have WinRM enabled before you can use it:

Test if the service is listening:

```bash
Test-WSMan -ComputerName TargetPC
```

Enable via PSexec

```bash
psexec \\target -u PoorSysAdmin -p Passw0rd -h -d powershell.exe "enable-psremoting -force"
```

Insert your stager here

```bash
Invoke-Command -ComputerName TargetPC -ScriptBlock { BlahBlah } -credential jdoe
```

Get an interactive PSRemote Shell. 

```bash
$cred = Get-Credential
Enter-PSSession -ComputerName TargetPC -Credential $cred
```