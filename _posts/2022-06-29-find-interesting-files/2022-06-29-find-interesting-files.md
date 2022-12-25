---
layout: post
title: 'Find Interesting Files'
date: '2022-06-29'
description: 'A significant part of our job when searching around a target system is to look for interesting files.  Whether we have a need to look for SSH keys, Bash History files or database backups the commands typically all follow the same syntax. This article will look at using basic Linux commands to find and discover files.'
coverimage: https://evilsaint.com/media/articles/images/finding-interesting-files/cover_image/Finding_Interesting_Files.jpg
tags: enumeration linux
published: true
posttype: article
categories: article
---

# Finding Interesting Files

Find SUID files

```bash
find / -perm -4000 -type f 2>/dev/null
```

Find SUID files owned by root

```bash
find / -uid 0 -perm -4000 -type f 2>/dev/null
```

Find GUID files

```bash
find / -perm -2000 -type f 2>/dev/null
```

Find world-writeable files

```bash
find / -perm -2 -type f 2>/dev/null
```

Find world-writeable files excluding those in /proc

```bash
find / ! -path "*/proc/*" -perm -2 -type f -print 2>/dev/null
```

Find word-writeable directories

```bash
find / -perm -2 -type d 2>/dev/null
```

Find rhost config files

```bash
find /home –name *.rhosts -print 2>/dev/null
```

Find *.plan files, list permissions and cat the file contents

```bash
find /home -iname *.plan -exec ls -la {} ; -exec cat {} 2>/dev/null ;
```

Find hosts.equiv, list permissions and cat the file contents

```bash
find /etc -iname hosts.equiv -exec ls -la {} 2>/dev/null ; -exec cat {} 2>/dev/null ;
```

See if you can access other user directories to find interesting files

```bash
ls -ahlR /root/
```

Show the current users’ command history

```bash
cat ~/.bash_history
```

Show the current users’ various history files

```bash
ls -la ~/.*_history
```

Can we read root’s history files

```bash
ls -la /root/.*_history
```

Check for interesting ssh files in the current users’ directory

```bash
ls -la ~/.ssh/
```

Find SSH keys/host information

```bash
find / -name "id_dsa*" -o -name "id_rsa*" -o -name "known_hosts" -o -name "authorized_hosts" -o -name "authorized_keys" 2>/dev/null |xargs -r ls -la
```

Check Configuration of inetd services

```bash
ls -la /usr/sbin/in.*
```

Check log files for keywords (‘pass’ in this example) and show positive matches

```bash
grep -l -i pass /var/log/*.log 2>/dev/null
```

List files in specified directory (/var/log)

```bash
find /var/log -type f -exec ls -la {} ; 2>/dev/null
```

List .log files in specified directory (/var/log)

```bash
find /var/log -name *.log -type f -exec ls -la {} ; 2>/dev/null
```

List .conf files in /etc (recursive 1 level)

```bash
find /etc/ -maxdepth 1 -name *.conf -type f -exec ls -la {} ; 2>/dev/null
```

As above

```bash
ls -la /etc/*.conf
```

Find .conf files (recursive 4 levels) and output line number where the word ‘password’ is located

```bash
find / -maxdepth 4 -name *.conf -type f -exec grep -Hn password {} ; 2>/dev/null
```

List open files (output will depend on account privileges)

```bash
lsof -i -n
```

Can we read roots mail

```bash
head /var/mail/root
```