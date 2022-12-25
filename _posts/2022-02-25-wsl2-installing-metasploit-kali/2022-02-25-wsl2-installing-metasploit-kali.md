---
layout: post
title: 'WSL2 - Installing Metasploit Framework with Kali Linux'
date: '2022-02-25'
description: 'This article demonstrates how to install the Metasploit Framework with Kali Linux on WSL2.'
coverimage: WSL2_Installing_Metasploit_Framework_with_Kali_Linux.jpg
tags: kali-linux wsl2
published: true
posttype: article
categories: article
---

This article demonstrates how to install the Metasploit Framework with Kali Linux on WSL version 2. When installing Kali Linux for WSL2 using Microsoft Store, its file size is minimised by including the bare essentials by default.

#Pre-requisites

This example assumes that you have the following:

- Windows Subsystem For Linux version 2 (WSL2) - This example is not tested on WSL version 1
- Kali Linux for WSL2

#Instructions

<h5 class="step">Step 1 - Update the repositories</h5>

```
sudo apt update
```

<h5 class="step">Step 2 - Install and enable PostgreSQL</h5>

```
sudo apt install postgresql
sudo /etc/init.d/postgresql start
```

<h5 class="step">Step 3 - Download the Metasploit Framework installation file</h5>

```
wget https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb
```

<h5 class="step">Step 4 - Install the Metasploit Framework</h5>

```
./msfupdate.erb
```

<h5 class="step">Step 5 -Verify the installation</h5>

To confirm that the Metasploit Framework was successfully installed, attempt to launch components supported by the framework.

To launch the Metasploit Framework command-line interface, run:

```
msfconsole
```

The below screen capture displays `msfconsole` launched with an ASCII-art banner.
<img src="/static/429fbf45-6b3a-4372-8e75-15ad3e5d60cd.png">
![](/media/markdown/2022/02/26/)

You can use `exit` to exit msfconsole and attempt to launch the `msfvenom`, the command-line instance of Metasploit.

```
msfconsole
```

If the installation was successful, the help page will be displayed as demonstrated below.
<img src="/static/fa427e84-580d-4edb-847d-e09776009dd1.png">
![](/media/markdown/2022/02/26/)