---
layout: post
title: 'Vulnerabilities in SSL & TLS :- CRIME'
date: '2022-01-13'
description: 'This article will examine CVE 2012-4929, also known as the CRIME vulnerability. To obtain plaintext HTTP headers, attackers with person-in-the-middle context can compare length differences between a string in an HTTP request and an unknown string in an HTTP header.'
coverimage: crime.jpg
tags: crime ssl tls
published: true
posttype: article
categories: article
---
## Main Points 

- The attack was discovered by security researchers Juliano Rizzo and Thai Duong, who cooked up the BEAST SSL exploit.
- For this attack to work, the attacker must use a different exploit to become a person-in-the-middle and inject content into the network traffic stream.
- The CRIME (Compression Ratio Info-leak Made Easy) attack is a vulnerability in SSL/TLS compression.
- CRIME is a side-channel attack that uses the compressed size of HTTP requests to discover session tokens or other secret information.
- The technique exploits SSL/TLS-protected web sessions when they use one of two data-compression schemes (DEFLATE and gzip) built into the protocol and used to reduce network congestion or the loading time of web pages.
- In order to carry out the attack, the attacker must have control over the plaintext as well as the ability to intercept the encrypted message.
- On secret web cookies transmitted over compressed HTTPS or SPDY connections, this attack exposes cookie data to session hijacking.
- When the size of the compressed content is reduced, it can be inferred that some part of the injected content is likely to match some part of the source.

## Quick Reference

### Description

CRIME is a vulnerability in SSL/TLS compression that allows attackers with person-in-the-middle context to compare length differences between a string in an HTTP request and an unknown string in an HTTP header.

### Abbreviation

CRIME 

### Name

Compression Ratio Info-leak Made Easy

### CVE Number

CVE-2012-4929

### Type of Vulnerability

The vulnerability is within TLS compression. 

### Affected

TLS compression is required for the connection. The attacker requires a person-in-the-middle position, the ability to control plaintext on a website visited by the victim, and the ability to intercept encrypted messages.

### Remediation

Disable SSL/TLS compression to prevent the CRIME attack. CRIME can be defeated by preventing the use of compression, either at the client end, by disabling compression of SPDY requests in the browser, or by the website preventing the use of data compression on such transactions using the TLS protocol negotiation features.