---
layout: post
title: Area Under the Curve
tags: stats/maths
category: ml
summary: ""
img_post: 20161218-auroc.jpg
github-link: na
---



<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>



## 1. Confusion Matrix

Here a typical Confusion matrix after a test with a binary classification problem.

<table width="100rem" height="220rem">
  <tr style="border: 2px solid #000000; ">
    <th  style="border: 2px solid #000000; "></th>
    <th style="align: center;border: 2px solid #000000; " colspan="3">Ground Truth</th>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <td style="border: 2px solid #000000; "></td>
    <td style="border: 2px solid #000000; "></td>
    <td style="border: 2px solid #000000; ">Positive</td>
    <td style="border: 2px solid #000000; ">Negative</td>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <th style="transform: rotate(90deg);" rowspan="2"> Predicted</th>
    <td style="border: 2px solid #000000; ">Positive</td>
    <td style="border: 2px solid #000000; ">TP</td>
    <td style="border: 2px solid #000000; ">FP</td>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <td style="border: 2px solid #000000; ">Negative</td>
    <td style="border: 2px solid #000000; ">FN</td>
    <td style="border: 2px solid #000000; ">TN</td>
  </tr>
</table>

The distribution of the classes might look something like this:

<div>
<div id="graph" style="width:50%; height:500px; float:left"> </div>
<div id="graph2" style="width:50%; height:500px; float:right"> </div>
</div>


