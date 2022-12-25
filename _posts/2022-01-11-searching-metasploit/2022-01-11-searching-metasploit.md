---
layout: post
title: "Searching Metasploit"
date: "2022-01-11"
description: ""
coverimage: Searching_Metasploit.jpg
tags: metasploit
published: true
posttype: article
categories: tutorial
---

# Searching Metasploit

Search for payloads, exploits, or post modules

```ruby
search type:payload
search type:exploit
search type:post
```

Search for all modules that operate on a given port (e.g. FTP, SSH, HTTP)

```ruby
search port:21
search port:22
search port:80
```

Search all CVEs from 2011

```ruby
search cve:2011
```

Careful when analysing the results of this above command. If you use the above example you might see a row like

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/a706cbf8-d01d-4895-9c5d-ea82775626c3.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal">Snippet From Search for CVE from 2011 </figcaption></figure>

```ruby
68   exploit/unix/webapp/joomla_tinybrowser                            2009-07-22
```

This example shows the disclosure date as a date in 2009. If we check the module however the module information. We can see the CVE is actually dated 2011 ([https://nvd.nist.gov/vuln/detail/CVE-2011-4908](https://nvd.nist.gov/vuln/detail/CVE-2011-4908))

```ruby
info exploit/unix/webapp/joomla_tinybrowser
```

Search by Exploit DB

```ruby
search edb:9296
```

Search for exploits by their AKA
```ruby
search aka:shellshock
search aka:heartbleed
search aka:eternalblue
```

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/2dd2c2c8-f46d-4f43-ad73-d1688a5371de.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal">Metasploit Search For Heartbleed </figcaption></figure>

Search by platform
```ruby
search platform:windows
search platform:linux
search platform:mac
```

Or exclude a platform

```ruby
search platform:-windows
```

There are a few other options that we can add and we will go through some advanced search examples and introduce them. 

In this search, we are looking for results that contain only exploits with an associated CVE released in 2017. We exclude any results that are for the Windows platform. Then we filter out by the exploits that operate on port 80 with a rank of ‘excellent’ 

```ruby
search type:exploit cve:2017 platform:-windows port:80 rank:excellent
```

As you can see we can get very specific in what we are looking for by chaining search criteria together.

If we add the word ‘check’ to the end of our search then we will only see modules that have a Metasploit check included. 

<aside>
💡 the `check` command is available for some modules. The check command is designed so you can test if the target is vulnerable before attempting to exploit it.

</aside>

The full command

```ruby
search type:exploit cve:2017 platform:-windows port:80 rank:excellent check 
```

At the current time of writing, I am down to 5 results with the above search. We wanted to test all these modules but we wanted to order them. These are the modules as we currently see them. 

```ruby
#  Name                                                              Disclosure Date  Rank       Check  Description
   -  ----                                                              ---------------  ----       -----  -----------
   0  exploit/linux/http/jenkins_cli_deserialization                    2017-04-26       excellent  Yes    Jenkins CLI Deserialization
   1  exploit/linux/http/kaltura_unserialize_cookie_rce                 2017-09-12       excellent  Yes    Kaltura Remote PHP Code Execution over Cookie
   2  exploit/multi/http/october_upload_bypass_exec                     2017-04-25       excellent  Yes    October CMS Upload Protection Bypass Code Execution
   3  exploit/multi/http/shopware_createinstancefromnamedarguments_rce  2019-05-09       excellent  Yes    Shopware createInstanceFromNamedArguments PHP Object Instantiation RCE
   4  exploit/unix/http/zivif_ipcheck_exec                              2017-09-01       excellent  Yes    Zivif Camera iptest.cgi Blind Remote Command Execution
```

 If we add the `-s` option we can pick a target field for us to search on. Options are "rank", "disclosure_date", "name", "date", "type", "check”. 

We can see from the example that the results are already sorted by name so `-s name` at first might seem pointless as this is the default. 

- jenkins_cli_deserialization
- kaltura_unserialize_cookie_rce
- october_upload_bypass_exec
- shopware_createinstancefromnamedarguments_rce
- zivif_ipcheck_exec

However, we can also reverse the search entries so we get the results starting Z and working back to J. 

```ruby
search type:exploit cve:2017 platform:-windows port:80 rank:excellent check -s name -r
```

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/e83e8a98-93c0-46e4-a1a2-5f3ac4ad0111.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal">Reverse Name Order Search</figcaption></figure>


Or we can search by date order (which of course can be reversed)

```ruby
search type:exploit cve:2017 platform:-windows port:80 rank:excellent check -s date
search type:exploit cve:2017 platform:-windows port:80 rank:excellent check -s date -r 
```

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/4b1d7537-a1d0-4f8d-9860-302f79e2c2da.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal">Reverse Date Order Search</figcaption></figure>

What if we are happy with our results but we want to save them so we can work through them and mark them off? Metasploit allows you to export your results to a CSV

```
::ruby
search type:exploit cve:2017 platform:-windows port:80 rank:excellent check  -o /tmp/output.csv
[*] Wrote search results to /tmp/output.csv
```

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/3714d5e4-addd-4458-8d4f-9783762aec6d.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal">Export to CSV</figcaption></figure>

Another example where we search for all Metasploit modules of type exploit, for the Windows platform and operate on port 445 (SMB). We then specify the ‘check’ column as the search column meaning the exploits will be sorted by those that have a ‘check’ option first and then we output the result to CSV. 

```
::ruby
search type:exploit platform:windows port:445 -s check -o /tmp/windows-smb.csv
```

Lastly, what if we wanted to perform a search and then export that search to a CSV file as a oneliner without remaining in the Metasploit console. 

```ruby
msfconsole -qx 'search type:exploit platform:windows port:445 -o /tmp/win-smb.csv;exit'
```