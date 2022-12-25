---
layout: post
title: 'Windows Registry'
date: '2022-07-13'
description: 'The Registry has been a part of Windows since Windows 3.1. Simply put, its a database that stores Windows and programme settings. Some of those settings are quite complex and arent meant for humans to alter or comprehend; others are simple and can be safely tweaked.'
coverimage: Windows_Registry.jpg
tags: registry
published: true
posttype: article
categories: article
---

# Registry

### The Root Keys

- HKEY_CLASSES_ROOT (HKCR)
- HKEY_CURRENT_USER (HKCU)
- HKEY_LOCAL_MACHINE (HKLM)
- HKEY_USERS (HKU)
- HKEY_CURRENT_CONFIG (HKCC)

### Query Registry via Command Line

List all

```
reg query <hive>
```

Use the `/f` switch to search

```
reg query HKLM /f password
```

Add `/s` to make the search recursive.

```
reg query HKLM  /s /f password
```

Use `/d` to only search in the data only.

```
reg query HKLM /d /s /f password
```

### Examples

Installed Programs

```
reg query HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall >> installed_programs.txt
```

Recursively loop through all registry keys.

```
reg query HKEY_LOCAL_MACHINE\Software\ \s
```

Services

```
reg query HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ >> installed_services.txt
```

VNC Stored

```
reg query "HKCU\Software\ORL\WinVNC3\Password"
reg query "HKCU\Software\ORL\WinVNC4\Password"
```

Windows Autologin

```
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"
```

SNMP Parameters

```
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"
```

Run-on Boot

```
reg query "\\DC02\HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
reg query "\\DC02\HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce"
reg query "\\DC02\HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnceEx"
```

VNC Password

```
reg query "\\DC02\HKLM\SOFTWARE\RealVNC\WinVNC4 /v password"
```

Putty Password

```
reg query "\\DC02\HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
```

Putty clear text proxy credentials:

```
reg query "HKCU\Software\<user>\PuTTY\Sessions"
```

Check notification packages

```
reg query "\\DC02\HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v "Notification Packages"
```

Starting with Windows 2000-based computers, the machine account password automatically changes every 30 days. Check if this has been disabled

```
reg query "\\DC02\HKLM\System\CurrentControlSet\Services\Netlogon\Parameters" /v "DisablePasswordChange"
```

Search for passwords inside the registry

```
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
reg query HKLM /s | findstr /i password > temp.txt
reg query HKCU /s | findstr /i password > temp.txt
```

Check autologon

```
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"
```

SNMP Settings

```
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"
```

If BOTH registry values are set to 1, we can install a malicious MSI file.

```
reg query "HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer" /v "AlwaysInstallElevated"
reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer" /v "AlwaysInstallElevated"
```

View the Name of the Domain Controller

```
reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Group Policy\History" /v DCName
```

Check Browser Proxy Settings

```
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Wpad"
```

Internet Explorer History

```
reg query "HKCU\Software\Microsoft\Internet Explorer\TypedURLs"
```

Check physically attached hardware.

```
reg query "HKLM\System\MountedDevices"
```

Check recent apps

```
reg query "HKLM\Microsoft\Currentversion\Search\RecentApps"
```

Remotely query the registry for the last logged in user.

```
reg query "\\computername\HKLM\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\WinLogon"
```

### Terminal Server Service

Check if enabled

```
reg query "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "fDenyTSConnections"
```

Enable remote desktop.

```
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
```

Disable remote assistance

```
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fAllowToGetHelp /t REG_DWORD /d 1 /f
```

This setting enforces the deleting of Remote Desktop Services. Command checks if set.

```
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v "DeleteTempDirsOnExit"
```
