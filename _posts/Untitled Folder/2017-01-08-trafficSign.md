---
layout: post
title: A Deep Neural Network Classifier of Traffic Signs with 99.0% accuracy
comments: true
date: 2017-01-08
tags: convolutional network convnet DeepNetwork
category: ml
img_post: TrafficSign-SDCND.png
github-link: 
---

A Deep Neural Network Classifier of Traffic Signs with 99.0 % accuracy

{% highlight python %}
def foo():
  print('foo')
end
{% endhighlight %}


The Traffic Sign Classification is the 2nd project of the self Driving Car Nanodegree.
The goal is to implement a Neural Network that predicts, with good accuracy (hopefully better than Human performance), the label of German Traffic signs. It was an opportunity to implement a Deep Neural Network from scratch, and on a topic extremely relevant for Self Driving Cars.
Data visualization
The dataset (available here) includes the training set (about 39k images) and the test set (12k images) stored in separate pickle files. All the images are scaled to (32x32)px with 3 color channels.
There are 43 different labels. Here are a few examples of labels name:
0: speed limit (20km/h)
1: speed limit (30km/h)
10: No passing for vehicles over 3.5 metric tons
22: Bumpy Road
41: End of No Passing
42: End of no passing by vehicles over 3.5 metric tons
20 images drawn randomly from the training set. The red number is the associated label.A image-to-image comparison show variation in illumination (brightness/contrast), filling factor (the sign size versus the image size), position of the sign in the image, background sceneries/colors. Also, in the same image, there are local variations in illumination, background color and pattern uniformity.
Data Exploration
Now, let’s see how the label data is distributed.
Class Frequency in original training datasetThe class distribution in the train set is not uniform: the label with the highest frequency (2250 counts for label=2) has about 10 times more images than the label with the lowest count (210 counts for label=0).
What about the pixel intensity distribution?
Below are the histogram of 5 images from the same class (label=38): the histogram are clearly different although the images belong to the same class.
Red channel histogram of 5 images from the class label 38.

Image pre-processing
The pre-processing includes the following steps:
1. Shuffle the training dataset. When you first load the pickle file and print the label of the first 10 images for instance, one will notice that they all belong to the same class…so we need to mix things up!!
2. Split the data between a training set and a validation set in the ratio ~3:1
3. Data augmentation: the data augmentation is aimed at increasing the size of the training set, and at adjusting the class distribution. Because the test set might have a different class distribution than the training set, we want the model to be trained on a uniform set of data where all classes are represented equally. When trained on a skewed dataset, the model is likely to become bias towards the class with the highest count. I decided to perform data augmentation where all the labels have same count: 4000 images per class. That gives a total of 172k images, i.e a ~6X augmentation.
4. Reduction of color channels: RGB to Grayscale
I chose to feed the ConvNet with grayscale images. In most cases, the grayscale images showed better contrast than the RGB data. It also reduces the number of parameters since we now use images with 1 color channel as input.
5. Image pixel intensity scaling: X_s = (X/255) -0.5
(where X is the original image). The pixel intensities are now scaled from the range [0, 255] down to [-0.5, 0.5]. This normalization, unlike the standardization or z-scaling (X_z = (X-mu)/sigma ), preserves the shape of the histogram.
Optionally, a histogram equalizer is applied on the grayscale images. It enables to improve the contrast in the image by broadening the distribution of the histogram. I played with it a bit when I was developing the model, but did not use it in the final model because it has some drawbacks (will comment on that later).
Data augmentation
I randomly selected images from the training set, and applied different affine transformations. For those transformations, I followed Sermanet and Lecun’s approach in “Traffic Sign Recognition with Multi-Scale Convolutional Networks”: The extra data (what they call in the paper the “jittered dataset”) are images that are randomly perturbated. The parameter of each transformation is randomly drawn from a uniform distribution with values confined to a pre-determined range:
rotation in the range [-10, 10] deg — This might look like a narrow range, but the meaning of some traffic signs (like the signs with arrows) can change if the angle of rotation is too large.
Translation along the horizontal/vertical axis: range [-3, +3] px
scaling factor along the horizontal and vertical axis: [0.8, 1.2]

A few examples of transformed images.A larger training set makes the model more robust.One thing to notice though, and which might be detrimental to the performance of our model is the black edges/boundaries that are generated after translating and/or rotating the original images.
Edges filled with black pixels after the translation and rotation of the original image.Independently of the data augmentation scheme that is chosen, it is important to perform the data augmentation after the training/validation split. We don’t want our validation set to be contaminated with synthetic images with artefacts such as the black edges/boundaries from the affine transformation.


The model
The CNN architecture has 2 Convnet blocks and 1 fully connected block (dashed line boxes):
Model architecture

Convolutional blocks: each block are made of 2 convolutional operations back to back, and ends with a max pooling operation. 
The only difference between the 2 blocks of convolutional layers is the number of filters used, which increases with the Network depth
For example, for the first convolution operation we have 100 kernels of size (3x3): the input is (32x32x1) and the output is (30x30x100) feature maps.

