---
layout: post
title: List of cost functions used in Neural Networks
category: flashcards
subcategory: dl
tags: DeepLearning convnet keras
summary: ""
img_post:
github-link: na
---


<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

A cost function is a quantitatif measure of the quality of a fit: how good the model is at reproducing the data. A cost function is a single value which is the the sum of the deviation of the model from the real value for all points in the dataset.

### 1. Quadratic Cost function: regression

$$
\begin{align}
C = \frac{1}{2} \sum_j (\hat{y}_j - y^{pred} _j)^2
\end{align}
$$

where $$\hat{y}_j$$ and $$y^{pred} _j$$ are the true target value of point $$j$$, and the predicted target value respectively.

<hr>

### 2. Cross Entropy Cost: Classification

$$
\begin{align}
C = - \sum_j \left( \hat{y}_j \text{log}(y_j) + (1-\hat{y}_j) \text{log}(1-y_j) \right)
\end{align}
$$

<hr>
### 3. Exponential Cost

$$
\begin{align}
C = \tau \ \text{exp} \left[ \frac{1}{\tau} \sum_j \left( \hat{y}_j -y^{pred} _j\right)^2 \right]
\end{align}
$$

where $$\tau$$ is a hyper-parameter.


<hr>
### 4. Hellinger Distance

$$
\begin{align}
C = \frac{1}{\sqrt{2}} \sum_j \left( \sqrt{\hat{y}_j} - \sqrt{y^{pred} _j}\right)^2
\end{align}
$$

it needs to have positive values in [0, 1].

<hr>
### 5. Kullback-Leibler Divergence

Kullback-Leibler Divergence is also known as : *Information Divergence, Information Gain, Relative entropy, KLIC divergence* or *KL Divergence*, and is defined as:

$$
\begin{align}
D_{KL}(P || Q) = \sum_i P(i) \text{ln} \frac{P(i)}{Q(i)}
\end{align}
$$

where $$D_{KL}(P \|Q)$$ is a measure of the information lost when $$Q$$ is used to approximate $$P$$.

The **cost function** using *KL Divergence* is:

$$
\begin{align}
C = \sum_j \hat{y}_j \text{log}\frac{\hat{y}_j}{y ^{pred} _j}
\end{align}
$$


<hr>
### 6. Generalized Kullback-Leibler Divergence


$$
\begin{align}
C = \sum_j \hat{y}_j \text{log}\frac{\hat{y}_j}{y ^{pred} _j} - \sum_j \hat{y}_j - \sum_j y^{pred} _j
\end{align}
$$

<hr>
### 6. Itakura-Saito Distance


$$
\begin{align}
C = \sum_j \left(\frac{\hat{y}_j}{y ^{pred} _j} - \text{log}\frac{\hat{y}_j}{y^{pred} _j}  -1 \right)
\end{align}
$$
