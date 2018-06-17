---
layout: post
title: My notes of the Udacity class - Intro to Descriptive Statistics
tags: stats/maths
category: ds
summary: ""
img_post: descriptive stats.jpg
github-link: na
---



<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


I just completed the online class of **Udacity** titled: ["Intro to Descriptive Statistics"](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=3&cad=rja&uact=8&ved=0ahUKEwjGqP3klYHQAhULi1QKHa1wBiQQFggmMAI&url=https%3A%2F%2Fwww.udacity.com%2Fcourse%2Fintro-to-descriptive-statistics--ud827&usg=AFQjCNE04jVCvSWEih1jJbq6sXNkuwmPXA&sig2=5f9VYQpt1mKFgcdyrMRaAg). Overall, the class is well polished and focuses on key concepts. IMO, it is a first step if you want to refresh up on the basics of Descriptive Statistics. There are also a lot of Quizzes to test your understanding of the concepts that are taught, and how they apply to real world problems.

Below are my notes from the class. 


### 0. Some definitions

* **Expected Value** of a random variable (aka the mean):

$$
\mu = \mathbb{E}[x] = \sum_i x_i f(x_i) / N
$$

where $$f(x_i)$$ is the probability of the variable $$x_i$$

* **Variance** is a measure of dispersion (std=$$\sigma^2$$)

$$
Var(x) = \sigma_x ^2 = \mathbb{E}[(x - \mu)^2]
$$

* **Covariance**: relationship (linear) between 2 random variable:

$$
Cov(X, Y) = \sigma_{xy} = \mathbb{E}[(x - \mu_x)(y - \mu_y)]
$$ 


* Correlation (coefficient) is unit-less and between -1 and 1:

$$
correlation = \frac{\sigma_{xy}^2}{\sigma_x \sigma_y}
$$

### 1. Population and Sample
<hr>
The standard deviation $$\sigma$$ of a **population** is given by:

$$\sigma_{\text{pop}} = \sqrt{\frac{\sum_i (x_i - \mu_{\text{pop}})^2}{(n_{\text{pop}})}}$$

where $$n_{\text{pop}}$$ is the size of the population.