<script>
    var n = 500;
    var sigma_pos = 0.15;
    var sigma_neg = 0.1;
    var mean_pos = 0.7;
    var mean_neg = 0.3;
    var x = [];
    var y_pos = [];
    var y_neg = [];
 
    for (var i = 0; i < n; i++) {
      var x0 = i/n;
      x[i] = x0;
      y_pos[i] = 1/Math.sqrt(2*Math.PI * sigma_pos*sigma_pos) * Math.exp(-(x0 - mean_pos)*(x0-mean_pos) / (2 * sigma_pos*sigma_pos));
      y_neg[i] = 1/Math.sqrt(2*Math.PI * sigma_neg*sigma_neg) * Math.exp(-(x0 - mean_neg)*(x0-mean_neg) / (2 * sigma_neg*sigma_neg));
      }
    var layout = {
                  title: 'Histogram of classes - test1',
                  xaxis: {
                          title: 'Probability/score',
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
    var trace1 = {x: x, y: y_pos, line: {simplify: false}, name: "Positive" }
    var trace2 = {x: x, y: y_neg, line: {simplify: false}, name: "Negative" }
    var trace3 = {x: [0.47, 0.47], y: [0, 4], line: {simplify: false}, name: "threshold" }
    Plotly.plot('graph', [trace1, trace2, trace3 ], layout);
</script>

<script>
    var n = 500;
    var sigma_pos = 0.15;
    var sigma_neg = 0.1;
    var mean_pos = 0.7;
    var mean_neg = 0.3;
    var x = [];
    var y_pos = [];
    var y_neg = [];
    var thresh = 0.47;
    var x_fn = [];
    var y_fn = [];
    var x_fp = [];
    var y_fp = [];

    for (var i = 0; i < n; i++) {
      var x0 = i/n;
      x[i] = x0;
      y_pos[i] = 1/Math.sqrt(2*Math.PI * sigma_pos*sigma_pos) * Math.exp(-(x0 - mean_pos)*(x0-mean_pos) / (2 * sigma_pos*sigma_pos));
      y_neg[i] = 1/Math.sqrt(2*Math.PI * sigma_neg*sigma_neg) * Math.exp(-(x0 - mean_neg)*(x0-mean_neg) / (2 * sigma_neg*sigma_neg));
      
      if (i < thresh){
        x_fn.push(x0);
        y_fn.push(y_neg[i])
      }
      if (i > thresh){
        x_fp.push(x0);
        y_fp.push(y_pos[i])
      }


      }


    var layout = {
                  title: 'Histogram of classes - test1',
                  xaxis: {
                          title: 'Probability/score',
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
    var trace1 = {x: x, y: y_pos, line: {simplify: false}, name: "Positive", fill: 'tozeroy'}

    var trace2 = {x: x, y: y_neg, line: {simplify: false}, name: "Negative", fill: 'tonexty', fillcolor:"#0000FF"}
    var trace3 = {x: x_fp, y: y_fp, line: {simplify: false}, name: "False Positive", fill: 'tonexty', fillcolor:"#00FF00"}
    var trace4 = {x: x_fn, y: y_fn, line: {simplify: false}, name: "False Negative", fill: 'tonexty', fillcolor:"#FF0000"}

    Plotly.plot('graph2', [trace1, trace2, trace3, trace4 ], layout);
</script>

<br>
The *x-axis* is the predicted probability and the *y-axis* is the count or frequency of observation. 
For example, at prob=0.2, the number of observations is 2. The green line shows the threshold used to separate the positive and negative labels for this test.


## 2. Definitions

#### 2.1 Recall/Sensitivity

Recall/Sensitivity is a measure of the probability that the estimate is Positive given all the samples whose True label is Positive. In short, how many positive samples have been ientified as positive. It is also called the **True Positive Rate**.

$$sensitivity = recall = \frac{TP}{TP + FN}$$

<table width="100rem" height="220rem">
  <tr style="border: 2px solid #000000; ">
    <th  style="border: 2px solid #000000; "></th>
    <th style="align: center;border: 2px solid #000000; " colspan="3">Ground Truth</th>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <td style="border: 2px solid #000000; "></td>
    <td style="border: 2px solid #000000; "></td>
    <td style="border: 2px solid #000000;" >Positive</td>
    <td style="border: 2px solid #000000; ">Negative</td>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <th style="transform: rotate(90deg);" rowspan="2"> Predicted</th>
    <td style="border: 2px solid #000000; ">Positive</td>
    <td style="border: 2px solid #000000; background: repeating-linear-gradient(-55deg,#333,#333 10px,#444 10px,#444 20px);"><b>TP</b></td>
    <td style="border: 2px solid #000000;">FP</td>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <td style="border: 2px solid #000000; ">Negative</td>
    <td style="border: 2px solid #000000; background: repeating-linear-gradient(-55deg,#999,#999 10px,#888 10px,#888 20px);">FN</td>
    <td style="border: 2px solid #000000; ">TN</td>
  </tr>
</table>



#### 2.2 Precision

Precision is defined as the number of **TP** over the number of all samples predicted to be positive:

$$ P = \frac{TP}{TP + FP} $$

<table width="100rem" height="220rem">
  <tr style="border: 2px solid #000000; ">
    <th  style="border: 2px solid #000000; "></th>
    <th style="align: center;border: 2px solid #000000; " colspan="3">Ground Truth</th>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <td style="border: 2px solid #000000; "></td>
    <td style="border: 2px solid #000000; "></td>
    <td style="border: 2px solid #000000;" >Positive</td>
    <td style="border: 2px solid #000000; ">Negative</td>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <th style="transform: rotate(90deg);" rowspan="2"> Predicted</th>
    <td style="border: 2px solid #000000; ">Positive</td>
    <td style="border: 2px solid #000000; background: repeating-linear-gradient(-55deg,#333,#333 10px,#444 10px,#444 20px);"><b>TP</b></td>
    <td style="border: 2px solid #000000;  background: repeating-linear-gradient(-55deg,#999,#999 10px,#888 10px,#888 20px);">FP</td>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <td style="border: 2px solid #000000; ">Negative</td>
    <td style="border: 2px solid #000000;">FN</td>
    <td style="border: 2px solid #000000; ">TN</td>
  </tr>
</table>


#### 2.3 Specificity:

Specificity, also known as **True Negative Rate**, **is not precision**.

$$ S = \frac{TN}{TN + FP} $$

<table width="100rem" height="220rem">
  <tr style="border: 2px solid #000000; ">
    <th  style="border: 2px solid #000000; "></th>
    <th style="align: center;border: 2px solid #000000; " colspan="3">Ground Truth</th>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <td style="border: 2px solid #000000; "></td>
    <td style="border: 2px solid #000000; "></td>
    <td style="border: 2px solid #000000;" >Positive</td>
    <td style="border: 2px solid #000000; ">Negative</td>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <th style="transform: rotate(90deg);" rowspan="2"> Predicted</th>
    <td style="border: 2px solid #000000; ">Positive</td>
    <td style="border: 2px solid #000000;"><b>TP</b></td>
    <td style="border: 2px solid #000000;  background: repeating-linear-gradient(-55deg,#999,#999 10px,#888 10px,#888 20px);">FP</td>
  </tr>
  <tr style="border: 2px solid #000000; ">
    <td style="border: 2px solid #000000; ">Negative</td>
    <td style="border: 2px solid #000000;">FN</td>
    <td style="border: 2px solid #000000; background: repeating-linear-gradient(-55deg,#333,#333 10px,#444 10px,#444 20px);">TN</td>
  </tr>
</table>

## 3. Metrics to evaluate classification test/model:


#### 3.1 F score

The *F-score* is defined as:

$$F_{\beta} = \frac{(1 + \beta^2) (P + R)}{\beta^2 P + R}$$

where $$P$$ is the Precision, $$R$$ is the Recall and $\beta$ takes the common values $\beta=0.5, 1, 2$.



#### 3.2 Precision/Recall curve

The Precision/Recall curve shows the Recall (in *y*-axis) and Precision (*x*-axis) for various threshold values / decision boundary.
The ideal point is *Precision=1* and *Recall=1*.

<div id="graph_pr" style="width:500px; height:500px;"> </div>

<script>
    var x = [0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0];
    var y = [1, 0.9, 0.8, 0.83, 0.76, 0.6, 0.72, 0.3, 0.25, 0.1, 0];
    
    var layout = {
                  title: 'PR Curve',
                  xaxis: {
                          title: 'Precision',
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                    }
                        },
                  yaxis: {
                          title: 'Recall',
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                      }
                         }
                };
    var trace1 = {x: x, y: y, line: {simplify: false}, name:"" }
    Plotly.plot('graph_pr', [trace1], layout);
</script>




#### 3.3 ROC (Receiver Operating Characteristic) Metric

ROC is a visual way of inspecting the performance of a binary classification algorithm. ROC curve is the plot of TPR versus FPR (1-specificity) for different threshold values.

$$
\begin{align}
1 - Spec = 1 - \frac{TN}{TN + FP} = \frac{TN + FP - TN}{TN + FP} = \frac{FP}{TN + FP}
\end{align}
$$

Below is a typical ROC curve for 2 tests where each data point is the sesitivity and specificity when uisng a particular threshold.

<div id="graph_roc" style="width:500px; height:500px;"> </div>


<script>
    var x = [0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0];
    var y = [0, 0.6, 0.7, 0.78, 0.85, 0.92, 0.94, 0.96, 0.98, 1];
    var yy = [0, 0.1, 0.2, 0.3, 0.43, 0.6, 0.76, 0.83, 0.9, 1];

    var layout = {
                  title: 'ROC Curve',
                  xaxis: {
                          title: '(1-specificity)=FP rate',
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                    }
                        },
                  yaxis: {
                          title: 'sensitivity = TP rate',
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                      }
                         }
                };
    var trace1 = {x: x, y: y, line: {simplify: false}, name: "Positive" }
    var trace2 = {x: x, y: yy, line: {simplify: false}, name: "Positive" }
    Plotly.plot('graph_roc', [trace1, trace2], layout);
