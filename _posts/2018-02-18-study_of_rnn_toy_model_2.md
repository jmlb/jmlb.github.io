---
layout: post
title: Study of a RNN toy model
category: flashcards
subcategory: dl
tags: DeepLearning lstm rnn keras
author: "Jean-Marc Beaujour"
img_post: 2018-02-18-input_reconstruction.jpg
summary: 
github-link: "na"
---

<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


$$
\begin{align}
f = \sigma\left( W x_i + U h_i + b_f \right)
\end{align}
$$

Stateful = True/False
For True, the last state for each sample at index *i* in a batch will be used as initial state for the sample of index *i* in the following batch. Otherwise zero for each new batch.

Dynamic vs Unrolled RNNs


With unrolled RNN, we rebuild an identical computational graph for each time step, and pass the hidden state from one timestep forward to the next manually.
Advantage: if you want to process the hidden states from one step in any way before you pass them on, you can easily put more nodes in the graph.
Downsides: + the graph has to be fixed length, which means you'll have to revuild the graph for different length signals, or pad them out with zeros.
            + large graph takes longer to build and consume more RAM


tf.dynamic_rnn : transform your RNNCell into a dynamically generated graph that passes the state from one step to the next, and keeps track of the outputs. 