MaxPooling with (2x2) kernels and stride of 2. The Max pooling decreases the size of the output by 2.
Block of fully connected layers : has 2 hidden layers that feed the 43 units output layer.
Then, the logits (output values) are converted into a probability distribution by using the softmax function. The softmax computes the probability that the image X belongs to class i: P( y=i|X) with i=0,…,42 (more accurately it is P(y=i|X, W)
 The non-linear function used is ReLU  (it prevents gradient vanishing in particular for Deep Networks).
Optimizer: I used AdamOptimizer with the default values. I did not try other optimizers.

For the first iteration, when the weights are still in random state (i.e not yet updated), the loss is about 3.7. That is consistent with the expected maximum likelihood -ln(1/43)=3.76, if we assume equal probability for all classes.
Hyper-parameters
Weights Initialization: sigma=0.05 — One of the hyper-parameters that affects the most the performance of the model is sigma, the standard deviation of the weight initializer (a truncated Gaussian with mean 0). If it is too low or too high, the model will be barely learning. In some cases, the loss would remain constant at ~3.7, even after 50 EPOCHS. In other cases, the loss would go down but at a very slow pace. Depending on the normalization method used, the “optimum” sigma value is likely to be different.
learning rate: 0.001. The goal here is to find the right balance between a learning rate too high when the model does not learn, versus a learning rate too low when the convergence is desperately  slow. A value of 0.001 is a good start for several 10s of EPOCHS.
After 30–40 EPOCHS, the loss starts to fluctuate around a constant value. Form there, if the learning rate is decreased, it helps get the system to lower loss values, and the accuracy is enhanced.
Dropout : 0.5. The keep_rate was kept constant during training. Dropout helped prevent overfitting. I also tried to lower the keep_rate (< 0.5), but the learning was at best slow. 
Batch size is 200
EPOCHS: 60

I experimented with L2-regularization but I found it to be less efficient than Dropout. Combining Dropout and L2-regularization did not show any improvement. In fact, L2-regularization slowed down learning.
Results
The model achieves an accuracy on the test set of 99.43%. The training accuracy is 99.92% and the validation accuracy: 99.29%. 
The model performs better than Humans on this task (98.81% according to Sermanet et al.)
Predicted and True labels for Traffic signs from the test setNext
There are quite a few items that I plan to work on to further improve the model.
1. Lower the data augmentation with increasing EPOCH. The data augmentation with rotation / translation adds artefacts (black edges/borders) that are not present in the original images. Once the model has learned to recognize the most important characteristics of each traffic sign labels, it might be better to progressively reduce the data augmentation. Then, the frequency of images with artefacts that the model see is lower.
2. Use YUV image format as input rather than grayscale like it is done in this paper [here]. The Y channel is the the brightness component and the UV channels are the color components.
3. Add grayscale as a 4th channel to the YUV input images.
I experimented with RGB input versus (RGB+Grayscale) input and found that the accuracy of the latter to be about 0.5 to 1pt better (that was with another model though!).
5. The last item is more exploratory. There are some misclassifications which can have dramatic consequences: for example if the model “confuses” a stop with a speed limit sign.
The model could be changed to include 2 output layers: 1 output layer with the 43 labels and a new output layer with 8 units corresponding to the category of the traffic sign (see here for US Traffic signs categories). The model would work very similarly to that proposed by Ian Goodfellow et al. in “Multi-digit Number Recognition from Street View Imagery using Deep Convolutional Neural Networks”. First the category of the sign is predicted, then all the units in the label output layer that do not belong to that category are turned off, allowing only the relevant units to contribute to the backprop.
Conclusion
Overall, that was a very interesting project. Because we are asked by the SDCND Team to develop our own model, it was an opportunity to experiment with different architectures, to learn more about image processing and Deep Network optimization schemes. Also, it was a good exercise to read through several of some of the papers detailing various implementations of the traffic sign classifier.
Finally, thank you at Sahil Juneja (my mentor for the SDCND program) for the very fruitful discussions.
Check out my repo for more on the Self-driving Car Nanodegree.



The jittered dataset is a transformed version of the original dataset. Samples are randomly perturbated in position, scale and in rotation.
Jittered dataset yields more robust learning to potential deformation in test set. Other possible deformation includes other affine transformation brightness, contrast, blur.


State of the art: grayscale 99.17% | Human 98.81% and Color: 98.97%


A. Saxe : on random weights and unsupervised feature learning (A. M. Saxe, P. Wei Koh) Correlation between random weights performance and the pre-trained / fined tuned performance.

Data normalization sacling:
rescale [0, 255] to [0, 1] or [-1, 1] or [-0.5, 0.5]

Keep original distribution versus uniform class distribution. Split Train set and validation set before data augmentation because we want to know how our model works on real world datat so the validation set should not contain synthetic data.

Split 3:1 ratio
