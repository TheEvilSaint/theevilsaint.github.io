---
layout: post
title: 'Vulnerabilities in SSL & TLS :- Logjam'
date: '2022-01-10'
description: 'In this article we will look at the SSL Logjam vulnerability. This is a person-in-the-middle attack, similar to FREAK, that exposes Export Grade cipher suites. This time, Diffie-Hellman is used instead of RSA.'
coverimage: logjam.jpg
tags: logjam ssl tls
published: true
posttype: article
categories: article
---
## Main Points

- For this attack to work, the attacker must use a different exploit to become a person-in-the-middle and inject content into the network traffic stream.
- Very similar to FREAK except this time, Diffie-Hellman is used instead of RSA.
- Diffie-Hellman key exchange is a popular cryptographic algorithm that allows Internet protocols to agree on a shared key and negotiate a secure connection. It is essential to many protocols, including HTTPS, SSH, IPsec, SMTPS, and those that rely on TLS.
- The Logjam vulnerability allows a person-in-the-middle attacker to downgrade vulnerable TLS connections to 512-bit export-grade cryptography.

## Quick Reference

### Description

The SSL Logjam vulnerability allows attackers within person-in-the-middle context to exposes Export Grade cipher suites to downgrade TLS connections.

### Name

Logjam

### CVE Number

Closest thing to an official CVE number is CVE-2015-4000. 

### Type of Vulnerability

Method for attacking Die-Hellman (DH) key exchange

### Affected

The TLS protocol 1.2 and earlier when a DHE_EXPORT cipher suite is enabled. 

### Remediation

 Disable support for export cipher suites and use a 2048-bit Diffie-Hellman group.