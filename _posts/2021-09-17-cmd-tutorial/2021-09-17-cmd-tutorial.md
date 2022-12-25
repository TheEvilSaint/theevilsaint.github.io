---
layout: post
title: "CMD Tutorial"
date: "2021-09-17"
description: ""
coverimage: cmd_tutorial.jpg
tags: cmd windows
published: true
posttype: article
categories: tutorial
---
---
layout: post
title: CMD 
weight: 10
---
# CMD

## For Loops

Overview

FOR	Loop through a set of files in one folder
```
FOR /R	Loop through files (recurse subfolders)
FOR /D	Loop through several folders
FOR /L	Loop through a range of numbers
FOR /F	Loop through items in a text file
FOR /F	Loop through the output of a command
```

A loop with no modifier options will run a specified command for each file in a set of files (won't act on directories).
```
FOR %variable IN (set) DO command [command-parameters]
```

> To use the FOR command in a batch program, specify `%%variable` instead of `%variable`. Variable names are case sensitive, so `%i` is different from `%I`

### For Loop Modifiers

Using the /D modifier modifies are loop so that it only iterates and acts over files
```
FOR /D %variable IN (set) DO command [command-parameters]
```

Using the /R modifier we are able to walk recursively listing files. We can either add a drive and path after the /R option and we will start recursively from that point. If we miss out the drive and path then we start recursive from the working directory.
```
FOR /R [[drive:]path] %variable IN (set) DO command [command-parameters]
```

Using the /L modifier we can loop over sequences of numbers from start to end, and even indicate what amount to step up by
```
FOR /L %variable IN (start,step,end) DO command [command-parameters]
```
> The set is a sequence of numbers from start to end, by step amount. So (1,1,5) would generate the sequence 1 2 3 4 5 and (5,-1,1) would generate the sequence (5 4 3 2 1)

#### The /F modifier

The /F modifier can be used to construct some very powerful loops.

Each file is opened, read and processed before going on to the next file in file-set. Processing consists of reading in the file, breaking it up into individual lines of text and then parsing each line into zero or more tokens.
```
FOR /F ["options"] %variable IN (file-set) DO command [command-parameters]
FOR /F ["options"] %variable IN ('string') DO command [command-parameters]
FOR /F ["options"] %variable IN (`command`) DO command [command-parameters]
```

| Option | Description                                                         |
| ------ | ------------------------------------------------------------------- |
| eol=c  | specifies an end of line comment character (just one)               |
| skip=n | specifies the number of lines to skip at the beginning of the file. |
| delims=xxx  | 	specifies a delimiter set.  This replaces the default delimiter set of space and tab. |
| tokens=x,y,m-n |	specifies which tokens from each line are to be passed to the for body for each iteration. This will cause additional variable names to be allocated. The m-n form is a range, specifying the mth through the nth tokens. If the last character in the tokens= string is an asterisk, then an additional variable is allocated and receives the remaining text on the line after the last token parsed. |
| usebackq   |    	specifies that the new semantics are in force, where a back quoted string is executed as a command and a single quoted string is a literal string command and allows the use of double quotes to quote file names in filenameset. |

Examples of the different loops

Loop through all files
```
C:\ for %i in (C:\Windows\System32\*) do @echo name: %i attrib: %~ai
```
Loop through all directories (only difference is addition of /D)
```
C:\ for /D %i in (C:\Windows\System32\*) do @echo name: %i attrib: %~ai
```
Loop through all files recursively
```
for /R C:\Windows %i in (*) do @echo name: %i attrib: %~ai
```
Loop through all folders recursively
```
for /R C:\Windows /D %i in (*) do @echo name: %i attrib: %~ai
```
A loop around a sequence of numbers between 2-254 piped to ping to make a ping sweeper
```
for /L %i in (2,1,254) do ping -n 2 127.0.0.%i
```

Batch File Loops

Here is how we would write FOR loops inside a batch file with the %%.

Loop through text files and echo file name
```
for %%F IN (*.txt) DO @echo %%F
```

Loop through text files and output the file contents
```
for %%F IN (*.txt) DO @echo %%F
```