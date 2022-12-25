---
layout: post
title: "A Guide On How To Winget"
date: "2022-02-09"
description: "In this tutorial, we will take a look at Microsofts new package manager. A package manager is a tool designed to help you quickly search for and install other tools that your operating system supports. Microsoft's offering is called Winget."
coverimage: A_Guide_On_How_To_Winget.jpg
tags: winget
published: true
posttype: article
categories: tutorial
---
Winget is the name of Microsofts new package manager.  A package manager is a tool designed to help you quickly search for and install other tools that your operating system supports.  Windows users have waited a long time for a native package manager offering from Microsoft. Until now the best offering has been chocolatey; however, after exploring some of the features I will show you in this tutorial I have made the switch.

* Winget does not come installed by default. 
* There are several ways to install it
  - Microsoft Store
  - Release On Github
  - Being a Member of the Windows Insiders Program (Free but may take time to appear as an install option.)

![Winget Not Installed By Default](/static/3473ff07-a0b0-4652-8acd-58a5e0c2028c.png)	

Winget only supported on certain OS Versions.
@todo: Enter OS that Winget Supports

![CMD Version](/static/1423747c-29b1-4338-b389-f34eab49ddfb.png)

* Because you will be using Winget to install the software you typically need to open up the console as an Administrator. 

In this tutorial, we will be installing from Github however you can just as easily install this from the Windows Store if you have it enabled. 
* It can take a few days for the option to install Winget to appear 

I will be installing this release
* https://github.com/microsoft/winget-cli/releases/tag/v1.2.10271


![Installing Winget From Github](/static/3028d678-c368-4761-944a-2a448d8de2c4.png)

After you click to download you can install by selecting open. This saves you having to save to disk, find the file and then having to click again to install. 

![Click on Open To Install](/static/6e4154f2-466b-4bf6-bf0b-44841fdb29e3.png)

Windows will now install winget. We can verify this by seeing the version 
```
winget --version
```

![Winget Version](/static/499a492e-fdda-43de-a35a-9ca967dc4452.png)

The first time you actually use Winget for anything other than verifying its version you will see an agreement acceptance response like that below. 

![Accepting The Agreement](/static/00a6de3e-dac2-4ae2-955d-d38f2bd3eef5.png)


We can list packages currently installed on our machine. 
```
winget list
```

![Listing Packages Installed On The Machine](/static/538a8041-358f-4155-be0c-d9cbc2670bed.png)

We can query these packages 
```
winget list -q <term>
```

![Querying The List](/static/b7543e67-b171-47dc-b1ee-330d3d953be7.png)

We can show only the first `n` number of results
```
winget list -n 10
```

Searching for new packages
```
winget search <term>
```

![Searching Winget For The Term Git](/static/34d462bf-5a6f-4b4b-9ac0-58c3d38cddb6.png)

If you look at the last example you can see Winget searching for packages from two sources. We can list the sources that Winget knows about. 

```
winget source list
```

> Note you can install additional sources but that is out of scope for this tutorial. This is just a crash course to get you started. 


We can also inspect a source. 

![Inspecint A Source](/static/ce39df38-f611-405a-aced-f0badd8b2b47.png)

Okay, let us go back to searching.  This time we will limit our search to a source. 

![Searching By Source](/static/28a09c06-002d-4162-9b83-823e44c94bcb.png)

We can also search by Tag
```
winget search --tag github
```

![Searching By Tag](/static/513804fc-8bce-499e-a753-e8e1fd76a32c.png)

We will now look at installing packages. We will look at a worked example using Microsoft Visual Studio Code. 

First, we will search. 

![Searching For Microsoft Visual Studio Code](/static/576dd63e-cb51-44c0-bbf0-1e45dca2d7de.png)

The top result looks like what we want. We can use the `show` subcommand to find more information. 

![Using The Show Command](/static/937e36c4-3bd2-4a63-bccc-efc82e4860c1.png)


If you look at the arrow we can see the package has a moniker. Typing `Microsoft.VisualStudioCode` with the install command would work but it is a lot to type. Instead, we can reference the package using the moniker. 


![Referencing By Moniker](/static/965b468d-94aa-485c-99dd-9f8412668318.png)

We can now install the package
```
winget install vsscode
```

![Installing Visual Studio Code](/static/3a9134d0-16b8-4b59-aaa6-b4ad84f84d1e.png)

Great so our package is now installed. 

When installing you may see UI windows open like in the screenshot below. 

![Installing the Github CLI](/static/f43a3347-59c3-4dd0-8893-e0f64d56b23f.png)

We can prevent that happening, however. 

First I will show you how to uninstall a package and then we will install it a second time without the UI prompts. 

Uninstalling 
```
winget uninstall Github.cli
```
![Uninstall Github CLI](/static/e8b7f955-ffc7-43ce-803f-e925dc5a336d.png)

Installing with the `--silent` flag. 

![Installing Silentlly](/static/7ce4226c-49e4-4a9a-9127-6c2a655d1baf.png)

I have gone ahead and installed a few other programs and I like the collection of packages I have installed. What if I wanted to save this combination of packages for installation on another machine. 

```
winget export -o <file path>
```
![Exporting List Of Packages](/static/ed2c9105-f0ab-417a-8de1-a64c7ecbead3.png)

We can see that some Windows Packages do not have packages available in the Microsoft Store or in the Winget repository. 

Let us check the file we saved. Here it is where we specified on the desktop.

![Winget Output](/static/ad40e2ad-b3ef-4bd4-b269-87fb324455c7.png)

If we look inside we can see the file is in JSON and we can see a collection of packages I have installed. 

![Winget Packages Output Contents](/static/1b919405-011b-490a-9273-469437feab00.png)

I have removed some of the packages I installed but not all. Now I want to get the system up to sync. 

![Checking Packages](/static/0f1eeceb-4f94-4989-8d4d-d089b8bc577f.png)

As Winget gets to packages that are either not installed or have upgrades it gets to work. 

![Installing and updating missing packages](/static/dc2c1021-aaac-42b8-8776-53f1e06e45be.png)