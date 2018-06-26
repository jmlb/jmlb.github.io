---
layout: post
title: Statistics Fundamental - Part 1
category: flashcards
subcategory: "ml"
tags: math stats
summary: ""
img_post:
github-link: "na"
---


# Definitions

* **Population**: a particular group

* **sample: a subset of the population.

* **parameters**: numerical description of a *population* characteristic (mean height...).

* **sample statistics**: numeric description of particular sample characteristic.

* **Branches of statistics:
  1) *Descriptive statistics*
  2) *Inferential statistics*: use descriptive statistics to estimate population parameters - infer from the sample data the population statistics.


* mean:

 1) population mean: $\mu = \frac{\sum_i x_i}{N}$ 
 2) sample mean: $\bar{x} = \frac{\sum_i x_i}{n}$

* Median: middle value in an **ordered** list
+ If *odd* samples -> median is middle
+ If *even* samples -> average the 2 middle points

1 2 3 4 5 6 7 8 9 10 11 12 13
            |    

* Variance / Standard Deviation:

+ population variance: $\sigma^2 = \frac{\sum_i (x_i - \mu)^2}{N}$


+ sample variance: $\s^2 = \frac{\sum_i (x_i - \mu)^2}{n-1}$

* Coefficient of variation: CV is used to compare 2 dataset to know which one is more spread

$$ CV = \frac{s}{\bar{x}} \times 100 [%]$$

* Standard deviation of grouped data:

We cannot use $s = \sqrt{\frac{\sum_i (x_i - \bar{x})^2}{n-1}}$ because we do not know $x_i$ nor $\bar{x}$. For grouped data, the standard deviation is given by:

$$ 
\begin{align}
s &= \sqrt{ \frac{\sum_c f_c (x_c - \bar{x})^2}{n-1} } = \sqrt{\frac{\sum_c f_c x_c ^2 + \bar{x}^2 - 2 x_c \bar{x} }{n -1} } \\
&=\sqrt{\frac{\sum_c f_c x_c ^2 + \bar{x}^2 - 2 x_c \bar{x} }{n -1} } 

\end{align}$$

where $f_c$ is the count/frequency, $x_c$ is the midpoint of the class, $n=\sum_c f_c$ is the sample size and $\bar{x} = \frac{\sum_c f-c x_x}{\sum_c f_c}$ is the sample mean for a frequency distribution.

| Grades | Frequency | Midpoint |
|--------|-----------|----------|
|94-100  | 5         |       97 |
|87-93   |   8       |       90 |
|80-86   | 12        |       83 |
|73-79   | 7         |       76 |
|66 - 72 | 4         |       69 |
