---
layout: post
title: Ros Publish Subscibe - make a topic
meta: "ros topic subscribe publish"
category: "robotics"
subcategory: "ros"
img_post: 20180704-publisher-subscriber.jpg
summary: 
github-link: "na"
---


<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

Any node can **publish** (send) a message to any topic. And similarly, any node can **subscribe** (read) a message from any topic.

<div style="width: 100%; text-align: center;">
	<img src="/images/20180704/img_01.png" width="100rem" style="margin-right: 2rem; margin-left:0rem; margin-top: 0rem; margin-bottom: 0rem">
</div>


## A Message

The message is defined in the file with extension `.msg`. The messages will be compiled into C++/Python class after running `catkin build`.

{% highlight ruby %}
Header header

{% endhighlight %}

The `Header` is a bult-in ROS class for messages that comes with some handy methods that are automatically filled by ROS such as:

* `seq`: it's a counter 

The only exception is for the time which must be hard coded in the callback