---
layout: post
title: 'Auditing Juniper Network Devices'
date: '2021-03-22'
description: 'This article describes some of the querks of auditing Juniper network devices'
coverimage: Auditing_Juniper_Network_Devices.jpg
tags: auditing console-connection juniper
published: true
posttype: article
categories: article
---

## Lack of Console Connection Timeout

It is possible for a console connection to become unused without being terminated, for example by an administrator forgetting to terminate the connection. When this happens, it is possible that an attacker could gain access to the authenticated session (either by gaining physical access or, in the case of less secure network protocols, remotely) and thus gain administrative access.
In order to avoid this, the usual best practice is to set a timeout, so connections terminate within a specified time if no actions are taken. 

To check the current timeout sessions you can run the following command
```
get console
```

Or
```
show cli
```


To set the timeout
```
set console timeout <time in minutes>
```

Command introduced before Junos OS Release 7.4.
```
set cli idle-timeout <minutes>
```

## Tacacs+

Look for a missing TACACS+ encryption key. If one is not configured on the Juniper device then this configuration may affect the confidentiality of TACACS+ encrypted traffic, as an attacker may be able to intercept it without a password requirement.

Authentication requests are sent by the Terminal Access Controller Access Control System Plus (TACACS+) server, using TCP/49, and an access response is returned. A TACACS+ encryption key can be configured on both the client and server in order to encrypt and help protect the traffic. Although TACACS+ is considerably more secure than TACACS, a number of security issues exist with the protocol, such as:

* TACACS+ is vulnerable to replay attacks.
* TACACS+ encrypted packets leak information about the authentication credentials.
* Weaknesses exist within the TACACS+ encryption.

In order to address the last of these issues, a strong encryption key should be configured. However, if the key is leaked in plain text by another service an attacker could decrypt the authentication attempt, no matter how strong the encryption may be.