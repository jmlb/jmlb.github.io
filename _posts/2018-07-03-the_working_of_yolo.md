---
layout: post
title: YOLO - An Object Detection Algorithm 
category: flashcards
subcategory: dl
tags: Computer_Vision yolo detection convnet
summary: "Yolo is a detection algorithm. In this post, I ll explained how yolo works and the architecture of the model. I will discuss Yolov1 and Yolov3."
img_post: 20180630-segmentation.png
github-link: na
---



<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>



The original image is split into a grid where a **classifier** is run on each patch of the grid to determine whether the patch contains an object or not. Bounding boxes are then assigned to the patch that is classified as positive.



## 1. Yolo v1

<div style="width: 100%; text-align: center;">
<img src="/images/20180703/yolo1.png" width="800rem" style="margin-right: 2rem; margin-left:0rem; margin-top: 0rem; margin-bottom: 0rem">
</div>

The model is composed of 6 blocks *conv/leaky_relu/max_pool*, followed by 3 blocks *conv/leaky_relu*, and 2  fully connected layer, 1 *leaky_relu* layer, and 1 fully connected layer. The output layer has **1470** nodes. The model has about 45 million parameters. The grid is made of $$7\times7=49$$ patches.

The output layer is composed of:

* Classifier nodes: 980 nodes. For each patch, there are 20 classification nodes for each of the 20 classes. 

$$
980 = 7 \times 7 \times 20
$$ 

* Confidence scores: 98 nodes. There is a confidence score for each bounding box.  

$$
98 = 7 \times 7 \times 2 
$$

* Bounding boxes: 392 nodes.
There are 2 boxes that are predicted for each patch. A bounding box is defined by the parameters: $$x, y, w, h$$. 

$$
392 = 7 * 7 * 4 * 2
$$

$$(x, y)$$ is the coordinates of the center of the box relative to the bound of the grid cell. The $$w$$ and $$h$$ are relative to the whole image.

<div style="width: 100%; text-align: center;">
<img src="/images/20180703/img02.jpeg" width="800rem" style="margin-right: 2rem; margin-left:0rem; margin-top: 0rem; margin-bottom: 0rem">
</div>


## 2. Yolo v3

For YoloV3, each grid cell predict $$B$$ bounding boxes. 

<div style="width: 100%; text-align: center;">
<img src="/images/20180630/img03.jpeg" width="800rem" style="margin-right: 2rem; margin-left:0rem; margin-top: 0rem; margin-bottom: 0rem">
</div>



## 3. EXplanation

#### 3.1 Confidence Score

The confidence score reflects how confident the model is that the box contains an object and also how accurate it thinks that the box that it predicts.

$$
Confidence = Pr(Object) \times IOU
$$

If no object are detected, the confidence score = 0. Otherwise, $$Confidence=IOU$$.

#### 3.2 Bounding Box

Each bounding box consists of 5 predictions $$(x, y, w, h)$$ and confidence.