---
layout: post
title:  Recursion algorithm
category: flashcards
subcategory: algorithm
tags: algorithm
author: "Jean-Marc Beaujour"
summary: "Recursion implemented in Python"
img_post: algorithms.jpg
github-link: na
---

<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

> Recursion: a function that calls itself



{% highlight python linenos%}
def recursive(input):
  #base case: exit condition
  if input <= 0:
    return input
  else:
    output = recursive(input -1 )
    return output
{% endhighlight%}

Below is the demonstration with the input=2.


<img src="/images/20161216/recursion_exple.png" width="500rem">


### Implementation of the Fibonacci Sequence
<hr>
Now, we are going to take a look at recursion with a famous example: the **Fibonacci Sequence**. The Fibonacci Sequence follows one rule: get the next number in the sequence by adding the two previous numbers. Here is an example of the sequence:

    [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...]


We start with *(val_0 = 0)* --> *(val_1 = 0 + 1 = 1)*.

Then, we step through each value: *(val_2 = val_0 + val_1 = 0 + 1 = 1)  -->  *(val_3 = val_1 + val_2 = 1 + 1 = 2)*, ...etc


We implemente `get_fib()` in a recursive way:

{% highlight python linenos%}
def fb(position):
    if position > 1:
        output = fb(position-2) + fb(position-1)
        return output
    
    else:
        if position == 1:
            output = 1
            return output
        elif position == 0:
            output= 0
            return output
{% endhighlight%}


Actually, we can realize that for position 0 and 1, the output is actuaclly the position so:

{% highlight python linenos%}
 def fb(position):
    if position > 1:
        output = fb(position-2) + fb(position-1)
        return output
    
    else:
        if position == 1 or position == 0:
        output = position
        return position

        Here is the compact version:

 def fb(position):
 	if position == 1 or position == 0:
        output = position
        return position
    else:
        return fb(position-2) + fb(position-1)
{% endhighlight%}



**Tips**:

You may have noticed that this solution will compute the values of some inputs more than once. For example `get_fib(4)` calls `get_fib(3)` and `get_fib(2)`, `get_fib(3)` calls `get_fib(2)` and `get_fib(1)` etc... The number of recursive calls grows exponentially with *n*.

In practice if we were to use recursion to solve this problem we should use a **hash table** to store and fetch previously calculated results. This will increase the space needed but will drastically improve the runtime efficiency.
 
