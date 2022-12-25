---
layout: post
title: 'Linux Security :- Secure Shell (SSH) Introduction'
date: '2022-01-27'
description: 'This article serves as an introduction to the Secure Shell (SSH) protocol for Linux users. When you need to connect to Linux servers remotely, SSH is the most commonly used method. SSH provides a text interface by spawning a remote shell. All commands entered in your local terminal are sent to the remote server and executed there.'
coverimage: Secure_Shell_SSH_intro.jpg
tags: linux ssh
published: true
posttype: article
categories: article
---

###What is SSH?

SSH, or Secure Shell, is a "tried and tested" protocol that has been in use since 1995. The SSH protocol allows remote servers to be controlled and modified securely, even over insecure networks. This is accomplished through a text interface, which accepts input from your local terminal and sends it to the remote server for execution; All the while providing encryption for the communication exchanged.

###SSH enumeration with Linux

Obtaining information from running services is used by penetration testers during the enumeration phase of an engagement to gain insight into the target(s) under review. We can ascertain the following from the SSH protocol:
  
* SSH package version - You might be able to find the operating system and version.  
* SSH key fingerprint - Has the key been re-used somewhere (Another machine? Same machine, just another port/service?).  
* SSH banner - Any text (if at all) before the password prompt (often get legal warnings about connecting to it).

Let us look into acquiring this information with our Linux-based machine.

#### Server version

SSH operates on a client-server architecture. In this architecture, the host being accessed acts as an SSH server, while the host connecting the server acts as an SSH client. Both these utilise the SSH protocol with the help of software; The most common of which is the OpenSSH package for Linux-based systems. 

To find out the SSH server software and its version in use, we can use netcat.

```
nc 10.0.0.1 22

SSH-2.0-OpenSSH_8.4p1 Debian-6
```

#### Fingerprinting

SSH keys provide access without the need for passwords. They consist of public and private key pairs, which act as means to encrypt and decipher data exchanged. 

Public keys can also be used to verify the identity of the offering party. A hash obtained from the public key, is also known as fingerprint. SSH servers display their fingerprints to users when they first connect to the server, or the public key of the server has changed since the last time a connection was initiated. 

To obtain the fingerprint of a server, you can use the SSH client on Linux.

```
root@kali:~# ssh root@10.11.1.71  


The authenticity of host '10.11.1.71 (10.11.1.71)' can't be established.  
ECDSA key fingerprint is SHA256:AibCWx1KvdJmNHd3KVsYksWtveJPdLZAsHMIChsTeHE.  
Are you sure you want to continue connecting (yes/no)?  
```

#### SSH Banner

Before allowing authentication, the SSH server can display a pre-configured message to its users. If an SSH banner is configured, you can see it while fingerprinting the server as described above.

#### Nmap

Nmap is a network mapping tool that is an essential part of every penetration tester's arsenal. The installation comes with Nmap Scripting Engine (NSE), which allows users to write and share network enumeration scripts. These scripts are most typically located at /usr/share/nmap on Linux machines.

To view which SSH scripts are available with your version of nmap, use `ls` . 

```
bash  
root@root:~/# ls -ls /usr/share/nmap/scripts/*ssh*
8 -rw-r--r-- 1 root root 5659 Sep 2 2016 /usr/share/nmap/scripts/ssh2-enum-algos.nse  
16 -rw-r--r-- 1 root root 15363 Sep 2 2016 /usr/share/nmap/scripts/ssh-hostkey.nse  
4 -rw-r--r-- 1 root root 1446 Sep 2 2016 /usr/share/nmap/scripts/sshv1.nse`  
```

The nmap host key script allows fingerprinting and banner grabbing across the network.

```
nmap 192.168.1.0/24 -p 22 -sV --script=ssh-hostkey  

[...]
| ssh-hostkey: Possible duplicate hosts
| Key 1024 60:ac:4d:51:b1:cd:85:09:12:16:92:76:1d:5d:27:6e (DSA) used by:
|   192.168.1.1
|   192.168.1.2
| Key 2048 2c:22:75:60:4b:c3:3b:18:a2:97:2c:96:7e:28:dc:dd (RSA) used by:
|   192.168.1.1
|_  192.168.1.2
```

#### Hydra

SSH servers can be configured to support password authentication. This means that upon connecting, a username and password are required for the user to log in. Unlike key-based authentication, password authentication exposes the server brute force password attacks.

Hydra is a tool for performing brute force attacks on SSH. These attacks can be set to use a word list, a password list, or a host list.

Several hosts with a username and a password list:

```
hydra -L /usr/share/ncrack/default.usr  -P /usr/share/wordlists/rockyou.txt -M hosts.txt ssh
```

One host with a password list against a single user ("root"):

```
hydra -l root -P /usr/share/wordlists/rockyou.txt 10.1.1.1 ssh
```

Hydra flags:

```bash  
-M = (FILE) server list for parallel attacks, one entry per line  
-e nsr = "n" null password, "s" try login as pass, "r" try pass as login  
-s = (PORT) if the service is on a different default port, define it here  
-l or -L = single username login or username from FILE  
-p or -P = single password login or password from FILE  
```

#### Word lists  

The success rate of a brute force attack is only as high as the quality of the word list used. Word lists should be chosen with the basis of information gathered within the enumeration phase. For example, attacking the "root" username on a Windows-based SSH server is less likely to grant access than attacking it on a Linux-based SSH server.

Multi-purpose username lists:

```
/usr/share/ncrack/default.usr  

