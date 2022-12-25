---
layout: post
title: 'Auditing Cisco Network Devices'
date: '2021-04-15'
description: ''
coverimage: Auditing_Cisco_Network_Devices.jpg
tags: 
published: true
posttype: article
categories: article
---

# Administrative Line Login With No Password

Look out for device administrative (console and aux) lines that are configured without a password. An attacker with physical access to the ports, or with remote modem access, would be able to access the host. Any access could lead to system compromise, packet sniffing, or denial of service.

Ensure that authentication is configured on all ports. Shut down any ports that are not in use.
The authentication mechanism and authentication password can be configured on administrative lines using the following line mode commands:
```
login [tacacs | local]
password password
```