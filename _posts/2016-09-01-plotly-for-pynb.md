---
layout: post
title: Interactive plots with plotly
tags: jupyter notebook
category: ml
summary: "Interactive data Visualization in jupyter notebook"
img_post: plotly_notebook.png
github-link: na
---


<script src="/js/plotly-latest.min.js"></script>

A recurrent limitation of visualization tools like [matplotlib](http://matplotlib.org) or [seaborn](https://stanford.edu/~mwaskom/software/seaborn/) is that they produce only **static** plots. Once the plot is displayed on a **jupyther notebook**, a **html page** or other supports, you cannot zoom in/out or read the values of the data points for example, without modifying the original code. One option would be be to develop your own code for interactive plots. Another option is to use an existing tool.

[**Plotly**](https://plot.ly/) is an online/offline analytics and data visualization tool that enables to generate interactive plots. In the offline mode, you can use the API, without having to register.
[**Plotly**](https://plot.ly/) has free libraries for **Python**, **R**, **MATLAB**, **Perl**, **Julia**, **Arduino**, and **REST**. There is also a Chrome app, available [here](https://chrome.google.com/webstore/detail/plotly/khajkhinhblhaenlhpodnblkmpdgclne/related?hl=en\).

I will *not* go into details about what is or is not possible with **plotly** (you can check their website for that!). Instead, I will show you how to start and a few examples of interactive plots to be embedded in <a href="nb">Jupyther Notebooks</a> and in <a href="html">html pages</a>.
I might update this post later on, after learning more about the really cool stuffs that **plotly** can deliver.

##1.<a name="nb"></a>  plotpy in a Jupyther notebook

#1.1. Installation

First thing first, the installation of the **Plotly**'s package. It is as easy as:

`$ sudo pip install plotly `
    

### 1.2. Interactive plots in Jupyther notebook

Let's dive in, and generate our first interactive plot in a jupyther notebook. 


####1.2.1. **Exemple 1:** scatter plot of a single dataset in a Jupyther notebook


<script src="https://gist.github.com/jmlb/454d7aa05d3c90a3373e8f17d895cfd3.js"></script>

<div id="show_plot_01" style="width:600px; height:450px;"></div>
<script>
    myplot = document.getElementById('show_plot_01');
    Plotly.plot( myplot, [{
    x: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
    y: [13, 13.2, 11.3, 12, 8.4, 9, 9.5, 6.5, 6.8, 4.3, 3.2, 2.8, 1.1, 0] }], {
    margin: { t:0 } } );
</script>


####1.2.2. **Exemple 2:** scatter plot of 2 datasets in a Jupyther notebook

<script src="https://gist.github.com/jmlb/ffa0aaa34c0ab66460fa13bef6aebeaf.js"></script>

<div id="show_plot_02" style="width:600px; height:450px;"></div>
<script>
    myplot = document.getElementById('show_plot_02');
    Plotly.plot( myplot, [{
    x: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
    y: [13, 13.2, 11.3, 12, 8.4, 9, 9.5, 6.5, 6.8, 4.3, 3.2, 2.8, 1.1, 0] },
	{
    x: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
  	y: [1, 1.3, 13, 16, 12.5, 11, 11.5, 8.5, 8.8, 6.3, 5.2, 4.8, 3.1, 2]} 
     ], {
    margin: { t:0 } } );
</script>



####1.2.3. **Exemple 3:** BAR plot in a Jupyther notebook

<script src="https://gist.github.com/jmlb/6cbea8fcc1687e74d8adaeab2ef0114e.js"></script>
<div id="show_plot_03" style="width:600px; height:450px;"></div>
<script>
    myplot = document.getElementById('show_plot_03');
    var data = [
  {
    x: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
    y: [13, 13.2, 11.3, 12, 8.4, 9, 9.5, 6.5, 6.8, 4.3, 3.2, 2.8, 1.1, 0] ,
    type: 'bar'
  }
];
Plotly.newPlot(myplot, data);
</script>


###2. <a name="html"></a> pyplot in **html** using javascript

* (1) In the header of the html page, we call **plotly javascript** file
<script src="https://gist.github.com/jmlb/9c747dd0310d3c99428ca489a5978313.js"></script>


* (2) we create a `<div>` in which to display the plot:
<script src="https://gist.github.com/jmlb/3879b1a440ea9a06435e97e3a502e0db.js"></script>

* (3) Finally, we generate the plot

<script src="https://gist.github.com/jmlb/4cb1e0a693315d9512c95491662a4399.js"></script>

The plot will be displayed in the `<div id='show_plot'></div>`
