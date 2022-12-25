---
layout: post
title: 'Vulnerabilities in SSL & TLS :- TLS  Renegotiation'
date: '2022-01-23'
description: 'In this article, we will look at the TLS Renegotiation Vulnerability in the SSL and TLS protocols. This is a plaintext injection attack into previously sent packets. TLS and SSL 3.0 do not correctly associate renegotiation handshakes with existing connections. This allows a person-in-the-middle positioned attacker to insert data into a HTTPS session.'
coverimage: TLS_renegotiation.jpg
tags: insecure-tls-renegotiation ssl tls
published: true
posttype: article
categories: article
---
## Main Points

- SSL/TLS client-initiated renegotiation is a feature that allows the client to renegotiate new encryption parameters for an SSL/TLS connection within a single TCP connection.
- During the SSL/TLS handshake, the server incurs a higher computational cost. An attacker can exploit the higher computational cost of the server by opening an SSL/TLS connection to the server and repeatedly initiating renegotiation. This would cause the server to waste resources that would otherwise be used for the server's normal function. In addition, there is the possibility of a DOS.
- To exploit this vulnerability, the server must not support secure renegotiation and must allow client-side renegotiation.
- A TLS session can be renegotiated over an existing secure channel to rekey or perform further authentication. A flaw was discovered in the mechanism, by which an attacker with network access could intercept and hold handshake records from a legitimate client, establish a TLS session itself with a server, send application data, initiate renegotiation, and release the legitimate handshake records. As renegotiation is performed over an existing channel, the server believes the session is one and the same.

## Quick Reference

### TLDR

This is a plaintext injection attack into previously sent packets. TLS and SSL 3.0 do not correctly associate renegotiation handshakes with existing connections. This allows a person-in-the-middle positioned attacker to insert data into a HTTPS session.
 
### Name

TLS Renegotiation Vulnerability

### CVE Number

The TLS protocol, and SSL protocol 3.0 (and possibly earlier)

### Microsoft Vulnerability

MS10-049

### Type of Vulnerability

Within the protocol. 

### Affected

The TLS protocol, SSL protocol 3.0 and possibly earlier versions of SSL. 

### Remediation

Avoid renegotiation, or cryptographically bind original and renegotiation TLS handshakes with the secure renegotiation extension.