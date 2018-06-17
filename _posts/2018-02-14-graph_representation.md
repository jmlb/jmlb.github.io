---
layout: post
title: Graph representation - Adjancy list versus Adjancy Matrix
meta: data structures
category: coding
author: "Jean-Marc Beaujour"
img_post: 2018-02-14_graph_datastructure.png
summary: 
github-link: "na"
---

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

A **Graph** consists of *nodes* connected or not to other nodes by *edges*.

There are several ways of representing a graph. In this post, I will discuss 2 of those representations: *adjancy list*, *adjancy matrix*.
Depending on the nature of the graph (dense or sparse), one or the other representation would be favored.

* **Dense graph**: $$n\_edges = (n\_nodes)^2$$
* **Sparse graph**: $$n\_edges = (n\_nodes)$$



## Adjancy list

The key are the nodes, and the value is a list of connected nodes.
```
{
A: [B, C, E]
B: [A, C]
}
```

Pros:

* faster and uses less space for sparse Graphs

Cons:

* slower for dense Graphs

## Adjancy Matrix


$$  \begin{matrix}
      & A & B & C & D & E \\
    A & 0 & 1 & 1 & 0 & 1  \\ 
    B & 1 & 0 & 1 & 0 & 0  \\
    C & 1 & 1 & 0 & 1 & 1  \\
    D & 0 & 0 & 0 & 0 & 0  \\
    E & 1 & 0 & 1 & 0 & 0  \\
  \end{matrix} $$

For each connected nodes, the value of the matrix at the corresponding indexes is 1. 

If the edge is weighted, we can replace 1 by the weight.
For example:

$$
  \begin{matrix}
      & A & B & C & D & E \\
    A & 0 & 1 & 0.6 & 0 & 0.1  \\
    B & 1.2 & 0 & 1 & 0 & 0  \\
    C & 1.5 & 0.3 & 0 & 1.6 & 1  \\
    D & 0 & 0 & 0 & 0 & 0  \\
    E & 0.8 & 0 & 1 & 0 & 0  \\
  \end{matrix}
$$


Pros: 

* works well for dense Graphs
* faster for dense Graphs
* simple for implemented weighted edges

Cons:

* use more space: takes up to $$n\_nodes ^2$$ space. 
For example, 1,000 nodes will take up to 100,000 bytes.
Typically, $$n\_nodes$$ must be below 1000.



For one of of the Kaggle competitions: ["Quora, Quora Question Pairs"](https://www.kaggle.com/c/quora-question-pairs), I was looking for a datastructure that will enable to speedup the code to generate data augmentatiton.