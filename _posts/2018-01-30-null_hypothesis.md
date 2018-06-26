---
layout: post
title: Null hypothesis
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

**Null Hypothesis** states that the value of a population parameter ($\mu, \sigma$) is equal to claimed value.
Test Null Hypothesis** assumes it is true and test to reject or fail to reject $H_0$.

**Alternative hypothesis**: $H_1$ or $H_A$ is the statement that the parameter has a value that differs 
from that of the null  hypothesis.

If we conduct a study and want to use a hypothesis test to support our claim, the claim must 
be worded so that it because the alternative hypothesis.


Select the significance level $\alpha$ based on the seriousness of a type I error. 
Make $\alpha$ small is the consequneces of rejecting a true $H_0$ are severe. $\alpha=0.05$ and $\alpha=0.01$
are very common.

$\alpha$ is the probability that the test statistical will fall in the critical region whne the null $H_0$ is true.

The test statistic is a value used in making a decision about the null hypothesis, and is found by 
converting the sample statistic to score with the assumption that the null hypothesis is true.