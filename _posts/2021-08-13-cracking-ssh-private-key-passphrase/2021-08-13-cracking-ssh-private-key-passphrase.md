---
layout: post
title: 'Cracking SSH Private Key Passphrase'
date: '2021-08-13'
description: ''
coverimage: Cracking_SSH_Private_Key_Passphrase.jpg
tags: ssh
published: true
posttype: article
categories: article
---

```
root@evilsaint:~# file id_rsa
id_rsa: PEM RSA private key
```


```
root@evilsaint:~# whereis ssh2john
ssh2john: /usr/sbin/ssh2john
```


```
root@evilsaint:~# locate ssh2john
/usr/sbin/ssh2john
```

```
root@evilsaint:~# ssh2john id_rsa > converted_hash
```

```
root@evilsaint:~# john --wordlist=/usr/share/wordlists/rockyou.txt converted_hash
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA 32/64])
Press 'q' or Ctrl-C to abort, almost any other key for status
revenge2         (id_rsa)
1g 0:00:00:00 DONE (2021-08-13 21:13) 2.702g/s 636721p/s 636721c/s 636721C/s revenge2
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```


```
root@evilsaint:~# john converted_hash --show
id_rsa:revenge2

1 password hash cracked, 0 left
```