---
layout: post
title: 'Customising Windows 10'
date: '2020-09-21'
description: 'This guide is going to take you through a number of components to get your machine ready for subsequent parts of this guide.'
coverimage: Customising_Windows_10.jpg
tags: customising-windows vms vmware windows
published: true
posttype: article
categories: article
---

This guide is going to take you through a number of components to get your machine ready for subsequent parts of this guide. 

### Preparing

Hit the Windows key and type `cmd`.

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
  <img src="/static/734c9b10-1b51-4fb7-81d1-d56a5156429d.png" class="img-fluid" alt="Opening Windows CMD Prompt from Start Menu">
<figcaption class="figure-caption text-center fw-normal text-dark">Opening Windows CMD Prompt from Start Menu.</figcaption>
</figure>

Hold down `CTRL` + `SHIFT` and now hit the `Enter` key to open the command prompt as an elevated user. 

Now hit the Windows key and type `PowerShell`

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
  <img src="/static/e02c6fea-e50d-4b4d-9bf0-d6734745df23.png" class="img-fluid" alt="Elevated PowerShell Prompt">
<figcaption class="figure-caption text-center fw-normal text-dark">Elevated PowerShell Prompt.</figcaption>
</figure>

Hold down `CTRL` + `SHIFT` and now hit the `Enter` key to open the PowerShell prompt as an elevated user. 

Windows Version

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
  <img src="/static/1e875c32-3ae8-4948-b3c4-8d8d0704f647.png" class="img-fluid" alt="Windows Version Information Displayed Via winver Command">
<figcaption class="figure-caption text-center fw-normal text-dark">Windows Version Information Displayed Via winver Command.</figcaption>
</figure>

## PowerShell 7

Quick one-liner to install the latest version (PowerShell 7 is current at time of print) on Windows
```
iex "& { $(irm https://aka.ms/install-powershell.ps1) } -UseMSI"
```

Follow the Wizard to the step headed "Optional Actions" and check the selection to "Enable PowerShell remoting". I also like to select "Add 'Open here' context menus to Explorer". 

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
 <img src="/static/747d8148-d1a5-49f1-b37a-20a03d0726e4.png" class="img-fluid" alt="PowerShell 7 Installing Optional Actions">
<figcaption class="figure-caption text-center fw-normal text-dark">PowerShell 7 Installing Optional Actions.</figcaption>
</figure>

Select the "Launch PowerShell" in the bottom left of the next wizard. 

Now "Right Click" the PowerShell icon and select "Pin to taskbar". 

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
 <img src="/static/b5b9a5ef-6cab-469e-b646-cdf2e102ab88.png" class="img-fluid" alt="Right click pin to taskbar">
<figcaption class="figure-caption text-center fw-normal text-dark">Right click pin to taskbar.</figcaption>
</figure>

Now enter `$PSVersionTable` to confirm the version of PowerShell. 


<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/d5102f1f-3474-4de9-b731-3c000bbff513.png" class="img-fluid" alt="PowerShell 7 Get Version Information with $PSVersionTable"><figcaption class="figure-caption text-center fw-normal text-dark">PowerShell 7 Get Version Information with $PSVersionTable.</figcaption>
</figure>

To install on Linux
```
wget https://aka.ms/install-powershell.sh; sudo bash install-powershell.sh; rm install-powershell.sh
```

## WSL

WSL version 2 is real Linux on real Windows :)

Next, we will install the Windows Subsystem for Linux and the VirtualMachinePlatform. 

