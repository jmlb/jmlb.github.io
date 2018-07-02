---
layout: post
title: Root Mean Squared Error Versus Mean Absolute Error 
category: flashcards
subcategory: ml
tags: Loss_function
summary: "Comparison of the RMSE and MAE. Those 2 error terms, although they compute the average error, are more or less sensitive to the examples error. In this post, I review the definition of those 2 losses term and how they are affected by outliers or examples with large errors."
img_post: 20180701-mse_vs_mae.png
github-link: na
---


<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


## MAE: Mean Average Error

MAE measures the average of the absolute length between **the predicted value** $$\hat{y}_i$$ and the **ground truth** $$y_i$$: 

<div style="width: 100%; text-align: center;">
<img src="/images/20180701/img_01.png" width="500rem" style="margin-right: 2rem; margin-left:0rem; margin-top: 0rem; margin-bottom: 0rem">
</div>

$$
MAE = \frac{1}{n} \sum_{j=1} ^{n}|y_i - \hat{y}_{i}|

$$

All the errors have same weights.

## RMSE: Root Mean Squared Error

RMSE measures the average of the absolute length between the predicted value $$\hat{y}_i$$ and the ground truth $$y_i$$: 

<div style="width: 100%; text-align: center;">
<img src="/images/20180701/img_02.png" width="500rem" style="margin-right: 2rem; margin-left:0rem; margin-top: 0rem; margin-bottom: 0rem">
</div>

$$
RMSE = \sqrt{ \frac{1}{n} \sum_{j=1} ^{n} (y_i - \hat{y}_{i})^2 }

$$

The RMSE computesthe average magnetitude of the squared distance and then take the square root.


## RMSE vs. MAE

Fundamentally, the too term, **MAE** and **RMSE** measures about the same item: the average distance of the error between the ground truth and the predicted value. However:

* RMSE is more sensitive to the examples with the largest difference
This is because the error is squared before the average is reduced with the square root.

* RMSE is more sensitive to ouliers: so the example with the largest error would skew the RMSE. MAE is less sensitive to outliers.

Here is an example to illustrate out the MSE and RMSE are more sensitive to outliers. In this example, the ground truth is $$y=1$$. The error in absolute value is 

$$|y_i - \hat{y}_i| = 1$$

for most point except for the point at $$i=4$$ where 

$$|y_i - \hat{y}_i| = 4$$

The point $$i=4$$ is clearly an outlier.


<div style="width: 100%; text-align: center;">
<img src="/images/20180701/img_03.png" width="500rem" style="margin-right: 2rem; margin-left:0rem; margin-top: 0rem; margin-bottom: 0rem">
</div>

 

The MAE is: 

$$MAE = \frac{1}{8} * (1 + 1 + 1 + 4 + 1 + 1 + 1 + 1) = 1.375$$

The RMSE is:

$$RMSE = \sqrt{ \frac{1}{8} * (1^2 + 1^2 + 1^2 + 4^2 + 1^2 + 1^2 + 1^2 + 1^2) } = 1.70$$

Clearly, RMSE is more senstive to the outliers with a larger average error.
<br>
> In some cases, it might be desirabe to pay attention to the example with the largest error, in this case the RMSE is a good choice for the loss function. However, in the case we want the model to not be skewed by outliers, MAE is a better choice.
