---
layout: post
title: 'How to Fix: Virtualbox on Windows 11 Mouse Not Working Inside VM'
date: '2022-05-07'
description: 'This article demonstrates how to fix a mouse that doesnt work inside VirtualBox VM on a computer with Windows 11.'
coverimage: 
tags: virutalbox vms windows
published: true
posttype: article
categories: article
---
# The problem

Mouse cannot be used inside a VirtualBox VM (guest) on Windows 11 (host).

#How to fix it

<h5 class="step">Step 1 - Make Sure Your VirtualBox is Version 6.1.28 or Later</h5>

These later versions have been improved by Oracle to minimise compatibility issues with Hyper-V. You can check the version by opening the VirtualBox Manager and navigating to:

`Help` > `About VirtualBox...`
<img src="/static/8ff29026-eb9d-4044-b6ab-bd89052f590f.png">



The version is displayed here. If you are running a version older than 6.1.28, consider upgrading it.
<img src="/static/63677c92-6c55-4458-bfe0-629b9da2fb7c.png">



<h5 class="step">Step 2 - Change your Pointing Device settings</h5>

Power off each virtual machine that needs fixing. 

On VirtualBox manager, click one machine at a time. Navigate to:

 `Machine` > `Settings`.
<img src="/static/65c7a514-4057-4dc6-8b12-18c7c3f4a6f4.png">



Under the `System` tab, change `Pointing Device` from `PS/2 Mouse` to `USB Tablet`. Click `OK` to save the changes.
<img src="/static/80917044-71b0-49a0-b45a-12a2b014e699.png">



<h5 class="step">Step 3 - Verify the fix</h5>

Power on your machine as usual and wait for the machine to boot. Try to move your mouse inside the VM.

If the fix has been successful, congratulations! Let us know in the comments below if you were able to fix your VirtualBox using our article.

#Why this error occurs

This is a mouse that connects to a PS/2 port:
<img src="/static/25ebb32f-2632-4ced-851d-bebe9ed89656.png">



Even though the other option, "USB Tablet", has the word "tablet" in it - it does not mean that it will only work on tablets. 

From the VirtualBox manual (Chapter 3.4.1):

```
The default virtual pointing devices for older guests is the traditional PS/2 mouse. If set to USB tablet, VirtualBox reports to the virtual machine that a USB tablet device is present and communicates mouse events to the virtual machine through this device.
```
<img src="/static/5157faa7-03ef-4249-a625-4f889c318415.png">

