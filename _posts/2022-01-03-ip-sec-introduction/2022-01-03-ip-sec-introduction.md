---
layout: post
title: 'IP Sec - Introduction'
date: '2022-01-03'
description: 'This is a two part article that will go over the fundamentals of IPSec. We will start with a background of IPSec, and take a look at configuring an IPSec tunnel. Then we will run the pentest tool ike-scan against our configured IPSec tunnel and collect and analyse traffic in wireshark.'
coverimage: IP_Sec_-_Introduction.jpg
tags: ike isakmp
published: true
posttype: article
categories: article
---
<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/8b11686c-7367-4f73-9278-6d8b142cf1f5.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal">IPSec Overview</figcaption></figure>

IPSec is a framework of open standards. The benefit of being a framework is that if one component gets superseded, it can be replaced, or additional items can be added. The suite of protocols that make up IPSec allows secure, encrypted communication between two computers.

Uses of IPsec

- Authenticating and encrypting host-to-host traffic.
- Authenticating and encrypting traffic to specific servers.
- Using L2TP/IPsec for VPN connections.
- Site-to-site tunneling.
- Enforcing logical networks.

## Overview

IPSec was designed to provide security options and enhancements to Internet Protocol (IP) and negate Internet Protocol weaknesses.

IPsec provided the following security.

- Authentication - IP spoofing and packet source forgery issues.
- Data Integrity - Modification of data within IP packets.
- Anti-replay - Replaying packet attacks.
- Data Confidentiality - Prevent packet sniffing attacks.

IPSec consists of three core components. IKE, ESP and AH.

**Internet Key Exchange (IKE)** is a network security Protocol designed to allow two devices to dynamically exchange Encryption Keys and negotiate Security Associations (SA). Internet Key Exchange (IKE) Security Associations (SA) can be established dynamically and removed after a negotiated period.

**Encapsulating Security Payload (ESP)** provides IPSec data integrity, encryption, authentication, and anti-replay functions. ESP authenticates the data within the VPN, ensuring the integrity of the data and that it is coming from the correct source. Encapsulating Security Payload can provide encryption via two modes. Transport mode and Tunnel mode. In transport mode, the payload is encrypted, and in tunnel mode, the entire packet is encrypted.

**Authentication Header (AH)** provides data integrity, authentication, and anti-replay functions for IPSec. Authentication Header (AH) does not, however, provide any data encryption. Authentication Header (AH) is used to provide data integrity services to ensure that no data tampering has occurred during transmission.

So just to run that back again, the authentication header (AH) is concerned with ensuring that packets get delivered and maintain their integrity. It will block against replay attacks and make sure data has not been modified; however, it does not encrypt. Encapsulating Security Payload (if used) encapsulates and encrypts the IP datagrams to protect them from sniffing attacks in addition to what AH does alone.

<aside>
💡 Why would we ever use AH if ESP does everything? In short, when overhead and packet size becomes issues. ESP encapsulates packets with much overhead.

</aside>

## A look at IKE and ISAKMP

IKE or Internet Key Exchange protocol is a protocol that sets up Security Associations (SAs) in the IPSec protocol suite.

Internet Key Exchange (IKE) is a hybrid protocol that consists of 3 protocols.

- ISAKMP: It is not a key exchange protocol per se. It is a framework on which key exchange protocols operate.
- Oakley: Describes the "modes" of key exchange (e.g. perfect forward secrecy for keys, identity protection, and authentication)
- SKEME: Provides support for public-key-based Key exchange, key distribution centres, and manual installation. It also outlines methods of secure and fast key refreshment.

In many texts, the terms IKE and ISAKMP are used interchangeably, which often confuses people in picking up this topic. Although we have briefly touched on their relationship, let us clarify the difference between IKE and ISAKMP.

Internet Security Association Key Management Protocol (ISAKMP) is a framework for authentication and key exchange between two peers to establish, modify, and tear down SAs. It is designed to support many different kinds of key exchanges, not just IKE. ISAKMP uses UDP port 500 for communication between peers and is why port 500 is commonly associated with IPSec VPN.

IKE is the implementation of ISAKMP using the Oakley and Skeme key exchange techniques. Oakley provides perfect forward secrecy (PFS) for keys, identity protection, and authentication; Skeme provides anonymity, reputability, and quick key refreshment.

Think of ISAKMP as a framework, and IKE is an implementation of ISAKMP using the Oakley and Skeme key exchange techniques. Oakley provides perfect forward secrecy (PFS) for keys, identity protection, and authentication; Skeme provides anonymity, reputability, and quick key refreshment.

Establishing an IPSec tunnel requires two IKE phases.

## IKE Phases

IPsec VPNs are negotiated in phases

- Successful Phase I negotiation results in an IKE Security Association (SA)
- Successful Phase II negotiation results in two separate IPsec SAs for the directions in and out.
- Phase II or (Quick Mode) happens through the Phase I tunnel.

### Phase I

- This Phase is where the IKE SA (Internet Key Exchange Security Association) is negotiated. This Phase can be performed in one of two Modes: Main (MM) or Aggressive (AM)

### Phase II

- This Phase is where the IPSec SA is negotiated. The IPSec tunnel (also called the IKE Phase II tunnel) build is complete when this Phase is completed. This Phase only has one mode called Quick Mode.

## IPSec Implementation

Most IPsec implementations use the Internet Key Exchange (IKE) service. Some older IPsec implementations use manual keying (Which involves exchanging encryption and authentication keys in advance), but this is now considered obsolete. As discussed briefly in the introduction, the Internet Key Exchange (IKE) protocol is used to negotiate the cryptographic algorithm choices and generate the associated keys. The Authentication Header (AH) and Encapsulating Security Payload (ESP) then use these choices.

The Internet Security Association and Key Management Protocol (ISAKMP) provides the framework for establishing SAs. A security association (SA) is a logical connection involving two devices that transfer data. With the help of the defined IPsec protocols, SAs offer data protection for unidirectional traffic. Generally, an IPsec tunnel features two unidirectional SAs, which offer a secure, full-duplex channel for data.

A security association consists of parameters I remember as HAGLE:

H = Hash Algorithm

A = Authentication Method

G = Group Number

L = Lifetime Value

E = Encryption

Both sides store the SA parameters in their security Association database when complete, sometimes referenced as SAD.