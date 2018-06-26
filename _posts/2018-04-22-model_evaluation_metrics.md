---
layout: post
title: Evaluation metrics
category: flashcards
subcategory: dl
tags: accuracy model evaluation
summary: ""
img_post:
github-link: na
---


<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

Evaluation metrics enables to evaluate a model performance: it is often good to compare the predictive model with a random model usign same evaluation metrics. In this post we list the evaluations metrics typically used in evaluation.

## 1. Confusion metrics

For classification problem, the confusion matrix provides a tabular summary of the actual class labels vs. the predicted ones. 

## 2. Accuracy

The accuracy is a key metrics for classification problems; it is the sum of well classified items over all items.

$$
\begin{align}
accuracy = \frac{\# correct predictions}{\# total data points}
\end{align}
$$
## 3. Per class metrics: precision, recall, F1-score

A variation of accuracy is the average per-class accuracy - the average of the accuracy for each class. Accuracy is an example of a micro-average, and average per class accuracy is a macro-average. 

Those metrics are particularly useful when the class labels are not uniformly distributed. When the classes are imbalanced, i.e there are a lot more examples of one class than the other, then the accuracywill give a very distorted picture, because the class with more examples will dominate the statistics.

* Precision is defined as the fraction of correct predictions for a certain class

* recall is the fraction of instances of a class that were correctly predicted

There is a trade-off between precision and recall. when a classifier attempts to predict one class, say class *a*, most of the time, it will achieve a high recall for *a* (most of the instances of that class will be identified). However, instances of other classes will most likely be incorrectly predicted as *a* in that process, resulting in a lower precision for *a*.

In addition of precision and recall, the F1 score is also commonly reported.

$$
\begin{align}
F1-score = \frac{precision \ \times \ recall}{recall + precision}
\end{align}
$$

Per-class accuracy is not without its own caveats. For instance, if there are a very few examples of one class, then test statistics for that class will be unreliable (i.e they have large variance) so it's not statistical sound to average quantities with different degrees of variance.

## Macro average metrics: 1-vs-all

When the instances are not uniformly distributed over the classes, it is useful to look at the performance of the classifier with respect to one class at a time before averaging the metrics.

We can compute the confusion matrix for the 1-vs-all case.
For example, we can think of the problem of 3 classes taks where one class is considered the positive and the combination of all the other class makes up the negative class.

## Log-loss
*log-loss* or logarithmic lossis used when the raw output of the classifier is a numeric probability instead of a class label of 0 or 1.  The probability essentially serves as a gauge of confidence. 

$$
\begin{align}
log-loss = -\frac{1}{N} \sum_i \hat{y}_i log p_i + (1- \hat{y}_i) log(1- p_i)
\end{align}
$$

where $p_i$ =is the probability that the $i$-th data belongs t oclass 1, as judged by the classifier, $y_i$ is the true label and is either 0 or 1.

## AUC
AUC stands for Area under the curve. Here the curve is the Receiver operating Characteristic Curve (ROC). 
The ROC curve shows the sensitivity of the classifier by plotting the rate of true positives to the rate of false positives. In other words, it shows how many correct positive classifications can  be gained as you allow for more and more false positives.

The ROC Curve provides nuanced details about the behaviour of the classifier, but it's hard t oquickly compare many ROC curves to each other.
The AUC is one way to summarize the ROC curve into a single number, so that it can be compared easily and automatically. A good ROC curve has a lot of space under it (because the true positive rate shoots up to 100% quickly). A bad  ROC curve covers very little area .So high AUC is good, and low AUC is not so good.

Ref. Tom Fawcett's 2006 Pattern Recognition Letters paper on An Introduction to ROC Analysis. 