---
layout: post
title: "Getting Started With i3wm"
date: "2021-06-08"
description: ""
coverimage: Getting_Started_With_i3wm.jpg
tags: 
published: true
posttype: article
categories: tutorial
---
# Overview

Today we will be looking at i3. A productivity tool.

<h5 class="step">Step 1: Install the software</h5>

Install the following packages. Note, adding `-y` will automatically confirm instal and you will not be prompted. 
```
sudo apt install -y i3 i3-wm i3status rofi polybar 
```

<h5 class="step">Step 2: Sign-out and sign-in selecting the i3 Xsession.</h5>

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
    <img src="/static/7960e59e-6713-4b03-835a-0b9a261cfc1d.png" class="figure-img img-fluid border border-1 border-dark" alt="Sign-out and Log back in with i3."> 
    <figcaption class="figure-caption text-center fw-normal text-dark">Sign-out and Log back in with i3.</figcaption>
</figure>

We are presented with a dialogue asking us if we would like a config generated. Press `Enter`. 

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
    <img src="/static/c713f525-d2c4-4593-9191-e783e0dd6d19.png" class="figure-img img-fluid border border-1 border-dark" alt="First Configuration Prompt.">
    <figcaption class="figure-caption text-center fw-normal text-dark">First Configuration Prompt.</figcaption>
</figure>

Next, we can confirm which we will use as the default modifier key. Press `Enter` to accept the `Win` key

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
    <img src="/static/403fb618-ebcf-4f1d-abd2-54f93c54cb54.png" class="figure-img img-fluid border border-1 border-dark" alt="Choose Default Modifier and Write To File">
    <figcaption class="figure-caption text-center fw-normal text-dark">Choose Default Modifier and Write To File</figcaption>
</figure>

Hit `Win` + `Enter` to open up a terminal.

<h5 class="step">Step 3: Configure Polybar.</h5>

Make a folder for the configuration file and copy the sample provided. 
```
mkdir -p ~/.config/polybar/
cp /usr/share/doc/polybar/config ~/config/polybar/
```

Create a Polybar launcher script
```
nano ~/config/polybar/launcher.sh
```

Content of the launcher field should look similar to this 
```
#!/usr/bin/env bash

# Terminate already running bar instances
killall -q polybar
# If all your bars have ipc enabled, you can also use 
# polybar-msg cmd quit

# Launch saint custom toolbar
echo "---" | tee -a /tmp/saintbar.log
polybar saint 2>&1 | tee -a /tmp/saintbar.log & disown

echo "Bar launched..."
```

Make it executable:
```
chmod +x $HOME/.config/polybar/launch.sh
```

Edit the i3 config file. Add the `launch.sh` script and comment out the default i3bar.
```
nano ~/.config/i3/config
```

<figure class="figure text-center col-xs-12 col-sm-12 col-lg-12">
    <img src="/static/619b7283-5ca4-41e2-a5be-a8e4e541ede0.png" class="figure-img img-fluid border border-1 border-dark" alt="Launcher Added and Default Panel Commented Out">
    <figcaption class="figure-caption text-center fw-normal text-dark">Launcher Added and Default Panel Commented Out</figcaption>
</figure>

<h5 class="step">Step 4: Install Rofi Themes.</h5>

```
git clone --depth=1 https://github.com/adi1090x/rofi.git ~/.config/rofi
cd rofi
chmod +x setup.sh
./setup.sh
```