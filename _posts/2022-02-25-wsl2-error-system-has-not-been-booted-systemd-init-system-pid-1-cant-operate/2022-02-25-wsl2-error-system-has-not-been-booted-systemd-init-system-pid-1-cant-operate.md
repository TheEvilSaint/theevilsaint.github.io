---
layout: post
title: 'WSL2 Error - System has not been booted with systemd as init system (PID 1). Cant operate.'
date: '2022-02-25'
description: 'This article demonstrates how to fix the error System has not been booted with systemd as init system (PID 1). Cant operate. Failed to connect to bus: Host is down when using WSL version 2.'
coverimage: WSL2_Error_System_has_not_been_booted_with_systemd_as_init_system.jpg
tags: error-message linux windows wsl2
published: true
posttype: article
categories: article
---

This article demonstrates how to fix the error "System has not been booted with systemd as init system (PID 1). Can't operate. Failed to connect to bus: Host is down" when using WSL version 2.

# The error message

This example is done using WSL 2 with a Linux-based application.

The below error message is displayed:

```
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
```

# How to fix it

To fix, operate the service using `/etc/init.d/` instead:

```
sudo /etc/init.d/<service> <operation>
```

Where the operation is `start`, `status`, `stop`, `restart`, etc and service is the service you need to operate.

For example, to start postgresql, this command looks like:

```
sudo /etc/init.d/postgresql start
```

# Why this error occurs

At the time of this article, WSL 2 does not support the `systemd` command. To work around this, `/etc/init.d` is used.

# Example

The below screen capture displays running into the error when attempting to enable the postgresql service.
<img src="/static/f860de4c-277c-4b77-913b-ad08b27c3668.png">

The below screen capture displays a workaround to start postgresql using /etc/init.d instead.
<img src="/static/3f989df5-1149-4223-b472-09b65c45499e.png">

The below screen capture displays that the service was successfully started via the `status` operation.
<img src="/static/89efedbc-b091-4103-92ce-8b6e8d319b9b.png">
