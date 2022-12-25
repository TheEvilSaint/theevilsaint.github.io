---
layout: post
title: 'Querying Active Directory With PowerShell'
date: '2022-06-22'
description: 'Active Directory lies at the heart of most organisations networks. PowerShell is a versatile Scripting Language that Windows natively support. We can leverage PowerShell to enumerate Active Directory for various pieces of information crucial to our penetration tests.'
coverimage: https://evilsaint.com/media/articles/images/querying-active-directory-with-powershell/cover_image/Querying_Active_Directory_With_PowerShell.jpg
tags: active-directory enumeration powershell
published: true
posttype: article
categories: article
---
# Active Directory Querying With PowerShell

PowerShell v1: .NET & ADSI

PowerShell v2 & newer: PowerShell Active Directory Module

```
Import-module servermanager; add-windowsfeature rsat-ad-tools
Import-module servermanager; add-windowsfeature rsat-ad-PowerShell
```

### .NET

Example of .NET

```
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().DomainControllers
```

### ADSI

[http://www.selfadsi.org/extended-ad/search-domain-controllers.htm](http://www.selfadsi.org/extended-ad/search-domain-controllers.htm)

Example of ADSI

```
$UserID = "JoeUser"
$root = [ADSI]''
$searcher = new-object System.DirectoryServices.DirectorySearcher($root)
$searcher.filter = "(&(objectClass=user)(sAMAccountName=$UserID))"
$user = $searcher.findall()
$user
```

### Active Directory Module

Example of Active Directory Module

- Requires AD Web Services (ADWS) running on targeted DC (TCP 9389)
  - (Side Note) PowerShell Remoting uses TCP 5985 (HTTP) or TCP 5986 (HTTPS)
- SOAP XML messages over HTTP translated on DC

```
Import-Module ActiveDirectory
$UserID = "JoeUser"
Get-ADUser $UserID -Property *
```

Here are some of the more useful Active Directory Module commands

```
Get-Module -ListAvailable
Get-Command -Module ActiveDirectory
```

Server 2008 R2

```
Get/Set-ADForest
Get/Set-ADDomain
Get/Set-ADDomainController
Get/Set-ADUser
Get/Set-ADComputer
Get/Set-ADGroup
Get/Set-ADGroupMember
Get/Set-ADObject
Get/Set-ADOrganizationalUnit

Enable-ADOptionalFeature
Disable/Enable-ADAccount
Move-ADDirectoryServerOperationMasterRole
New-ADUser
New-ADComputer
New-ADGroup
New-ADObject
New-ADOrganizationalUnit
```

Server 2012+

```
*-ADResourcePropertyListMember
*-ADAuthenticationPolicy
*-ADAuthenticationPolicySilo
*-ADCentralAccessPolicy
*-ADCentralAccessRule
*-ADResourceProperty
*-ADResourcePropertyList
*-ADResourcePropertyValueType
*-ADDCCloneConfigFile
*-ADReplicationAttributeMetadata
*-ADReplicationConnection
*-ADReplicationFailure
*-ADReplicationPartnerMetadata
*-ADReplicationQueueOperation
*-ADReplicationSite
*-ADReplicationSiteLink
*-ADReplicationSiteLinkBridge
*-ADReplicationSubnet
*-ADReplicationUpToDatenessVectorTable
Sync-ADObject
```

A nice looking inventory of DC’s

```
Get-ADDomainController -Filter * | select hostname,IPv4Address,IsGlobalCatalog,IsReadOnly,OperatingSystem | Format-Table -auto
```

### Active Directory Drive

Example of Active Directory drive

```
Imort-Module ActiveDirectory
dir ad:
Set-Location ad:
Set-Location "dc=lab,dc=pentest,dc=local"
dir
```

### ANR

ANR enables you to find a user when you have some information about a user, but don’t know exactly to which attribute that data corresponds. For example, if you know the user has “Thor” somewhere, but don’t know exactly what the SAMAccountName is (or DN, SID, name, etc). Submitting an ANR search will query the AD attributes flagged for ANR (attributes must be indexed) and replies with the results (may be more than one user found).

Example of ANR

```
Import-Module ActiveDirectory
Get-ADObject -LDAPFilter {(&(ObjectClass=User)(ANR=Thor))}
```

## Active Directory Enumeration with .NET

Here are some alternatives to using Get-ADForest & Get-Domain:

### Get Active Directory Forest Information

```
$ADForestInfo = [System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest()
$ADForestInfo.Name
$ADForestInfo.Sites
$ADForestInfo.Domains
$ADForestInfo.GlobalCatalogs
$ADForestInfo.ApplicationPartitions
$ADForestInfo.ForestMode
$ADForestInfo.RootDomain
$ADForestInfo.Schema
$ADForestInfo.SchemaRoleOwner
$ADForestInfo.NamingRoleOwner
```

OR

```
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().Name
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().Sites
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().Domains
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().GlobalCatalogs
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().ApplicationPartitions
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().ForestMode
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().RootDomain
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().Schema
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().SchemaRoleOwner
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().NamingRoleOwner
```

### Get Active Directory Domain Information

Target the current (local) computer’s domain:

```
$ADDomainInfo = [System.DirectoryServices.ActiveDirectory.Domain]::GetComputerDomain()
```

Target the current user’s domain:

```
$ADDomainName = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
```

```
$ADDomainInfo.Forest
$ADDomainInfo.DomainControllers
$ADDomainInfo.Children
$ADDomainInfo.DomainMode
$ADDomainInfo.Parent
$ADDomainInfo.PdcRoleOwner
$ADDomainInfo.RidRoleOwner
$ADDomainInfo.Name
```

OR

```
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().Forest
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().DomainControllers
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().Children
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().DomainMode
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().Parent
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().RidRoleOwner
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().Name
```

- Note: Use `[System.DirectoryServices.ActiveDirectory.Domain]::GetCOMPUTERDomain().Attribute` for the local computer’s domain info.

Example:

```
[System.DirectoryServices.ActiveDirectory.Domain]::GetCOMPUTERDomain().Forest
```

### Get the local computer’s site information

```
$LocalSiteInfo = [System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite()
$LocalSiteInfo.Name
$LocalSiteInfo.Domains
$LocalSiteInfo.Subnets
$LocalSiteInfo.Servers
$LocalSiteInfo.AdjacentSites
$LocalSiteInfo.SiteLinks
$LocalSiteInfo.InterSiteTopologyGenerator
$LocalSiteInfo.Options
$LocalSiteInfo.Location
$LocalSiteInfo.BridgeheadServers
$LocalSiteInfo.PreferredSmtpBridgeheadServers
$LocalSiteInfo.PreferredRpcBridgeheadServers
$LocalSiteInfo.IntraSiteReplicationSchedule
```

OR

```
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().Name
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().Domains
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().Subnets
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().Servers
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().AdjacentSites
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().SiteLinks
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().InterSiteTopologyGenerator
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().Options
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().Location
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().BridgeheadServers
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().PreferredSmtpBridgeheadServers
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().PreferredRpcBridgeheadServers
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite().IntraSiteReplicationSchedule
```

## Sean Metcalf Fave Enumeration Commands

Get a Computer’s Site:

```
[System.DirectoryServices.ActiveDirectory.ActiveDirectorySite]::GetComputerSite()
```

Get a User’s Domain:

```
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().Name
```

Get a Computer’s Domain:

```
[System.DirectoryServices.ActiveDirectory.Domain]::GetComputerDomain().Name
```

List Active Directory FSMOs:

```
([System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest()).SchemaRoleOwner
([System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest()).NamingRoleOwner
([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).InfrastructureRoleOwner
([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).PdcRoleOwner
([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).RidRoleOwner
```

List All Domain Controllers in a Domain:

```
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().DomainControllers
```

Get Active Directory Domain Mode:

```
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().DomainMode
```

Get Trusts for current Active Directory Domain:

```
([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).GetAllTrustRelationships()
```

Get Active Directory Forest Name:

```
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().Name
```

Get a List of Sites in the Active Directory Forest:

```
[array] $ADSites = [System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().Sites
```

Get Active Directory Forest Domains:

```
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().Domains
```

Get Active Directory Forest Global Catalogues:

```
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().GlobalCatalogs
```

Get Active Directory Forest Application Partitions:

```
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().ApplicationPartitions
```

Get Active Directory Forest Mode:

```
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().ForestMode
```

Get Active Directory Forest Root Domain:

```
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().RootDomain
```

Get Active Directory Forest Schema DN:

```
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().Schema
```

## PowerShell Mitigations

PowerShell version 5 will be out very soon and has several compelling security enhancements.

### System-wide Transcripts

Use group policy to have PowerShell log all system PowerShell commands and save the transcripts to a share for parsing.

### Script Block Logging

PowerShell logs the obfuscated code as well as the dynamically generated code that PowerShell actually executes.

### Constrained PowerShell

Automatically enables PowerShell constrained mode when AppLocker policy is set to "Allow". This limits PowerShell code execution to only core capability. The offensive PowerShell tools typically used by attackers leverage advanced PowerShell functionality disabled in Constrained Mode.

### Windows 10 - Antimalware Integration

Windows 10 adds Antimalware Integration which automatically passes all code PowerShell processes to an installed antimalware solution before execution. If the code is deemed as malicious it doesn’t execute. This also includes code downloaded into memory from the Internet and executed.

PowerShell Security Recommendations.

- Limit PowerShell Remoting (WinRM) - Limit WinRM listener scope to admin subnet & Disable PowerShell Remoting (WinRM) on DCs.
- Audit/block PowerShell script execution via AppLocker. Once you have PowerShell v3+, Enable PowerShell Module logging (via GPO). This Enables tracking of PowerShell command usage providing capability to detect invoke-mimikatz use - just search PowerShell logs for “mimikatz”. [Note this won’t catch everything]
- PowerShell v3+: Enable PowerShell Module logging (via GPO).
- Leverage Metering for PowerShell usage trend analysis - JoeUser ran PowerShell on 10 computers today?
- Track PowerShell Remoting Usage through NetFlow data OR check the PowerShell logs on clients (event ID 06) & servers (event id 400)
- Deploy PowerShell v5 and implement system-wide transcripts
