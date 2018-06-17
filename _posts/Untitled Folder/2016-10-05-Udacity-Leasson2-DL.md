 ---
layout: post
title: Artificial Neural Network - Deep Learning (@ Udacity)
comments: true
date: 2016-10-05
tags: Neural-Network UDacity Deep-Learning
categories: MachineDeepLearning
---

## How many parameters:

Dimension of input : N

K outputs

Nbr of parameters: (N+1)*K


## RELU: Non linear function

![](/pics/posts_pics/udacity-DL/reLu_function.png)

The ReLu has a very simple derivative
![](/pics/posts_pics/udacity-DL/reLu_function_derivative.png)

## Artificial Neural Network with 2 layers:
![](/pics/posts_pics/udacity-DL/ANN.png)

### chain rule

$$g(f'(x)) = g'(f(x)) \times f'(x)$$

Forward propagation (from input to output)

Backpropagation from output to input

Deep Network: Deeper (mor layers) is very often better than wider (more nodes).


## Techniques to Prevent overfitting

### Early Termination


![](/pics/posts_pics/udacity-DL/udacity-DL-earlytermination.png)


### Regularization
L2 regularization

$$\mathcal{L}' = \mathca{L} + \beta \frac{1}{2} || W ||^2 $$ with $$M$$.


The L2-regularization penalizes large weights

$$||W||^2$$ has a simple derivative: $$||W||$$

### drop out
Take the activation

Randomly set 1/2 of the activation to zero

The idea is to prevent the network to rely on one weight or a set of weight in particular

IF drop out, you should use a bigger network

During Training use, 0 activations that are drop out but also scale the remaining activation by $$x2$$s
