---
layout: post
title: Maximum Likelihood Estimation
tags: stats/maths
category: ml
summary: ""
img_post: 2018-02-28_mle.png
github-link: na
---



<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


### 1. Definition

#### 1.1 Probability
We assume we know something about the underlying reality, the population  or the true model. We can then ask what is the chance/probability of observing a particular data/sample given a specific model/population

#### 1.2 Likelihood 
Likelihood is the reverse process. For likelihood, we ask given the observed data, what is the chance/likelihood that a given model is True. If we observe some data $$x$$, what are the best parameters $$(\mu, \sigma)$$ to describe that distribution.

{% tikz 20180228_01 %}
  \node (bx1) at (0,0) [draw,thick,minimum width=3cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=0] at (bx1.center) {Reality \\ Model \\ Population};
  \node (bx2) at (10,0) [draw,thick,minimum width=3cm,minimum height=5cm] {};
  \node[align=center,font=\large,rotate=0] at (bx2.center) {Data \\ Sample};
  \draw [->,>=stealth] (1.5,1) -- node[pos=.7,fill=white,inner sep=1pt]{probability}(8.5,1);
  \draw [->,>=stealth] (8.5,-1) -- node[pos=.7,fill=white,inner sep=1pt]{likelihood}(1.5,-1) ;
{% endtikz %}


#### 1.3 Maximum Likelihood

Maximum Likelihood Estimate (MLE) has 2 goals:

* Determine best model parameters (reality) that fit given the data
This is done by maximizing log-likelihood function to estimate parameters.

* Compare multiple models to determine the best fit to the data.

### 2. Probability Density Function

Probability Density Function (**PDF**) for discrete  and Probability Mass Funtion **(PMF**) for continuous.


<div>
<div id="graph1" style="width:50%; height:500px; float:left"> </div>
<div id="graph2" style="width:50%; height:500px; float:right"> </div>
</div>

<script>
    var x = [-5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8];
    var y = [10, 20, 30, 50, 80, 20, 50, 80, 70, 50, 25, 15, 10, 10]
    var layout = {
                  title: ' ',
                  xaxis: {
                          title: 'data',
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                    }
                        },
                  yaxis: {
                          title: 'Frequency',
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                      }
                         }
                };
    var trace1 = {x: x, y: y, type: "bar", name: "Model" }
    Plotly.plot('graph1', [trace1], layout);
</script>

<script>
    var n = 500;
    var sigma_pos = 0.15;
    var mean_pos = 0.7;
    var x = [];
    var y_pos = [];
 
    for (var i = 0; i < n; i++) {
      var x0 = i/n;
      x[i] = x0;
      y_pos[i] = 1/Math.sqrt(2*Math.PI * sigma_pos*sigma_pos) * Math.exp(-(x0 - mean_pos)*(x0-mean_pos) / (2 * sigma_pos*sigma_pos));
      }
    var layout = {
                  title: ' ',
                  xaxis: {
                          title: '',
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                    }
                        },
                  yaxis: {
                          title: '',
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                      }
                         }
                };
    var trace1 = {x: x, y: y_pos, line: {simplify: false}, name: "Model" }
    Plotly.plot('graph2', [trace1], layout);
</script>

$$ f(x_1, x_2, ....., x_n \| \theta) $$ is the probability of observing the data $$x_1, x_2, ...., x_n$$ given parameters $$\theta$$ (unknown). 

$$
\begin{align}
f(x_1, x_2, ...., x_n | \theta) = f(x_1|\theta) \times f(x_2|\theta) \times .... f(x_n|\theta) = \Pi _i f(x_i|\theta) 
\end{align}
$$


#### 2.2 Likelihood function $$\mathcal{L}$$

$$\mathcal{L}$$ is the likelihood of the parameters being some value given the observed data $$x_i$$.

$$
\begin{align}
&\mathcal{L} = f(x_1, x_2, ...., x_n|\theta) \\
& \mathcal{L}(\theta | x_1, x_2, ...x_n) = \Pi_i f(x_i | \theta)
\end{align}
$$ 


<div id="graph3" style="width:50%; height:500px;"> </div>

<script>
    var x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15];
    var y = [1, 2, 3, 3.8, 3.2, 3.1, 3.5, 4, 3.8, 3.7, 4, 4.1, 3.5, 1.6, 0.8, 0.6]
    var layout = {
                  title: ' ',
                  xaxis: {
                          title: 'theta',
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                    }
                        },
                  yaxis: {
                          title: 'L($\theta$)',
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                      }
                         }
                };
    var trace1 = {x: x, y: y, line: {simplify: false}, name: "Model" }
    Plotly.plot('graph3', [trace1], layout);
</script>

The higher $$\mathcal{L}$$, the more likely the $$\theta$$ value is. What we want is found $$\theta= \theta_{max}$$ is called the maximum likelihood estimator (MLE), the value of $$\theta$$ that maximize $$L(\theta)$$.


### 3.2 Log likelihood

Log is monotonic function that allows to simplify calculations. We define the log likelihood (natural log) $$\mathcal{l}$$:

$$
\begin{align}
\mathcal{l}(\theta| x_1, ...., x_n) = log( \mathcal{L}(\theta|x_1, ...., x_n) = \sum_i ln f(x_i|\theta)
\end{align}
$$