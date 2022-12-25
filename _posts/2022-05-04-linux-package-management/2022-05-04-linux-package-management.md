---
layout: post
title: 'Linux Package Management'
date: '2022-05-04'
description: 'A package manager is a tool that allows users to install, remove, upgrade, configure and manage software packages on an operating system. The package manager can be a graphical application like a software centre or a command-line tool like apt-get or pacman. This article looks at installing packages on Linux using various package management systems.'
coverimage: Linux_Package_Management.jpg
tags: apt dpkg yum
published: true
posttype: article
categories: article
---
# Linux Package Management

Sometimes when installing packages you need to know the version of the current distribution that you have.

Output shows Debian version number.
```
cat /etc/Debian_version
```

Output shows Ubuntu version number.
```
cat /etc/*-release
```

Output shows Redhat / CentOS version number.
```
cat /etc/redhat-release
```

## Debian Package Management

The sources list of all the repos
```
cat /etc/apt/sources.list
```

Or sometimes
```
cat /etc/apt/sources.list.d/official-package-repositories.list
```

Search the apt package manager cache

```
apt-cache search nginx
```

Update the cache

```
apt-get update
```

Show cache information on the package.

```
apt-cache show nginx
```

Show some basic statistics

```
apt-cache stats
```

Remove packages and config files

```
apt-get purge
```

autoclean - Erase old downloaded archive files

```
apt-get autoclean
```

Remove automatically all unused packages

```
apt-get autoremove
```

List installed packages, shows version and package description

```
dpkg -l
```

Lists installed packages but no extra info

```
dpkg --get-selections
```

Install a package

```
dpkg -i <deb file>
```

### Unable to lock the administration directory (/var/lib/dpkg/)

Occasionally when this error occurs the following steps are normally sufficient to solve

Kill any running apt processes

```
ps -A | grep apt
kill -9 processnumber
```

Delete the lock files

```
rm /var/lib/dpkg/lock
```

Force the packages to reconfigure

```
dpkg --configure -a
```

Update package source list

```
apt-get update
```

## Redhat / CentOS / RPM Based Distros

List all installed RPMs on an RPM based Linux distro.

```
rpm -qa
```

Check installed RPM is patched against CVE, grep the output for CVE.

```
rpm -q --changelog openvpn
```

**YUM Commands**

Package manager used by RPM based systems, you can pull some useful information about installed packages and or install additional tools.

Update all RPM packages with YUM, also shows whats out of date.

```
yum update
```

Update individual packages, in this example HTTPD (Apache).

```
yum update httpd
```

Install a package using YUM.

```
yum install package
```

Exclude a package from being updates with YUM.

```
yum --exclude=package kernel* update
```

Remove package with YUM.

```
yum remove package
```

Remove package with YUM.

```
yum erase package
```

Lists info about yum package.

```
yum list package
```

What a packages does, e.g Apache HTTPD Server.

```
yum provides httpd
```

Shows package info, architecture, version etc.

```
yum info httpd
```

Use YUM to install local RPM, settles deps from repo.

```
yum localinstall blah.rpm
```

Shows deps for a package.

```
yum deplist package
```

List all installed packages.

```
yum list installed | more
```

Show all YUM groups.

```
yum grouplist | more
```

Development Tools Install YUM group.

```
yum groupinstall
```