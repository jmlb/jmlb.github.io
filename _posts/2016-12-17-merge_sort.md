---
layout: post
title: Sorting Algorithms - Merge Sort (Part 2/3)
category: flashcards
subcategory: algorithm
tags: sorting algo
author: "Jean-Marc Beaujour"
summary: "Implementation of Merge Sort"
img_post: algorithms.jpg
github-link: na
---

<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

The goal of a **Sorting Algorithm** is to change the order of the elements in an array. The sorting can be done either **in-place**, re-order the datastructure within, or **not-in-place** by copying to a new datastructure. The in-place algorithms are low space complexity. The choice of an algorithm for an application is mainly driven by the application itself.

This is the 2nd part of a series of 3 short posts on the topic of Sorting Algorithms:

1. [Bubble Sort]({% post_url 2016-12-17-bubble_sort %})
2. Merge Sort
3. [Quick Sort]({% post_url 2016-12-17-quicksort %})



### Part II: Merge Sort
<hr>
The *Merge Sort* consists in splitting an array as much as possible:

<img src="/images/20161217/merge_sort_1.png" width="400px">


It uses the principle of *Divide and Conquer*: breaking up an array, sorting all the parts of it, and building up it back again.

Here is a demonstration:

<img src="/images/20161217/merge_sort_2.png" width="400px">


### Efficiency
<hr>
Let's record how many steps are there per array size?

If $$m$$ is the array size, then it looks like we are doing $$m-1$$ comparisons

<img src="/images/20161217/merge_sort_table1.png" width="400px">


The number of comparisons increases when the array size is doubles.

The time efficiency is: O( (Nbr of comparisons per steps) * (nbr of steps) )


* Number of comparisons per steps:

<img src="/images/20161217/merge_sort_3.png" width="400px">

We could approximate and say that the number of comparisons at every step is about the size of the array: $$n$$.


So Merge Sort efficiency has an efficiency of : 

$$
O(n \text{log} n)
$$

It is better than the bubble sort ( $$O(n^2)$$ ).


Space efficiency of Merge Sort is worst than Bubble Sort as we need to create extra arrays. **Auxiliary space is O(n)** or the extra space is linear assuming we delete the array after using them.


### Quizz
<hr>
*This is an exemple if you were to perform a merge sort on the following array. Which is a possibility for what the 2nd iteration might look like?*

<img src="/images/20161217/merge_sort_4.png" width="400px">

Response: step2 
