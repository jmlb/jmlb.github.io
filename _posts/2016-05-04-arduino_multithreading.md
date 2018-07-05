---
layout: post
title: Arduino Multithreading
category: "robotics"
subcategory: "hardware"
tags: "raspberry_pi, python"
img_post: 20160504_arduino_multithreading.jpg
summary: 
github-link: "na"
---

For most **Arduino**, hardware multi-threading is not supported:

  * Threads
  * Delays
  * Hardware Interrupts


## Hardware Interrupt

Run a short piece of code everytime a pin state changes. It breaks from the `loop()` and goes back where it left off once the ISR has been ran.


2 pins : [D2] and [D3] pins (Digital)
```attachInterrupt(0, IRS, ...)``` for [D2]
```attachInterrupt(1, IRS, ...)``` for [D3]


{% highlight ruby linenos%}
void setup() //Initialize
{
pinMode(sensorPin, Input)

}

void loop()
{
  int counter = 0;
  attachinterrupt(0, notify, RISING) ; //or FALLING
}

void notify()
{
digitalWrite(notifyPin, HIGH);
}
{% endhighlight %}


Alternative code :
{% highlight ruby linenos%}
void loop()
{
  noInterrupts();
  //critical, time sensitive code here
  Interrupts();
  //other code here
}
{% endhighlight %}
