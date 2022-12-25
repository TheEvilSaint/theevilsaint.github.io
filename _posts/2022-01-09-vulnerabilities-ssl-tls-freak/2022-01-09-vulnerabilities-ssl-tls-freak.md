---
layout: post
title: 'Vulnerabilities in SSL & TLS :- FREAK'
date: '2022-01-09'
description: 'In this article, we will look at CVE-2015-0204, also known as the FREAK attack. This attack makes use of a person-in-the-middle position to allow end clients to communicate using RSA Export Keys, which were a form of weak strength RSA encryption keys capped at 512 bits. The US government imposed this in the 1990s to prevent the encryption from being used against them. The idea was to give protection from a modest system but not prevent the NSA from decrypting it.'
coverimage: freak.jpg
tags: freak ssl tls
published: true
posttype: article
categories: article
---
## Main Points 

- In the 1990s, the US government established rules governing the strength of encryption that could be exported ("Export keys").
- In any Secure Socket Layer (SSL) implementations aimed at export, these rules limited the strength of the RSA encryption keys to a maximum of 512 bits.
- The RSA Export keys were designed to allow exports to contain encryption that could not be broken by a typical computing resource but could be broken by the NSA.
- For this attack to work, the attacker must use a different exploit to become a person-in-the-middle and inject content into the network traffic stream.
- The FREAK attack necessitates a person-in-the-middle position in order to change the initial HTTP packet that is sent out when negotiating cipher use to request "Export Keys."
- The use of “Export Keys” suites was stopped, and by the year 2000, browsers could use a higher-security SSL.
- Attack Narrative
    1. An attacker gains a person-in-the-middle position between two computers. 
    2. Victim requests a secure connection to a webpage. The victims browser sends a "Client Hello message" to the server asking for typical cipher suites. 
    3. The attacker intercepts the conversation and substitutes the requested cipher suites with a request for ‘export RSA’
    4. The server agrees and accepts the request (not understanding that it was modified). It responds with a 512-bit export RSA key.
    5. The client accepts ‘export RSA’ (not understanding that its request was modified).
    6. Attacker breaks weaker keys (first done by a researcher in 1999, modern cloud computing makes this much easier) and obtains information.

## Quick Reference 

### Abbreviation

FREAK 

### Name

Factoring RSA Export Keys

### CVE Number

CVE-2015-0204

### Type of Vulnerability

The vulnerability is in the ciphers with which the system is willing to communicate. They cannot be negotiated if the weak cipher is removed. It is a simple fix.

### Affected

Any system willing to negotiate RSA Export Keys. 

### Remediation

Disable support for weak export-grade ciphers

### Mediation

Remove any weak ciphers from your configuration. Refuse to communicate using weak ciphers. This way the person-in-the-middle cipher negotiation cannot be modified to support RSA Export Keys.