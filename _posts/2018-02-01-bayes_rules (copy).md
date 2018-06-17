---
layout: post
title: Naive Bayesian Algorithm
meta: statistics
category: ds
author: "Jean-Marc Beaujour"
img_post: 20180115-confidence-pearson.jpg
summary: 
github-link: "na"
---

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

**Naive Bayesian Algorithm** uses a statistical approach: the Bayesian approach is robust and 
less likely to find false patterns in noisy data (overfit the data).

Bayes theorem describes the probability of an event, based on prior knowledge of conditions that 
might be related to the event.

$$
P(A|B) = \frac{P(B|A) P(A)}{P(B)}
$$

where $P(A)$ is the probability of event A occuring, P(A|B) is the probability A conditioned on B ocuring.
$A$ is the desired outcome.

Bayesian  inference allows to make justified decisions on a granular level by modeling the variationin the observed data.

roas= revenue/cost = revenue / conversions conversions/cost= revenue/conversion 1/cpa

we can assume that the ad sets are similar to each other:

Log-normal probability density function:

$$
y= f(x|\mu, \sigma) = \frac{1}{x \sigma \sqrt{2 \pi}} exp\left( -\frac{(ln x - \mu)}{}2 \sigma^2 \right)
$$
