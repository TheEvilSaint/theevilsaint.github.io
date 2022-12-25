---
layout: post
title: "Making A YouTube Scraper"
date: "2021-08-25"
description: ""
coverimage: scraping_youtube.jpg
tags: powershell
published: true
posttype: article
categories: tutorial
---
## Making A YouTube Scraper

[PowerShell](https://www.notion.so/PowerShell-ebe45d6947664ef293d1c94398b1f1a3) 

What do we need:

1. Get an API Credential for Youtube
2. Query the data from Google API

** Get API Credential for YouTube **

<h5 class="step">Step 1</h5>
 First navigate to the following page to setup the Google API [https://console.developers.google.com/apis/dashboard](https://console.developers.google.com/apis/dashboard)

<h5 class="step">Step 2</h5>
 Create a new project

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/61e4f937-0373-499d-9a47-c00e0120097a.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">  </figcaption>Setting Up a New Project</figure>


<h5 class="step">Step 3</h5>
 Then click on Enable APIs and Services. We will need to search for the Youtube API to specify the scope of access.

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/41b26f1e-698d-4a12-9f88-827f76ef36bb.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">  </figcaption>Enabling APIs and Services</figure>


<h5 class="step">Step 4</h5>
 Search for Youtube and select Youtube Data API v3, then select `ENABLE`.

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/cc0c9cea-105e-4f9e-b086-87e457363469.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">  </figcaption>You Need to Enable API Access in Order to Use it</figure>


<h5 class="step">Step 5</h5>
 Finally, click on Credentials on the left side, then select Create Credential

   <figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/21e9104b-a37c-4e94-8498-2eb74bad8705.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">  </figcaption>Select Credentials From The Left Sidebar</figure> 

    

   <figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/fc96e423-c920-4960-a992-8a6d02cccb65.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">  </figcaption>Select Create Credentials to Generate New Credentials For The Project</figure>

    

<h5 class="step">Step 6</h5>
 We are presented with two choices. If in doubt you can select "Help me choose"

   <figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/039b5aba-f7d9-4be1-bd66-63cf1a8edb96.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">  </figcaption>Create Credentials Dropdown</figure>

   

  <h5 class="step">Step 7</h5>
 In this tutorial we will be scraping public data so select that radio option.

   <figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/217f98c7-63fa-4823-bc22-df10cc458db5.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">  </figcaption>Hit Next to Configure Public Data Access</figure> 

        

After hitting 'Next' you should be presented with the following screen.



  <figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/1dad564e-b11c-42d9-a63e-4357be4dcc2f.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">  </figcaption>Once </figure>  

    

  <h5 class="step">Step 8</h5>
 Click on done to be returned to the dashboard. 

   <figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/cc561404-020d-443a-94ea-10b6a50bf8b1.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">  </figcaption> Copy Google API Credentials From The Dashboard</figure> 

   

<h5 class="step">Step 9</h5>
 Select your API Key and open up PowerShell



## Querying data from the YouTube API 

 You can find more information on the YouTube API at the following URL: https://developers.google.com/youtube/v3/docs/

  <h5 class="step">Step 1</h5>
 In the first example we will use gather a list of videos for the the EminemVEVO user. In order to get a list of videos we first need a channel ID which we can search for by username. Once we have the channel ID and the number of videos on the channel we can request a list of videos.

   <figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/52d3665f-15b6-4500-8979-85b921def13b.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">  </figcaption>Using PowerShell ISE to Query The EminemVEVO YouTube Channel</figure> 

    
Here is the example code.

    ```bash
    # Get the API Key
    $GoogleApiKey = 'REDACTED'

    # Query Google API for the details of the EminemVEVO channel
    $data = Invoke-RestMethod -uri "https://youtube.googleapis.com/youtube/v3/channels?part=snippet&part=statistics&part=status&forUsername=EminemVEVO&key=$GoogleApiKey" | Select-Object -expand items

    $channelId = $data.id
    $maxResults = $data.statistics.videoCount

    # Now we can use the search method to search for all the eminem videos in this channel
    $eminem = Invoke-RestMethod -uri "https://www.googleapis.com/youtube/v3/search?channelId=$channelId&order=date&part=snippet&type=video&maxResults=$maxResults&key=$GoogleApiKey" | Select-Object -expand items

    # List all eminem videos by title
    $eminem | ForEach-Object { $_.snippet.title }
    ```

The output of this command gives us the following.

    ```bash
    Eminem - Killer (Remix) [Official Audio] ft. Jack Harlow, Cordae
    Eminem - Alfred&#39;s Theme (Lyric Video)
    Eminem - Godzilla (Lyric Video) ft. Juice WRLD
    Eminem - Darkness (Official Video)
    Eminem - Good Guy (Behind The Scenes) ft. Jessie Reyez
    Eminem - Good Guy ft. Jessie Reyez
    Eminem - Venom
    Eminem - Lucky You ft. Joyner Lucas
    Eminem - Fall
    Eminem - Framed
    Eminem - Nowhere Fast (Extended Version) [Audio] ft. Kehlani
    Eminem - River (Behind the Scenes) ft. Ed Sheeran
    Eminem - Untouchable (Lyric Video)
    Eminem - River ft. Ed Sheeran (Official Video)
    Eminem - River (Trailer: Boxing) ft. Ed Sheeran
    Eminem - Walk On Water ft. Beyoncé (Official Behind The Scenes Video)
    Eminem - Walk On Water (Official Video)
    Eminem - River (Audio) ft. Ed Sheeran
    Eminem ft. Beyoncé - Walk On Water (Official Lyric Video)
    Eminem - Untouchable (Audio)
    Walk On Water/Stan/Love The Way You Lie (Medley/Live From Saturday Night Live/2017)
    Eminem - Walk On Water (Audio) ft. Beyoncé
    Eminem - Partners In Rhyme: The True Story of Infinite (Official Trailer)
    Eminem - Infinite (F.B.T. Remix) (Official Audio)
    Eminem - Phenomenal (Behind The Scenes)
    Eminem - Phenomenal
    Eminem - Kings Never Die (Lyric Video) ft. Gwen Stefani
    Eminem - Kings Never Die (Audio) ft. Gwen Stefani
    Eminem - Phenomenal (Lyric Video)
    Detroit Vs. Everybody (Behind The Scenes)
    Detroit Vs. Everybody (Lyric Video)
    Eminem, Royce da 5&#39;9&quot;, Big Sean, Danny Brown, Dej Loaf, Trick Trick - Detroit Vs. Everybody
    Eminem - Eminem Interview (CD:UK)
    Eminem, Slaughterhouse, Yelawolf - CXVPHER (Behind The Scenes)
    Eminem - Guts Over Fear ft. Sia (Official Video)
    Vevo Presents: Shady CXVPHER (Official Video)
    Detroit Vs. Everybody
    Vevo Presents: SHADY CXVPHER
    Eminem - Guts Over Fear ft. Sia (Lyric Video)
    Eminem - Guts Over Fear (Audio) ft. Sia
    Eminem - Headlights ft. Nate Ruess (Official Music Video)
    Eminem - The Monster Explained (Behind The Scenes) ft. Rihanna
    Eminem - The Monster (Edited) ft. Rihanna
    Eminem ft. Rihanna - The Monster (Explicit) [Official Video]
    Eminem - The Monster (Teaser) ft. Rihanna
    Eminem - Rap God (Explicit)
    Eminem - Survival (Live on SNL)
    Eminem - Berzerk (Live on SNL)
    Eminem - The Monster ft. Rihanna (Audio)
    Eminem - Survival (Explicit)
    ```

<h5 class="step">Step 2</h5>
I am aware Eminem might not to be everyone's taste so in this second example we will list ippsec videos.  Here we have the channel ID hardcoded.

   <figure class="figure text-center col-xs-12 col-sm-12 col-lg-12"><img src="/static/aa981265-ebdc-42a2-8748-264062c92d55.png" class="figure-img img-fluid border border-1 border-dark" alt=" "><figcaption class="figure-caption text-center fw-normal text-dark">  </figcaption>Using PowerShell ISE to Query The IppSec YouTube Channel</figure> 

     
The example code


```bash
# Get the API Key
$GoogleApiKey = 'AIzaSyCAhq9IRVmDccixZlOvQ9FFYAWx7Hx3yvY'

$channelId = 'UCa6eh7gCkpPo5XXUDfygQQA'
$maxResults = 100

# Now we can use the search method to search for all the eminem videos in this channel
$ippsec = Invoke-RestMethod -uri "https://www.googleapis.com/youtube/v3/search?channelId=$channelId&order=date&part=snippet&type=video&maxResults=$maxResults&key=$GoogleApiKey" | Select-Object -expand items

# List all eminem videos by title
$ippsec | ForEach-Object { $_.snippet.title }
```

This gives us the following output.

```bash
HackTheBox - Proper
HackTheBox - Crossfit2
HackTheBox - Love
HackTheBox - TheNotebook
HackTheBox - Armageddon
HackTheBox - Breadcrumbs
HackTheBox - Atom
HackTheBox - Ophiuchi
HackTheBox - Spectra
HackTheBox - Tentacle
HackTheBox - Tenet
HackTheBox - ScriptKiddie
HackTheBox - Cereal
HackTheBox - Delivery
HackTheBox - Ready
HackTheBox - Attended
HackTheBox - Sharp
HackTheBox - Bucket
HackTheBox - Laboratory
HackTheBox - APT
HackTheBox - Time
HackTheBox - Luanne
HackTheBox - Crossfit
HackTheBox - Reel2
HackTheBox - Passage
HackTheBox - Academy
HackTheBox - Feline
HackTheBox - Jewel
HackTheBox - Doctor
HackTheBox - Worker
HackTheBox - Compromised
HackTheBox - Rope2
HackTheBox - Omni
HackTheBox - Laser
HackTheBox - OpenKeyS
HackTheBox - Unbalanced
HackTheBox - SneakyMailer
HackTheBox - Buff
HackTheBox - Intense
HackTheBox - Academy Intro
HackTheBox - Tabby
HackTheBox - Fuse
HackTheBox - Dyplesher
HackTheBox - Blunder
HackTheBox - Cache
HackTheBox - Blackfield
HackTheBox - Admirer
HackTheBox - Multimaster
HackTheBox - Travel
HackTheBox - Remote
```