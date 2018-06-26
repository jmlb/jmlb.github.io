---
layout: post
title: Save and Load Model with Tensorflow
comments: true 
tags: GPU
category: ml
img_post: install TF_NVIdia.jpg
summary: This is a short post that shows a starting code to save and reload models created with Tensorflow. The model includes the weights and biases.
github-link: na
---


# Save a model

{% highlight python %}
# The file path to save the data
save_file = './model.ckpt'

# Two Tensor Variables: weights and bias
weights = tf.Variable(tf.truncated_normal([2, 3]))
bias = tf.Variable(tf.truncated_normal([3]))

# Class used to save and/or restore Tensor Variables
saver = tf.train.Saver()

with tf.Session() as sess:
    # Initialize all the Variables
    sess.run(tf.global_variables_initializer())

    # Show the values of weights and bias
    print('Weights:')
    print(sess.run(weights))
    print('Bias:')
    print(sess.run(bias))

    # Save the model
    saver.save(sess, save_file)
{% endhighlight %}


# Restore the model

The `tf.train.Saver.restore()` function loads the saved data into weights and bias. Since `tf.train.Saver.restore()` sets all the TensorFlow Variables, we don't need to call `tf.global_variables_initializer()`.

{% highlight python %}
# Remove the previous weights and bias
tf.reset_default_graph()

# Two Variables: weights and bias
weights = tf.Variable(tf.truncated_normal([2, 3]))
bias = tf.Variable(tf.truncated_normal([3]))

# Class used to save and/or restore Tensor Variables
saver = tf.train.Saver()

with tf.Session() as sess:
    # Load the weights and bias
    saver.restore(sess, save_file)

    # Show the values of weights and bias
    print('Weight:')
    print(sess.run(weights))
    print('Bias:')
    print(sess.run(bias))
{% endhighlight %}