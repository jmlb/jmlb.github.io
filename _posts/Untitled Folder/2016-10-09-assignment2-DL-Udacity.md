---
layout: post
title: Assignment 2 (Neural Net) - Deep Learning (@ Udacity)
comments: true
date: 2016-10-09
tags: logistic-classifier notMNIST ReLu 
categories: MachineDeepLearning
---



**OBJECTIVE: Train deeper and more accurate models to classify digits.**

The dataset includes letters A to Z made with a variety of fonts (notMNIST dataset). 
We use **TensorFlow** framework and compute the accuracy on a smaller dataset for letters **A** through **J**.
[Notebook](https://github.com/jmlb/Udacity_DeepLearning/blob/master/2_fullyconnected.ipynb)

![png](/pics/posts_pics/udacity-DL/nmn.png)


Previously in `1_notmnist.ipynb`, we created a pickle with formatted datasets for training, development and testing on the [notMNIST dataset](http://yaroslavvb.blogspot.com/2011/09/notmnist-dataset.html).


# Import required modules

<script src="https://gist.github.com/jmlb/f25a90d5c3b4f9794a21fa0d52981e25.js"></script>

First reload the data we generated in `1_notmnist.ipynb`.

<script src="https://gist.github.com/jmlb/8e846945126fd231e319ea9e99340e0c.js"></script>

    Training set (200000, 28, 28) (200000,)
    Validation set (10000, 28, 28) (10000,)
    Test set (10000, 28, 28) (10000,)


# Pre-process the data
** Reformat into a shape that's more adapted to the models we're going to train: **
- data as a flat matrix,
- labels as float 1-hot encodings.


<script src="https://gist.github.com/jmlb/2c5ee2a3dc5dab00be58f3f9d4228d04.js"></script>

    Training set (200000, 784) (200000, 10)
    Validation set (10000, 784) (10000, 10)
    Test set (10000, 784) (10000, 10)


# Multinomial Logistic Regression with Gradient Descent
We're first going to train a multinomial logistic regression using simple gradient descent.

TensorFlow works like this:
+ First you describe the computation that you want to see performed: what the inputs, the variables, and the operations look like. These get created as nodes over a computation graph. This description is all contained within the block below:

      ```python
            with graph.as_default():
          ...
      ```

+ Then you can run the operations on this graph as many times as you want by calling `session.run()`, providing it outputs to fetch from the graph that get returned. This runtime operation is all contained in the block below:
      ```python
      with tf.Session(graph=graph) as session:
          ...
      ```
Let's load all the data into TensorFlow and build the computation graph corresponding to our training:

<script src="https://gist.github.com/jmlb/274bd8b6d1950cc4e2499b266ca69fab.js"></script>

    (10000, 784)


Let's run this computation and iterate:


<script src="https://gist.github.com/jmlb/51ef8da80c64c315e197ab3d66648a18.js"></script>

    Initialized
    Loss at step 0: 19.750315
    Training accuracy: 10.1%
    Validation accuracy: 11.3%
    Loss at step 100: 2.318357
    Training accuracy: 71.8%
    Validation accuracy: 70.8%
    Loss at step 200: 1.852480
    Training accuracy: 74.6%
    Validation accuracy: 73.2%
    Loss at step 300: 1.606010
    Training accuracy: 75.8%
    Validation accuracy: 74.1%
    Loss at step 400: 1.439607
    Training accuracy: 76.7%
    Validation accuracy: 74.4%
    Loss at step 500: 1.315972
    Training accuracy: 77.6%
    Validation accuracy: 74.9%
    Loss at step 600: 1.219987
    Training accuracy: 78.3%
    Validation accuracy: 75.1%
    Loss at step 700: 1.142962
    Training accuracy: 78.9%
    Validation accuracy: 75.1%
    Loss at step 800: 1.079285
    Training accuracy: 79.6%
    Validation accuracy: 75.1%
    Test accuracy: 82.4%


![png](/pics/posts_pics/udacity-DL/output_9_2.png)


# Multinomial Logistic Regression with Stochastic Gradient Descent

**Let's now switch to stochastic gradient descent training instead, which is much faster.**

The graph will be similar, except that instead of holding all the training data into a constant node, we create a `Placeholder` node which will be fed actual data at every call of `session.run()`.

<script src="https://gist.github.com/jmlb/7abf5356fe3020e77dbadef49645b732.js"></script>

Let's run it:


<script src="https://gist.github.com/jmlb/eb706bb4ade772eb9aa709ad9c671420.js"></script>

    Initialized
    Minibatch loss at step 0: 15.359711
    Minibatch accuracy: 15.6%
    Validation accuracy: 15.9%
    Minibatch loss at step 500: 1.367732
    Minibatch accuracy: 78.9%
    Validation accuracy: 74.8%
    Minibatch loss at step 1000: 1.218930
    Minibatch accuracy: 78.1%
    Validation accuracy: 76.4%
    Minibatch loss at step 1500: 0.736552
    Minibatch accuracy: 81.2%
    Validation accuracy: 77.0%
    Minibatch loss at step 2000: 0.791639
    Minibatch accuracy: 84.4%
    Validation accuracy: 77.5%
    Minibatch loss at step 2500: 1.034929
    Minibatch accuracy: 78.1%
    Validation accuracy: 78.0%
    Minibatch loss at step 3000: 1.020504
    Minibatch accuracy: 80.5%
    Validation accuracy: 79.0%
    Test accuracy: 86.1%



![png](/pics/posts_pics/udacity-DL/output_13_1.png)


# 1 Hidden Layer Neural Net

Turn the logistic regression example with SGD into a 1-hidden layer neural network with rectified linear units [nn.relu()]() and 1024 hidden nodes. This model should improve your validation / test accuracy.


<script src="https://gist.github.com/jmlb/afa8c4d380d14d84ec2f8c35b3cecf55.js"></script>

    Initialized
    Minibatch loss at step 0: 328.333710
    Minibatch accuracy: 10.2%
    Validation accuracy: 24.1%
    Minibatch loss at step 500: 14.172101
    Minibatch accuracy: 82.8%
    Validation accuracy: 81.0%
    Minibatch loss at step 1000: 15.775556
    Minibatch accuracy: 82.0%
    Validation accuracy: 81.3%
    Minibatch loss at step 1500: 4.820332
    Minibatch accuracy: 89.8%
    Validation accuracy: 82.9%
    Minibatch loss at step 2000: 7.876987
    Minibatch accuracy: 87.5%
    Validation accuracy: 82.7%
    Minibatch loss at step 2500: 7.677795
    Minibatch accuracy: 87.5%
    Validation accuracy: 83.0%
    Minibatch loss at step 3000: 4.360731
    Minibatch accuracy: 82.8%
    Validation accuracy: 83.2%
    Test accuracy: 90.2%




# SUMMARY
    
+ **A multinomial logistic regression with simple gradient descent**: 
         test accuracy = 82.4%
    
+ **A multinomial logistic regression with simple gradient descent**:
         test accuracy = 86.1%

+ **1 hidden layer Neural Net with ReLU activation function**:
         test accuracy = 90.2%
