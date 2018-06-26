---
layout: post
title: Tensorflow Intro
meta: "GPU"
category: ml
author: "Jean-Marc Beaujour"
img_post: tensorflow_logo.jpg
summary: Some attributes to 
github-link: na
---

## TensorBoard

Tensorboard can be used for Model Visualization and Logging, and is open with Terminal command:

{% highlight python %}
tensorboard --logdir=path/to/log_dir
{% endhighlight %}


####   Create Summary Log

1. `Summary` are operations:

{% highlight python %}
loss_summary = tf.summary.scalar('loss', loss)
{% endhighlight %}


2. `Summary` writes some summary to a log file

{% highlight python %}
summary_writer = tf.summary.FileWriter('logs/', session.graph)
{% endhighlight %}


3. Example

{% highlight python %}
pred, summary = sess.run([out, loss_summary], feed_dict={x: x_, labels: y_})
summary_writer.add_summary(summary, global_step)
{% endhighlight %}


#### Name Scoping

{% highlight python %}
with tf.variable_scope("foo"):
    with tf.variable_scope("bar"):
        v = tf.Variable("v", [1])

v.name
>>> "foo/bar/v:0"
{% endhighlight %}


**1. Name scoping** is useful for sharing weights

{% highlight python %}
with tf.variable_scope("foo"):
    with tf.variable_scope("bar"):
        v = tf.get_variable("v", [1])

{% endhighlight %}


**2. Name scoping** is useful for cleaner code when similar structures can be re-used

{% highlight python %}
def layer(input, input_size, output_size, scope_name):
    tf.variable_scope(scope_name):
        W = tf.Variable("W", tf.random_normal(input_size, output_size)))
        b = tf.Variable("b", tf.zeros(output_size))
        z = tf.matmul(input, W) + b
        return z


input = .......
h0 = layer(input, 10, 20, "h0")
h1 = layer(h0, 20, 20, "h1")
tf.get_variable("h0/W")
tf.get_variable("h1/b")
{% endhighlight %}


## Build a basic Neural Network Model

{% highlight python %}
graph = tf.Graph #create an instance graph

with graph.as_default():
    w=tf.variable() #initialization to truncated normal and bias set to 0
    logits = WX + b
    #loss take mean over all training examples
    loss = softmax_cross_entropy_with_logits(logits, train)
    #gradient + optimizatiom
    optimizer = GradientDescentOptimizer(learning_rate).minimize(loss)
{% endhighlight %}