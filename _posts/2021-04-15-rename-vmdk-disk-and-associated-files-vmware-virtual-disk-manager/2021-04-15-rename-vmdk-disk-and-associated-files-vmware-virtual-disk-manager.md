---
layout: post
title: "Rename a VMDK disk and associated files with VMware Virtual Disk Manager"
date: "2021-04-15"
description: ""
coverimage: Rename_a_VMDK_disk_NXkvZv4.jpg
tags: vmdk vmware windows
published: true
posttype: article
categories: tutorial
---
I have a template Kali Linux Virtual Machine that I keep around in case in case of an unfixable error on my current engagement machine. The filenames inside this folder are quite verbose and are intentional to remind me what platform and version it is come to using the backup. This is fine for keeping backups but due to my OCD, I like to rename my engagement machines. Inside the below explorer window it can look quite daunting seeing all the verbose filenames associated with the machine; many consultants do not attempt to rename their machines in fear of messing them up. After all the machine has many associated files we need to rename. 

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
  <img src="/static/2d42660e-1b6f-4ccb-a499-b4ea35c8ad7e.png" class="img-fluid" alt="Explorer Windows Showing our Existing Machine.">
<figcaption class="figure-caption text-center fw-normal text-dark">Explorer Windows Showing our Existing Machine.</figcaption>
</figure>

The VMware Virtual Disk Manager is a tool that is included with VMware Workstation. 

> TLDR - What we are about to do is make a new machine with the exact naming convention and filesystem we want. We will then delete its hard disk drive and using the VMware Virtual Disk Manager tool included with VMware Workstation we will rename (and move) the old disk from the template to match. 

> NB - I said we will be renaming and moving the hard disk drive so if you would like to use this baseline in the future then I suggest it is something you keep additional copies of. 


It is typically installed into the following folder:

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
  <img src="/static/02a92320-760d-4845-916a-e4edc0ab6fdf.png" class="img-fluid" alt="Explorer window of our old vmware machine with unfriendly name">
<figcaption class="figure-caption text-center fw-normal text-dark">Explorer window of our old vmware machine with unfriendly name.</figcaption>
</figure>


This folder contains lots of files. Check for the presence of the VMware Disk Manager tools by using a wildcard character following some of the initial prefix e.g.  `dir vmware*`

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
 <img src="/static/6be777e4-da34-4b67-9528-367bbffcd5cb.png" class="img-fluid" alt="cmd prompt set that the location of 'vmware workstation`">
<figcaption class="figure-caption text-center fw-normal text-dark">cmd prompt set that the location of 'vmware workstation`.</figcaption>
</figure>



Great so the tools are present. We want to rename our VMware Virtual Machine contained in the following folder. 

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
 <img src="/static/2a5184cd-9612-41a4-bee7-12ba20938960.png" class="img-fluid" alt="cmd promp with the `dir vmware*` showing all files begining with vmware">
<figcaption class="figure-caption text-center fw-normal text-dark">cmd promp with the `dir vmware*` showing all files begining with vmware`.</figcaption>
</figure>


As I do not need the VM Image to function from inside VM Workstation I am going to remove the image. Note this does not delete it from the disk. 


<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
 <img src="/static/a254b426-3829-407b-8d4c-f7a71bb0ba91.png" class="img-fluid" alt="Explorer title bar navigated to the directory of the machine we are going to be cleaning">
<figcaption class="figure-caption text-center fw-normal text-dark">Explorer title bar navigated to the directory of the machine we are going to be cleaning.</figcaption>
</figure>

Next, I am going to install a new VM in a directory with the desired name in my desired location. 

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
 <img src="/static/0dd01745-5d59-4429-8564-dcc8d3cb1ebf.png" class="img-fluid" alt="New Virtual Machine Wizard">
<figcaption class="figure-caption text-center fw-normal text-dark">New Virtual Machine Wizard.</figcaption>
</figure>

When asked choose "I will install the operating system later."

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
 <img src="/static/0d7e254f-af24-47cd-be8f-08eedb7aeaaa.png" class="img-fluid" alt="I will install the operating system later.">
<figcaption class="figure-caption text-center fw-normal text-dark">I will install the operating system later.</figcaption>
</figure>

Set this new VM in your desired location and choose the name you wish for your VM

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"> 
<img src="/static/d269f440-83d2-413a-a1d5-b50e8ab33fef.png" class="img-fluid" alt="Naming the machine BlackWidow through the New Virtual Machine Wizard">
<figcaption class="figure-caption text-center fw-normal text-dark">Naming the machine BlackWidow through the New Virtual Machine Wizard.</figcaption>
</figure>


Before finishing I like to customise the machine's hardware. For this machine, I am going to perform the following


<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"> 
<img src="/static/a9e02412-2cbe-473a-8cab-b30e69d5a07d.png" class="img-fluid" alt="New Virtual Machine Wizard - Customise Options">
<figcaption class="figure-caption text-center fw-normal text-dark">New Virtual Machine Wizard - Customise Options.</figcaption>
</figure>


* Remove Printer
* Remove Sound Card
* Increase memory to 8GB
* Go to "Display" and select "Accelerate 3d Graphics". Then change the Graphics memory to 1GB. 
* Under the CPU I will select the following checkboxes. 
  * Virutalize Intel VT-x/EPT or AMD-V/RVI
  * Virtualize CPU Performance Counters
  * Virtualize IOMMU (IO memory management unit)


With our machine installed our first job is to delete the brand new created virtual disk. 

Either go to the `Settings` or press CTRL` + `D` and remove the hardisk

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"> 
<img src="/static/8a3dd6b1-6db2-48ba-9dd6-cccf8e6586b6.png" class="img-fluid" alt="Remove Hardrive from machine settings">
<figcaption class="figure-caption text-center fw-normal text-dark">Remove Hardrive from machine settings.</figcaption>
</figure>

