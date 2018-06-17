---
layout: post
title: Convolution as a matrix multiplication
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



## A convolution



## Convolution operation as a mtarix multiplication

Let's take the Sobel operator (for edge detection along x-axis)

The kernel is $$(3 \times 3)$$ matrix:
$$
\begin{align}
k = \left[ \begin{matrix} 1 & 0 & -1 \\ 2 & 0 & -2 \\ 1 & 0 & -1 \end{matrix} \right]
\end{align}
$$

and the input matrix is  $$(4 \times 4)$$ matrix:
$$
\begin{align}
input = \left[ \begin{matrix} 1 & 0 & -1 & 1 \\ 1 & 2 & -2 & 0 \\ 1 & 1 & -1 & 1 \\ 0 & 1 & 1 & 0 \end{matrix} \right]
\end{align}
$$

To calculate the convolution operation  $$input * k$$, we would slide the kernel on top of the input and perform the dot-product between the kernel and part of the input covered by the kernel window. In the regular convolution, we go from $$(4 \times 4)$$ to $$(2\times2)$$ This can be represented as a matrix multiplication performing the following steps:

* flattening out the matrix $$input$$ from $$(4 \times 4)$$ to a vector $$(16 \times 1)$$.

$$
\begin{align}
\hat{input} = \left[ \begin{matrix} 1 \\ 0\\ -1 \\1 \\ 1\\ 2 \\-2 \\ 0 \\ 1 \\1 \\ -1 \\ 1 \\ 0 \\ 1 \\1 \\ 0 \end{matrix} \right]
\end{align}
$$


* Reshape the kernel from $$(3 \times 3 )$$ into a $$(4\times16)$$:

$$
\begin{align}
\hat{k} = \left[ \begin{matrix} 1 & 0 & -1 & 0 & 2 & 0 & -2 & 0 & 1 & 0 & -1 & 0 & 0 & 0 & 0 & 0 \\
                          0 & 1 & 0 & -1 & 0 & 2 & 0 & -2 & 0 & 1 & 0 & -1 & 0 & 0 & 0 & 0 \\
                          0 & 0 & 0 & 0 & 1 & 0 & -1 & 0 & 2 & 0 & -2 & 0 & 1 & 0 & -1 & 0 \\
                          0 & 0 & 0 & 0 & 0 & 1 & 0 & -1 & 0 & 2 & 0 & -2 & 0 & 1 & 0 & -1 \\  
 \end{matrix} \right]
\end{align}
$$

The result is $$(4 \times 16) . (16 \times 1) = (4 \times 1)$$

$$
\begin{align}
\hat{k} . \hat{input} = \left[ \begin{matrix} 122 \\ 148 \\ 126 \\ 134 \end{matrix} \right]
\end{align}
$$


Then the result feature map os reshaped from $$(4 \times 1)$$ into $$(2 \times 2)$$

{% tikz 20180112_01 %}
  \node[text width=3cm] at (0.5,1.8) {kernel (3x3)};
  \node (00) at (0,0) [draw,thick,minimum width=0.5cm,minimum height=0.5cm] {};
  \node[align=center,font=\large,rotate=0] at (00.center) {$^1$};
  \node (01) at (0.5,0) [draw,thick,minimum width=0.5cm,minimum height=0.5cm] {};
  \node[align=center,font=\large,rotate=0] at (01.center) {0};
  \node (02) at (1,0) [draw,thick,minimum width=0.5cm,minimum height=0.5cm] {};
  \node[align=center,font=\large,rotate=0] at (02.center) {-1};  
  \node (10) at (0,0.5) [draw,thick,minimum width=0.5cm,minimum height=0.5cm] {};
  \node[align=center,font=\large,rotate=0] at (10.center) {2};
  \node (11) at (0.5,0.5) [draw,thick,minimum width=0.5cm,minimum height=0.5cm] {};
  \node[align=center,font=\large,rotate=0] at (11.center) {0};
  \node (12) at (1,0.5) [draw,thick,minimum width=0.5cm,minimum height=0.5cm] {};
  \node[align=center,font=\large,rotate=0] at (12.center) {-2};
  \node (20) at (0,1) [draw,thick,minimum width=0.5cm,minimum height=0.5cm] {};
  \node[align=center,font=\large,rotate=0] at (20.center) {1};
  \node (21) at (0.5,1) [draw,thick,minimum width=0.5cm,minimum height=0.5cm] {};
  \node[align=center,font=\large,rotate=0] at (21.center) {0};
  \node (22) at (1,1) [draw,thick,minimum width=0.5cm,minimum height=0.5cm] {};
  \node[align=center,font=\large,rotate=0] at (22.center) {-1};

{% endtikz %}


###  Cross-correlation versus convolution

In typical math text book, the convolution $$(n \times n) * (k \times k)$$ requires to mirror the kernel along the vertical and horizontal axis.
Here is an example

$$
\begin{align}
k = \left[ \begin{matrix} 1 & 0 & -1 \\ 2 & 0 & -2 \\ 1 & 0 & -1 \end{matrix} \right] = \left[  \begin{matrix} -1 & -2 & -1 \\ 0 & 0 & 0 \\ 1 & 2 & 1 \end{matrix}  \right]
\end{align}
$$

Then the convolution is operated as usual using the mirrored kernel.


Typically in DL, we use Cross correlation rather than convolution because in DL the kernel matrix is not mirrored.


## Transposed convolution

There are multiple names : **transpose convolution**, **deconvolution**, **fractionally strided convolution**. It is a method for upsampling the previous layer to a desired resolution/dimension.