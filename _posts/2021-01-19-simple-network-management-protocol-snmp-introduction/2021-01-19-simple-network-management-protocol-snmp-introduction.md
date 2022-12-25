---
layout: post
title: "Simple Network Management Protocol (SNMP) :- Introduction"
date: "2021-01-19"
description: ""
coverimage: Simple_Network_Management_Protocol_SNMP_intro.jpg
tags: snmp snmpwalk community-string
published: true
posttype: article
categories: tutorial
---
SNMP stands for Simple Network Management Protocol. It is generally used by network monitoring applications to read and write network state variables
(state of the interface, no. of sent packets etc.). Types of managed devices include servers, workstations and network devices.  
  
SNMP services run by default on the 161 UDP port.  
  
SNMP communications are generally protected by using an authentication string called community string. There are two types of community strings, a read-only
community string that gives us access to the general variables of the network and a read-write one, that also allows us to set specific variables.  
  
If you can "guess" the read-only or read-write strings you can obtain quite a bit of access you would not normally have. In addition, if Windows based
devices are configured with SNMP, often times with the RO/RW community strings, you can extract patch levels, services running, last reboot times,
usernames on the system, routes, and various other amounts of information that is valuable to an attacker.  
  
If you can find a Cisco device running a private string for example, you can actually download the entire device configuration, modify it, and upload your
own malicious config. Often the passwords themselves are level 7 encoded which means they are trivial to decode and obtain the ‘enable‘ or login password for
the specific device  
  
When querying through SNMP, there is what's called an MIB API. The MIB stands for the Management Information Base (MIB). This interface allows you to query
the device and extract information.  
  
  
RFC MIB = http://www.oidview.com/mibs/0/HOST-RESOURCES-MIB.html  
  
VENDOR SPECIFIC = http://www.oidview.com/mibs/detail.html  
  
MICROSOFT = http://www.oidview.com/mibs/311/md-311-1.html  
  
```  
1.3.6.1.2.1.25.1.6.0 System Processes  
1.3.6.1.2.1.25.4.2.1.2 Running Programs  
1.3.6.1.2.1.25.4.2.1.4 Processes Path  
1.3.6.1.2.1.25.2.3.1.4 Storage Units  
1.3.6.1.2.1.25.6.3.1.2 Software Name  
1.3.6.1.4.1.77.1.2.25 User Accounts  
1.3.6.1.2.1.6.13.1.3 TCP Local Ports  
```

## SNMPWalk

Snmpwalk is a popular tool for testing SNMP. This tool acts as SNMP client, and we can use it for our penetration testing where we require making requests
to the SNMP service on the target host.  
  
### Common Usage

Shows Help  
```  
snmpwalk --help
```  

Looks at one host, specifying a community string of public  
```  
snmpwalk -v 1 -c public 10.11.1.22
```  

Here we are asking snmpwalk to walk one node subset in the MIB  
```  
snmpwalk -v 1 -c public 10.11.1.22 .1.3.6.1.2.1.25.1.6.0
```  

`-M` option allows us to include a file of MIB's to look throgh  
```  
snmpwalk -v 1 -c public 10.11.1.22 -M mibs.txt
```  

Some MIB nodes display in hex, in order to force display of those in ASCI we add the -Oa options  
```  
snmpwalk -v 1 -c public 10.11.1.73 iso.3.6.1.2.1.2.2.1.2 -Oa
```  

Adding the `n` to the `-O` option means it displays full OID's  
```  
snmpwalk -v 1 -c public 10.11.1.73 iso.3.6.1.2.1.2.2.1.2 -Oan
```  

Searching snmpwalk for a specific MID string.  
```  
for ip in $(cat out | cut -d" " -f1 | sort -u | grep "10.11"); do echo $ip ;
snmpwalk -c public -v1 $ip 1.3.6.1.2.1.25.4.2.1.2 ; done
```  

Filtering output of a lookup for a specific string  
```  
snmpwalk -v 1 -c public 10.11.1.73 -Oa | grep "iso.3.6.1.2.1.2.2.1.2"
```  

### Important MIB's ID for Pentestesters

#### Windows  
  
```
RUNNING PROCESSES 1.3.6.1.2.1.25.4.2.1.2  
INSTALLED SOFTWARE 1.3.6.1.2.1.25.6.3.1.2  
SYSTEM INFO 1.3.6.1.2.1.1.1  
HOSTNAME 1.3.6.1.2.1.1.5  
DOMAIN 1.3.6.1.4.1.77.1.4.1  
UPTIME 1.3.6.1.2.1.1.3  
USERS 1.3.6.1.4.1.77.1.2.25  
SHARES 1.3.6.1.4.1.77.1.2.27  
DISKS 1.3.6.1.2.1.25.2.3.1.3  
SERVICES 1.3.6.1.4.1.77.1.2.3.1.1  
LISTENING TCP PORTS 1.3.6.1.2.1.6.13.1.3.0.0.0.0  
LISTENING UDP PORTS 1.3.6.1.2.1.7.5.1.2.0.0.0.0  
SYSTEM PROCESSES 1.3.6.1.2.1.25.1.6.0  
STORAGE UNITS 1.3.6.1.2.1.25.2.3.1.4  
```

#### Linux

