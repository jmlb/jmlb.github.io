---
layout: post
title: Bayesian Optimization
tags: DeepLearning convnet keras
category: flashcards
subcategory: dl
summary: ""
img_post: 20180325-bayesian_optimization_nn.png
github-link: na
---

Bayesian Optimization assume the equation:

$$
\begin{align}
x \rightarrow y=f(x) \\
input \ \ \   f
\end{align}
$$

$f$ is a black box and tries to acquire distribution of the output by exploring and observing various inputs-outputs.

Library = gpyopt


BO is a sequential strategy for global optimizations.
