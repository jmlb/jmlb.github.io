---
layout: post
title: Statistics Fundamental - Part 1
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


# Definitions

* **Population**: a particular group

* **Sample**: a subset of the population.

* **parameters**: numerical description of a *population* characteristic (mean height...).

* **sample statistics**: numeric description of particular sample characteristic.

* **Branches of statistics**:

  1) *Descriptive statistics*

  2) *Inferential statistics*: use descriptive statistics to estimate population parameters, i.e infer from the sample data the population statistics.


* **mean**:

    1) **population mean**: \\(\mu = \frac{\sum_i x_i}{N} \\) 

    2) **sample mean**: \\( \bar{x} = \frac{\sum_i x_i}{n} \\)

* **Median**: middle value in an **ordered** list

    + If *odd* samples -> median is middle
    + If *even* samples -> average the 2 middle points

{% tikz 20160320_03 %}
    \node at (-0.5,0.5) (nodeAA) {};
    \node at (0,0) (nodeA) {1};
    \node at (1,0) (nodeB) {2};
    \node at (2,0) (nodeC) {3};
    \node at (3,0) (nodeD) {4};
    \node at (4,0) (nodeE) {5};
    \node at (5,0) (nodeF) {6};
    \node at (6,0) (nodeG) {7};
    \node at (7,0) (nodeH) {8};
    \node at (8,0) (nodeI) {9};
    \node at (9,0) (nodeJ) {10};
    \node at (10,0) (nodeK) {11};
    \node at (11,0) (nodeL) {12};
    \node at (12,0) (nodeM) {13};
    \node at (13,0.5) (nodeMM) {};
    \node at (6,1) (nodemid) {I};
    \draw[ultra thick](nodeAA) -- (nodeMM);
{% endtikz %}
  

* **Variance and Standard Deviation**:

$$
\begin{align}
\sigma^2 &= \frac{\sum_i (x_i - \mu)^2}{N} \text{ population variance}   \\  
s^2 &=  \frac{\sum_i (x_i - \mu)^2}{n-1}  \text{ sample variance}
\end{align}
$$


* **Coefficient of variation**: CV is used to compare 2 dataset to know which one is more spread

$$
\begin{align}
CV = \frac{s}{\bar{x}} \times 100 [\%]
\end{align}
$$

* **Standard deviation of grouped data**:

We cannot use \\(s = \sqrt{\frac{\sum_i (x_i - \bar{x})^2}{n-1}}\\) because we do not know \\(x_i\\) nor \\(\bar{x}\\). For grouped data, the standard deviation is given by:

$$ 
\begin{align}
s &= \sqrt{ \frac{\sum_c f_c (x_c - \bar{x})^2}{n-1} } = \sqrt{\frac{\sum_c f_c x_c ^2 + \bar{x}^2 - 2 x_c \bar{x} }{n -1} } \\
&=\sqrt{\frac{\sum_c f_c x_c ^2 + \bar{x}^2 - 2 x_c \bar{x} }{n -1} } 
\end{align}
$$

where \\(f_c\\) is the count/frequency, \\(x_c\\) is the midpoint of the class, \\(n=\sum_c f_c\\) is the sample size and \\(\bar{x} = \frac{\sum_c f-c x_x}{\sum_c f_c}\\) is the sample mean for a frequency distribution.

| Grades | Frequency | Midpoint |
|--------|-----------|----------|
|94-100  | 5         |       97 |
|87-93   |   8       |       90 |
|80-86   | 12        |       83 |
|73-79   | 7         |       76 |
|66 - 72 | 4         |       69 |


# Empirical rule for Standard deviation

For a Normal distribution:

* about 68% of the data lies within 1 \\(\sigma\\)
* about 95% ........................ 2 \\(\sigma\\)
* about 99.7% ........................ 3 \\(\sigma\\)


{% tikz 20160320_05 %}

% define normal distribution function 'normaltwo'
    \def\normaltwo{\x,{4*1/exp(((\x-3)^2)/2)}}
 
% input y parameter
    \def\y{4}
 
% this line calculates f(y)
    \def\fy{4*1/exp(((\y-3)^2)/2)}
 
% Shade orange area underneath curve.
    \fill [fill=orange!60] (2,0) -- plot[domain=2:4] (\normaltwo) -- ({\y},0) -- cycle;
 
% Draw and label normal distribution function
    \draw[color=blue,domain=0:6] plot (\normaltwo) node[right] {};
 
% Add dashed line dropping down from normal.
    \draw[dashed] ({\y},{\fy}) -- ({\y},0) node[below] {$\sigma$};
 
% Optional: Add axis labels
    \draw (-.2,2.5) node[left] {$p$};
    \draw (6,-.1) node[below] {$x$};
    \draw (3,0) -- (3, 4);
    \draw (3.3,-0.4) node[left] {$\mu$};
 
% Optional: Add axes
    \draw[->] (0,0) -- (6.2,0) node[right] {};
    \draw[->] (0,0) -- (0,5) node[above] {};
{% endtikz %}



# Chebyshev's Theorem

When the data **is not** normally distributed, Chebyshev's Theorem gives a lower bond for the data size:

** The % of the data that lies within \\(K\\) standard deviations is at least:

$$
\begin{align}
1 - \frac{1}{K^2}  \text{  for } K>1
\end{align}
$$


# Standard z-score

\\(z\\) score tells us how many standard deviation a number is above or below the mean:

$$
\begin{align}
z &= \frac{x_i - \mu}{\sigma} \text{  in a population } \\
z &= \frac{x_i - \bar{x}}{s} \text{  in a sample}
\end{align}
$$


# Quartiles

{% tikz 20160320_01 %}

    \node at (0,0) (nodeA) {};
    \node at (5,0) (nodeB) {$Q_1$};
    \node at (10,0) (nodeC) {$Q_2$};
    \node at (15,0) (nodeD) {$Q_3$};
    \node at (20,0) (nodeE) {$Q_4$};
    \draw[ultra thick](nodeA) -- (nodeB);
    \draw[ultra thick](nodeB) -- (nodeC);
    \draw(nodeA) -- (nodeC) node [midway, below] (TextNode) {lower half};
    \draw[ultra thick](nodeC) -- (nodeD);
    \draw[ultra thick,arrows=->](nodeD) -- (nodeE);
    \draw (nodeC) -- (nodeE) node [midway, below] (TextNode) {upper half};
{% endtikz %}


* \\(Q_1\\) = first quartile  (25% of the data)
* \\(Q_2 \\) = 2nd quartile (50% of the data)
* \\(Q_3 \\) = 3rd quartile (75% of the data)


# BoxPlots

{% tikz 20160320_02 %}
    \draw (0,0) -- (1,0) -- (1,0.8) -- (0,0.8) -- (0,0);
    \draw (0,0.8) -- (1,0.8) -- (1,2.6) -- (0,2.6) -- (0,0.8);
    \node at (0.5,3.5) (nodeA) {max};
    \node at (0.5,-1) (nodeB) {min};
    \node at (1.5,0) (nodeC) {$Q_1$};
    \node at (1.5,0.8) (nodeD) {$Q_2$};
    \node at (1.5,2.6) (nodeE) {$Q_3$};
    \draw[ultra thick](nodeA) -- (nodeB);
{% endtikz %}
