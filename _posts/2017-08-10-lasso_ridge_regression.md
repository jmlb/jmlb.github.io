---
layout: post
title: Ridge and Lasso Regression
meta: "deep learning"
category: ml
author: "Jean-Marc Beaujour"
img_post: 20170810-ridge_lasso_regression.jpg
summary: 
github-link: ""
---

<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>



In this article, I will be discussing 2 regularization techniques that are commonly used to limit the amplitude of the coefficients in Regression problems: **Ridge regularization** and **Lasso regularization**. Those techniques are appropriate when the number of features is more than 2 and that some features are highy correlated. This is the case for example in [polynomial regression](https://en.m.wikipedia.org/wiki/Polynomial_regression): 

$$\hat{y}_i = w^1 x_i + w^2 x_i ^2$$ 

where for example $$x_i ^3$$ is highly correlated with $$x_i$$. 

Note that generally, regularizing the intercept is not a good idea.

## Constraint 

The goal of the regularizer is to have some constraints on the values of the coefficients/the weights.


The cost functions is defined as follow

* **Ordinary Least Square Regression: OLS**:

$$
J_0 = \frac{1}{2m} \sum_i (y - \hat{y}_i)^2
$$ 

where $$y$$ is the ground truth and $$\hat{y}$$ is the predicted output.

* **Ridge regularizer (L2 regularization)**:

$$
J_{L2} = \frac{1}{2m} \sum_i (y - \hat{y}_i)^2 + \lambda_2 \sum_k ||w^k||^2
$$ 

* **Lasso regularizer (L1 regularization)**:

$$
J_{L1} = \frac{1}{2m} \sum_i (y - \hat{y}_i)^2 + \lambda_1 \sum_k |w^k|
$$


where $$\|w^k\|$$ is the absolute value.


* **ElasticNet regularizer (combined L1 and L2 regularizer)**:

$$                                                      
J_{L} = \frac{1}{2m} \sum_i (y - \hat{y}_i)^2 + \lambda_2 \sum_k ||w^k||^2 + \lambda_1 \sum_k |w^k|
$$ 



### Surface of the constraint
Let's consider a quadratic model with 2 features and 2 weights:

$$
\hat{y}_i = b + w^{(1)} x_i + w^{(2)} x_i ^2
$$

Although, regularization is to be applied when the number of features $$> 2$$, in this example we set the number of features to 2 because it makes the visualization easier.

<table style="border: 1px solid #000; font-size:0.8rem;">
  <tr>
    <td>
      Lasso (Least Absolute Shrinkage and Selection Operator)
    </td>
    <td>
      Ridge
    </td>
  </tr>
  <tr>
    <td>
      Regularization term is \( \lambda \sum_k |w^{(k)}| = |w^{(1)}| + |w^{(2)}| \).
    </td>
    <td>
    Regularization term is \( \lambda \sum_k |w^{(k)}| = |w^{(1)}| + |w^{(2)}| \).
    </td>
  </tr>
  <tr>
<td>
\( min_w ||y_i -\hat{y}_i ||^2 \) subject to \( (|w^{(1)}| + |w^{(2)}| \leq c \)
</td><td>
\( min_w ||y_i -\hat{y}_i ||^2 \) subject to \( (||w^{(1)}||^2 + ||w^{(2)}||^2 \leq c^2 \)

</td></tr>
<tr>
<td>
{% tikz 20170810_03 %}[scale=2]

\draw[->,ultra thick] (-2,0)--(3,0) node[right]{$w_{1}$};
\draw[->,ultra thick] (0,-2)--(0,3) node[above]{$w_2$};
\draw[blue, dotted, ultra thick] (-1, 0)--(0, 1);
\draw[blue, dotted, ultra thick] (0, 1)--(1, 0);
\draw[blue, dotted, ultra thick] (1, 0)--(0, -1);
\draw[blue, dotted, ultra thick] (0, -1)--(-1, 0);
\node[text width=2cm] at (1.9,2.4) {$argmin_w$};
\node[text width=2cm, rotate=35] at (1.2,2.46) {+};
\draw[red, rotate=35] (2,1.3) ellipse (1.9cm and 0.75cm);
\draw[red, rotate=35] (2,1.3) ellipse (2cm and 0.8cm);
\draw[red, rotate=35] (2,1.3) ellipse (1.7cm and 0.7cm);
\draw[red, rotate=35] (2,1.3) ellipse (1.4cm and 0.6cm);
\draw[red, rotate=35] (2,1.3) ellipse (1.1cm and 0.5cm);
\draw[red, rotate=35] (2,1.3) ellipse (0.8cm and 0.4cm);
\draw[red, rotate=35] (2,1.3) ellipse (0.5cm and 0.3cm);
\draw[red, fill=red] (0,1) circle (.2ex) node[right]{$w_{L1}$};
\node[text width=2cm] at (1.6, 0.2) {+c};
\node[text width=2cm] at (-0.8, 0.2) {-c};
{% endtikz %}
</td><td align="center">

{% tikz 20170810_04 %}[scale=2]
\draw[->,ultra thick] (-2,0)--(3,0) node[right]{$w_{1}$};
\draw[->,ultra thick] (0,-2)--(0,3) node[above]{$w_2$};
\draw[blue, dotted, ultra thick] (0, 0) ellipse (1cm and 1cm);
\node[text width=2cm] at (1.9,2.4) {$argmin_w$};
\node[text width=2cm, rotate=35] at (1.2,2.46) {+};
\draw[red, rotate=35] (2,1.3) ellipse (1.9cm and 0.75cm);
\draw[red, rotate=35] (2,1.3) ellipse (2cm and 0.8cm);
\draw[red, rotate=35] (2,1.3) ellipse (1.7cm and 0.7cm);
\draw[red, rotate=35] (2,1.3) ellipse (1.4cm and 0.6cm);
\draw[red, rotate=35] (2,1.3) ellipse (1.1cm and 0.5cm);
\draw[red, rotate=35] (2,1.3) ellipse (0.8cm and 0.4cm);
\draw[red, rotate=35] (2,1.3) ellipse (0.5cm and 0.3cm);
\node[text width=2cm] at (1.6, 0.2) {+c};
\node[text width=2cm] at (-0.8, 0.2) {-c};
\draw[red, fill=red] (0.1,0.98) circle (.2ex) node[right]{$w_{L2}$};
{% endtikz %}

<br>
The center is the min of the regular residual square error without constraint.  The constraint 
</td></tr>
<tr>
<td> 
</td>
<td>The 2 constraints are : 
$$min_w \sum_i (y_i - \hat{y}_i)^2$$ and
$$\sum_k ||w^k||^2 \leq c^2$$

We use the method of Lagrange multiplier:

$$f(w, \lambda) = \sum_i (y_i - \hat{y}_i)^2 + \lambda \left( \sum_k ||w^k||^2 -C^2 \right) $$
and $$w^k = argmin_w f(W, \lambda)$$
Note that \( \lambda \times C^2 c= cst \) so really, we need to minimize:
\( f(w, \lambda) = \sum_i (y_i - \hat{y}_i)^2 + \lambda \left( \sum_k |w^k|^2 \right) \)

The basic idea in Lagrangian multiplier method is to augment the objective function with a weighted sum of the constraints in the optimization.
</td></tr>
<tr>
<td>The derivative of \(||w^k||\) is +1 for \(w_k > 0\) and -1 for \(w_k < 0\) but it is not not known at \(w^k = 0.\)</td>
<td>The derivative of \(||w^2||\) is known and can be computed</td></tr>

<tr><td>Coordinate descent: it consists in keeping all updating one parameter at the time, keeping the other parameter constant</td>
<td>Gradient descent. Note that this method is not exclusive to Lasso: can also be applied to \(J_0\)</td></tr>
</table>



## Lasso Regularization

### Derivation of the Coordinate Descent for Lasso

The goal of Coordinate Descent is to minimize the loss function, one weight at a time: fixing all coordinates and take the partial derivative with respect to $$w^{(j)}$$. Here we consider the case where the features are not normalize, the general case.


* Derivative of the LSO term

$$
\begin{align}
\frac{\partial J^{LSO}}{\partial w^{(j)}}= \frac{-1}{m} \sum_i (\hat{y}_i - y_i) x_i ^{(j)}
\end{align}
$$

$$x_i ^{(j)}$$ is the feature $$(j)$$ of the sample $$i$$. 

Now, let's separate the $$j^{th}$$ feature of each example. 

$$
\begin{align}
\frac{\partial J^{LSO}}{\partial w^{(j)}} &= \frac{-1}{m} \sum_i (\hat{y}_i - y_i) x_i ^{(j)} \\
&= \frac{-1}{m} \sum_i (\hat{y}_i - \sum_{k\neq j} x_i ^{(k)} w^{(k)} - w^{(j)} x_i ^{(j)}) x_i ^{(j)} \\
&= \frac{-1}{m} \underbrace{ \sum_i x_i ^{(j)} \left(\hat{y}_i - \sum_{k\neq j} x_i ^{(k)} w^{(k)}\right)}_{ \rho^{(j)} } + \frac{1}{m} w^{(j)} \underbrace{ \sum_i (x_i ^{(j)})^2 }_{z^{(j)}}
\end{align}
$$

* Sub-derivative of the L1 term

$$
L1 = \lambda \sum_j ||w^{j}||
$$

* The derivative of $$ \|w^{(j)}\|$$  when $$w^{(j)} \neq 0$$ is known:

$$
\frac{\partial L1}{\partial w^{j}}_{w^{j} \neq 0} = \lambda \ sign(w^{(j)}) = \lambda (\pm 1)
$$

* The derivative of $$ \|w^{(j)}\|$$  when $$w^{(j)}=0$$ is not known. Nevertheless the sub-gradient $$V$$ of $$\|w^{(j)}\|$$ at $$w^{(j)}=0$$ is usch that $$V \in [-1, 1]$$.

{% tikz 20170810_01 %}[scale=5]

\draw[->,ultra thick] (-1,0)--(1,0) node[right]{$x$};
\draw[->,ultra thick] (0,-0.1)--(0,1) node[above]{$y$};
\draw (-1,1)--(0,0);
\draw (0, 0)--(1, 1)node[above]{$||w||$};
\node[text width=4cm, red] at (-0.6,0.5) {grad = -1};
\node[text width=4cm, red] at (1,0.5) {grad = +1};
\node[text width=5cm] at (-0.1,-0.2) {$grad_0$ = unknown};
\draw[dotted, blue] (-1, -0.9)--(1, 0.9);
\draw[dotted, blue] (-1, -0.6)--(1, 0.6);
\draw[dotted, blue] (-1, -0.2)--(1, 0.2);
\draw[dotted, blue] (-1, -0.1)--(1, 0.1);
\draw[dotted, blue] (-1, 0.9)--(1, -0.9);
\draw[dotted, blue] (-1, 0.6)--(1, -0.6);
\draw[dotted, blue] (-1, 0.2)--(1, -0.2);
\draw[dotted, blue] (-1, 0.1)--(1, -0.1);
{% endtikz %}


Hence, the derivative of the 

$$
\partial ||w^{(j)}|| = \lambda \left\{
                \begin{array}{ll}
                  -\lambda \ \ when \ w^{(j)} < 0 \\
                  [-\lambda, +\lambda]  \ \ when \ w^{(j)}=0  \\
                  \lambda  \ \ when \ w^{(j)} > 0 
                \end{array}
              \right.
$$

Including the LSO term:

$$
\begin{align}
\partial L1 = z^{(j)} w^{(j)} - \rho ^{(j)} + \left\{
                \begin{array}{ll}
                  -\lambda \ \ when \ w^{(j)} < 0 \\
                  [-\lambda, +\lambda]  \ \ when \ w^{(j)}=0  \\
                  \lambda  \ \ when \ w^{(j)} > 0 
                \end{array}
              \right.
            = \left\{
                \begin{array}{ll}
                  z^{(j)} w^{(j)} - \rho^{(j)} - \lambda \ \ when \ w^{(j)} < 0 \\
                  - \rho^{(j)} + [-\lambda, +\lambda]  \ \ when \ w^{(j)}=0  \\
                   z^{(j)} w^{(j)} - \rho^{(j)} + \lambda  \ \ when \ w^{(j)} > 0 
                \end{array}
              \right.
\end{align}
$$

The optimum solution $$w^{(j)}$$ is obtained by minimizing \(L1\), i.e $$\partial L1 = 0$$.


* case1: $$w^{(j)}$$ < 0

$$
\begin{align}
w^{(j)} = \frac{\lambda + \rho^{(j)}}{z^{(j)}}
\end{align}
$$

and for $$w^{(j)} < 0 \rightarrow \rho^{(j)} < - \frac{\lambda}{2}$$


* case2: $$w^{(j)}=0$$

Minimize :
$$
\begin{align}
\rho^{(j)} + [-\lambda, +\lambda] = 0
\end{align}
$$

We want the range to contains 0 so that \(w^{(j)}=0\) is an optimum:

$$
\begin{align}
-\rho^{(j)} - \lambda &\leq 0  \rightarrow \rho^{(j)} \geq \lambda\\
-\rho^{(j)} + \lambda &\geq 0 \rightarrow \rho^{(j)} \leq \lambda\\
\end{align}
$$


* case3: $$w^{(j)}$$ > 0

$$
\begin{align}
w^{(j)} = \frac{\rho^{(j)} - \lambda/2}{z^{(j)}}
\end{align}
$$

and for $$w^{(j)} > 0 \rightarrow \rho^{(j)} > \frac{\lambda}{2}$$


In summary:

$$
\begin{align}
w^{(j)}  =  \left\{ 
            \begin{array}{ll}
                  \frac{\rho^{(j)} + \lambda /2}{z^{(j)}} \ \ if \ \rho^{(j)} < -\frac{\lambda}{2} \\
                  0  \ \ if \ \rho^{(j)} \in [-\lambda/2, \lambda/2]  \\
                  \frac{\rho^{(j)} + \lambda/2}{z^{(j)}} \ \ if \rho^{(j)} > \lambda/2 
            \end{array}
          \right.
\end{align}
$$


### Pseudo-code Coordinate descent for Lasso

1. precompute: $$z^{(j)}=\sum_i (x_i ^{(j)})^2$$
2.initialize $$w^{(j)}=0$$

3. while not converged
for j=0, 1, ...D

\rho_j=

set $$w^{(j)}$$:
$$
\begin{align}
w^{(j)}  = \left\{ 
\begin{array}{ll}
                  (\rho^{(j)} + \lambda/2) / z^{(j)} \ \ if \ \rho^{(j)} < -\frac{\lambda}{2} \\
                  0  \ \ if \ \rho^{(j)} \in [-\lambda/2, \lambda/2]  \\
                  (\rho^{(j)} + \lambda/2) / z^{(j)} \ \ if \rho^{(j)} > \lambda/2 
                \end{array}
              \right.
\end{align}
$$

Assess convergence:

MEasure the weight update step for all weihts and if one or more weight step is larger than the tolerance $$\epsilon$$, then run new loop

{% tikz 20170810_02 %}[scale=2]

\draw[->,ultra thick] (-3,0)--(3,0) node[right]{$w_{LSO}$};
\draw[->,ultra thick] (0,-3)--(0,3) node[above]{$w$};
\draw (-2,-2)--(2,2);
\draw[red, dotted, ultra thick] (-3, -2)--(-1, 0);
\draw[red, dotted, ultra thick] (-1, 0)--(1, 0);
\draw[red, dotted, ultra thick] (1, 0)--(3, 2);
\node[text width=2cm] at (2,2.3) {L1};
\node[text width=2cm] at (2.5,0.5) {LSO};
\node[text width=2cm] at (-1,0.2) {$-\lambda/2$};
\node[text width=2cm] at (1.2,-0.3) {$\lambda/2$};
{% endtikz %}


Lasso Regularization favors spasity of coefficients, and therefore shut off some features. This is useful when we need to simplify a complex model with a lot of parameters, and/or we want to get an interpretable model.

## Ridge Regularization

Let's start with the following model :
$$
\hat{y_i} = \sum_k x_i ^k w^k
$$k

The cost function is :
$$
J_{L2} = \frac{1}{2m} \sum_i (y - \hat{y}_i)^2 + \lambda_2 \sum_k ||w^k||^2
$$ 

This cost function is convex and differentiable, so we can use Gradient Descent

$$
\frac{\partial J_{L2}}{\partial w^j} = -\frac{1}{m} \sum_i (y - \hat{y}_i)x_i ^j + 2 \lambda_2 ||w^j||
$$ 

The coefficient update is:

$$
\begin{align}
w^j &= w^j - \alpha \frac{\partial J}{\partial w^j} \\
&= w^j - \alpha \left( -\frac{1}{m} \sum_i (y - \hat{y}_i)x_i ^j + 2 \lambda_2 ||w^j|| \right) \\
&= w^j(1 - 2 \alpha \lambda_2) - \alpha (-\frac{1}{m} \sum_i (y - \hat{y}_i)x_i ^j)
\end{align}$$

So, Ridge is equivalent to reducing the weight.



## Sub-gradients

To understand subgradient, let's look at the gradient of a convex function. 

If $$g$$ is convex and differentiable at $$x$$ then $$\partial g(a) = \left\{ \nabla_a g \right\}$$, i.e its gradient is 
its only sub-gradient ($$\partial g$$).

If $$g$$ is convex, it implies the following inequality:

$$
g(b) \geq g(a) + \nabla_a g (b - a)
$$

where $$\nabla_a g $$ is the gradient of $$g$$ at $$a$$ (the slope of the tangent).


{% tikz 20170810_06 %}[scale=1]
\draw[->] (-2.2,0) -- (2.2,0) node[right] {$x$};
\draw[->] (0,-0.2) -- (0,8.2) node[above] {$y$};
\draw[scale=1,domain=-2.2:2.2,smooth,variable=\x,blue] plot ({\x},{\x*\x + 2}) node[above]{g};
\draw[red, fill=red] (-2,6) circle (.2ex);
\draw[red, fill=red] (-1,3) circle (.2ex);
\draw[red, dotted] (-1, 3)--(0, 3) node[right]{$g(b)$};
\draw[red, dotted] (-2, 6)--(0, 6) node[right]{$g(a)$};
\draw[red, dotted] (-1, 3)--(-1, 0) node[right]{$b$};
\draw[red, dotted] (-2, 6)--(-2, 0) node[right]{$a$};
\draw[black] (-2.2, 6.84)--(0, -1.8) node[right]{$\nabla_a g$};
{% endtikz %}


This can be generalized to non-differentiable points. **Sub-gradients** $$\partial g$$ is a set of all the planes that lower 
bound a function. The sub-gradient $$\left( V \in \bar{\nabla}_x g \right)$$ if:

$$
g(b) \geq g(a) + V (b - a)
$$
