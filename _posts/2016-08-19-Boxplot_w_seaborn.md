---
layout: post
title: How to generate boxplots with the library seaborn
category: ml
author: "Jean-Marc Beaujour"
meta: "Machine-Learning statistics python seaborn visualization"
summary: "Boxplots is a an informative way to display the distribution of data. in this post, we will will go through the steps to create boxplots with seaborn."
author: "Jean-Marc Beaujour"
img_post: boxplot_seaborn.jpg
github-link: na
---

One of the first steps in analysing a dataset is the Data Exploration. 
The box plots is a standardized way of displaying the distribution of data. Boxplots gives information about the minimum, first quartile, median, third quartile, and maximum.

In this short post, I will focus on the vizualization of the data and the outliers using BoxPlot. To learn more on **Data Exploration**, check this very thorough <a href="https://www.analyticsvidhya.com/blog/2016/01/guide-data-exploration/"> post </a>.

<img src="/images/201608/boxplots.jpg" align="center">

I will be using data from an assignment of the Machine Learning Nanodegree: [customer segmentation]("2016-08-19-customer_segmentats.md"). The dataset is made of 400 businesses: restaurants, retailers, etc..., with their purchasing pattern for Milk products, Grocery products, and more...

<script src="https://gist.github.com/jmlb/d9aea93e58869efa309f637a1074a84d.js"></script>

<img src="/images/201608/market_segmentation_output_30_1.png" align="center">

You can also check out this post where Matplotlib was used to generate the boxplots. [link]("http://blog.bharatbhole.com/creating-boxplots-with-matplotlib/")
