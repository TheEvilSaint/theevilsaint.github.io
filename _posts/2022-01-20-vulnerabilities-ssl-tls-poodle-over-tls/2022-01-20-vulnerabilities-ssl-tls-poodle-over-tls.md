---
layout: post
title: 'Vulnerabilities in SSL & TLS :- POODLE over TLS'
date: '2022-01-20'
description: 'In this article, we will look at CVE-2014-8730, also known as the Padding Oracle On Downgraded Legacy Over Transport Layer Security (POODLE over TLS) vulnerability. The vulnerability is caused by a TLS server failing to verify the block cipher padding when used with a cipher suite utilising a block cipher, such as AES and DES. Due to the lack of padding checking, encrypted TLS traffic can be decrypted.'
coverimage: poodle_over_tls.jpg
tags: poodle-over-tls ssl tls
published: true
posttype: article
categories: article
---
## Main Points

- The attacker must use a different exploit to become a person-in-the-middle and to inject content into the network traffic stream for this attack to work.
- Uses a padding-oracle attack in the CBC padding because of the vulnerable TLS implementation.

## Quick Reference

### Description

This is the POODLE attack; however, the writing over the CBC padding is possible due to TLS implementation not checking cipher padding. 

### Abbreviation

POODLE over TLS 

### Name

Padding Oracle On Downgraded Legacy Over Transport Layer Security

### CVE Number

CVE-2014-8730

### Type of Vulnerability

The vulnerability is caused by the TLS server failing to verify block cipher padding when used with a cipher suite that utilises a block cipher, such as AES and DES. Due to the lack of padding checking, encrypted TLS traffic can be decrypted.

### Affected

The SSL profiles components in F5 BIG-IP LTM, APM, and ASM 10.0.0 through 10.2.4 and 11.0.0 through 11.5.1
AAM 11.4.0 through 11.5.1, AFM 11.3.0 through 11.5.1, Analytics 11.0.0 through 11.5.1, Edge Gateway, WebAccelerator, and WOM 10.1.0 through 10.2.4 and 11.0.0 through 11.3.0, PEM 11.3.0 through 11.6.0, and PSM 10.0.0 through 10.2.4 and 11.0.0 through 11.4.1 and BIG-IQ Cloud and Security 4.0.0 through 4.4.0 and Device 4.2.0 through 4.4.0, when using TLS 1.x before TLS 1.2, does not properly check CBC padding bytes when terminating connections.

### Remediation

Check CBC padding in TLS stack. 

Patch vulnerable TLS implementations (ask vendor).