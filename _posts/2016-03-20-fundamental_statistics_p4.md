---
layout: post
title: Statistics Fundamental - Part 4
category: flashcards
subcategory: stats
tags: math statistics
summary: ""
img_post:
github-link: "na"
---

<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


# 1. Definitions

### 1.1 Univriate Gaussian Distribution

The probability density function (*pdf*) of a gaussian distribution is:

$$
\begin{align}
p(x) = \frac{1}{\sqrt{2 \pi \sigma}} e^{-\frac{1}{2 \sigma^2}(x -\mu)^2}
\end{align}
$$

where $$\sigma$$ is the standard deviation, $$\mu$$ the mean and $$\int_{-\infty} ^{+\infty} p(x) dx =1$$

<img src="/images/20160320/gaussian.png" width="400px">


The expectation is:

$$
\mathbb{E}(x) = \int x p(x) dx = \frac{1}{N} \sum_{i=1} ^N x^{(i)}
$$

The Covariance:

$$
cov[X, Y] = \mathbb{E}\left( (x - \mathbb{E}(x)) (y - \mathbb{E}(y)\right)
$$

where $$\mathbb{E}(x) = \mu_x$$ and $$\mathbb{E}(y) = \mu_y$$.

$$Cov(x, x) = Var(x)$$

If $$x$$ is a *d*-dimension random vector, its covariance matrix is:

$$
\begin{align}
cov[x] = \left[ \begin{matrix} var(x_1) & covar(x_1, x_2) & ... & ... & ... & ... \\
covar(x_2, x_1) & var(x_2) &... & ... & ... & ... \\
... & ...& ... & ... & ... & ... \\
... & ...& ... & ... & ... & ... \\
... & ...& ... & ... & ... & ... \\
... & ...& ... & ... & ... & var(x_i) \\
\end{matrix} \right]
\end{align}
$$

$$
\begin{align}
\mathcal{N}(x | \mu, \Sigma) = \frac{1}{(2 \pi)^{d/2} | \Sigma|^{1/2}} \text{exp} \left[ -\frac{1}{2} (x - \mu)^{\intercal} \Sigma^{-1} (x - \mu)\right]
\end{align}
$$

### Bi-variate Gaussian Distribution

Assume we have 2 independent univariate Gaussian Variables: $$x_1 \sim \mathcal{N}(\mu_1, \sigma^2)$$ and $$x_2 \sim \mathcal{N}(\mu_2, \sigma^2)$$. Their joint distribution $$p(x_1, x_2)$$ is:

$$
\begin{align}
p(x_1, x_2) &= p(x_2|x_1) p(x_1) \\
&=p(x_2)p(x_1)
\end{align}
$$