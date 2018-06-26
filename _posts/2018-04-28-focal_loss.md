---
layout: post
title: Focal Loss function to address class imbalance
category: flashcards
subcategory: dl
tags: DeepLearning convnet keras
summary: ""
img_post: 20180320-genetic_algo_nn.png
github-link: na
---



<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

Focal loss to address class imbalance

==> improve performance of 1 stage detector in object detection

Contribution of the loss of well classified example are reduced

* Typical CE:

$$
\begin{align}
CE = - y log(\hat{y}) - (1- y)log(1-\hat{y})
\end{align}
$$

If we include class imbalance:

$$
\begin{align}
CE = -\alpha y log(\hat{y}) - (1-y)(1- \alpha) log(1 - \hat{y})
\end{align}
$$

where typically $$\alpha = 1/f$$ where $$f$$ is the count, but it can be adjusted as a hyperparameter.
$$\alpha$$ balances positive/negative but does not correct hard/easy.




We can write $$CE$$ as :

$$
\begin{align}
CE(p_t) = - log(p_t)
\end{align}
$$



$$
\begin{align}
FL(p) = -y(1-p)^{\gamma} log(p) + (1- y) p^{\gamma} log(1- p)
\end{align}
$$

where $$y \in \{0, 1\}$$ ground truth, and $$p \in [0, 1]$$ model probability for the class with label $$y=1$$. $$\gamma$$ is the focusing parameter tunable

FL with balance factor

$$
\begin{align}
FL = -\alpha_t (1-p_t)^{\gamma} log(p_t)
\end{align}
$$


### What's the application to multiclass?

### Can we refine old models with new loss?

Thank you for the reply.
Yes, I have read and still going through the paper. I am actually spending a lot of time on the intro (and the references therein) to learn more and understand better the different algos for 2-stage and 1-stage detection. I was not very familiar with the details of those  algorithms.
In any case,  my main take away from this paper is the formulation of a novel Loss function (Focal Loss) to improve accuracy in object detection problems where the negative class ("the background") dominates over the positive class (the object(s) of interest). In the Focal Loss function, more weights are "given" to hard examples. In the Focus Loss,  the focusing parameter $\gamma$ is a hyper-parameter that can be fine-tuned using cross-validation set. The authors found that a "quadratic weighting" works best (i.e with $\gamma=2$) with the detector used (RetinaNet).

This "focus weighting" can be combined with the more traditional "frequency" weighting for class imbalance in minibatch : the $\alpha$ term in the final Loss function.

I think, the focus Loss function could have application in semantic segmentation problems with binary masks (using similar Cross entropy Focus loss as in Equation 4 & 5, but also for multi-masks cases (using  the Focus Loss generalized to the Softmax function).
In the case of semantic segmentation, more weights would be "applied" to hard example pixels versus easy example pixels. 
In the case of the binary mask for example, if a pixel is labeled 1 with p=0.6:
    * the error of that pixel would be amplify by the focus term, 
    * the enhanced error will propagate back through the model, and
    * have a stronger effect on the Weight update
In contrast, if a pixel is labeled 1 with p=0.95, then the error from that pixel would have a weaker effect on the Weights update.

I can see 2 domains where the Focus Loss could be applied:
1) Medical imaging: for example segmentation/masking of eye disease, brain tumors (typically in those images the number of pixels associated with the desease/tumors have a smaller count compared to the background
2) segmentation of road lanes for self driving cars: similarly to (1), the count of pixels associated with the lane is much smaller than the number of background pixels.

Also, this loss function could be used on pre-trained models, as a way to fine tune the Weights to improve performance. 

I am still going through the paper. But those are my thoughts about it so far.
Please, let me know what you think.

Regards,

JM


Hi Manu,

> We should be in touch very shortly about decisions regarding your admissions status.  
That's great!

About the paper:
I am currently "playing" with YOLO v3, and found out that in their paper (https://pjreddie.com/media/files/papers/YOLOv3.pdf) they tried the focal loss but it resulted in a degradation of the performance model...i.e their model was already robust against class imbalance. 
So, the effect of the focal loss on the performance is very likely model dependent.

The use of the Focal loss seems to have a regularizing effect, and I wonder how it compares to traditional regularizer like L1/L2 or dropout (or even pooling).

Regards,

JM

I agree,
In the Focal Loss paper, the authors wrote that a quadratic penalty worked better for their model/dataset, but the optimum value of that hyper-parameter is likely to be model and dataset dependent

>I know that people tried it out for classification of imbalanced datasets, and have had a hard time making it work though, so it might be brittle.
I think that there is a drawback to the focal loss...

When including the focal loss the "slope" of the total loss (orange) is much more negative than the slope of the log loss alone (blue), which intuitively "means" (to me) that the model can now learn more. However,  when $x>0.75$, the total loss kind of saturates. At that point the model will not learn much, the weight update would be very slow.
Basically, a loss that includes the focal loss helps correct for misclassification much more than a log loss alone, but as soon as the probability is > 0.75, the loss dies off. That will certainly be detrimental for generalization.

<img src="/images/20180428/loss_shape.png" width=400px>

I think a better loss model could be one where the loss does not flatten so quickly (x>0.75)

Let me know what you think.

Best,

JM


