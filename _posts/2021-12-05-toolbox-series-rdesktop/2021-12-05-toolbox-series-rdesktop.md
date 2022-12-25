---
layout: post
title: "Toolbox  Series :- rdesktop"
date: "2021-12-05"
description: ""
coverimage: Toolbox_Series_rdesktop.jpg
tags: rdesktop
published: true
posttype: article
categories: tutorial
---
As a general rule you should check every windows machine to see if you can `rdesktop` to it in the enumeration phase. This will greatly aid you in
Operating System Identification  
  
Basic Syntax for rdesktop is:  
```
root@kali:/# rdesktop 10.0.0.1  
```

There are several other options we might use to further improve our experience  
  
This will log us in with a username and password  of 'Administrator' and 'password'.
```
root@kali:/# rdesktop -u Administrator -p password 10.0.0.1  
```

If we change the password to a `-` it will prompt for a password on the next line before we log in  
```
root@kali:/# rdesktop -u Administrator -p- 10.0.0.1  
```

Adding `-f` to our syntax will open up the RDP in full screen mode.  
```
root@kali:/# rdesktop -u Administrator -f 10.0.0.1  
```

Adding a `-T` will allow us to name the desktop anything we like.  
```
root@kali:/# rdesktop -u Administrator -p password -T 'my window title' 10.0.0.1  
```

If we dont wish to go full screen we can use the `-g` to adjust the windows geometry
```
root@kali:/# rdesktop -u Administrator -p password -T 'my window title' -g 90% 10.0.0.1  
```

Important to note, we should never put the username in quotes unless it has spaces in it and if we use the RDP title option we put it in single quotes.