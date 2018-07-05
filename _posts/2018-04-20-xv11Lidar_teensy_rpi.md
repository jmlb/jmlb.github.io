---
layout: post
title: 2D Lidar Cloud Points from XV11 Lidar with Teensy Microcontroller and Raspberry Pi
tags: "robotics, lidar"
category: "robotics"
subcategory: "hardware"
img_post: 20180420-lidar_teensy.png
summary: 
github-link: ""
---

<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


In this post, I will show how to pull data from a 2D Lidar (Piccolo XV-11) using a Teensy 3.2, and a Raspberry Pi.

There are quite a few examples well documented showing hwo to connect the Lidar to a Raspberry Pi (Thobias), or to a Arduino.
There are code examples out there.

To interface the Lidar, we need:

* DC motor: 
Power : ~3V. (Red/Black)
According to the specs, the motor must spin at 300 RPM

* the electronic module
Powered at +5V (Red/Black). To confirm the number, remove the cap of the unit and check the board where the red wire is soldered.
Data line: serial TX (Orange)

<img src="images/20180420/circuit.png">


* Typical data output 
```
FA
C3
E3
48
34
3
98
0
1E
3
CB
0
12
....
```


```
FA:BE:19:49:FF: 4:A :0 :35:80:0 :0 :35:80:0 :0 :25:80:0 :0 :4F:1B
FA:BF:E3:48:25:80:0 :0 :35:80:0 :0 :CB:9 :8 :0 :25:80:0 :0 :16:43
FA:C0:E3:48:35:80:0 :0 :35:80:0 :0 :35:80:0 :0 :35:80:0 :0 :71:7E
FA:C1:E3:48:35:80:0 :0 :35:80:0 :0 :B4:3 :1C:0 :95:3 :8D:0 :21:22
FA:C2:E3:48:90: 3:99:0 :92: 3:BA:0 :5F:3 :63:0 :4B:3 :AB:0 :87:6A
      [spe] [Data    1 ][Data    2 ][data    3 ][data    4 ]
```


Credits: http://www.tobias-weis.de/neato-xv-laser-scanner-lidar/

https://www.getsurreal.com/product/xv-lidar-controller-v1-2/
https://github.com/getSurreal/XV_Lidar_Controller/blob/master/XV_Lidar_Controller.ino

https://github.com/bombilee/NXV11/blob/master/XV-11_test_python/XV-11_test.py