Dism vs Enable-WindowsOptionalFeature

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux -NoRestart
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform -NoRestart
```

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/f01e5588-7993-4a9a-963f-0a6f28881677.png" class="img-fluid" alt="Installing Optional Features">
<figcaption class="figure-caption text-center fw-normal text-dark">Installing Optional Features.</figcaption>
</figure>


Setting version 2 of the Windows Subsystem for Linux to the default
```
wsl –set-default-version 2
```

If you get an error message saying 
    
> WSL 2 requires an update to its kernel component. For information please visit https://aka.ms/wsl2kernel
    
This means you need to install the MSI another component. 

Go to [https://aka.ms/wsl2kernel](https://aka.ms/wsl2kernel)

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/20b5093a-ed64-44f2-9f85-330caca77ad6.png" class="img-fluid" alt="Download the Linux Kernel Update package for WSLv2">
<figcaption class="figure-caption text-center fw-normal text-dark">Download the Linux Kernel Update package for WSLv2.</figcaption>
</figure>

Download by clicking the link "WSL2 Linux kernel update package for x64 machines"

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/c7684b40-9f63-46d9-aa4f-2ed48d6c4bfc.png" class="img-fluid" alt="Click the msi installer">
<figcaption class="figure-caption text-center fw-normal text-dark">Click the msi installer.</figcaption>
</figure>

If at this point you need to restart your virtual machine. 
```
restart-computer -Confirm
```

List various versions of Linux 
```
wsl --list
wsl -l -v
```

Listing versions we can see the difference

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/dc8ef383-d7c5-4d2d-9be0-90afdb2b03ce.png" class="img-fluid" alt="Using the wsl --list versions command">
<figcaption class="figure-caption text-center fw-normal text-dark">Using the wsl --list versions command.</figcaption>
</figure>

We will want to upgrade any WSL Linux machines running version 1 (Hyper-V method)

Reasons to upgrade to WSLv2 [Reasons to Upgrade to WSLv2](https://docs.microsoft.com/en-gb/windows/wsl/about)

```
wsl --set-version kali-linux 2
```
<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/edde105e-d633-41fc-a530-09a3eea6a75d.png" class="img-fluid" alt="Coverting Kali-Linux WSLv1 to WSLv2">
<figcaption class="figure-caption text-center fw-normal text-dark">Coverting Kali-Linux WSLv1 to WSLv2.</figcaption>
</figure>

Run the Kali Linux Distribution
```
wsl -d kali-linux
```

### Debugging

```
wsl --shutdown
dism /Online /Cleanup-Image /RestoreHealth
```

Install Code by typing `code .`

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/c40c343c-f360-46fb-8ac9-b4b008324e35.png" class="img-fluid" alt="Installing Visual Studio Code">
<figcaption class="figure-caption text-center fw-normal text-dark">Installing Visual Studio Code.</figcaption>
</figure>

Now type `code .` again and watch as Visual Studio opens up in the Windows Host showing files from the WSL

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/c91f8c40-7c3e-4239-b2a2-6e2e4173bb37.png" class="img-fluid" alt="Running and Installing Visual Studio inside WSL">
<figcaption class="figure-caption text-center fw-normal text-dark">Running and Installing Visual Studio inside WSL.</figcaption>
</figure>


## Docker for Windows

Requirements

* Windows 10 64-bit: Pro, Enterprise, or Education (Build 16299 or later).
* Hyper-V and Containers Windows features must be enabled.

> **Note** - For Windows Home Edition follow this link [https://docs.docker.com/docker-for-windows/install-windows-home/](https://docs.docker.com/docker-for-windows/install-windows-home/)

1. Grab the installer [https://hub.docker.com/editions/community/docker-ce-desktop-windows/](https://hub.docker.com/editions/community/docker-ce-desktop-windows/)

2. Double-click the blue "Get Docker Desktop for Windows (stable)" button to download the executable.

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/c92e6a86-96b6-4893-a656-865c78d9f0a4.png" class="img-fluid" alt="Docker Installer">
<figcaption class="figure-caption text-center fw-normal text-dark">Docker Installer.</figcaption>
</figure>

3. Double-Click the "Docker Desktop Installer.exe" to run the installer.

4. When prompted, ensure the Enable Hyper-V Windows Features option is selected on the Configuration page.

5. Follow the instructions on the installation wizard to authorize the installer and proceed with the install.

> If your admin account is different to your user account, you must add the user to the docker-users group. Run Computer Management as an administrator and navigate to  Local Users and Groups > Groups > docker-users. Right-click to add the user to the group. Log out and log back in for the changes to take effect.

## Terminal App

The terminal app is seemingly turning out to be a boon for developers and those who have always looked at Windows machines with huge expectations. The open-source terminal app boasts a range of powerful features including multiple tabs, Unicode and UTF-8 character support, and GPU accelerated text rendering engine. It’s designed to be an all-in-one platform for Command Prompt, PowerShell, WSL and SSH so that developers can have seamless access to all the tools. Even better, this all-new command-line app also features custom themes and styles for a more personalized experience

[Terminal App Releases](https://github.com/microsoft/terminal/releases/)

The new Shell

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/f62567a0-2ccb-4b42-a55c-a4e2d29a1a79.png" class="img-fluid" alt="The New Shell">
<figcaption class="figure-caption text-center fw-normal text-dark">The New Shell.</figcaption>
</figure>

Pin terminal to the taskbar

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/94d8933c-ec62-43c3-afb0-6300e5dc86a3.png" class="img-fluid" alt="Pin terminal to the taskbar">
<figcaption class="figure-caption text-center fw-normal text-dark">Pin terminal to the taskbar.</figcaption>
</figure>

## Customisation

### Cascadia Fonts

Next, I am going to install [Microsofts Cascadia Code Font](https://github.com/microsoft/cascadia-code/releases)

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/0223b9ef-976a-491a-bed5-c703f8b79a3f.png" class="img-fluid" alt="Pin terminal to the taskbar">
<figcaption class="figure-caption text-center fw-normal text-dark">Pin terminal to the taskbar.</figcaption>
</figure>

Click "Install for all users"

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/2c1826a5-6222-4c83-9f06-9999146253f0.png" class="img-fluid" alt="Install Font for all users">
<figcaption class="figure-caption text-center fw-normal text-dark">Install Font for all users.</figcaption>
</figure>

### Git

Install [Git for Windows](https://git-scm.com/download/win)

Posh-Git adds Git status information to your prompt as well as tab-completion for Git commands, parameters, remotes, and branch names. Oh-My-Posh provides theme capabilities for your PowerShell prompt. PSReadline lets you customize the command line editing environment in PowerShell.

```
Install-Module posh-git -Scope CurrentUser
Install-Module oh-my-posh -Scope CurrentUser
```

PowerShell Core
```
Install-Module -Name PSReadLine -Scope CurrentUser -Force -SkipPublisherCheck
```

Oh My Posh Themes
Pick a [theme for `Oh My Posh`](https://github.com/JanDeDobbeleer/oh-my-posh#themes)
```
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme Paradox
```

### Customise your Kali

Install Powerline
```
sudo apt install golang-go
go get -u github.com/justjanne/powerline-go
```

Install Hyper for Windows
https://releases.hyper.is/download/win

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/03e3cc64-862a-4dd2-9b5a-460007938604.png" class="img-fluid" alt="Install Hyper For Windows">
<figcaption class="figure-caption text-center fw-normal text-dark">Install Hyper For Windows.</figcaption>
</figure>

With the Hypershell open, enter the following commands

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/ad210a8d-8793-4ffc-b538-7e36b1e2d0a4.png" class="img-fluid" alt="Open HyperShell">
<figcaption class="figure-caption text-center fw-normal text-dark">Open HyperShell.</figcaption>
</figure>

**settings.json**

The settings.json file as the name suggests contains settings for the terminal application. A few of the important settings like what should be your default profile, color scheme, key bindings, etc can be found here.

To open the default.json file hold the alt key while opening the settings.json file as mentioned above.

**defaults.json**

The defaults.json file contains all the default configuration values for the terminal. This file can be used for reference, as it is an auto-generated file and contains all complete default configuration of the terminal application.

## Install Chocolatey

From an elevated PowerShell Prompt
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Confirm the installation of Chocolatey

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/47315b04-6d39-4fb7-b421-83d9e7f2b346.png" class="img-fluid" alt="Confirm the installation of Chocolatey">
<figcaption class="figure-caption text-center fw-normal text-dark">Confirm the installation of Chocolatey.</figcaption>
</figure>

Let us install some packages

```
choco install wsl-kalilinux
```

## FireEye Commando-vm

Download the latest from:
[https://github.com/fireeye/commando-vm](https://github.com/fireeye/commando-vm)

Unzip the folder.

Use my custom profile evilsaint.json. 

[Custom Profile]({{site.url}}/assets/evilsaint.json)
[Windows 10]({{site.url}}/posts/customising-windows-10/Customising_Windows_10.jpg)


My main additions are 
```
{"name": "wsl.fireeye"},
{"name": "hyperv.fireeye"},
{"name": "markdownmonster"},
{"name": "wsl-ubuntu-2004"},
{"name": "wsl-archlinux"},
{"name": "wsl-debiangnulinux"},
{"name": "microsoft-windows-terminal"},
{"name": "everything"},
```

I like to remove
```
{"name": "burp.free.fireeye"},
```

```
cinst install <package>
```

```
cup all
```

## Customising WSL

### Kali Machine

```
sudo apt-get update
sudo apt-get dist-upgrade
sudo apt-get clean
sudo apt-get --yes --force-yes install kali-desktop-xfce xorg xrdp
sed -i 's/port=3389/port=3390/g' /etc/xrdp/xrdp.ini
sudo apt install kali-win-kex
sudo apt install kali-linux-large
```

Run Win-KeX

* Windows mode
* seamless mode

## Moving Around 

```
start WT 'new-tab "PowerShell" ; split-pane -p "KaliGeneral" ; split-pane -H -p "KaliC2" | set-focus -n wsl.exe
```

```
mkdir C:\Users\consultant\AppData\Local\KaliC2
wsl --import KaliC2 C:\Users\consultant\AppData\Local\KaliC2 .\kali-base.tar --version 1
mkdir C:\Users\consultant\AppData\Local\KaliGeneral
wsl --import KaliGeneral C:\Users\consultant\AppData\Local\KaliGeneral .\kali-base.tar --version 2
```

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/d65ee216-1714-45cf-b4ac-8bc5587beba9.png" class="img-fluid" alt="Windows Terminal Help">
<figcaption class="figure-caption text-center fw-normal text-dark">Windows Terminal Help.</figcaption>
</figure>

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/41873b36-7aac-435e-8b51-1a6a566f0a31.png" class="img-fluid" alt="Windows Terminal Help - Split Pane">
<figcaption class="figure-caption text-center fw-normal text-dark">Windows Terminal Help - Split Pane.</figcaption>
</figure>

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/21ca4b80-a019-4029-b4ea-41fa193bef73.png" class="img-fluid" alt="Windows Terminal Help - New Tab">
<figcaption class="figure-caption text-center fw-normal text-dark">Windows Terminal Help - New Tab.</figcaption>
</figure>

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/1b6288b6-c34f-4939-b680-c11726d58814.png" class="img-fluid" alt="Windows Terminal Help - Focus Tab">
<figcaption class="figure-caption text-center fw-normal text-dark">Windows Terminal Help - Focus Tab.</figcaption>
</figure>

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/06677c06-0359-4f30-b953-8fb6cfd31724.png" class="img-fluid" alt="Two Versions Of Kali">
<figcaption class="figure-caption text-center fw-normal text-dark">Two Versions Of Kali.</figcaption>
</figure>

## Customise Toys

* Groupy
* Taskbar X
* T Clock
* Power Toys
* Everything
* Rocket / Launcher
* wox
* sharex
* ditto