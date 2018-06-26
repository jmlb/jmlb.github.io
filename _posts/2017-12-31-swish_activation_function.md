---
layout: post
title: Swish, a new activation function for Neural Network
meta: "maths, cross-entropy, classifier, softmax, gradient"
category: ml
author: "Jean-Marc Beaujour"
img_post: 20171231_swish_activation_function.png
summary: 
github-link: na
---

<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_CHTML'></script>

With **ReLU**, half of the values of the input $$x$$ are set to 0 after applying the activation function. With a derivative = 0, the weight is not updated, and if the weight is already 0 that results in Dead neurons.

Leaky Relu and Selu (self-normalizing Neural Networks) are potential alternatives but they do not perform very well.

Swish out-performs Relu for deep NN (more than 40 layers). Although, the performance or relu and swish model degrades with increasing batch size, swish performs better than relu.
 

### 1. *SWISH* activation function

$$
\begin{align}
f(x) = x \ \sigma(x) = \frac{x}{1 - e^{-x}}
\end{align}
$$


* Defining the *swish* activation function in Tensorflow:

{% highlight python linenos%}
def swish(x):
  return x * tf.nn.sigmoid(x)
{% endhighlight %}


### 2. *b*-*SWISH* activation function

$$
\begin{align}
f(x) = x \ \sigma(bx) = \frac{x}{1 - e^{-bx}}
\end{align}
$$

where $$b$$ is a **learnable parameter**.

* Defining the *b-swish* activation function in Tensorflow:

{% highlight python linenos%}
def alt_swish(x):
  beta = tf.Variable(initial_value=1.0, trainable=True, name='swish-beta')
  return x * tf.nn.sigmoid(beta*x)
{% endhighlight %}