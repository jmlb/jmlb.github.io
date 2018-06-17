---
layout: post
title: Data Augmentation Scheme Histogram Equalizer 
category: flashcards
subcategory: dl
tags: DeepLearning computer vision
summary: ""
img_post: 20180103_word_embedding.jpg
github-link: na
---

Histogram Equalization Techniques consists in taking a low contrast image and increase the contrast between the image relative heights and lows to create a higher contrast image.

## 1. histogram Equalization 

Increases contrast by detecting the distribution of pixel densities. If there are ranges of pixel brightness that aren't currently being utilized, the histogram is then stretched to cover those ranges.


## 2. Contrast Strtching:

scale the range to include all intensities that fall within the 2nd and 98 percentiles.

## 3. Adaptative equaliztion:

Cons: has a tendency to over-amplify noise in otherwise uninteresting sections.