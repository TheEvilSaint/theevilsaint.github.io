---
layout: post
title: 'Vulnerabilities in SSL & TLS :- POODLE'
date: '2022-01-18'
description: 'In this article, we will look at CVE-2014-3566, also known as the Padding Oracle On Downgraded Legacy (POODLE) vulnerability, which results from support for SSL version 3.0. This attack requires a person-in-the-middle context to force the use of SSL v3.0, which contains an Oracle attack.'
coverimage: poodle.jpg
tags: poodle ssl tls
published: true
posttype: article
categories: article
---
## Main Points

- Discovered by Google security team on October the 14th, 2014.
- The attack is on SSL 3.0 (SSLv3), an obsolete and insecure protocol.
- Attacker needs to somehow force the client and the server to agree to use the SSL 3.0 protocol. An attacker can do this by tampering with the SSL negotiation process.
- SSL handshake will try to use the most recently supported version of TLS (starting with say TLS 1.2) at both the client and the server. However, for compatibility reasons, many clients have implemented a fallback mechanism that lets them retry a failed negotiation by using an older version. If an attacker interferes with the SSL negotiation process to disrupt it, the client will then fall back successively to TLS 1.1, TLS 1.0, and finally to SSL 3.0.
- The vulnerability exploits the way the SSL version 3.0 handles padded bytes when used with Cipher Block Chaining (CBC) mode of operation.
- Due to SSLv3's under-specification of the contents of the CBC padding bytes, and since SSLv3 did not say what the padding bytes should be, SSL implementations cannot check those padding bytes which opens SSLv3 up to an oracle attack.
- This may allow a person-in-the-middle attacker to decrypt a selected byte of ciphertext.
- To decrypt the stream requires as few as 256 SSL version 3 connections; however, they would need to force a victim application to repeatedly send the same data over a newly created SSL version 3.0 connection. This would require social engineering.
- To exploit this as a web vulnerability, the attacker must possess the following:
    1. The attacker must control the connection between the client and the server.
    2. The attacker must be able to inject code into the victim's browser (e.g. JavaScript code) to launch the attack successfully.

## Quick Reference

### Description

Via a person-in-the-middle position, the victim is forced to downgrade to SSL v3.0, which contains an oracle attack. Whether a server or a client - and you do not wish to fall short of this vulnerability - then disable support for SSL completely as per the remediation section. 

### Abbreviation

POODLE

### Name

Padding Oracle On Downgraded Legacy

### CVE Number

CVE-2014-3566

### Type of Vulnerability

A padding-oracle attack - The SSL protocol 3.0 uses nondeterministic CBC padding, which makes it easier for person-in-the-middle attackers to obtain cleartext data via a padding-oracle attack.

### Affected

This vulnerability affects all SSL version 3.0 compliant products.

### Remediation

Do not Support SSL version 3 or lower.

Please note that this will break the website connection for IE6 users, should this browser be important for some reason. 

```bash
For Apache: SSLProtocol all -SSLv2 -SSLv3
For Nginx: ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
```

## How to Configure Web Server to Mitigate POODLE Vulnerability

To protect our web server against [POODLE](https://evilsaint.com/article/vulnerabilities-ssl-tls-poodle/), it is recommended to stop support for SSL version 3.0 or lower. This, however, would still leave us vulnerable to [BEAST](https://evilsaint.com/article/vulnerabilities-ssl-tls-beast/), where it is recommended to stop support for TLS 1.0 as well. 

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