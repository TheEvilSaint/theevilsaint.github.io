---
layout: post
title: 'Vulnerabilities in SSL & TLS :- Heartbleed'
date: '2022-01-06'
description: 'In this article, we look at CVE Number CVE-2014-0160 or what is commonly referred to as the Heartbleed vulnerability; a buffer overflow in the Heartbeat extension of OpenSSL. A malicious client could send a specially crafted packet to disclose a limited portion of the server or computers memory per request from a connected client or server. The disclosed portions of memory could include sensitive information, such as private keys, names, usernames, passwords and/or any other data on the system.'
coverimage: Heartbleed.jpg
tags: heartbleed openssl ssl tls
published: true
posttype: article
categories: article
---
## Main Points

- **Does not** require a person-in-the-middle position to work -- unlike many other attacks in our "Vulnerabilities in SSL/TLS" series.
- Found in OpenSSL's implementation of the SSL/TLS protocol -- rather than the protocol itself or one of its components.
- OpenSSL 1.0.1 added a "Heartbeat" extension in December 2011 whose goal was to avoid the need to re-establish SSL sessions and extend their lifespan. Later, a flaw was introduced into the Hearbeat extension's code.
- Apache and Nginx power much of the internet's infrastructure and by default use OpenSSL. Some sources claim that at the time of the Heartbleed attack, 17% of the internet was vulnerable.
- Heartbleed is a programming error. The TLS heartbeat extension lacks a bounds check, causing incorrect input validation. The extension returns memory contents without checking how much is returned.
- PHD Student Robin Seggelmann, co-author of the Heartbeat extensions RFC, introduced the bug into the code. Stephen N. Henson, one of OpenSSL's four core developers, reviewed the code. Henson overlooked a flaw in Seggelmann's implementation, which was introduced into OpenSSL's source code repository on December 31, 2011. The flaw spread rapidly with the release of OpenSSL 1.0.1 on March 14, 2012, with Heartbeat support enabled by default. 1.0.1g, released on April 7, 2014.
- Affects either a server or a client using a vulnerable OpenSSL TLS instance.

## Quick Reference

### Abbreviation

No abbreviation, just referred to as Heartbleed. 

### Name

Heartbleed

### CVE Number

CVE-2014-0160

### Where is Vulnerability

The vulnerability is in the OpenSSL implementation of SSL/TLS and not the SSL/TLS protocol itself.  

### Affected

Only 1.0.1 and 1.0.2-beta releases of OpenSSL are affected including 1.0.1f and 1.0.2-beta1.

### Remediation

As long as the vulnerable version of OpenSSL is in use, it can be abused. Upgrade to a patched version of OpenSSL.