/usr/share/metasploit-framework/data/wordlists/default_users_for_services_unhash.txt  
```

The most 14344392 popular passwords from a breached password database collection:

```
/usr/share/wordlists/rockyou.txt  
```

### About SSH keys  
  
SSH keys are a more secure way of logging into a server with SSH than a password alone. As we have uncovered earlier, a brute force attack can eventually crack a password; However, it is nearly impossible to decipher SSH keys with brute force alone. 

When generating a key pair, you will end up with two long strings of characters: a public and a private key. You can put the public key on any server and then unlock it by connecting with a client that has the private key. When the two match, the system unlocks without requiring a password. You can boost security even further by encrypting the private key with a password.

#### How to set up SSH keys 

<https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2>  
  
In case of key-based authentication, the SSH connection requires two key pairs:

* Your client software compares the server's (host) public key to data encrypted with the host private key. This necessitates having a copy of the server's public key, which you are given at the first connection and which your client stores if you approve of it.
* The server verifies your authentication data, which is encrypted with your user private key, using a copy of your user public key that it has because you placed it there (or had someone put there for you).
  
#### Step One - Create the RSA key pair  
  
The first step is to create the key pair on the client machine (which is likely just your computer):
  
```
ssh-keygen -t rsa  
```

#### Step Two - Store the keys and passphrase  
  
Once you have entered the Gen Key command, you will get a few more questions:  
  
```
Enter file in which to save the key (/home/demo/.ssh/id_rsa):  
```
  
You can press enter here, saving the file to the user's home (in this case, my example user is called demo). 

Next, you will be prompted for a passphrase.
  
```
Enter passphrase (empty for no passphrase):  
```

It is entirely up to you whether or not to use a passphrase. Entering a passphrase has advantages: the security of a key, no matter how encrypted, is still dependent on the fact that it is not visible to others. If an unauthorised user obtains a pass-protected private key, they will be unable to log in to the accounts associated with it until they figure out the passphrase, buying the hacked user some time. The only disadvantage of having a passphrase is having to type it in each time you use the key pair.
  
The entire key generation process looks like this:  

```bash  
ssh-keygen -t rsa  
Generating public/private rsa key pair.  
Enter file in which to save the key (/home/demo/.ssh/id_rsa):  
Enter passphrase (empty for no passphrase):  
Enter same passphrase again:  
Your identification has been saved in /home/demo/.ssh/id_rsa.  
Your public key has been saved in /home/demo/.ssh/id_rsa.pub.  
The key fingerprint is:  
4a:dd:0a:c6:35:4e:3f:ed:27:38:8c:74:44:4d:93:67 demo@a  
The key's randomart image is:  
+--[ RSA 2048]----+  
| .oo. |  
| . o.E |  
| + . o |  
| . = = . |  
| = S = . |  
| o + = + |  
| . o + o . |  
| . o |  
| |  
+-----------------+  
  
The public key is now located in /home/demo/.ssh/id_rsa.pub The private key (identification) is now located in /home/demo/.ssh/id_rsa  
```

#### Step Three - Copy the public key  

Once the key pair has been generated, the public key should be placed on the server that we intend to use.
  
Using the ssh-copy-id command, you can copy the public key into the new machine's authorized_keys file. Replace the example username and IP address below with your own.
  
```
ssh-copy-id user@10.1.1.1
```

Alternatively, you can paste in the keys using SSH:  
  
```
cat ~/.ssh/id_rsa.pub | ssh user@10.1.1.1 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"  
```

No matter which command you use, you should see something like this:  

```  
The authenticity of host '10.1.1.1 (10.1.1.1)' can't be established.  
RSA key fingerprint is b1:2d:33:67:ce:35:4d:5f:f3:a8:cd:c0:c4:48:86:12.  
Are you sure you want to continue connecting (yes/no)? yes  
Warning: Permanently added '10.1.1.1' (RSA) to the list of known hosts.  
user@12.34.56.78's password:  
```

You can now log in to user@10.1.1.1 without being prompted for a password. However, if you set a password, you will be prompted to enter it at that time (and every time you log in in the future). 

#### Optional Step Four -Disable the password for root login  
  
Once you have copied your public key to the server and confirmed that you can log in using just the SSH keys, you can restrict root login to SSH keys only.

To do this, open the SSH configuration file:

```
sudo nano /etc/ssh/sshd_config  
```

Find the line that includes PermitRootLogin and change it to ensure that users can only connect using their SSH key:

```
PermitRootLogin without-password  
```

Put the changes into action:

```
reload ssh
```