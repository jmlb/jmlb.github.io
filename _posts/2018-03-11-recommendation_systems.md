---
layout: post
title: Recommendation Systems 
category: flashcards
subcategory: ml
tags: DeepLearning convnet keras
summary: ""
img_post: 20180320-genetic_algo_nn.png
github-link: na
---


<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


## 1. Collaborative Filtering: 

Collaborative Filtering recommend items based on crowdsourced information about user's preferences for items

    * user based 
    * items based

### 1.1 Item based

below is an example of item based recommender:

<img src="/images/20180311/graph_01.png" width="500rem">

`userD` and `userB` gave 4 stars to the cellphone and the cellphone case, and `userA` gave 4 stars to the cellphone. So the model would recommend the `cellphone case` to `userA`.


### 1.2 User based


<img src="/images/20180311/graph_2.png" width="500rem">

`userB` is similar to `userD`, `userD` likes his life insurance, so let's recommend it to `userB` also.

## 2. Cotent-based Recommender

Content-based recommender recommend items based on similarities between features.


## 3. Popularity based recommenders

Based on simple count statistics but don't take user