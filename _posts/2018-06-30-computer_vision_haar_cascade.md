---
layout: post
title: Haar Cascade Classifier
category: flashcards
subcategory: dl
tags: Computer_Vision Udacity haar_cascade
summary: "Those are my notes from the Udacity Nanodegree on Computer Vision. The notes are about Haar Cascade."
img_post: 20180630-haar_cascade.jpg
github-link: na
---



<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


### Haar Cascade

**Haar Cascade Classifier** is a machine learning based approach where a *cascade* function is trained to localize an object (the positive class) in an image. For example: find faces in the image. 

<div style="width: 100%; text-align: center;">
<img src="/images/20180630/img_11a.png" width="300rem" style="margin-right: 2rem; margin-left:0rem; margin-top: 0rem; margin-bottom: 0rem">
</div>

The Classifier is trained on a set of images with positive and negative class: images with and without faces. The model objective is to localize the positive class in an image by extracting features, using the Haars filters/kernels (see below)

<div style="width: 100%; text-align: center;">
<img src="/images/20180630/img_12a.jpg" width="300rem" style="margin-right: 2rem; margin-left:0rem; margin-top: 0rem; margin-bottom: 0rem">
</div>

**The Haas filters are very much like convolutional kernels.** Each feature is a single value obtained by subtracting sum of pixels under the white rectangle from sum of pixels under the black rectangle.
Features are calculated for the different filters, for different size of the filters and for different position of the filters.

In an image, not all areas include the positive class: if a feature calculated by Haar filter is irrelevant, it is discared. The best features are selected using **Adaboost**. For each feature, a threshold value is determined for classification of the positive and negative example. Furthermore, the best features (the features that lead to the smaller classification error) are selected. 
Like an ensemble model, the final classifier is a weighted sum of the weak classifiers to form a strong classifier.

**This method enables to downsample an image from a number of features equal to its original size to a smaller number of features.**


> In an image, most of the image is negative class region. So to accelerate the classification process, If a part of an image is not from the positive class, it is discarded in a single shot. This is the concept of Cascade of Classifiers. Instead of applying all 6000 features on a window, the features are grouped into different stages of classifiers and applied one-by-one. (Normally the first few stages will contain very many fewer features). If a window fails the first stage, discard it. We don't consider the remaining features on it. If it passes, apply the second stage of features and continue the process. The window which passes all stages is a face region. How is that plan! 


### Summary

1. For any input image, Haar cascade applies Haar feature detectors and performs classification on the entire image. If the algorithm does not get enough of a feature detection response, it classifies the corresponding area of the image as negative class, and discards the information.

2. The reduced image area is fed to the next feature detector and classifies the image again, discarding irrelevant negative area at every step.

Haar Cascade is a fast algorithm that is commonly used in real-time on a laptop, because at every step it throw away irrelevant information.


```python
import cv2
import numpy as numpy
import matplotlib.pyplot as plt

# load_model as xml file
model = cv2.CascadeClassifier(xml_file)

#Prediction
pred = model.detectMultiScale(gray, scaleFactor=4, minNeighbors=6)
```

DetectMultiScale is a function that aims at detecting faces for example, of varying size. For example a small `scaleFactor` and lower `minNeighbors` enables t odetect more faces.

The output `pred` is a list of box coordinates: `[[x0, w, y0, h`], ....`.