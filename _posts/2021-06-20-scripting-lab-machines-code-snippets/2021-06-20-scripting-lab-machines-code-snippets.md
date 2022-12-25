---
layout: post
title: 'Scripting Lab Machines: Code Snippets'
date: '2021-06-20'
description: 'The below is an assortment of commands I frequently use when scripting the building of Windows machines and the creation of Active Directory environments.'
coverimage: Scripting_Lab_Machines_Code_Snippets.jpg
tags: 
published: true
posttype: article
categories: article
---
When creating home lab machines you may have a common set of needs that you wish to apply to each one. 

	· Enable Workplace Network Sharing
	· Disable ICE Protections on Servers
	· Turn off Windows Defender
	· Disable Updates

The below assortment of commands is what I used to script the building of Windows machines and the creation of Active Directory Environments. 


## Language and Locale


PowerShell
```
Set-WinUserLanguageList -LanguageList en-GB -Force
Set-WinSystemLocale en-GB
Set-Culture en-GB
```

## Set Power Options

CMD
```
powercfg.exe -change -monitor-timeout-ac 0
powercfg.exe -change -monitor-timeout-dc 0
powercfg.exe -change -disk-timeout-ac 0
powercfg.exe -change -disk-timeout-dc 0
powercfg.exe -change -standby-timeout-ac 0
powercfg.exe -change -standby-timeout-dc 0
powercfg.exe -change -hibernate-timeout-ac 0
powercfg.exe -change -hibernate-timeout-dc 0
```

## Firewall

PowerShell
```
Set-NetFirewallProfile -All -Enabled False
```

CMD
```
netsh firewall set opmode disable
netsh advfirewall set allprofiles state off
```

If keeping the firewall on then for labs you might need to enable File and Print sharing.
```
netsh firewall set service type=fileandprint mode=enable profile="ALL"
```



## Turn off Shutdown Events


Powershell
```
Set-ItemProperty -Path "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Reliability" -Name "ShutdownReasonOn" -Value 0
Set-ItemProperty -Path "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Reliability" -Name "ShutdownReasonUI" -Value 0
```

CMD
```
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Reliability" /v ShutdownReasonOn /t REG_DWORD /d 0 /f
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Reliability" /v ShutdownReasonUI /t REG_DWORD /d 0 /f
```

## Turn off Windows Updates (On Desktop)


PowerShell
```
Set-ItemProperty -Path "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -Name "AUOptions" -Value 1
Set-ItemProperty -Path "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update" -Name "AUOptions" -Value 1
```

CMD (Windows 10)
```
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v AUOptions /t REG_DWORD /d 1 /f
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update" /v AUOptions /t REG_DWORD /d 1 /f
```

On Windows 10 builds I sometimes also need to do the following
```
net stop wuauserv
net stop bits
net stop dosvc
```

## Disable Screen Saving (On Desktop)

PowerShell
```
Set-ItemProperty -Path "HKCU\Software\Policies\Microsoft\Windows\Control Panel\Desktop" -Name "ScreenSaveActive" -Value 0
```

CMD
```
reg add "HKCU\Software\Policies\Microsoft\Windows\Control Panel\Desktop" /v ScreenSaveActive /t REG_SZ /d 0 /f
```

## Stop Windows Defender

PowerShell
```
Stop-Service -Name WinDefend
Remove-WindowsFeature Windows-Defender
```

## Enable RDP

PowerShell
```
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-name "fDenyTSConnections" -Value 0 
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1
```

CMD
```
netsh advfirewall firewall set rule name="Remote Desktop - User Mode (TCP-In)" new enable =Yes profile="domain,private,public"
netsh advfirewall firewall set rule name="Remote Desktop - User Mode (UDP-In)" new enable =Yes profile="domain,private,public"
```

## Turn Off IE Enhanced Security (Servers)

PowerShell
```
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0
Stop-Process -Name Explorer
```

## Activate Administrator Account and Set Password


CMD
```
net user Administrator /active:yes
net user Administrator Passw0rd!
```

## Rename Computer Hostname 

PowerShell
```
$ComputerInfo = Get-WmiObject -Class Win32_ComputerSystem
$ComputerInfo.Rename($mchname)
```

CMD
```
Rename-Computer -NewName $hostname
```