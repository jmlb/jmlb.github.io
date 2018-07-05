---
layout: post
title: How to mount WD Mirror Nas drive on raspberry-pi?
category: robotics
subcategory: hardware
tags: WDMirror NAS RaspberryPi
summary: "Access a WD Mirror Nas Drive from a RaspberryPi is as easy as that!"
img_post: mount_nas.jpg
github-link: na
---


## **Manual mount**
<hr>
In the raspberry-pi Terminal, use: <br>

<script src="https://gist.github.com/jmlb/9e93941ccd00fc1a92efc21257c01ea3.js"></script>

We will have to mount the Nas everytime we boot the raspberry-pi.

## **Automatic mount**
<hr>
1. Open the file /etc/fstab

2. add the following line to the bottom:

<script src="https://gist.github.com/jmlb/045d8110b19bb2d7df5d19df10b3d82f.js"></script>