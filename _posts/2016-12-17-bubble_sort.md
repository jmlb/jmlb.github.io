---
layout: post
title: Sorting Algorithms - Bubble Sort (Part 1/3)
category: flashcards
subcategory: algorithm
tags: sorting algo
author: "Jean-Marc Beaujour"
summary: "Bubble sort implemented in Python"
img_post: algorithms.jpg
github-link: na
---

<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


The goal of a **Sorting Algorithm** is to change the order of the elements in an array. The sorting can be done either **in-place**, re-order the datastructure within, or **not-in-place** by copying to a new datastructure. The in-place algorithms are low space complexity. The choice of an algorithm for an application is mainly driven by the application itself.

Here is a series of 3 short posts on the topic of Sorting Algorithms...in fact the most common ones:

1. Bubble Sort
2. [Merge Sort]({% post_url 2016-12-17-merge_sort %})
3. [Quick Sort]({% post_url 2016-12-17-quicksort %})



### Part I: Bubble Sort
<hr>
Bubble Sort is a in-place sorting algorithm, also called Naive approach. The idea is to travel through every element of the array and compare with the next element.

<img src="/images/20161217/bubble_01.png" width="400px">


Additional iterations might be required to completely sort the list.

## Efficiency

<img src="/images/20161217/bubble_sort_table1.png" width="400px">


### Time efficiency:

 $$ eff = (n - 1) \times (n-1) = n^2 -2n +1 \approx n^2$$

Therefore, the Bubble Sort time efficiency is $$O(n^2)$$:

* in the worst case:  $$O(n^2)$$ 

* in the average case:  $$O(n^2)$$

* in the best case (the list is already sorted): $$O(n)$$
but w e still need to go through the array at once.

### Space efficiency: 

Because there is no extra array (the sorting is in-place), the space complexity is constant.


### Quizz
<hr>
The Quizz below is from the Machine Learning Nanodegree of Udacity, and in particular the module related to Technical Interviews.

*If you were to perform Bubble Sort on the following array, what would be the 3rd iteration look like?*

<img src="/images/20161217/bubble_sort_quizz1.png" width="400px">


**Answer:**

<img src="/images/20161217/bubble_sort_quizz.png" width="400px">
