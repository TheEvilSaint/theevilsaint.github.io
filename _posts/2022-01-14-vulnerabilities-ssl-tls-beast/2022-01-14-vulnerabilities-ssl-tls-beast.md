---
layout: post
title: 'Vulnerabilities in SSL & TLS :- BEAST'
date: '2022-01-14'
description: 'In this article, we will look at CVE Number CVE-2011-3389, also known as BEAST. BEAST, or Browser Exploit Against SSL/TLS, is a largely theoretical attack that requires a person-in-the-middle to eavesdrop on a connection and downgrade encryption support to (vulnerable) TLS 1.0. This allows attackers to decrypt the traffic and listen in on conversations.'
coverimage: beast.jpg
tags: beast ssl tls
published: true
posttype: article
categories: article
---
## Main Points 

- The attacker must use a different exploit to become a person-in-the-middle and to inject content into the network traffic stream for this attack to work.
- The attack was carried out for the first time in 2011 by security researchers Thai Duong and Juliano Rizzo (Google Chrome developers), but the theoretical vulnerability was discovered in 2002 by Phillip Rogaway.
- Thai Duong and Juliano Rizzo created their POC in JavaScript.
- Researchers found that TLS 1.0 (and older) encryption can be broken quickly, giving the attacker an opportunity to listen in on the conversation.
- If your server supports TLS 1.0, the attacker can make it believe that this is the only protocol that the client can use. This is called a protocol downgrade attack. Then, the attacker can use the BEAST attack to eavesdrop.
- The attack leverages weaknesses in cipher block chaining (CBC) to exploit the Secure Sockets Layer (SSL) protocol
- If the attacker can get into a person-in-the-middle position while the victims are using TLS 1.0, they may silently decrypt secrets.

## Quick Reference

### Description

A vulnerability that allows a person-in-the-middle attacker to eavesdrop on a connection and downgrade encryption support to (vulnerable) TLS 1.0. This allows attackers to decrypt the traffic and listen in on conversations.

### Abbreviation

BEAST

### Name

Browser Exploit Against SSL/TLS

### CVE Number

CVE-2011-3389

### Type of Vulnerability

In the SSL protocol and TLS 1.0

### Affected

If you support TLS 1.0 or any version of SSL, then you are vulnerable to BEAST. 

Originally, the RC4 cipher was recommended for use to mitigate BEAST attacks (because it is a stream cipher, not a block cipher). However, RC4 was later found to be unsafe.

## How to Configure Web Server to Mitigate BEAST Vulnerability

To protect our web server against [POODLE](https://evilsaint.com/article/vulnerabilities-ssl-tls-poodle/) it is recommended to stop support for SSL version 3.0 or lower. This, however, would still leave us vulnerable to [BEAST](https://evilsaint.com/article/vulnerabilities-ssl-tls-beast/), where it is recommended to stop support for TLS 1.0 as well. 

### Apache Web Server

Edit the SSLProtocol directive in the ssl.conf file, which is usually located in /etc/httpd/conf.d/ssl.conf. For example, if you have:
```
SSLProtocol all
```

change it to:
```
SSLProtocol TLSv1.1 TLSv1.2
```
Then, restart httpd.

### NGINX

Edit the ssl_protocols directive in the nginx.conf file. For example, if you have:
```
ssl_protocols TLSv1.1 TLSv1.2;
```

change it to:
```
ssl_protocols TLSv1.1 TLSv1.2;
```

Then, restart nginx.

### Microsoft IIS

To disable TLS 1.0 in Microsoft IIS, you must edit the registry settings in Microsoft Windows.

1. Open the registry editor
	* Find the key `HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server`
	* Change the DWORD value of the Enabled entry to `0`.
	* Create a `DisabledByDefault` entry and change the DWORD value to `1`.

2. Repeat the above steps for all versions of SSL.