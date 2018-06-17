---
layout: post
title: Statistics Fundamental - Part 3
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


# z-test

*z-test* is a statistical test used to determine whether 2 population means are different when the population variance is known and the sample size is large.

The test statistic is assumed to have a normal distribution

{% tikz 20160320_01 %}

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
    \draw[dashed] ({\y},{\fy}) -- ({\y},0) node[below] {$\sigma_{\bar{x}}$};
 
% Optional: Add axis labels
    \draw (-.2,2.5) node[left] {$sampling distribution$};
    \draw (6,-.1) node[below] {$x$};
    \draw (3,0) -- (3, 4);
    \draw (3.3,-0.4) node[left] {$\mu_{\bar{x}}=100$};
 
% Optional: Add axes
    \draw[->] (0,0) -- (6.2,0) node[right] {};
    \draw[->] (0,0) -- (0,5) node[above] {};
{% endtikz %}