---
layout: post
title: 'Setting PowerShell Terminal Options'
date: '2022-05-16'
description: 'During a pentest engagement, you often come across the need for a screenshot for the report. Using the `$host.UI` parameter inside our PowerShell terminal we can do things like set the window title, change the foreground and background colours along with adjusting the size and position. This article looks at how setting values within the `$host.UI` variable can customise our PowerShell terminal.'
coverimage: Setting_PowerShell_Terminal_Options.jpg
tags: powershell
published: true
posttype: article
categories: article
---

# Set PowerShell Terminal Options

We can set values within the terminal to customise how the terminal output is displayed, the title of the terminal window shown or how much buffer size to use. Using these values can be helpful for several things, such as distinguishing windows, staging the title for a report screenshot or improved recognition of terminal output. 
<img src="/static/4c64758d-9b0a-46d7-99f2-0c355aeb20b4.png">
![Terminal Displaying Styling](4c64758d-9b0a-46d7-99f2-0c355aeb20b4.png)

### Screen Buffer

Get the current screen buffer.

```
:::powershell
$host.UI.RawUI.BufferSize
```

Enlarge the terminal screen buffer to capture more output

```
:::powershell
$host.UI.RawUI.BufferSize = New-Object System.Management.Automation.Host.Size(120,999)
```

<aside>
💡 Note that when you attempt to decrease the width to below the current width, you will be presented with an error
</aside>

### Console Style (for quick identification).

Set Console Title

```
:::powershell
$host.ui.RawUI.WindowTitle = "Staged Window Title For Screenshot"
```

Set Console Colours

```
:::powershell
$Host.UI.RawUI.BackgroundColor = 'Black'
$Host.UI.RawUI.ForegroundColor = 'Green'
```

## Set Window Position

Find out what the current window position is

```
:::powershell
$host.UI.RawUI.WindowPosition
```

Set window position

```
:::powershell
$host.UI.RawUI.WindowPosition = New-Object System.Management.Automation.Host.Coordinates(0,160)
```