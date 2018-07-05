---
layout: post
title: What is Field of View?
category: "flashcards"
subcategory: "dl"
tags: "Computer_Vision Cameras field_of_view fov"
summary:
img_post:
github-link: na
---



<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

**FOV** stands for *Field of View*.

The field of view a typical webcam is around 78.
Typically, the raw image captured by a webcam is cropped and resized to: (640, 480).

Using the raw image size captured by the camera, let's assume (1920, 1080), then the *angle resolution* is:

$$
angle_res = \frac{FOV}{\sqrt{1920^2 + 1080^2}}
$$