Now lets remove the harddrive from the physical disk. Remember the quick way to get to the folder is to right-click on the machine name and select "Open VM directory". With the folder open we can now see our new VM and all of its files. Open up the Virtual Machine by right clicking on it similar to earlier

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"> 
<img src="/static/05d4c2f1-4f1b-4093-b9a4-4a28d1de2c2a.png" class="img-fluid" alt="Open VM Directory of new BlackWidow Machine">
<figcaption class="figure-caption text-center fw-normal text-dark">Open VM Directory of new BlackWidow Machine.</figcaption>
</figure>


Remove the Hard disk, the file ending in `vmdk` from the folder. VM in workstation. 


<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"> 
<img src="/static/4abdd554-77f8-4e9c-aaad-1ce982df7ff7.png" class="img-fluid" alt="Remove the BlackWidows.vmdk file from explorer">
<figcaption class="figure-caption text-center fw-normal text-dark">Remove the BlackWidows.vmdk file from explorer.</figcaption>
</figure>


Delete the file from the hard drive 

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"> 
<img src="/static/e1ff1309-bdda-4c13-ba0f-bdaef6f9f371.png" class="img-fluid" alt="Remove the BlackWidows.vmdk file from explorer">
<figcaption class="figure-caption text-center fw-normal text-dark">Remove the BlackWidows.vmdk file from explorer.</figcaption>
</figure>




Next we use the Virtual Disk Manager to rename the old hardrive and place it in the new location. 

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"> 
<img src="/static/727b3493-64b1-4206-b5df-4e4fa84ddb90.png" class="img-fluid" alt="Using the vmware-vdiskmanager.exe to rename and move old machines vmdk">
<figcaption class="figure-caption text-center fw-normal text-dark">Using the vmware-vdiskmanager.exe to rename and move old machines vmdk.</figcaption>
</figure>


> Note: This will move the hard drive from the original folder. You might want to make a copy. 

Now all is left is to attach the hard drive. 

Got to settings `CTRL` + `D` and click "Add". Select Hard Disk.

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"> 
<img src="/static/ec12321f-6996-4673-a974-11eccd2a86c1.png" class="img-fluid" alt="Add Hardware Wizard - Select Hard Disck and Click Next">
<figcaption class="figure-caption text-center fw-normal text-dark">Add Hardware Wizard - Select Hard Disck and Click Next.</figcaption>
</figure>


<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"> 
<img src="/static/6800f692-e604-40a1-8a06-bbdee9ff7904.png" class="img-fluid" alt="Add Hardware Wizard - Select Use an existing vitural disk">
<figcaption class="figure-caption text-center fw-normal text-dark">Add Hardware Wizard - Select Use an existing vitural disk.</figcaption>
</figure>



Choose "Use an existing virtual disk". 

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"> 
<img src="/static/81aedfc6-cd7f-48b5-bf5f-e42774157a6c.png" class="img-fluid" alt="Add Hardware Wizard - Select Use an existing vitural disk">
<figcaption class="figure-caption text-center fw-normal text-dark">Add Hardware Wizard - Select Use an existing vitural disk.</figcaption>
</figure>


Select the disk you just created and click "Finish" 

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"> 
<img src="/static/996e9216-5c62-4fd9-9da4-cf27af2d52eb.png" class="img-fluid" alt="Add Hardware Wizard - Browser to newly created BlackWidow.vmdk">
<figcaption class="figure-caption text-center fw-normal text-dark">Add Hardware Wizard - Browser to newly created BlackWidow.vmdk.</figcaption>
</figure>


You can now delete the old folder that contained your machine Hard Disk and boot your new machine up.