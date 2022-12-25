---
layout: post
title: "Install A DNS Server For Out Of Band Attacks"
date: "2022-01-01"
description: "An out of band DNS Server can be useful for a variety of use cases during a pentest or Red Team engagement. Two common purposes are for DNS Data Exfiltration and DNS Spoofing.  Maradns makes it easy for us to do this."
coverimage: Install_A_DNS_Server_For_Out_Of_Band_Attacks.jpg
tags: dns
published: true
posttype: article
categories: tutorial
---
Update the Sources List
```
apt-get update
```

Upgrade the Server
```
apt-get upgrade
```

Install the Mardns server
```
apt-get install maradns
```

Move into the Maradns folder then start editing the config
```
cd /etc/maradns/
nano mararc 
```

Make the following edits to the `marac` config file
```
csv2 = {}
csv2["evilsaint.com."] = "db.evilsaint.com"
bind_address = "45.32.176.126"
```

Create the Zone File
```
nano db.evilsaint.com
```

Inside our file
```
%        NS        ns.% ~
*.%      A         45.32.176.126 ~
```


Run the DNS Server
```
maradns -f mararcls
```