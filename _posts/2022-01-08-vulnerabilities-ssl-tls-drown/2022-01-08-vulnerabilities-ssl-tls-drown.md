---
layout: post
title: 'Vulnerabilities in SSL & TLS :- DROWN'
date: '2022-01-08'
description: 'In this article we look at CVE Number CVE-2016-0800, also known as the DROWN vulnerability. Decrypting RSA with Obsolete and Weakened eNcryption (DROWN) is an attack that demonstrates that even if you do not allow SSLv2, simply supporting it can be dangerous.'
coverimage: drown.jpg
tags: drown ssl tls
published: true
posttype: article
categories: article
---
## Main Points

- For this attack to work, the attacker must use a different exploit to become a person-in-the-middle and inject content into the network traffic stream.
- At the time of public disclosure on March 2016, it was indicated 33% of all HTTPS servers were vulnerable to the attack.
- At the time of the public disclosure in March 2016, it was estimated that 33% of all HTTPS servers were vulnerable to the attack.
- Takes hours to retrieve data unless a specific version of OpenSSL is used, which is known to allow for a one minute average decryption rate rather than a four hour average decryption rate.
- Another attack that relies on export grade cryptography; introduced to comply with US government restrictions in the 1990s.
- DROWN demonstrates that simply allowing SSLv2, even if no legitimate clients use it, poses a risk to modern servers and clients. It allows an attacker to decrypt modern TLS connections between up-to-date clients and servers by sending probes to any server that supports SSLv2 and uses the same private key.
- At the time of publication (August 2016), the team behind the paper "DROWN: Breaking TLS Using SSLv2" was able to attack and decrypt a TLS 1.2 handshake using 2048-bit RSA in under eight hours, at a cost of $440 on Amazon EC2.

## Quick Reference 

### Description

Decrypting RSA with Obsolete and Weakened eNcryption (DROWN) is an attack that demonstrates that even if you do not allow SSLv2, simply supporting it can be dangerous. It allows an attacker to decrypt TLS connections between clients and servers.

### Abbreviation

DROWN

### Name

Decrypting RSA with Obsolete and Weakened eNcryption

### CVE Number

CVE-2016-0800

### Type of Vulnerability

Within the SSL v2.0 protocol when paired with export grade ciphers. 

### Affected

Two types of systems are vulnerable. 

1. A system supporting SSLv2. (Systems that support SSLv2 negotiation are still surprisingly common!)
2. A system that uses its private key on any other server that allows SSLv2 connections, even if it is for a different protocol. TLS, for example, is another protocol. The original system is vulnerable as long as SSLv2 can be negotiated on the second system.

### Remediation

Remove support for SSL v2, and best practises would suggest removing anything older than TLS 1.1. TLS 1.0 does not need to be supported. 

Even if you are confident that you have disabled SSLv2 on your HTTPS server, you could be reusing your private key on another server (such as an email server) that does support SSLv2. We recommend manually inspecting all servers that use your private key.