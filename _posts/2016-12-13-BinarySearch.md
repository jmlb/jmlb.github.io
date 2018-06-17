---
layout: post
title: Binary Search Algorithm
category: flashcards
subcategory: algorithm
tags: search
author: "Jean-Marc Beaujour"
img_post: 20161213-binary_search.jpg
summary: Implementation of Binary Search 
github-link: "na"
---

<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

> Search for an element in a sorted array


## Example

Let's consider an **array** with element values **sorted in numerical order** (see below)

<img src="/images/20161213/img_01.png" width="800rem" alt="">

We want to see if the number **[25]**, we will call it the target value, exists in the array. If we go through the entire array, starting from the first element, it will take *O*(*n*) time. A faster approach is detailed below:


#### 1. start in the middle of the array

<img src="/images/20161213/img_02.png" width="400rem" alt="">

```
id_mid = (id_end - id_start)//2
```

#### 2. Check if the number in the middle is smaller or bigger than the target value. If **TRUE** (smaller), do step 3.

```
if array[id_mid] == target:
    return id_mid

elif array[id_mid] > target:
    use top subarray
elif array[id_mid] < target:
    use bottom subarray

```

#### 3. Divide again the 2nd half of the array and find the new middle

<img src="/images/20161213/img_03.png" width="400rem" alt="">


We can reiterate the assumption on the 2nd half of the array.

If the array is not splittable, then always choose the number with lower index.


## Binary Search Efficiency

Let's look at the number of splits/steps for different array size:

    * array of 1 element : 1 split
    * array of 2 element : 2 splits
    * array of 3 element : 2 splits
    * array of 4 element : 3 splits


<img src="/images/20161213/img_04.png" width="400rem" alt="">

One can see that the number of splits/steps increases when the array length is doubled.

$$ 
\begin{align}
n &= 0 \rightarrow 1 \\
n &= 2 \rightarrow 4 \\
\end{align}
$$

Hence the number of splits scales as $$2^n$$

When the array double in size, the number of steps increases by 1, so: `O(power of 2 exponent + 1)` ($$2^3 = 8 \rightarrow \text{log}_2 8  = 3$$

Hence, 

$$
\begin{align}
O(\text{log}_2 (n) + 1) \approx O(\text{log}(n))
\end{align}
$$

We can dismiss the constant 1 and typically is typically $$\text{log}_2$$.

$$
\begin{align}
\text{array} &= 2^{\text{iter}} = n \\
\text{iter} &= \text{log}_2 n + 1
\end{align}
$$

## Python implementation:

{% highlight python linenos %}

def binary_search(input_array, value):
    
    if len(input_array) == 0:
        return -1
        
    id_start = 0
    id_end = len(input_array)
    idx = -1
    
    while idx == -1 and id_start <= id_end:
        idx_split = (id_start + id_end) // 2
        
        if input_array[idx_split] == value:
            idx = idx_split
        
        elif value > input_array[idx_split]:
                id_start = idx_split + 1
            
        elif value < input_array[idx_split]:
            id_end = idx_split - 1
    
    return idx
    
test_list = [1,3,9,11,15,19,29]
test_val1 = 25
test_val2 = 15

print(binary_search(test_list, test_val1) )
print(binary_search(test_list, test_val2) )

{% endhighlight %}