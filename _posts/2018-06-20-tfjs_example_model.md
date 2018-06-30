---
layout: post
title: A simple exemple of a TFJS model
category: tensorflowjs
tags: DeepLearning
author: "Jean-Marc Beaujour"
img_post: 20180620_a_simple_example_tfjs.jpeg
summary: "Perform a polynomial regression using Tensorflow JS, and run it in the browser."
github-link: "na"
---

The model is a simple polynomial regression, where we fit the following model to the dataset

$$
y = ax^2 + bx + c
$$

where $$a$$, $$b$$, $$c$$ are the coefficients to learn. They are marked as variables so that the optimizer can change them.

```
import * as tf from '@tensorflow/tfjs'

const a = tf.tensor(0, 1).variable();
const b = tf.tensor(0, 1).variable();
const c = tf.tensor(0, 1).variable();

const f = x => a.mul(x.square()).add(b.mul(x)).add(c);

const loss = (preds, labels)=> preds.sub(label).square.mean();

const optimizer = tf.train.sgd(learningRate)

for (let i=0; i<EPOCHS;i++){
  optimizer.minimize(() => loss(f(data.xs), data.ys));
}

```