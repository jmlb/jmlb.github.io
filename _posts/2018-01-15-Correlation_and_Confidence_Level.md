---
layout: post
title: Correlation and Confidence Level
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
## Pearson Correlation coefficient: *r*
Pearson $$r$$ is a measure of the the strength of a linear relationship between 2 variables. 
The **Pearson Product Moment** attempts to draw a line of best fit through the data 
of 2 variables and *r* quantifies how far any data ppoints are from the line.
$$r $$ is the correlation coefficient for the sample an $$\rho$$ is the correlation coefficient for the population. 

$$r \in [-1, 1]$$
* if $$r=0$$, no relationship (not linearly correlated)
* $$r$$ => strong positive relationship
* $$r$$ < strong negative relationship

The coefficient of determination $$r^2$$ is the square of the Pearson correlation $$r$$:

$$
r = \frac{\sum_i (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_i (x_i -\bar{x})^2 \sum_j (y_j - \bar{y})^2}}
$$

Pearson correlation assumption:

* interval or ratio level
* linearly related
* bivariate normally distributed

If the data does not meet the above assumptions, then use Spearman's rank correlation.


## t-test

The t-test is used to establish if the correlation coefficient is significantly different from zero, and hence that there is evidence of a relationship between the 2 variables. 
Formula for the *t*-test for the correlation coefficient:

$$
t = r\sqrt{\frac{N-2}{1-r^2}}
$$
$$t$$ is called the t-score, with degrees of freedom $$df = (N-2)$$, i.e the number of pairs (x, y) of data minus 2.

Consult a t-table to compare the calculated *t*-value (*t*-score) with the critical t value to determine statistical significance. The probability that the correlation between $$x$$ and $$y$$ is simply due to error or chance is less than the calculated $$p$$ value.

The $$t$$ score formula is;

$$
t = \frac{\bar{x} - \mu_0}{s/\sqrt{N}}
$$

where $$\bar{x}$$ is the sample mean, $$\mu_0$$ the population mean and $$s$$ the sample standard deviation and $$N$$ the sample size.

We use the degrees of freedom along with the confidence level we are willing to accept to decide whether to support or reject the Null hypothesis.


## *p*-test
The p-test determines if the correlation between variables is significant by comparing the p-value to the significance level $$\alpha$$. Typically, $$\alpha=0.05$$: it means the risk of concluding that the correlation exists when actually there is no correlation is 5%.
The level of confidence $$C=95%, 99%$$, tells how confident we are in our decision. The level of signiicance is : $$\alpha = 1 - C$$.

p-value is used for testing statistical hypothesis: to test or reject the null hypothesis.

* $$H_0$$: The null hypothesis is the hypothesis that there is no difference or no correlation between specified populations $$x$$ and $$y$$, any observed difference being due to sampling or experimental error. 
* $$H_1$$: the alternative hypothesis means that there is a significant correlation in the population.

The null hypothesis and the alternative hypothesis are opposite. For example $$H_0: \mu=5g$$ and $$H_1: \mu \neq 5g$$.
We always assume the Null hypothesis is true, and the result of the test is:

1. reject the null hypothesis $$H_0$$
2. Fail to reject the null hypothesis

The *p*-value for Pearson correlation uses the *t*-distribution:

$$
t = \frac{r \sqrt{N-2}}{\sqrt{1 - r^2}}
$$

* $$p \leq 0.05$$ strong evidence against the null hypothesis - reject the null hypothesis
* $$p \geq 0.05$$ weak evidence against the null hypothesis

** Decision rule:**

1. if $$p \leq \alpha$$, the correlation is statistically significant

2. if $$p \geq \alpha$$, the correlation is not statistically significant

 The size of the sample population has an influence on the significance.

* a strong relationship $$r$$, may not be significant
* a weak relationship $$r$$, can be significant



## Correlation coefficients
Pearson correlations can be used when the residuals have a normal distribution. If the residual are not normal, use Spearman correlations.

APearson correlation coefficient is when the outcome variable is continuous, the independent variable is continuous and we want to see if the 2 variables have a systematic relationship. Spearman correlation is similar but it also tsts for non linear relationship.
