---
layout: post
title: 'Vulnerabilities in SSL & TLS :- BREACH'
date: '2022-01-15'
description: 'In this article we look at CVE Number CVE-2013-3587 or what is commonly referred as the BREACH attack. The HTTPS protocol can encrypt compressed data without properly obfuscating the length of the unencrypted data, which makes it easier for man-in-the-middle (MitM) attackers to obtain plaintext secret values by observing length differences during a series of guesses in which a string in an HTTP request URL potentially matches an unknown string in an HTTP response body.'
coverimage: breach.jpg
tags: breach ssl tls
published: true
posttype: article
categories: article
---
## Main Points 

- The attacker must use a different exploit to become a man-in-the-middle and to inject content into the network traffic stream for this attack to work.
- The BREACH attack is very similar to how CRIME work but BREACH exploits of vulnerability within the HTTP compression allowing an attacker to identify if text exists within a page.
- BREACH attack on HTTP request responses, whereas CRIME looks at HTTP requests.
- The BREACH attack is agnostic to the version of TLS/SSL, and does not require TLS-layer compression (only HTTP compression).
- The BREACH attack works against any cipher suite.
- The BREACH attack against stream ciphers is simpler as when performing attacks against block ciphers additional work must be done to align the output to the cipher text blocks.
- The BREACH attack can be exploited with just a few thousand requests, and can be executed in under a minute**.** The number of requests required will depend on the secret size. The power of the attack comes from the fact that it allows guessing a secret one character at a time.


## Quick Reference

### TLDR

The HTTPS protocol can encrypt compressed data without properly obfuscating the length of the unencrypted data, which makes it easier for man-in-the-middle (MitM) attackers to obtain plaintext secret values by observing length differences during a series of guesses in which a string in an HTTP request URL potentially matches an unknown string in an HTTP response body.

### Abbreviation

BREACH

### Name

Browser Reconnaissance & Exfiltration via Adaptive Compression of Hypertext

### CVE Number

CVE-2013-3587

### Type of Vulnerability

The vulnerability is in HTTPS protocol compression. 

### Affected

To be vulnerable, a web application must:

- Be served from a server that uses HTTP-level compression
- Reflect user-input in HTTP response bodies
- Reflect a secret (such as a CSRF token) in HTTP response bodies

### Remediation

No clear mitigation to this attack. The following is a list of potential mitigations in order of effectiveness (not practicality)  provided by breachattack.com

1. Disabling HTTP compression
2. Separating secrets from user input
3. Randomizing secrets per request
4. Masking secrets (effectively randomizing by XORing with a random secret per request)
5. Protecting vulnerable pages with CSRF
6. Length hiding (by adding random number of bytes to the responses)
7. Rate-limiting the requests