</script>


Let's look at several ROC curve shape and what it tells us about the performance of the classifier. The **baseline** indicates that the classifier is not better than making random guess. The **Underperforming** model would have data points below the baseline. A **good** classifier will points above the baseline.



<div>
<div id="roc_baseperfect" style="width:50%; height:500px; float:left"> </div>
<div id="roc_goodpoor" style="width:50%; height:500px; float:right"> </div>
</div>

<script>
    var x = [0, 0.2, 0.4, 0.6, 0.8, 1];
    var y = [0, 0.2, 0.4, 0.6, 0.8, 1];
    var yy = [1, 1, 1, 1, 1, 1];
 
    var layout = {
                  title: 'ROC Curve Baseline - test1',
                  xaxis: {
                          title: 'FP rate (1-spec)',
                          range: [-0.1, 1.1],
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                    }
                        },
                  yaxis: {
                          title: 'TP rate',
                          range: [-0.05, 1.1],
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                      }
                         }
                };
    var trace1 = {x: x, y: y, type:'line', name: "Baseline", marker: {size: 10}, line: { dash: 'dashdot', width: 4}}
    var trace2 = {x: x, y: yy, type:'line', name: "Perfect", marker: {size: 10}, line: { dash: 'dot', width: 4}}
    Plotly.plot('roc_baseperfect', [trace1, trace2 ], layout);
