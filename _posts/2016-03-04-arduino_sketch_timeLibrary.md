---
layout: post
title: Arduino Sketch - Time Library
category: "robotics"
subcategory: "hardware"
tags: "raspberry_pi, python, arduino"
img_post: 20160304_arduino_time_lib.jpg
summary: 
github-link: "na"
---


## 1. `timeLib.h`
The library install:
1. copy clone "github.com/PaulStoffregen/Time"
2. copy folder Time to libraries
This library uses only seconds resolution

{% highlight ruby linenos%}
# include <TimeLib.h>
now() //returns the current time as secs since Jan 1 1970
micros() //time in microseconds
millis() //time in milliseconds
unsigned long startTime = millis // when event 1 occurs
unsigned long elapsedTime = millis()  - startTime; //when event 2 occurs
{% endhighlight %}

If time is saved in a queue, make sure the datatype of the queue is the same type as StartTime