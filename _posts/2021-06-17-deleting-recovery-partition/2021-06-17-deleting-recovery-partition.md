---
layout: post
title: "Deleting the Recovery Partition"
date: "2021-06-17"
description: "Ever expanded the size of a disk to find a recovery partition taking up some of your continuous space?"
coverimage: Deleting_the_Recovery_Partition.jpg
tags: windows diskpart admin diskmngr
published: true
posttype: article
categories: tutorial
---
Ever expanded the size of a disk to find a recovery partition taking up some of your continuous space?

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
<img src="/static/ccf2a991-55e3-4555-8b2d-19bc63b297fb.png" class="figure-img img-fluid border border-1 border-dark" alt="Recovery Partition in Our Continuos Space.">
<figcaption class="figure-caption text-center fw-normal text-dark">Recovery Partition in Our Continuos Space.</figcaption>
</figure>

<h5 class="step">Step 1 - Open diskpart.exe</h5>

Open up a CMD and type in diskpart. The software will open up an interactive console. 

<h5 class="step">Step 2 - Listing Disks</h5>

```
LIST DISK
```

<h5 class="step">Step 3 - Select A Disk</h5>

```
SELECT DISK 0
```

<h5 class="step">Step 4 - Listing Disks</h5>

```
LIST PARTITION
```

<h5 class="step">Step 5 - Select Partition</h5>

```
SELECT PARITION 4
```

<h5 class="step">Step 5 - Deleting Partitions</h5>

```
DELETE PARTITION OVERRIDE
```