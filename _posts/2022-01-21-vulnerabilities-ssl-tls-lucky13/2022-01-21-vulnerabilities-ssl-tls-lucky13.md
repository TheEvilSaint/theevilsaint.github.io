---
layout: post
title: 'Vulnerabilities in SSL & TLS :- Lucky13'
date: '2022-01-21'
description: 'In this article, we will look at CVE-2013-0169, also known as the Lucky 13 vulnerability, which exists within SSL and TLS. The TLS protocols 1.1 and 1.2, as well as the DTLS protocols 1.0 and 1.2, do not properly consider timing side-channel attacks when processing malformed CBC padding, allowing remote attackers to conduct plaintext-recovery.'
coverimage: lucky13.jpg
tags: lucky13 ssl tls
published: true
posttype: article
categories: article
---
## Main Points

- The attacker must use a different exploit to become a person-in-the-middle and to inject content into the network traffic stream for this attack to work.
- First reported by the Information Security Group at Royal Holloway, University of London.
- TLS headers contain 13 bytes of data for the secure handshake protocol, which can be exploited.
- When processing malformed CBC padding, the TLS protocols 1.1 and 1.2, as well as the DTLS protocols 1.0 and 1.2, as used in OpenSSL, OpenJDK, PolarSSL, and other products, do not properly consider timing side-channel attacks on a MAC check requirement, allowing remote attackers to conduct distinguishing attacks and plaintext recovery via statistical analysis of timing data for crafted packets.
- To have enough data to attack, you need to collect several days' worth of packets and traffic via a person-in-the-middle attack. This is why Lucky 13 is a theoretically possible attack vector.

## Quick Reference

### Description

The TLS protocols 1.1 and 1.2, as well as the DTLS protocols 1.0 and 1.2, do not properly consider timing side-channel attacks when processing malformed CBC padding, allowing remote attackers to conduct plaintext-recovery.

### Name

Lucky13 

### CVE Number

CVE-2013-0169

### Type of Vulnerability

Weakness in SSL and TLS themselves and not in a particular implementation of them.

### Affected

The TLS protocols 1.1 and 1.2 and the DTLS protocols 1.0 and 1.2, as used in OpenSSL, OpenJDK, PolarSSL, and other products.

### Remediation

Avoid CBC mode cipher-suites (use AEAD cipher-suites).

Use Encrypt-then-MAC TLS extension.