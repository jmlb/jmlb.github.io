---
layout: post
title: Logistic Classifier - Deep Learning (@ Udacity)
comments: true
date: 2016-10-05
tags: logistic-classifier 
categories: MachineDeepLearning
---

\begin{align}
W \times X + b = Y = \left( \begin{smallmatrix} 0.7 \\ 0.2 \\ 0.1 \\ \end{smallmatrix}\right)
\end{align}

$$W$$ is the weight, $$X$$ is the input, $$b$$ the bias and $$Y$$ the score.

To turn the score into probability, we use softmax:

\begin{align}
S(y_i) = \frac{e^{y_i}}{\sum_i e^{y_i}}
\end{align}

## 1 Hot encoding
1-Hot encoding can cause problem if there are many many classes, therefore creating vectors with mainly 0s.

Using **embedding** can address the problem.

## Cross Entropy
To compare our softmax() output $$S = \left( \begin{smallmatrix} 0.7 \\ 0.2 \\ 0.1 \end{smallmatrix}\right) $$ output to the hard coded label $$L = \left( \begin{smallmatrix} 1 \\ 0 \\ 0 \end{smallmatrix}\right)$$, we use the Cross entropy :
\begin{align}
D(S, L) = -\sum_i L_i \text{log}(S_i)
\end{align}

Be Carefull: Cross entropy is not symmetriic : $$D(S, L) \neq D(L, S) $$. $$S_i$$ will always have non zero values so $$\text{log}(s_i)$$ is defined.

## Summary : Multinomial Logistic Classification
$$
\begin{align}
Input = \left( \begin{smallmatrix} . \\ . \\ . \\ \end{smallmatrix}\right) \rightarrow _{WX + b} score Y = \left( \begin{smallmatrix} 2 \\ 1 \\ 0.1 \\ \end{smallmatrix}\right) \rightarrow _{S(Y)} S(Y) = \left( \begin{smallmatrix} 0.7 \\ 0.2 \\ 0.1 \\ \end{smallmatrix}\right) \rightarrow_{X-entropy: compare to 1-hot label} L = \left( \begin{smallmatrix} 1 \\ 0 \\ 1 \\ \end{smallmatrix}\right)


## Objective:
* The LOss is :
$$\mathcal{L} = \frac{1}{N} \sum D(S(WX_i + b), L_i)$$

* Optimization: Gradient Descent

## Variable conditionning
A Note about Numerical Stability: 
Computing very small numbers and very large numbers can lead to wrong output
gist here

We get 0.934 instead of 1.

It is therefore preferable to involved values that are never to o big or too small in the loss calculation.

A guiding principal is that the variables must have mean X = 0 and equal variance : i.e $$\sigma(X_i) = \sigma(X_j)$$

Here are examples of badly conditionned variables and well conditionned variables
![](/pics/posts_pics/udacity-DL/udacity-DL-fig1.png)

* Image inputs:
Typically image inputs are RGB with pixel value ranging from 0 to 255. One way to conditionned the input is :

$$R_{new} = \frac{R -128}{128}$$,  $$G_{new} = \frac{G -128}{128}$$, $$B_{new} = \frac{B -128}{128}$$

* Initialization of weigts and biases:
Draw weights randomly from Gaussian distribution with mean 0 and standard deviation $$\sigma$$. Use a small $$\sigma$$ for initialization.

The optimization is:

$$w \leftarrow w - \alpha \nabla_w \mathcal{L}$$

$$b \leftarrow b - \alpha \nabla_b \mathcal{L}$$


## Training/ Validation / Testing set
> A change that affects 30 examples in the data is typically significant and can be trusted

Typically, validation set must have more than 30000 examples so accuracy to 0.1% is meaningful

If not enough data to start with, use cross validation.


## Stochastic gradient descent (SGD)
* random sample of the data
* calculate their loss
* compute derivative
* perform gradient descent
* SGD scales with data and model size 

Tricks:

### Momentum
Running average of the gradient:

$$M \leftarrow 0.9 M + \nabla \mathcal{L}$$

replace $$\alpha \nabla_{w} \mathcal{L}$$ with $$M$$.

### Learning rate decay

### SGD Hyper parameters
* initial learning rate
* learning rate decay
* momentum
* batch size
* weight initialization

### Adagrad
Modification of SGD with momentum.


Links to Assignments: