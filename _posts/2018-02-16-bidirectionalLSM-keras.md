---
layout: post
title: Bidirectional LSTM in Keras
meta: "deep, learning NLP"
category: ml
author: "Jean-Marc Beaujour"
img_post: 20180216_bidirectionalLSTM.jpg
summary: 
github-link: "na"
---

A LSTM is a special kind of RNN that is designed to solve the long term dependency  problem by using a series of gates, that control the flow of information to the cell state. 



Bidirectional uses a mirror image of the LSTM cel.

So instead of concatenating the `x_i` and the `h_tm1`, we concatenate `x_i` and `h_tp1`. We have a positive time direction (forward states), and a part for the negative time direction (backward states). The Bidirectional LSTM enable to provie context.


For the Bi-directional, there  are differnet type of merge modes: combining schem for the forward and backward outputs before being passed onto the next layer:

* sum : add
* mul
* concat (default) providing 2 times more outputs
* average: average of the outputs