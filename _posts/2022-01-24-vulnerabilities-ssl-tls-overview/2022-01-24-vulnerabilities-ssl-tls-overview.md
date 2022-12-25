---
layout: post
title: 'Vulnerabilities in SSL & TLS :- Overview'
date: '2022-01-24'
description: 'Since January 6th, we have been looking at individual SSL/TLS vulnerabilities. This article will provide an overview of the series and provide background information on SSL/TLS for those who are unfamiliar with the subject. If you scroll to the bottom, you will find a handy reference sheet for when you are on the phone with customers.'
coverimage: overview.jpg
tags: ssl tls
published: true
posttype: article
categories: article
---
Since January 6th, we have been looking at individual SSL/TLS vulnerabilities. This article will provide an overview of the series and provide background information on SSL/TLS for those who are unfamiliar with the subject. 

A timeline of SSL and TLS development:

- SSL 2.0. Released in 1995, this version of SSL is now prohibited by the Internet Engineering Task Force (see RFC-6176).
- SSL 3.0. Released in 1996, SSL 3.0 is deprecated, but a few browsers still support it (RFC-7568).
- TLS 1.0. Released in 1999 and deprecated in 2020.
- TLS 1.1. Released in 2006 and deprecated in 2020.
- TLS 1.2. Released in 2008 and still has no security issues.
- TLS 1.3. Released in 2018 and continues to be the main protocol used today without any known vulnerabilities.

In this article series we will cover:

1. [Heartbleed](https://evilsaint.com/article/vulnerabilities-ssl-tls-heartbleed/) 
2. [SWEET32](https://evilsaint.com/article/vulnerabilities-ssl-tls-sweet32/) 
3. [DROWN](https://evilsaint.com/article/vulnerabilities-ssl-tls-drown/) 
4. [FREAK](https://evilsaint.com/article/vulnerabilities-ssl-tls-freak/) 
5. [logjam](https://evilsaint.com/article/vulnerabilities-ssl-tls-logjam/) 
6. [BEAST](https://evilsaint.com/article/vulnerabilities-ssl-tls-beast/) 
7. [BREACH](https://evilsaint.com/article/vulnerabilities-ssl-tls-breach/) 
8. [RC4 Biases](https://evilsaint.com/article/vulnerabilities-ssl-tls-rc4-byte-biases/) 
9. [CCS injection vulnerability](https://evilsaint.com/article/vulnerabilities-ssl-tls-ccs-injection-vulnerability/) 
10. [POODLE](https://evilsaint.com/article/vulnerabilities-ssl-tls-poodle/) 
11. [POODLE over TLS](https://evilsaint.com/article/vulnerabilities-ssl-tls-poodle-over-tls/) 
12. [Lucky13](https://evilsaint.com/article/vulnerabilities-ssl-tls-lucky13/) 
13. [TLS Renegotiation](https://evilsaint.com/article/vulnerabilities-ssl-tls-tls-renegotiation/) 


## Quick Guide

Right now, if you have the client on the phone, however...

Attack | CVE | Affects | Mitigation 
:-----:|:-----:|:-----|:-----
Logjam | CVE-2015-4000 | The TLS protocol 1.2 and earlier when a DHE_EXPORT cipher suite is enabled. | Enforce DH group sizes of 1,024 bits and above
POODLE | CVE-2014-3566 | SSL version 3.0 | Disable support for SSL 3.0
BEAST | CVE-2011-3389  | TLS 1.0 or any version of SSL | Enforce TLS 1.1 and higher
CRIME | 2012-4929  | TLS compression| Disable TLS compression
BREACH and TIME | CVE-2013-3587 | HTTP compression | Disable HTTP compression
Lucky 13 | CVE-2013-0169 | TLS protocol 1.1 and 1.2 and the DTLS protocol 1.0 and 1.2 in several vendors products | Disable CBC ciphers if your server implementation is flawed
RC4 byte biases | CVE-2013-2566  | Connections supporting RC4 | Disable support for RC4 cipher suites
FREAK | CVE-2015-0204  | Any system willing to negotiate RSA Export Keys.  | Disable support for weak export-grade ciphers
SWEET32 | CVE-2016–2183 and CVE-2016–6329 | Long term client browser foothold | Do not support or negotiate 3DES cipher-suites. At a minimum, AES should be preferred over 3DES. Limit length of TLS session.