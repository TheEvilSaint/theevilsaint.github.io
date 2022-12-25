---
layout: post
title: 'Understand NetBIOS Names in Minutes'
date: '2022-02-26'
description: 'This article serves as an introduction and quick reference to NetBIOS names'
coverimage: Understand_NetBIOS_Names_in_Minutes.jpg
published: true
posttype: article
categories: article
---

NetBIOS names made easy. This article serves as a simplified introduction to NetBIOS names and suffixes; consider this if you are running late for exam prep or having to debug output from your terminal.

If you wish you learn more about NetBIOS, see EvilSaint's article at <https://evilsaint.com/article/protocol-dissection-netbios-smb/>.

## What is the purpose of NetBIOS?

It lets computers in a local area network to share resources.
<img src="/static/832745c2-95cc-45f6-a712-56f0da41b6e4.png">

## Does Linux support NetBIOS?

Yes - by installing SAMBA.

## What are NetBIOS names?

These are names that have specific meanings. These meanings explain what the computer can do in context of NetBIOS resource sharing.

Basic syntax of a NetBIOS name is:

```
<computername/domain/workgroup>  <suffix>  <type>
```

Or in form of an example,

```
MYCOMPUTER <00> UNIQUE
```

The above example means:

```
My computer is called "MYCOMPUTER". 
It supports the Workstation Service ("<00>") which means that it supports file and printer sharing.
My computer name is the only one with this name in this network, making it "UNIQUE".
```

Your computer probably has <b>at least three</b> NetBIOS names (depending what is installed on it). I.e., your computer wears "at least three different hats",  

You can view your computers' NetBIOS names with:

```
nbtstat  -n
```

Sample output is below.

```
       Name               Type         Status
    ---------------------------------------------
    MYCOMPUTER     <00>  UNIQUE      Registered
    MYCOMPUTER     <20>  UNIQUE      Registered
    WORKGROUP      <00>  GROUP       Registered
```

The `status` just means it is registered by broadcast or with a WINS server; however this is not of interest for now.

The latter two NetBIOS names translate to:

```
My computer is called "MYCOMPUTER". 
It supports File Sharing ("<20>") which means that it shares files within the network. 
My computer name is the only one with this name in this network, making it "UNIQUE".
```

and

```
My computer is part of a "WORKGROUP" and not a domain. 
It supports the Workstation Service ("<00>") which means that it supports file and printer sharing within the workgroup. 
The Workgroup my computer is a part of is a "GROUP" and it other computers are part of this.
```

As demonstrated above, NetBIOS names tend to identify the UNIQUE and GROUP attributes of the computer in question.

## Unique

A unique name applies to a single IP address.

The UNIQUE attribute refers to:

* uniquename; workstation, server, etc,
* username (in some cases)

## Group - Domain vs Workgroup

A group name, which applies to a subnet group of IP addresses

The WORKGROUP attribute refers to:

* peer to peer network
* users being able to login using their own device only
* smaller group than a domain

The DOMAIN attribute refers to:

* users being able to log in with their credentials to any "office" device
* bigger group than a workgroup

## Technical details - NetBIOS Names + Suffixes

Most common ones.

UNIQUE
------

COMPUTERNAME  <00>  UNIQUE Workstation Service   Can access network shares
COMPUTERNAME  <03>  UNIQUE        Windows Messenger Service  Sends messages between users in network
COMPUTERNAME  <06>  UNIQUE Remote Access Service                    Provides remote access services   
COMPUTERNAME  <20>  UNIQUE File Service     Shares files in network
COMPUTERNAME  <21>  UNIQUE Remote Access Service client  Acts as a remote access service client
DOMAIN    <1B>  UNIQUE Domain Master Browser   Primary Domain Controller of domain <-- <b>DC</b>
WORKGROUP/DOMAIN <1D>  UNIQUE Master Browser    Local browser server for subnet of domain or workgroup

GROUP
-----

WORKGROUP/DOMAIN         <00>  GROUP   Workgroup/domain name Is part of this workgroup or domain and can access shared resources within it
__MSBROWSE__   <01>  GROUP  Master Browser   Is the primary browser for this workgroup/domain
WORKGROUP/DOMAIN      <1C>  GROUP  Domain Controllers   Is part of Domain Controllers group of workgroup or domain 
WORKGROUP/DOMAIN      <1E>  GROUP  Browser Service Elections Browser service election server

## `net` commands to use

All `net` commands all use NetBIOS host names.

To list NetBIOS connections (file share, printer share), use:

```
net use 
net use UNC
```

To check your computer,
nbtstat -a IP ADDRESS

Your computer name --> probably at least three NetBIOS names depending what installed with Windows NT.

workstation service = enables print and file sharing
win10 -- > automatically started on boot
enable access to network shares

over LAN
files + printers
session layer

service or name record type such as host record, master browser record, domain controller recors, or other services
individual service --> suffix
NetBIOS name --> mac 16 ASCII chars --> 16th char = suffix
    --> HEX : 00 is one byte
--> friendly name --> 15-char name : computer name, domain name, name of user logged on
--> domain, computer or user name + service type / function

<https://en.wikipedia.org/wiki/NetBIOS>
For unique names:
single IP address
workstation
server
messenger service

00: Workstation Service (workstation name)
03: Windows Messenger service
06: Remote Access Service
20: File Service (also called Host Record)
21: Remote Access Service client
1B: Domain Master Browser – Primary Domain Controller for a domain
1D: Master Browser

For group names:

00: Workstation Service (workgroup/domain name)
1C: Domain Controllers for a domain (group record with up to 25 IP addresses)
1E: Browser Service Elections

Example:

What is
<computername> 00 U

A: Workstation

<https://evilsaint.com/article/protocol-dissection-netbios-smb/>
nbtscan <ip-range>
 NetBIOS name and MAC

nbtscan -v -s : <ip-range>
 service Identifier

* workgroup `<00>` group - This name is a remnant of the original LAN Manager browse service.
* workgroup `<1D>` unique - This name identifies the Local Master Browser (LMB, sometimes called simply "Master Browser") for a subnet. <-- SUBNET of DOMAIN or WORKGROUP(?)
* workgroup `<1E>` -  group A node that is capable of acting as a "Browser" registers this group name to listen for election announcements.
* nt_domain `<1B>` unique Name registered by the Domain Master Browser. Must be registered with the NBNS in order to be of any real use. <-- combines infor for DOMAIN = DOMAIN CONTROLLER
* nt_domain `<1C>` Internet group Registered by all Domain Controllers in the given NT Domain.

MASTER BROWSER : computer that

How to list for individual computer
> nbtstat /a WIN-TUEFA0TGVTB
--> Name: computer name
--> Type: computer name type (unique or group)
--> Status: registered / not

 distinguished by whether they are

A multihomed name, which applies to a multicast group of IP addresses
