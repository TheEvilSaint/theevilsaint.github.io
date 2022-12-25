---
layout: post
title: 'Firefox Browser Forensics'
date: '2021-08-12'
description: 'A look at Firefox Browser forensics'
coverimage: Firefox_Browser_Forensics.jpg
tags: browser
published: true
posttype: article
categories: article
---
# Firefox: History


## SQLite Database

> Inside the places.sqlite database file, there are two tables, moz_places and moz_annos, that store information needed to find downloaded files

content-prefs.sqlite: stores site-specific content preferences.
cookies.sqlite: stores cookie data.
formhistory.sqlite: stores form input data.
permissions.sqlite: stores site-specific permissions.
places.sqlite: stores the history of visited sites.
webappstore.sqlite: stores Document Object Model (DOM) storage data.
Note: This list was compiled from multiple sources. Not all files for every system are listed, and some files on this list may not be found on other systems.


Locate the Mozilla storage files. 
```
student@attackdefense:/$ cd ~/.mozilla/firefox/zevp8nk2.default/
student@attackdefense:~/.mozilla/firefox/zevp8nk2.default$ ls *.sqlite
content-prefs.sqlite  favicons.sqlite     kinto.sqlite        places.sqlite        storage.sqlite
cookies.sqlite        formhistory.sqlite  permissions.sqlite  storage-sync.sqlite  webappsstore.sqlite
student@attackdefense:~/.mozilla/firefox/zevp8nk2.default$ sqlite3 places.sqlite
SQLite version 3.11.0 2016-02-15 17:29:24
Enter ".help" for usage hints.
sqlite>
```

We can query all records
```
sqlite> select * from moz_places;
1|https://support.mozilla.org/en-US/products/firefox||gro.allizom.troppus.|0|0|0|137||iZpT3cYccuf2|1|47357795150914||2|https://www.mozilla.org/en-US/firefox/customize/||gro.allizom.www.|0|0|0|137||mkU9pzj7KxEp|1|47357014640010||
3|https://www.mozilla.org/en-US/contribute/||gro.allizom.www.|0|0|0|137||qFX_kGoubYl7|1|47358034485371||4|https://www.mozilla.org/en-US/about/||gro.allizom.www.|0|0|0|137||gHDaQFcqAxTO|1|47358774953055||
5|http://www.ubuntu.com/||moc.utnubu.www.|0|0|0|137||1-dNfm-NfyWA|1|125508050257634||6|http://wiki.ubuntu.com/||moc.utnubu.ikiw.|0|0|0|137||4Em1q9WFjljD|1|125511519733047||
```

We can use SQL queries to interact
```
select * from moz_places where url like '%releases.ubuntu.com%';
```

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/af6edeb8-15ff-4712-a559-6416ddd859d6.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">Querying The Database</figcaption></figure>

Counting the unique hosts.
```
sqlite> select count(*) from moz_hosts;
36
sqlite>
```

History
```
select * from moz_historyvisits where place_id=41;
```

Table Schema 
```
sqlite> .schema
```

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/6fa64eb7-e269-4b8a-99fa-0a7388cac17c.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">Retrieving Schema</figcaption></figure>



## Logins

```
cat logins.json | jq
cat logins.json | python -m json.tool
```

```
consultant@evilsaint:~/.mozilla/firefox/esdp8nk2.default$ cat logins.json | python -m json.tool
{
    "disabledHosts": [],
    "logins": [
        {
            "encType": 1,
            "encryptedPassword": "REDACTED",
            "encryptedUsername": "REDACTED",
            "formSubmitURL": null,
            "guid": "{eedbf6dd-588b-46c5-b122-1999171667f6}",
            "hostname": "chrome://FirefoxAccounts",
            "httpRealm": "Firefox Accounts credentials",
            "id": 1,
            "passwordField": "",
            "timeCreated": 1539732575514,
            "timeLastUsed": 1539732575514,
            "timePasswordChanged": 1539732624376,
            "timesUsed": 1,
            "usernameField": ""
        },
        {
            "encType": 1,
            "encryptedPassword": "REDACTED",
            "encryptedUsername": "REDACTED",
            "formSubmitURL": "https://github.com",
            "guid": "{ccd2077a-2f06-406a-bf42-d8848243c86f}",
            "hostname": "https://github.com",
            "httpRealm": null,
            "id": 2,
            "passwordField": "password",
            "timeCreated": 1539733882060,
            "timeLastUsed": 1539733882060,
            "timePasswordChanged": 1539820282060,
            "timesUsed": 1,
            "usernameField": "login"
        }
    ],
    "nextId": 5,
    "version": 2
}
```


Time GitHub Password changed
```
date -d @1539820282060
# Fri Dec 18 00:07:40 UTC 50764
```