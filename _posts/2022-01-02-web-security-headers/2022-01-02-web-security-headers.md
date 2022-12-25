---
layout: post
title: 'Web Security Headers'
date: '2022-01-02'
description: 'Web application security headers can make a big difference in reducing the attack surface of a clients application. Knowing the differences is an essential part of a consultants repertoire.'
coverimage: Web_Security_Headers.jpg
tags: browsers content-security-policy csp hsts security-headers strict-transport-security x-content-type-options x-frame-options
published: true
posttype: article
categories: article
---
HTTP Headers are typically easy to implement and can significantly increase the security of your website and help prevent security vulnerabilities like Cross-Site Scripting, Clickjacking, Information disclosure and more. In this article, we are concerned chiefly with five security headers that can be implemented to improve the security posture of your website.

- X Frames Options
    - X-Frames-Options
- XSS Protection
    - *X-XSS-Protection*
- X Content-Type Options
    - *X-Content-Type-Options*
- Strict Transport Security Header (HSTS)
    - *Strict-Transport-Security*
- Content Security Policy (CSP)
    - *Content-Security-Policy*

While these headers are certainly not in order of importance, I have purposely left Security Headers with more discussion points until later in this article.

A quick word on setting headers. While the exact location of a webservers domain configuration file can be customised, it is quite common for the Nginx and Apache Configuration files to be at the following locations named typically after the web domain name or utilising the default configuration example. If your website was `something.com`, then check for a file called `something.com.conf` or potentially `default.conf` if the default template file was used.

- /etc/apache2/sites-enabled/<website name|default>.conf
- /etc/nginx/sites-enabled/<website name|default>.conf file:

## X Frames Options - X-Frames-Options

The X-Frame-Options HTTP response header can be used to indicate whether or not a browser should be allowed to render a page in a `<frame>`, `<iframe>`, `<embed>` or `<object>`. Sites can use this to avoid clickjacking attacks by ensuring that their content is not embedded into other sites.

There are three ways to configure X-Frame-Options:

**DENY:** This will disable iframe features altogether.

**SAMEORIGIN:** iframe can be used only by someone of the same origin.

**ALLOW-FROM:** This will allow pages to be put in iframes only from specific URLs.

Nginx

```bash
add_header X-Frame-Options "SAMEORIGIN";
```

Apache

```bash
Header always set X-Frame-Options "SAMEORIGIN"
```

## XSS Protection - *X-XSS-Protection*

The XSS Protection header is an older security header that enables cross-site scripting protection in Internet Explorer, Chrome and Firefox. Although this functionality is now provided by CSP, which allows us to block inline JavaScript, the header can still provide protection when used with older browsers that do not support the Content Security Policy header.

You can implement XSS protection using the three options depending on the specific need.

**X-XSS-Protection: 0**: This will disable the filter entirely.

**X-XSS-Protection: 1**: This will enable the filter but only sanitizes potentially malicious scripts.

**X-XSS-Protection: 1; mode=block**: This will enable the filter and completely blocks the page.

Nginx

```bash
add_header X-XSS-Protection "1; mode=block";
```

Apache

```bash
Header set X-XSS-Protection "1; mode=block"
```

## X Content-Type Options - X-Content-Type-Options

This header tells the browser that the Multipurpose Internet Mail Extensions (MIME) types advertised in the Content-Type header should be followed. This header was first introduced by Microsoft to prevent MIME sniffing.

MIME sniffing is where a browser looks at the contents of a given resource and attempts to detect the MIME type. An attack vector can open up if an attacker can control the content of a resource or upload a new resource to a website, and this Security Header is not advertised in the resource response. The attacker might make non-executable content appear to be executable content and trick the browser into executing it in the victim's browser.

Nginx

```bash
add_header X-Content-Type-Options "nosniff"
```

Apache

```bash
Header set X-Content-Type-Options "nosniff"
```

## Strict Transport Security Header (HSTS) - *Strict-Transport-Security*

The first header for discussion is the Strict Transport Security Header (HSTS). The HSTS forces web browsers or clients to communicate with servers but only through HTTPS connections. HSTS ensures that connections only use HTTPS and prevent man in the middle, downgrade, and cookie hijacking attacks. HSTS is a trust on first use (sometimes abbreviated as TOFU), meaning it must send at least one insecure connection over HTTP to the host to transfer the security header.

The HSTS preload list is an effort to provide browsers with a list of sites that support HSTS to avoid this initial, insecure connection. However, suppose a site is not on the list but uses the 'strict-transport-security' header; after the initial exchange, the browser will only request access to the website over Transport Layer Security (TLS).

Nginx

```bash
add_header Strict-Transport-Security 'max-age=31536000; includeSubDomains; preload';
```

Apache

```bash
Header set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
```

## Content Security Policy (CSP) - *Content-Security-Policy*

Content Security Policy, sometimes abbreviated as CSP, is a security header that helps mitigate the risk of certain types of data injection attacks such as cross-site scripting. CSP allows website administrators to eliminate or mitigate cross-site scripting by defining a policy that stipulates what locations browsers should trust and allows script execution. The Content Security Policy header is sometimes referred to as a poor man's Web Application Firewall (WAF).

This header is set in HTTP response when an HTML document is requested by a user. Content Security Policy enables the website to list precisely which domains the HTML document can load scripts from. Browsers then deny requests for scripts from any other servers.

The architecture of this policy allows sites to safelist only servers containing resources they need, such as CDNs and plugins. As a result, if an attacker successfully gets a page to request a script through a cross-site scripting attack, the browser will refuse to load the script because the origin isn't on the safelist.

In addition to disallowing non-whitelisted sites, a site that implements the Content Security Policy header no longer supports inline JavaScript. This means that sites must remove code within script tags in an HTML file, JavaScript URLs, and inline event handlers and handle those tasks using script files instead.

As an ultimate form of protection, sites that want to never allow scripts to be executed can opt to globally disallow script execution. In addition to restricting the domains from which content can be loaded, the server can specify which protocols were allowed. So, for example, a server can specify that all content must be loaded using HTTPS.

However, it should be noted that as part of a defence in depth approach, all cookies should be marked with a secure flag to ensure that they can't be transmitted over HTTP.

Nginx

```bash
add_header Content-Security-Policy "default-src 'self'; font-src *;img-src * data:; script-src *; style-src *";
```

Apache

```bash
Header always set Content-Security-Policy "default-src 'self'; font-src *;img-src * data:; script-src *; style-src *;"
```