```  
RUNNING PROCESSES 1.3.6.1.2.1.25.4.2.1.2  
SYSTEM INFO 1.3.6.1.2.1.1.1  
HOSTNAME 1.3.6.1.2.1.1.5  
UPTIME 1.3.6.1.2.1.1.3  
MOUNTPOINTS 1.3.6.1.2.1.25.2.3.1.3  
RUNNING SOFTWARE PATHS 1.3.6.1.2.1.25.4.2.1.4  
LISTENING UDP PORTS 1.3.6.1.2.1.7.5.1.2.0.0.0.0  
LISTENING TCP PORTS 1.3.6.1.2.1.6.13.1.3.0.0.0.0  
SYSTEM PROCESSES 1.3.6.1.2.1.25.1.6.0  
STORAGE UNITS 1.3.6.1.2.1.25.2.3.1.4  
```

#### Cisco

```
LAST TERMINAL USERS 1.3.6.1.4.1.9.9.43.1.1.6.1.8  
INTERFACES 1.3.6.1.2.1.2.2.1.2  
SYSTEM INFO 1.3.6.1.2.1.1.1  
HOSTNAME 1.3.6.1.2.1.1.5  
SNMPcommunities 1.3.6.1.6.3.12.1.3.1.4  
UPTIME 1.3.6.1.2.1.1.3  
IP ADDRESSES 1.3.6.1.2.1.4.20.1.1  
INTERFACE DESCRIPTIONS 1.3.6.1.2.1.31.1.1.1.18  
HARDWARE 1.3.6.1.2.1.47.1.1.1.1.2  
TACACS SERVER 1.3.6.1.4.1.9.2.1.5  
LOGMESSAGES 1.3.6.1.4.1.9.9.41.1.2.3.1.5  
PROCESSES 1.3.6.1.4.1.9.9.109.1.2.1.1.2  
SNMP TRAP SERVER 1.3.6.1.6.3.12.1.2.1.7`
```

## snmp-check

`snmp-check` allows you to enumerate the SNMP devices and places the output in a very human readable friendly format. It could be useful for 
penetration testing or systems monitoring.

```
snmp-check -w ip_address
-p, --port : SNMP port; default port is 161;
-c, --community: SNMP community; default is public;
-v, --version : SNMP version (1,2c); default is 1;
-w, --write : detect write access (separate action by enumeration);
-d, --disable_tcp : disable 'TCP
```

## snmpset

If we find write access to an SNMP server we can use snmpset to write to it.

`snmpset` - communicates with a network entity using SNMP SET requests

## Onesixtyone

Onesixtyone is an SNMP scanner that sends multiple SNMP requests to multiple IP addresses, trying different community strings and waiting for replies.

Key Features
* Very fast scanning speed (over 50,000 guesses per second)
* Scan a single host or thousands of hosts at the same time
* You can tune scan speed to support both LAN and WAN testing

### Prepping Your Dictionary

SNMP strings in onesixtyone can't be longer than 16 characters at time of this writing. If you have an error with your dictionary file you can find out which words are causing problems with the following command.

```  
cat snmpstrings.txt | grep -xv .\{0,16\}
```  

There is a current approve request in onesixtyone to increase the size as currently the following string which is default in the kali linux wordlists for snmp community strings can not be parsed for onesixtyone.

This is the APC default community string:
```  
TENmanUFactOryPOWER
```  

However if you wish to use onesixtyone and you need to filter your list down you can do the following to pipe all the words between 0-16 characters into a new list
```  
cat snmpstrings.txt | grep -x '.\{0,16\}' > snmp.txt
```  

### Example usage

Scanning Hosts/Community Strings From Files
```
onesixtyone -c snmp.txt -i opensnmp.txt 
-i = This flag allows you to input hosts from a list
-c = This flag allows you to input community strings from a list.
```

Scanning a single host.
```  
onesixtyone -c /dict/snmp.txt 10.0.0.1
```  

### Key Flags
```  
-c = file with community string names to try
-i = file with target hosts
-o = output log
-d = debug mode
-w = Wait in milliseconds between sending packets (default 10)
-q = Quiet mode, do not print to std out.
```  

Brute Force And Scan Network SNMP bruteforce script.
```
root@kali:/# echo "public" > community
root@kali:/# echo "private" > community
root@kali:/# echo "manager" > community
root@kali:/# for ip in $(seq 200 254); do echo 192.168.19.$ip ;done > ips.txt
root@kali:/# onesixtyone -c community -i ips.txt
```

## SNMP with NMAP

This is a standard network scan looking for potential snmp open port hosts.

```bash
nmap -Pn -sU -sV -p 161 --open 10.0.0.1/24
-Pn = Treat all hosts as online 
-sU = Specifys a UDP Scan
-p = Specify the port
-sV = Specify to perform service enumeration
--open = Returns on Open Ports
```

This should hopefully tell us the version number of the SNMP. Now we can run nmap with scripts to further enumerate
```  
nmap -Pn -sU -p 161 10.0.0.1 --script=snmp-brute
```  

### NMAP NSE Scripts for SNMP

```  
cd "/usr/share/nmap/scripts"
ls -la | grep "snmp"
```

#### Flags

```  
snmp-brute = Attempts to find an SNMP community string by brute force guessing
snmp-hh3c-logins = Attempts to enumerate Huawei / HP/H3C Locally Defined Users
snmp-info = Extracts basic information from an SNMPv3 GET request
snmp-interfaces = Attempts to enumerate network interfaces through SNMP
snmp-ios-config = Attempts to download Cisco router IOS configuration files
snmp-netstat = Attempts to query SNMP for a netstat like output
snmp-processes = Attempts to enumerate running processes through SNMP
snmp-sysdescr = Attempts to extract system information from an SNMP version 1 service
snmp-win32-services = Attempts to enumerate Windows services through SNMP
snmp-win32-shares = Attempts to enumerate Windows Shares through SNMP.
snmp-win32-software = Attempts to enumerate installed software through SNMP.
snmp-win32-users = Attempts to enumerate Windows user accounts through SNMP
```