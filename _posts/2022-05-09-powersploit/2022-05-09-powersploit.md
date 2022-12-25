---
layout: post
title: 'PowerSploit'
date: '2022-05-09'
description: 'Powersploits tagline was PowerSploit is a collection of Microsoft PowerShell modules that can be used to aid penetration testers during all phases of an assessment. While the library is no longer supported, it still lives up to its name as a good selection of modules we can use on our pentests.'
coverimage: PowerSploit.jpg
tags: powershell powersploit
published: true
posttype: article
categories: article
---
# Powersploit

### Domain Controllers

Get Domain Controllers

```
Get-NetDomainController -Domain MARVEL.LAB
```

Get Domain Controllers via LDAP Queries

```
Get-NetDomainController -Ldap
```

Get Domain Controllers (Easy Read Format)

```
Get-NetDomainController | Select-Object -Property Name,OSVersion,IPAddress,Roles,SiteName | Format-Table -AutoSize
```

Find a Domain Controller

```
Test-Connection <Domain Name>
Test-Connection MARVEL.LAB
```

List local admins on each Domain Controller.

```
Get-NetDomainController -Domain MARVEL.LAB -DomainController DC01 | Get-NetLocalGroup
```

### AD Users

Get Users in Domain (Easy Read Format)

```
Get-NetUser | Select-Object -Property name,samaccountname,badpwdcount,memberof,admincount,pwdlastset | Format-Table -AutoSize
```

Export All Users to CSV

```
Get-NetUser | Select-Object -Property * | Export-Csv -Path C:\enum\csv-users
```

Get Details on a single user.

```
Get-NetUser -UserName black.widow
```

### AD Computers

Get Computers Connected in Domain

```
Get-NetComputer
Get-NetComputer -FullData
Get-NetComputer -FullData | Select-Object -Property name,dnshostname,operatingsystem,adspath | Format-Table -AutoSize
```

### AD Objects

Get Domain SID

```
Get-DomainSID
```

Get AD Objects

```
Get-ADOject
Get-ADOject -SamAccountName Administrator
```

Set AD Object fields

```
Set-ADObject -SamAccountName Administrator -PropertyName countrycode -PropertyValue 99
Set-ADObject -SamAccountName Administrator -PropertyName description -PropertyValue "test this"
```

### Group Policy Objects (GPO)

Get Domain GPO

```
Get-NetGPO
```

Get Domain GPO (Easy Read Format)

```
Get-NetGPO | Select-Object -Property displayname,whencreated,whenchanged,gpcfilesyspath | Format-Table -AutoSize
```

### Discover Local Admins & Admin Access

Get Local Admins Accounts on machines across the domain.

```
Invoke-EnumerateLocalAdmin
```

Get Local Admin Accounts on machines across the domain (Nice Formatting)

```
Invoke-EnumerateLocalAdmin | Format-Table -Property ComputerName,AccountName,IsDomain,IsGroup,SID -AutoSize
```

Only Show Domain Admins and local admin groups

```
Invoke-EnumerateLocalAdmin | Where-Object { $_.IsDomain -eq $true } | Format-Table -Property ComputerName,AccountName,IsDomain,IsGroup,SID -AutoSize
```

Only Show Domain Admins

```
Invoke-EnumerateLocalAdmin -Threads 100 | Where-Object { $_.IsDomain -eq $true -and $_.IsGroup -eq $false } | Format-Table -Property ComputerName,AccountName,IsDomain,IsGroup,SID -AutoSize
```

Find machines on the domain where your credentials have admin access.

```
Find-LocalAdminAccess -Threads 10
```

Get Users logged on to a computer.

```
Get-NetLoggedOn -ComputerName WRK3
Get-NetComputer | Get-NetLoggedOn | Format-Table -Property ComputerName,wkui1_username
```

### Domain\Forest

Get Domain

```
Get-NetDomain -Domain travel.co.uk
```

Get Forest Details

```
Get-NetForest -Forest travel.co.uk
```

Get the inter-domain trusts.

```
Get-NetDomainTrust -DomainController 172.20.50.254 -Domain travel.co.uk
```

\\@todo: come back and try these out Get-NetForestTrust Get-NetForestDomain Invoke-MapDomainTrust

Get the Subnets

```
Get-NetSubnet -Domain travel.co.uk -DomainController 172.20.50.254
```

### AD Trusts & Forests

Find out if the domain is part of a forest.

```
Get-NetDomain
```

Find other domains which you have trust with using Powerview

```
Invoke-MapDomainTrust
Invoke-MapDomainTrust | Export-CSV -NoTypeInformation trusts.csv
```

Get users, groups and computers from each domain.

```
# Users
Get-NetUser -Domain trains.travel.co.uk | Select-Object -Property samaccountname
Get-NetUser -Domain flights.travel.co.uk | Select-Object -Property samaccountname
Get-NetUser -Domain travel.co.uk | Select-Object -Property samaccountname

# Groups
Get-NetGroup -Domain trains.travel.co.uk
Get-NetGroup -Domain flights.travel.co.uk
Get-NetGroup -Domain travel.co.uk

# Computers
Get-NetComputer -Domain trains.travel.co.uk
Get-NetComputer -Domain flights.travel.co.uk
Get-NetComputer -Domain travel.co.uk
```