</script>

<script>
    var x = [0, 0.2, 0.4, 0.6, 0.8, 1];
    var y = [0, 0.2, 0.4, 0.6, 0.8, 1];
    var xx = [0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1];
    var yy = [0, 0.05, 0.12, 0.18, 0.28, 0.32, 0.33, 0.41, 0.58, 0.71, 1];
    var yyy = [0, 0.6, 0.8, 0.85, 0.88, 0.92, 0.93, 0.95, 0.97, 0.98, 1];
 
    var layout = {
                  title: 'ROC Curve Baseline - test1',
                  xaxis: {
                          title: 'FP rate (1-spec)',
                          range: [-0.1, 1.1],
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                    }
                        },
                  yaxis: {
                          title: 'TP rate',
                          range: [-0.05, 1.1],
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                      }
                         }
                };
    var trace1 = {x: x, y: y, type:'line', name: "Baseline", marker: {size: 10}, line: { dash: 'dashdot', width: 4}}
    var trace2 = {x: xx, y: yy, type:'line', name: "Poor", marker: {size: 10}, line: { dash: 'dot', width: 4}}
    var trace3 = {x: xx, y: yyy, type:'line', name: "Good", marker: {size: 10}, line: { dash: 'dot', width: 4}}
    Plotly.plot('roc_goodpoor', [trace1, trace2, trace3 ], layout);
</script>


#### 3.3 AUROC: Area under ROC

**AUC** is useful when comparing different model/tests, as we can select the model with the highest AUC value. 

$$\begin{align} 
AUC &= 0  \rightarrow \text{  bad } \\
AUC &< 0.5  \rightarrow  \text{worst than random guessing } \\
AUC &= 0.5-0.6  \rightarrow  \text{same as random guessing } \\
AUC &= 0.6-0.7  \rightarrow  \text{Poor } \\
AUC &= 0.7-0.8   \rightarrow \text{Fair } \\
AUC &= 0.8-0.9  \rightarrow  \text{Good } \\
AUC &= 0.9-1.0  \rightarrow  \text{Excellent }
\end{align}$$

AUROC can be calculated using the trapezoid rule: adding the area from the rapezoids:

<div id="trapezoid" style="width:50%; height:500px"> </div>
<script>
    var xx = [0, 0.4, 0.6, 1];
    var yyy = [0, 0.88, 0.93, 1];
    var x0 = [0.4, 0.4]
    var y0 = [0, 0.88]
    var x1 = [0.6, 0.6]
    var y1 = [0, 0.93]
 
    var layout = {
                  title: 'ROC Curve Baseline - test1',
                  xaxis: {
                          title: 'FP rate (1-spec)',
                          range: [-0.1, 1.1],
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                    }
                        },
                  yaxis: {
                          title: 'TP rate',
                          range: [-0.05, 1.1],
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                      }
                         }
                };
    var trace1 = {x: x0, y: y0, type:'line'}
    var trace2 = {x: x1, y: y1, type:'line'}
    var trace3 = {x: xx, y: yyy, type:'line', name: "Poor", marker: {size: 10}, line: { dash: 'dot', width: 4}}
    Plotly.plot('trapezoid', [trace1, trace2, trace3 ], layout);
</script>


## 4. Some Notes

### 4.1 Imbalanced dataset

Most problems do not deal with balanced class and ROC becomes less powerful when using imbalanced datasets.

The distribution of the classes might look something like this:

<div id="graph_imbalanced" style="width:50%; height:500px; float:left"> </div>


