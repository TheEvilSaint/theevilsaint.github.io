---
layout: post
title: "VB Script Tutorial"
date: "2021-09-17"
description: ""
coverimage: vb_script_tutorial.jpg
tags: vbscript
published: true
posttype: article
categories: tutorial
---
# VB Script

WSH (Windows Scripting Host) is a powerful scripting environment that comes with Windows Operating System. By default, there are two scripting languages supported, which is JScript and VBScript. You could actually open any text editor and write the scripts and save as *.vbs or *.js. Double click them invokes the interpreter, which is cscript.exe or wscript.exe depending on the settings. The `cscript.exe` will output to console (e.g. using `WScript.Echo`) while the `wscript.exe` will treat `WScript.Echo` as `MsgBox` (output using dialog). Sometimes, you could choose other languages as the host languages in WSH, but this is not installed by default, and you have to download corresponding installer.

The advantages of writing scripts to run in WSH are:
- You do not need to compile them, just save and run immediately, which are suitable for lightweight small and daily tasks.
- The scripts can be edited/changed and distributed easily.
- The same scripts can be interpreted for both 32 and 64 bit, just specify the corresponding cscript.exe or wscript.exe respectively.

You could actually use `WScript.Shell` object (COM Automation) to `SendKeys` to the active window, in which case, you could do something as powerful as macros. The keystrokes are being sent to the active window.

The following runs the `notepad.exe` and bring the notepad to the front. The second parameter 9 means: make active and display the window. If the window is minimized or maximized, the system restores it to its original size and position.
```vbscript
Set WshShell = WScript.CreateObject("WScript.Shell")
WshShell.Run "notepad", 9
```

Now the following will type in "Hello, World!"" one character after another, with a short pause 200ms between characters. And finally, it will simulate Alt+F4 to exit. At this time, the dialog will pop up for saving, and press Tab to navigate to Don't Save button and press Enter to exit.
```vbscript
' Give Notepad time to load
WScript.Sleep 500

Dim Msg: Msg = "Hello, World!"

' Type in One Character at a time
For i = 1 To Len(Msg)
        WScript.Sleep 200
        WshShell.SendKeys Mid(Msg, i, 1)
Next

WshShell.SendKeys "{ENTER}"
WshShell.SendKeys "%{F4}"
WshShell.SendKeys "{TAB}"
WshShell.SendKeys "{ENTER}"
```

![VBScript Hello World in Notepad](images/2018/03/vbscript-hello-world-in-notepad.png)

> The specified keys, functional keys, ctrl, alt are enclosed by {braces}.

![SendKey Lookup For Special Characters](images/2018/03/sendkey-lookup-for-special-characters.png)

You can combine keys, for example, + (plus) is the prefix for shift, Ctrl is denoted using prefix ^ and % (percentage) is for ALT.

Let's review the above example one more time, but in JScript.
```vbscript
(function () {
          var WshShell = WScript.CreateObject("WScript.Shell");
          WshShell.Run("notepad", 9);
          var msg = "Hello, World!";
          // Give Notepad time to load
          WScript.Sleep(500);
          for (i = 0; i < msg.length; i ++) {
              WScript.Sleep(200);
              WshShell.SendKeys(msg.charAt(i));
          }
          WshShell.SendKeys("{ENTER}");
          // Alt + F4
          WshShell.SendKeys("%{F4}");
          // Naviagate to Don't Save
          WshShell.SendKeys("{TAB}");
          // Exit
          WshShell.SendKeys("{ENTER}");
})();
```

## Another SendKey Example

```vbscript
set WshShell = WScript.CreateObject("WScript.Shell")
WshShell.run "runas /user:ladm " & WScript.Arguments.Item(0)
WScript.Sleep 1000
WshShell.SendKeys "Pm2fUXScqI"
WshShell.SendKeys "{ENTER}"
WScript.Sleep 3000
```

Contains a function that will take an argument to the script with "WScript.Arguments.Item(0)".

When you run the vbscript, make sure you supply the executable name as an argument, which should result in the second line doing `runas /user:ladm executable_name.exe`.

Your command should be something along the lines of:
```
cscript vbscript.vbs executable_name.exe
```

You should be then able to run the script with your executable as an argument, and it should work.