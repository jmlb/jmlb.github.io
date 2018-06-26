---
layout: post
title: Convolution explained
category: flashcards
subcategory: dl
tags: DeepLearning convnet keras
summary: ""
img_post: 20180320-genetic_algo_nn.png
github-link: na
---


<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>



## 1D convolution


   > input = [$$w$$]  , filter=[$$k$$] $$\rightarrow$$ output=[$$w$$]


For example:

 input =[1111111] and filter=[0.25,0.5,0.25], hence output=[1111111]


## 2D Convolution operation as a mtarix multiplication

   > input = [$$w, h$$]  , filter=[$$k, k$$] $$\rightarrow$$ output=[$$w, h$$]

$$
\begin{align}
(f * g)(t) = \int _0 ^t f(t - \tau) g(\tau) d \tau
\end{align}
$$

**Convolution consists in blending 2 functions together**.

The example below shows the convolution of a step function and a Gaussian function.

<img src="/images/20171012/img01.png" width="500rem">

Each point $$t$$ in a convolve signal ($$f*g$$) has signal:

$$
\begin{align}
(f*g)(t) = \int_{-\inf} ^{+\inf} f(\tau) g(t - \tau) d \tau
\end{align}
$$

It is the integral of a point wise multiplication between all elements in the signal $$f(\tau)$$ and all elements in the signal $$g(t - \tau)$$ shifted by $$t$$.

## 2D Convolution: step by step

We will convolve those 2 signals: $$x(\tau)$$ and $$h(\tau)$$.


<img src="/images/20171012/img02.png" width="500rem">

$$
\begin{align}
y(t) = \int_{-\inf} ^{+\inf} x(\tau) h(t - \tau) d \tau
\end{align}
$$

$$h(\tau)$$ is the function that we shift. Typically, we shift the shortest or simplest signal. We will maintain and shift $$h(t - \tau)$$.