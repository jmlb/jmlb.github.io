---
layout: post
title: Visualize streaming data in real-time using Plotly
meta: "python"
category: coding
author: "Jean-Marc Beaujour"
summary: "Plotly enables to include dynamic plots in html page or python notebook using js API."
img_post: 20170821-plotly.png
github-link: na
---


<script src="/js/plotly-latest.min.js"></script>


One of the many powerful functionalities of <a href="https://plot.ly/">Plotly API</a> is *extendTraces()*. It enabes to plot real-time sensor data in your html page for example. This is particularly useful for IOT projects. Here is an <a href="https://plot.ly/javascript/plotlyjs-function-reference/#plotlyextendtraces"> example</a> from plotly website: the <i>x</i> axis is the timestamp. 

For one of my projects, I wanted to use that functionality to visualize data from  not just one but multiple sensors. In fact that is pretty straight forward if you want each sensor to have its own graph (Plotly shows the plots in a div object typically called "graph") or if you want to plot the sensors data all together on the same graph. Actually, the later would be a bad idea if the values output by the sensors are in different range: the visualization becomes useless. A third possibility is to use a single graph object, and dynamically write on that graph when a sensor is selected. Below, I propose 2 examples: read and plot the data in a unique graph object with or without clearing the graph off previous plots.  

For this demo, let's consider 2 sensors. In order to simplify things, the output of the sensors is chosen to be constant: sensor1 = 4 and sensor2 = 40. We then propose a solution with and without purging/refreshing the graph.

## **EXAMPLE 1: the sensors data is plotted without refreshing/clearing the graph**
<hr>
When the user click on the input box, the javascript function plot() is called, and generates the plot in the \<div\> with <i>id="graph"</i>.

{% highlight html linenos%}
 <html>
  <head>
   <script src="js/plotly-latest.min.js"></script>
   
<script>
    function rand() {
      return Math.random();
    }
// plot_cnt tells if function plot has been ran or not
// Onload plot_cnt = 0, 
plot_flag = 0
function plot(elt){
  if (plot_flag == 1){
    clearInterval(interval)
  }
  else{
    plot_flag = 1
  }
  var x_source = Number(elt.value);
  var time = new Date();
  var data = [{
    x: [time],
    y: [x_source],
    mode: 'lines',
    line: {color: '#80CAF6'}
  }]
  Plotly.plot('graph', data);
  //Make interval a global variable
  interval = setInterval(function() {
    var time = new Date();
    var update = {
      x:  [[time]],
      y: [[x_source]]
    }
    Plotly.extendTraces('graph', update, [0])
    //if(cnt != 0) clearInterval(interval);
      }, 100);
}
</script>

    
<body>

<div id="graph"></div>

<ul id="sensors">
  <li>sensor 1  <input class='value_telemetry' name="sensor1" onclick="plot(this)" value = "4"></li>
  <li>sensor 2 <input class='value_telemetry' name="sensor2" onclick="plot(this)" value = "40"></li>
</ul>       

  </body>

</html>
{% endhighlight %}


<script>
    function rand() {
      return Math.random();
    }
// plot_cnt tells if function plot has been ran or not
// Onload plot_cnt = 0, 
plot_flag = 0
function plot(elt){
  if (plot_flag == 1){
    clearInterval(interval)
  }
  else{
    plot_flag = 1
  }
  var x_source = Number(elt.value);
  var time = new Date();
  var data = [{
    x: [time],
    y: [x_source],
    mode: 'lines',
    line: {color: '#80CAF6'}
  }]
  Plotly.plot('graph', data);
  //Make interval a global variable
  interval = setInterval(function() {
    var time = new Date();
    var update = {
      x:  [[time]],
      y: [[x_source]]
    }
    Plotly.extendTraces('graph', update, [0])
    //if(cnt != 0) clearInterval(interval);
      }, 100);
}
</script>

    
Give it a try!
Click on any input boxes, and switch back and forth between sensors. You can also change the value in the input box.


<script>
plot_flag = 0
function plot(elt){
  if (plot_flag == 1){
    clearInterval(interval)
  }
  else{
    plot_flag = 1
  }
  var x_source = Number(elt.value);
  var time = new Date();
  var data = [{
    x: [time],
    y: [x_source],
    mode: 'lines',
    line: {color: '#80CAF6'}
  }]
  Plotly.plot('graph', data);
  //Make interval a global variable
  interval = setInterval(function() {
    var time = new Date();
    var update = {
      x:  [[time]],
      y: [[x_source]]
    }
    Plotly.extendTraces('graph', update, [0])
    //if(cnt != 0) clearInterval(interval);
      }, 100);
}
</script>

<div id="graph"></div>
<ul id="sensors">
<li>sensor 1  <input class='value_telemetry' name="sensor1" onclick="plot(this)" value = "4" width="30px"></li>
<li>sensor 2 <input class='value_telemetry' name="sensor2" onclick="plot(this)" value = "40" width="30px"></li>
</ul>       


## **EXAMPLE 2: Refresh the graph everytime a different sensor is selected**
<hr>
{% highlight html linenos%}
plot_flag = 0
function plot(elt){
  Plotly.purge("graph")
  if (plot_flag == 1){
    clearInterval(interval)
  }
 .......
{% endhighlight %}


<script>
plot_flag_ = 0
function plot_(elt){
  Plotly.purge("graph2")
  if (plot_flag_ == 1){
    clearInterval(interval_)
  }
  else{
    plot_flag_ = 1
  }
  var x_source = Number(elt.value);
  var time = new Date();
  var data = [{
    x: [time],
    y: [x_source],
    mode: 'lines',
    line: {color: '#80CAF6'}
  }]
  Plotly.plot('graph2', data);
  //Make interval a global variable
  interval_ = setInterval(function() {
    var time = new Date();
    var update = {
      x:  [[time]],
      y: [[x_source]]
    }
    Plotly.extendTraces('graph2', update, [0])
    //if(cnt != 0) clearInterval(interval);
      }, 100);
}
</script>
<div id="graph2"></div>
<ul id="sensors">
<li><b>sensor 1</b>   <input name="sensor1a" onclick="plot_(this)" value = "4" width="30px"></li>
<li><b>sensor 2</b> <input name="sensor2a" onclick="plot_(this)" value = "40" width="30px"></li>
</ul>       



Here you have it: a nice way to visualize real-time data from multiple sensors of a robot, of a IOT project, or whathaveyou.