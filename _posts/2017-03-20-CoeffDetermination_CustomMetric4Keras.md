---
layout: post
title: Implement a Custom Metric Function in Keras
comments: true
date: 2017-03-20
tags: Keras
author: "Jean-Marc Beaujour"
meta: "DeepLearning"
category: ml
summary: "I show how to implement a Custom Metric fnction in Keras, in particular the coefficient of determination. The coefficient of determination is particularly useful if we want to compare a model to a basic classification model based on the frequency of occurence of the classes in the training set."
img_post: "keras_custom_metric.jpg"
github-link: na
---


When developing a model, it is useful to quantify its performance with a pre-defined metric. For classification problem for example, the use of [log loss](http://scikit-learn.org/stable/modules/model_evaluation.html#log-loss) is common, and for regression problem the [mean squared error](http://scikit-learn.org/stable/modules/model_evaluation.html#mean-squared-error) is typically used. Keras integrates those 2 metrics per default. 

However, we might come across problems where we might need to come up with our own metrics. In **Keras**, it is possible to define [custom metrics](https://keras.io/metrics/), as well as custom loss functions.

In this post, I will show you: *how to create a function that calculates the coefficient of determination R2, and how to call the function when compiling the model in Keras*.


# **Definition of the Coefficient of Determination R2**
 
The coefficient of determination **R2** can describe how "good" a model is at making predictions: it represents the proportion of the variance in the dependent variable that is predictable from the independent variable:

$$ R^2 = 1 - \frac{SS_{res}}{SS_{tot}} $$

where:

1. $$SS_{tot} = \sum(y_i - \bar{y})^2$$ is the total sum of squares, with $$\bar{y} = 1/n \sum y_i $$,

2. $$SS_{res} = \sum(y_i - \hat{y}_i)^2$$ is the residual sum of squares, and $$\hat{y}_i$$ is the predicted value.


**R2** ranges from 0 to 1:
1.  if **R2**=0 : the model always fails to predict the target variable,
2.  if **R2**=1 : the model perfectly predicts the target variable. 

Any value between 0 and 1 indicates what percentage of the target variable, using the model, can be explained by the features.
If **R2**< 0, it indicates that the model is no better than one that constantly predicts the mean of the target variable.

# **Implementation**

OK, now that we've got that out of the way, let's write the code!

<script src="https://gist.github.com/jmlb/3ab6aa96e96e5ae75953a8756fdcc0d0.js"></script>


To avoid division by zero, I use `K.epsilon()` which has the value 1E-8.

Pretty simple, right?
Hope you found this post useful!!!
