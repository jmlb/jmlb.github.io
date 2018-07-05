---
layout: post
title: Arduino Sketch - Queues Library
category: "robotics"
subcategory: "hardware"
tags: "raspberry_pi, python, arduino"
img_post: 20160504_arduino_queues.jpg
summary: 
github-link: "na"
---

## 1. Copy Queue List folder

{% highlight ruby linenos%}
# include <QueueArray.h>
QueueArray <int> queue_name; //data type = int
queue.push()
queue.pop()  //add item to the queue
queue.peek() //take item from the front of the queuebut do not remove it
bool isEmpty() // check if the queue is empty
int count //nbr of items in queue
{% endhighlight %}



{% highlight ruby linenos%}
if queue.count == 4{
  val_in = ....
  queue.push(val_in);
  queue.pop();
  initi = queue.peek()
  delta = val_in - init
}
elif queue.count < 4{
  queue.push(val_in)
}
else{
  //reset array queueto empty
  while queue.count != 0{
  queue.pop()
  }
{% endhighlight %}
