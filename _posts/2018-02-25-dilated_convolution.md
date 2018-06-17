---
layout: post
title: Dilated Convolution
tags: deep learning, semantic segmentation, stats/maths
category: ml
summary: ""
img_post: 20180225-semantic_segmentation.jpg
github-link: na
---



<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

A $$D$$-dilated $$K \times K$$ convolution is equivalent of dilating the filter before doing the usual convolution

<img src="/images/20180225/Screen-Shot-2016-05-12-at-09-47-12.png" width="400rem">


From https://arxiv.org/pdf/1511.07122.pdf


Dilated convolution is typically used for image segmentation, as an alternative to the deconvolution layers that do the upsampling. This approach is computationally expensive, because there are more parameters to learn, but also lacks good resolution. With dilated convolution, the resolution of the output is higher and it avoids the need of upsampling.
The dilated convolution operator can apply the same filter at different ranges using different dilation factors. Fiilter dilated convolution enables to expand the receptive fields without loss of resolution.

Here is a great article on semantic segmentation: with a review of the segmentation techniqures, algos.

(also called as atrous convolution in DeepLab) 

Last two pooling layers from pretrained classification network (here, VGG) are removed and subsequent convolutional layers are replaced with dilated convolutions. In particular, convolutions between the pool-3 and pool-4 have dilation 2 and convolutions after pool-4 have dilation 4. With this module (called frontend module in the paper), dense predictions are obtained without any increase in number of parameters.

A module (called context module in the paper) is trained separately with the outputs of frontend module as inputs. This module is a cascade of dilated convolutions of different dilations so that multi scale context is aggregated and predictions from frontend are improved.

The dilated convolution enables to expand the receptive fields (field of view) without loss of resolution. The dilated convolution operator can apply the same filter at different ranges using different dilatation factors. Dilated convolutions delivers wide field of view at the same computational cost. Dilated convolution are popular in the field of ral-time segmentation.

MULTI-SCALE CONTEXT AGGREGATION BY
DILATED CONVOLUTIONS
Fisher Yu
Princeton University
Vladlen Koltun
Intel Labs

Last two pooling layers from pretrained classification network (here, VGG) are removed and subsequent convolutional layers are replaced with dilated convolutions. In particular, convolutions between the pool-3 and pool-4 have dilation 2 and convolutions after pool-4 have dilation 4. With this module (called frontend module in the paper), dense predictions are obtained without any increase in number of parameters.

A module (called context module in the paper) is trained separately with the outputs of frontend module as inputs. This module is a cascade of dilated convolutions of different dilations so that multi scale context is aggregated and predictions from frontend are improved.


VGG trained on 224x224 images

{% tikz 20160928_02 %}
  \node (bx1) at (0,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx1.center) {input image 3 ch};

  \node (bx21) at (1.6,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx21.center) {conv(64, 3x3, relu)};
  \node (bx22) at (2.4,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx22.center) {conv(64, 3x3, relu)};
  \node (bx23) at (3.2,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx23.center) {maxpool(2, 2)};

  \node (bx31) at (4.8,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx31.center) {conv(128, 3x3, relu)};
  \node (bx32) at (5.6,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx32.center) {conv(128, 3x3, relu)};
  \node (bx33) at (6.4,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx33.center) {maxpool(2, 2)};

  \node (bx41) at (8,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx41.center) {conv(256, 3x3, relu)};
  \node (bx42) at (8.8,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx42.center) {conv(256, 3x3, relu)};
  \node (bx43) at (9.6,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx43.center) {conv(256, 3x3, relu)};
  \node (bx44) at (10.4,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx44.center) {maxpool(2, 2)};

  \node (bx51) at (12,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx51.center) {conv(512, 3x3, relu)};
  \node (bx52) at (12.8,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx52.center) {conv(512, 3x3, relu)};
  \node (bx53) at (13.6,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx53.center) {conv(512, 3x3, relu)};

  \node (bx61) at (15.2,0) [draw,thick,minimum width=0.6cm,minimum height=6cm] {};
  \node[align=center,font=\large,rotate=90] at (bx61.center) {ATrousconv(512, 3x3, relu, d=2)};
  \node (bx62) at (16,0) [draw,thick,minimum width=0.6cm,minimum height=6cm] {};
  \node[align=center,font=\large,rotate=90] at (bx62.center) {ATrousconv(512, 3x3, relu, d=2)};
  \node (bx63) at (16.8,0) [draw,thick,minimum width=0.6cm,minimum height=6cm] {};
  \node[align=center,font=\large,rotate=90] at (bx63.center) {ATrousconv(512, 3x3, relu, d=2)};

  \node (bx71) at (18.4,0) [draw,thick,minimum width=0.6cm,minimum height=6cm] {};
  \node[align=center,font=\large,rotate=90] at (bx71.center) {ATrousconv(4096, k=7, relu, d=4)};
  \node (bx72) at (19.2,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx72.center) {Dropout(0.5)};

  \node (bx81) at (20.8,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx81.center) {conv(4096, k=1, relu)};
  \node (bx82) at (21.6,0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx82.center) {Dropout(0.5)};

  \node (bx91) at (23.2, 0) [draw,thick,minimum width=0.6cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=90] at (bx91.center) {conv(n\_classes, k=1, linear)};



  \node (bx101) at (2,-8) [draw,thick,minimum width=0.6cm,minimum height=7cm] {};
  \node[align=center,font=\large,rotate=90] at (bx101.center) {conv(n\_classes, 3x3, pad=1, relu)};
  \node (bx102) at (2.8,-8) [draw,thick,minimum width=0.6cm,minimum height=7cm] {};
  \node[align=center,font=\large,rotate=90] at (bx102.center) {conv(n\_classes, 3x3, pad=1, relu)};

  \node (bx111) at (4.4,-8) [draw,thick,minimum width=0.6cm,minimum height=9cm] {};
  \node[align=center,font=\large,rotate=90] at (bx111.center) {ATrousconv(n\_classes, k=3, d=2, pad=2, relu)};
  \node (bx112) at (5.2,-8) [draw,thick,minimum width=0.6cm,minimum height=9cm] {};
  \node[align=center,font=\large,rotate=90] at (bx112.center) {ATrousconv(n\_classes, k=3, d=4, pad=4, relu)};
  \node (bx113) at (6,-8) [draw,thick,minimum width=0.6cm,minimum height=9cm] {};
  \node[align=center,font=\large,rotate=90] at (bx113.center) {ATrousconv(n\_classes, k=3, d=8, pad=8, relu)};
  \node (bx114) at (6.8,-8) [draw,thick,minimum width=0.6cm,minimum height=9cm] {};
  \node[align=center,font=\large,rotate=90] at (bx114.center) {ATrousconv(n\_classes, k=3, d=16, pad=16, relu)};

  \node (bx121) at (8.4,-8) [draw,thick,minimum width=0.6cm,minimum height=6cm] {};
  \node[align=center,font=\large,rotate=90] at (bx121.center) {conv(n\_classes, 3, relu)};
  \node (bx122) at (9.2,-8) [draw,thick,minimum width=0.6cm,minimum height=6cm] {};
  \node[align=center,font=\large,rotate=90] at (bx122.center) {conv(n\_classes, 1, linear)};

  \node (bx131) at (10.8,-8) [draw,thick,minimum width=1.2cm,minimum height=9cm] {};
  \node[align=center,font=\large,rotate=90] at (bx131.center) {Deconv(n\_out=n\_classes, k=16, stride=8, \\pad=4, bilinear, group=n\_classes, bias=False)}; 
{% endtikz %}