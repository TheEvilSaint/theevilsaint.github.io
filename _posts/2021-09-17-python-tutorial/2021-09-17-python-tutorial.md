---
layout: post
title: "Python Tutorial"
date: "2021-09-17"
description: ""
coverimage: python_tutorial.jpg
tags: python
published: true
posttype: article
categories: tutorial
---
---
layout: post
title: Python
weight: 10
---
# Python

sha-bang
```python
#!/usr/bin/python
```

More portable sha-bang
```python
#!/usr/bin/env python
```

SMTP Script for detecting usernames
```python
#!/usr/bin/env python
import socket
import sys
if len(sys.argv) != 3:
    print "Usage: smtp.py <list usernames> <list of ips>"
    sys.exit(0)

usernames = sys.argv[1]
ip_address = sys.argv[2]


with open(ip_address, "r") as ips:

    for ip in ips:

        print "[x]"+ip
        # Create a Socket
        s=socket.socket(socket.AF_INET, socket.SOCK_STREAM)

        # Connect to the Server
        connect=s.connect((ip,25))

        # Receive the banner
        banner=s.recv(1024)
        print banner

        with open(usernames, "r") as users:
            for user in users:
                print "[x]"+user
                # VRFY a user
                s.send('VRFY ' + user + '\r\n')
                result=s.recv(1024)
                print result

        # Close the socket
        s.close()
```

## Scapy


Lists supported protocol layers. If a protocol layer is given as parameter, lists its fields and types of fields.
```python
ls()
```

We can give the `ls` command individual protocols
```python
ls(ARP)
ls(ICMP)
```

Lists some user commands. If a command is given as parameter, its documentation is displayed.
```python
lsc()
```

This object contains the configuration.
```python
conf
```

### Sniffing

Sniff 100 packets on interface `eth0`. Important no quotes around count integer.
```python
pkts = sniff(iface="eth0", count=100)
```

Sniff 10 `Arp` packets on interface `eth0`. Important no quotes around count integer.
```python
arppkts = sniff(iface="eth0", filter="arp", count=10)
```

Display a packet summary after collection
```python
pkts
```

Look at one packet (indexed from 0)
```python
print pkts[0]
```

Shown in more user friendly way
```python
print pkts[0].show()
```

Shown a summary
```python
print pkts[0].show()
```

Write to a pcap file
```python
wrpcap("/tmp/tcpdump-demo", mypkts)
```

Read from a pcap file
```python
read_pkts = rdpcap("/tmp/tcpdump-practice")
```

Send an icmp packet
```python
>>> pkt = IP(dst="192.168.1.69")/ICMP()/"EvilSaint was here so look out"
>>> send(pkt)
```

### Scanning

Arp scanner
```python
#!/usr/bin/python

from scapy.all import *

# Subnet Scanner
for host in range(64,254):
        ip = "192.168.1." +str(host)
        print ip
        arpRequest = Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst=ip, hwdst="ff:ff:ff:ff:ff:ff")
        arpResponse = srp1(arpRequest, timeout=1, verbose=0)
        if arpResponse:
                print "IP: " + arpResponse.psrc + " MAC: " + arpResponse.hwsrc
```

### Scraping

Print Web Page Headers
```python
import urllib
httpResponse = urllib.urlopen("http://www.nottingham.ac.uk/")
for header, value in httpResponse.headers.items():
    print header + ' : ' + value
```

Read the html on the page
```
httpResponse.read()
```

#### BeautifulSoup


Grab Page and Parse it script
```python
from bs4 import BeautifulSoup
import urllib

html = urllib.urlopen("http://www.securitytube.net/video/3000")
bt = BeautifulSoup(html.read(), "lxml")

bt.title
# <title>Defcon 19  - The Art Of Trolling</title>

bt.title.string
# u'Defcon 19  - The Art Of Trolling'

all_meta_tags = bt.find_all('meta')
print all_meta_tags
# [<meta content="6xTaa3p8o4OQd0L-iD2eRvaDjJR8rxUdSyZ1Etm5LMI" name="google-site-verification"/>, <meta content="text/html;charset=unicode-escape" http-equiv="Content-Type"/>]

print all_meta_tags[0]
# <meta content="6xTaa3p8o4OQd0L-iD2eRvaDjJR8rxUdSyZ1Etm5LMI" name="google-site-verification"/>
print all_meta_tags[0]['content']
# 6xTaa3p8o4OQd0L-iD2eRvaDjJR8rxUdSyZ1Etm5LMI

atags = bt.find_all('a')
for link in atags:
    print link['href'], link.string

# Get all the text on a page
bt.get_text()
```