<script>
    var n = 500;
    var sigma_pos = 0.15;
    var sigma_neg = 0.1;
    var mean_pos = 0.7;
    var mean_neg = 0.3;
    var x = [];
    var y_pos = [];
    var y_neg = [];
 
    for (var i = 0; i < n; i++) {
      var x0 = i/n;
      x[i] = x0;
      y_pos[i] = 1/Math.sqrt(2*Math.PI * sigma_pos*sigma_pos) * Math.exp(-(x0 - mean_pos)*(x0-mean_pos) / (2 * sigma_pos*sigma_pos));
      y_neg[i] = 1/Math.sqrt(1000*Math.PI * sigma_neg*sigma_neg) * Math.exp(-(x0 - mean_neg)*(x0-mean_neg) / (2 * sigma_neg*sigma_neg));
      }
    var layout = {
                  title: 'Histogram of classes - test1',
                  xaxis: {
                          title: 'Probability/score',
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
    var trace1 = {x: x, y: y_pos, line: {simplify: false}, name: "Positive" }
    var trace2 = {x: x, y: y_neg, line: {simplify: false}, name: "Negative" }
    var trace3 = {x: [0.47, 0.47], y: [0, 4], line: {simplify: false}, name: "threshold" }
    Plotly.plot('graph_imbalanced', [trace1, trace2, trace3 ], layout);
</script>

One effective approach to avoid potential issues wit himbalanced datasets is using the **early retrival area**, where the model performance can be analyze with fewer false positive or small FPR.

<div id="early" style="width:50%; height:500px; float:right"> </div>

<script>
    var x = [0, 0.2, 0.4, 0.6, 0.8, 1];
    var y = [0, 0.2, 0.4, 0.6, 0.8, 1];
    var xx = [0, 0.2, 1];
    var yy = [0, 0.6, 1];
    var xxx = [0, 0.5, 1];
    var yyy = [0, 0.8, 1];
 
    var layout = {
                  title: 'ROC Curve Baseline - test1',
                  xaxis: {
                          title: 'FP rate (1-spec)',
                          range: [-0.1, 1.1],
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                    }
                        },
                  yaxis: {
                          title: 'TP rate',
                          range: [-0.05, 1.1],
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                      }
                         }
                };
    var trace1 = {x: x, y: y, type:'line', name: "Baseline", marker: {size: 10}, line: { dash: 'dashdot', width: 4}}
    var trace2 = {x: xx, y: yy, type:'line', name: "Good", marker: {size: 10}, line: { dash: 'dot', width: 4}}
    var trace3 = {x: xxx, y: yyy, type:'line', name: "Good", marker: {size: 10}, line: { dash: 'dot', width: 4}}
    Plotly.plot('early', [trace1, trace2, trace3 ], layout);
</script>


#### 4.2 Probability calibration

ROC and AUROC are insensitive to whether the predicted probabilities are properly calibrated to actually represent probabilities of class membership.


#### 4.3 Multi-class problems

ROC curves can be extended to problems with 3 or more classes.

#### 4.4 Choosing Threshold

Choosing a classification threshold is a business decision: minimize FPR or maximize TPR.


## 5. Precision/Recall Curve

PR is more appropriate if the data class distribution is highly skewed class imbalance.

<div id="pr_curve" style="width:50%; height:500px"> </div>

<script>
    var xx = [0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7,  0.95, 1];
    var yy = [1, 0.99, 0.98, 0.95, 0.94, 0.93, 0.84, 0.8, 0.5, 0];
 
    var layout = {
                  title: 'ROC Curve Baseline - test1',
                  xaxis: {
                          title: 'Recall',
                          range: [-0.1, 1.1],
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                    }
                        },
                  yaxis: {
                          title: 'Precision',
                          range: [-0.05, 1.1],
                          titlefont: {
                                      family: 'Courier New, monospace',
                                      size: 18,
                                      color: '#7f7f7f'
                                      }
                         }
                };
    var trace1 = {x: xx, y: yy, type:'line', name: "Baseline", marker: {size: 10}, line: { dash: 'dashdot', width: 4}}
    Plotly.plot('pr_curve', [trace1], layout);
</script>


