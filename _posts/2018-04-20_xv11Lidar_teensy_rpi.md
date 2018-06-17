---
layout: post
title: Ridge and Lasso Regression
meta: "deep learning"
category: ml
author: "Jean-Marc Beaujour"
img_post: 2017-08-10-input_reconstruction.jpg
summary: 
github-link: ""
---

<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>



In this article, I will be discussing 2 regularization techniques that are commonly used to limit the amplitude of the coefficients in Regression problems: **Ridge regularization** and **Lasso regularization**. Those techniques are appropriate when the number of features is more than 2 and that some features are highy correlated. This is the case for example in [polynomial regression](https://en.m.wikipedia.org/wiki/Polynomial_regression): $\hat{y}_i = w^1 x_i + w^2 x_i ^2$ where for example $x_i ^3$ is highly correlated with $x_i$. Note that generally, regularizing the intercept is not a good idea.

## Constraint 

The goal of the regularizer is to have some constraint on the values of the coefficients/the weights.


The cost functions is defined as follow

* Residual square error:

$$
J_0 = \frac{1}{2m} \sum_i (y - \hat{y}_i)^2
$$ 

where $y$ is the ground truth and \hat{y} is the predicted output.

* Ridge regularizer (L2 regularization):

$$
J_{L2} = \frac{1}{2m} \sum_i (y - \hat{y}_i)^2 + \lambda_2 \sum_k ||w^k||^2
$$ 

* Lasso regularizer (L1 regularization):

$$
J_{L1} = \frac{1}{2m} \sum_i (y - \hat{y}_i)^2 + \lambda_1 \sum_k |w^k|
$$
where $|w^k|$ is the absolute value.                                    
* ElasticNet regularizer (combined L1 and L2 regularizer):

$$                                                      
J_{L} = \frac{1}{2m} \sum_i (y - \hat{y}_i)^2 + \lambda_2 \sum_k ||w^k||^2 + \lambda_1 \sum_k |w^k|
$$ 


## Ridge Regularization

Let's start with the following model :
$$
\hat{y_i} = \sum_k x_i ^k w^k
$$k

The cost function is :
$$
J_{L2} = \frac{1}{2m} \sum_i (y - \hat{y}_i)^2 + \lambda_2 \sum_k ||w^k||^2
$$ 

This cost function is convex and differentiable, so we can use Gradient Descent

$$
\frac{\partial J_{L2}}{\partial w^j} = -\frac{1}{m} \sum_i (y - \hat{y}_i)x_i ^j + 2 \lambda_2 ||w^j||
$$ 

The coefficient update is:

$$
\begin{align}
w^j &= w^j - \alpha \frac{\partial J}{\partial w^j} \\
&= w^j - \alpha \left( -\frac{1}{m} \sum_i (y - \hat{y}_i)x_i ^j + 2 \lambda_2 ||w^j|| \right) \\
&= w^j(1 - 2 \alpha \lambda_2) - \alpha (-\frac{1}{m} \sum_i (y - \hat{y}_i)x_i ^j)
\end{align}$$

So, Ridge is equivalent to reducing the weight.



## Lasso Regularization

Lasso Regularization favors spasity of coefficients, and therefore shut off some features. This is useful when we need to simplify a complex model with a lot of parameters, and/or we want to get an interpretable model.