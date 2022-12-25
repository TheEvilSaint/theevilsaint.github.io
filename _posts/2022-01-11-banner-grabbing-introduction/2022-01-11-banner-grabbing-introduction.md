---
layout: post
title: "Banner Grabbing Introduction"
date: "2022-01-11"
description: "In this tutorial we will look at banner grabbing using various software. Banner grabbing is a reconnaissance technique that retrieves software banner information by querying services running on an open port. This banner can typically contain important information about the service; potentially, we can find information such as the name and version of the software. If we can successfully identify the software, we can check for known vulnerabilities."
coverimage: Banner_Grabbing_Introduction.jpg
tags: banner-grabbing enumeration
published: true
posttype: article
categories: tutorial
---

## Netcat

By sending the following HTTP request via:

```bash
nc 192.168.0.10 80 
GET / HTTP/1.1  
Host: vulnerable 
<enter>  
<enter>
```

Saving Banners to a file and parsing in input to ncat rather than typing
```
echo "GET / HTTP/1.1" > input.txt
echo "" >> input.txt
echo "" >> input.txt
nc -nvv -o banners.txt 192.168.0.10 80 < input.txt
```

  
It is possible to retrieve information on the version of PHP and the web server used just by observing the HTTP headers sent back by the server:  
```
HTTP/1.1 200 OK  
Date: Thu, 24 Nov 2011 04:40:51 GMT  
Server: Apache/2.2.16 (Debian)  
X-Powered-By: PHP/5.3.3-7+squeeze3  
Vary: Accept-Encoding  
Content-Length: 1335  
Content-Type: text/html  
```

We can try a banner grab with a spoofed User Agent Browser
```bash
nc 192.168.0.10 80  
GET / HTTP/1.1  
Host: 192.168.0.10  
User-Agent: SPOOFED-BROWSER  
Referrer: K0NSP1RACY.COM  
<enter>  
<enter>
```
  
  
  
## HTTPS
  
If the application was only available via HTTPS, telnet or netcat would not be able to communicate with the server, the tool openssl can be used:  
```
root@kali:~/# openssl s_client -connect vulnerable:443  
```

## Curl

```bash
root@root:/# curl -I domain.co.uk
HTTP/1.1 200 OK
Date: Wed, 05 Apr 2017 20:26:26 GMT
Server: Apache/2.4.23 (Unix)
X-Powered-By: PHP/5.4.45
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Pragma: no-cache
Set-Cookie: PHPSESSID=cfn55sh9vqnr0dm55go05a2852; path=/
X-Hostname: sweb14
Content-Type: text/html; charset=UTF-8
```


## NMAP

```bash
root@evilsaint:/# nmap -sV -p 80 --script=banner yahoo.co.uk

Starting Nmap 7.40 ( https://nmap.org ) at 2017-04-05 21:32 BST
Nmap scan report for yahoo.co.uk (217.12.15.37)
Host is up (0.0052s latency).
Other addresses for yahoo.co.uk (not scanned): 72.30.203.4 206.190.42.177
PORT   STATE SERVICE    VERSION
80/tcp open  http-proxy Apache Traffic Server
|_http-server-header: ATS
Service Info: Host: media-router-rc3.prod.media.ir2.yahoo.com

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.35 seconds
```

## Telnet

Banner Grab FTP
```
telnet ip_address 21
```

Banner Grab SSH
```
telnet ip_address 22
```

Banner Grab Telnet
```
telnet ip_address 
```

Banner Grab HTTP
```bash
root@root:/# telnet domain.co.uk 80
Trying 46.32.240.39...
Connected to domain.co.uk.
Escape character is '^]'.
GET / HTTP/1.1

HTTP/1.1 400 Bad Request
Date: Wed, 05 Apr 2017 20:50:08 GMT
Server: Apache/2.4.23 (Unix)
Content-Length: 303
Content-Type: text/html; charset=iso-8859-1
```