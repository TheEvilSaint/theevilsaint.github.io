---
layout: post
title: 'Vulnerabilities in SSL & TLS :- Sweet32'
date: '2022-01-07'
description: 'In this article, we will look at CVE Numbers CVE-2016–2183 and CVE-2016–6329, also known as the Sweet32 attack. The attack which involves collecting SSL traffic using legacy block ciphers via a person-in-the-middle context and subjecting it to a collision attack.'
coverimage: Sweet32.jpg
tags: ssl sweet32 tls
published: true
posttype: article
categories: article
---
## Main Points

- Sweet32 takes advantage of weaknesses in the design of some ciphers.
- Allows an attacker to recover small portions of plaintext encrypted with 64-bit block ciphers (such as Triple-DES and Blowfish).
- Based on the use of legacy block ciphers, which are vulnerable to a practical collision attack when used in CBC mode. A simple birthday attack can be used to identify 64-bit block cipher collisions when using the CBC mode of operation. When a collision occurs, it means that the input and output are the same, allowing the encrypted data to be exfiltrated.
- The use of a 64-bit block ciphers is likely to produce a collision after 32 GB of data, but for a practical attack the researchers found that up to 785 GB of data is required.
- A specific weakness in the OpenSSL implementation of SSLv2 allows for a 'special DROWN attack,' which greatly reduces the effort required to break the encryption, allowing for real-time person-in-the-middle attacks.

## Quick Reference

### Description

The attack which involves collecting SSL traffic using legacy block ciphers via a person-in-the-middle context and subjecting it to a collision attack.

### Abbreviation

SWEET32

### Name

SWEET32

### CVE Number

CVE-2016–2183 and CVE-2016–6329

### Type of Vulnerability

The attack takes advantage of flaws in the design of some block ciphers.

### Affected

Carrying out the TLS variant of the Sweet32 attack successfully requires a very specific set of capabilities on the part of the attacker:

- The attacker must be able to run JavaScript in the victim's browser to generate data for transmission to the server.
- The attacker must keep the victim on the malicious JavaScript page for one to two days to generate enough ciphertext blocks to find a collision.

### Remediation

Do not support or negotiate 3DES cipher-suites. At a minimum, AES should be preferred over 3DES. Limit the length of the TLS session.