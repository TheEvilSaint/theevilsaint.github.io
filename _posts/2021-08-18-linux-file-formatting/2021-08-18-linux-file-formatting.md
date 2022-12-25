---
layout: post
title: "Linux File Formatting"
date: "2021-08-18"
description: "a look at several tricks when processing linux files."
coverimage: Linux_File_Formatting.jpg
tags: linux
published: true
posttype: article
categories: tutorial
---

tabs to spaces (by default without the `t` it is 8 spaces)
```
expand -t 4 <file> > <new_file>
```

Leading tabs only we add the `i` option

https://linuxhandbook.com/convert-tabs-spaces/