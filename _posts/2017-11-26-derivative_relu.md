---
layout: post
title: Derivative of Relu
meta: "deep learning, maths"
category: flashcards
subcategory: deeplearning
author: "Jean-Marc Beaujour"
img_post: 20171126-gradient_softmax.png
summary: Derivative of Relu
github-link: na
---

<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_CHTML'></script>


The Relu (Rectified Linear Unit) activation function is:

$$
f(z) = max(0, z)
$$

The derivative is as follows:

$$
f'(z) = = \left\{
                \begin{array}{ll}
                  1 \text{\ \ if \ } x>0}\\
                  0 \text{\ \ otherwise}\\
                \end{array}
              \right.
$$

Technically, $f'(x=0)$ is not defined. The subgradient tells us that:

$$
[ f'(x < 0)=0 ] \leq f'(x=0) \leq  [ f'(x > 0)=1 ]
$$

Typically, we choose $f'(x=0) = 0$.

This has the nice property of favouring sparsity in the feature map.

Additoionally, we can also choose $f'(x=0) = 0.5, 1$