If we treat a set of data as **a sample** randomly picked from a population, then the standard deviation of that sample, or more accurately the **Standard Error** is (**Bessel's correction**):

$$\sigma_{\text{SE}} = \sqrt{ \frac{\sum_i (x_i - \mu_{\text{sample}})^2}{ (n_{\text{sample}} - 1)} }$$

where $$n_{\text{sample}}$$ is the size of the sample.

We will see later than there exists a correlation between $$\sigma_{\text{pop}}$$ and the **Standard Error**: so knowing for example the standard error of a sample enables to deduce the standard deviation of the population it is drawn from.


### 2. Shape of the distribution
<hr>
#### 2.1. Normal distribution

In a normal distribution:

+ $$\mu$$(mean) = median = mode

+ it is roughly symmetric 

In a normal distribution, **68%** of the data lies in the range [$$\mu - \sigma, \mu+\sigma$$], and **95%** of the data lies in the range [$$\mu - 2\sigma, \mu+ 2\sigma$$]

<div id="norm_dist" style="width:500px; height:500px;"> </div>

The curve above is called "**PDF**", short for "**Probability Density Function**", where the area under the curve represents probability (percentile).

#### 2.2. Skewed distribution 
Another type of distribution is the skewed distribution. Within that family, there can be distribution skewed to the right, or skewed to the left.

The distribution below is skewed to the right.

<div id="skewed_dist" style="width:500px; height:500px;"> </div>


### 3. *z*-score
<hr>
For a **normal distribution**, we define the *z*-score of a score $$x_i$$ as the number of standard deviations below or above the population mean a raw score is.

$$z_{\text{score}} = \frac{x_i - \mu}{\sigma}$$


<div id="auc" style="width:500px; height:500px;"> </div>


We can look-up for the probability using the [*z*-table](http://math2.org/math/stat/distributions/z-dist.htm). The *z*-table gives the probability of drawing a sample with value **less** than *z*.


*You might ask, why do we need to define z-score, yet another statistical parameter.*

The *z*-score is a way to standardize the score so that one can look-up the probability parameter. Imagine that you are dealing with several distributions (different means and standard deviations). In order to workout the probability, you would need a probability table for each distribution. That's very inconvenient! 
One option is to standardize all the distribution curves: the distribution is centered to 0 with ($$x_i - \mu$$), and then divided by $$\sigma$$ .


### 4. Central Limit Theorem
<hr>
Let's assume a population with mean $$\mu$$ and standard deviation $$\sigma$$. Let's say we take a sample from that population and calculate the sample mean. We take another sample of same size and calculate the new sample mean. If we keep doing that 100+ time, then the **sampling distribution** would look like a normal distribution. 

+ The mean of the sampling distribution is about same as the mean of the population.

+ The standard deviation of the sampling distribution is called the **Standard Error** (SE):

$$SE = \frac{\sigma_{pop}}{\sqrt{N}}$$

where $$N$$ is the sample size.


**The theorem would still hold true independently of the shape of the population distribution (multi-modal, skewed, ...).**


### 5. Quizz
<hr>
This Quizz is a good example on how the *z*-score can be used:

>A normally distributed **population** has a mean of $$\mu = 100$$ and standard deviation $$\sigma=20$$. What is the probability of randomly selecting a **sample** of size 4 that has a mean greater than 110?

From $$\mu=100$$, $$\sigma_{\text{pop}}=20$$, we calculate the standard error: $$SE = 20/\sqrt{4} = 10$$.

The *z*-score f that sample is: $$z = \frac{110 - 100}{10} = 1$$.
From the *z*-table, we find that the probability associated with *z*-score=1 is $$P(z=1) = 0.8413$$

This probability is for a sample that has a mean smaller than 110 (i.e it is the area under the curve in the range $$-\infty$$ to 110. So the probability that the sample has a mean greater than 110 is:
P = 1 - 0.8413 = 0.1587. 

<script>
    myplot1 = document.getElementById('norm_dist');
    var layout = {
  		autosize: false,
  		width: 500,
  		height: 400,
 	 	paper_bgcolor: '#FFFFFF',
		plot_bgcolor: '#FFFFFF'
	};
    var trace_norm1 = {
  		x: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18],
  		y: [0.01, 0.015, 0.02, 0.05, 0.12, 0.2, 0.4, 0.6, 0.7, 0.7, 0.6, 0.4, 0.2, 0.12, 0.05, 0.02, 0.015, 0.01 ],
  		type: 'line'} ;
  	var ann_mean = {
  		x: [6.5, 8.5, 10.5],
  		y: [0, 0, 0],
  		mode: 'markers+text',
  		name: 'parameters',
  		text: ['-s', 'mu', '+s'],
  		textposition: 'top',
  		type: 'scatter'};
  	 var ann_prob = {
  		x: [8.5],
  		y: [0.35],
  		mode: 'text',
  		name: 'prob',
  		text: ['68%'],
  		textposition: 'top',
  		type: 'scatter'};
  	var filled_dist = {
 		x: [6.5, 7, 8, 9, 10,10.5],
  		y: [0.5, 0.6, 0.7, 0.7, 0.6, 0.5],
  		fill: 'tonexty',
  		type: 'scatter'};
	all_trace_norm = [filled_dist, trace_norm1, ann_mean, ann_prob]
	Plotly.newPlot(myplot1, all_trace_norm);

    myplot2 = document.getElementById('skewed_dist');
    var trace_skewed1 = {
  		x: [0, 0.5, 1, 1.5, 2, 2.5, 3, 3.5,  4, 4.5,  5, 5.5, 6, 6.5,  7, 7.5, 8, 8.5,  9, 9.5,  10],
    	y: [0.4, 0.6, 0.7, 0.75, 0.7, 0.67, 0.6, 0.52, 0.4, 0.35, 0.2, 0.15, 0.12, 0.09, 0.05, 0.02, 0.017, 0.015, 0.01, 0.01, 0.01],
  		type: 'line'};

  	all_trace_skewed = [trace_skewed1]
	Plotly.newPlot(myplot2, all_trace_skewed);

	myplot3 = document.getElementById('auc');
    var layout = {
  		autosize: false,
  		width: 500,
  		height: 400,
 	 	paper_bgcolor: '#FFFFFF',
		plot_bgcolor: '#FFFFFF'
	};
    var trace_norm1 = {
  		x: [-7.5, -6.5, , -5.5, -4.5, -3.5, -2.5, -1.5, -0.5, 0.5, 1.5, 2.5, 3.5, 4.5, 5.5, 6.5, 7.5, 8.5, 9.5],
  		y: [0.01, 0.015, 0.02, 0.05, 0.12, 0.2, 0.4, 0.6, 0.7, 0.7, 0.6, 0.4, 0.2, 0.12, 0.05, 0.02, 0.015, 0.01 ],
  		type: 'line'} ;
  	var ann_mean = {
  		x: [10.5],
  		y: [0],
  		mode: 'markers+text',
  		name: 'parameters',
  		text: ['z'],
  		textposition: 'top',
  		type: 'scatter'};
  	var filled_dist = {
  		x: [-7.5, -6.5, , -5.5, -4.5, -3.5, -2.5, -1.5, -0.5, 0.5, 1.5, 2.5, 3.5],
  		y: [0.01, 0.015, 0.02, 0.05, 0.12, 0.2, 0.4, 0.6, 0.7, 0.7, 0.6, 0.4 ],
  		fill: 'tonexty',
  		type: 'scatter'};
	all_trace_norm = [filled_dist, trace_norm1, ann_mean]
	Plotly.newPlot(myplot3, all_trace_norm);

</script>
