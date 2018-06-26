---
layout: post
title: How to create a tensor mask in Tensorflow?
comments: true
tags: python
category: ml
author: "Jean-Marc Beaujour"
summary: "A tensor mask can be very useful when developing a Deep Learning Model. It can be used to turn on/off activations deterministically or randomly."
img_post: argparse_20171124.jpg
github-link: na
---
A tensor mask can be very useful when developing a Deep Learning Model. It can be used to turn on/off activations deterministically or randomly.

We will make a mask of `0` s and `1`s:
{% highlight python linenos%}
[[1,1,1,1,0,0,0,0],
[1,1,1,0,0,0,0,0],
[1,1,0,0,0,0,0,0],
[1,0,0,0,0,0,0,0],
]
{% endhighlight %}

Make a 4x8 matrix where each row contains the length repeated 8 times: `lengths=[4,3,5,2]`:

{% highlight python linenos%}
length_transposed = tf.expand_dims(lengths, 1)
{% endhighlight %}

Make a 4x8 matrix where each row contains [0, 1, ...7]


{% highlight python linenos%}
range = tf.range(0, 8 , 1)
range_row = tf.expand_dims(range, 0)
{% endhighlight %} 

Use the logical operations to create a mask:


{% highlight python linenos%}
mask = tf.less(range_row, lengths_transposed)
{% endhighlight %}

#use the select operation to select between  1 or 0 for each value:


{% highlight python linenos%}
result = tf.select(mask, tf.ones([4,8]), tf.zeros(4,8]))
{% endhighlight %} 

Check 

{% highlight python linenos%}
tf.sequence(lengths, max(en=None, dtype=tf.bool, name=None))
{% endhighlight %} 