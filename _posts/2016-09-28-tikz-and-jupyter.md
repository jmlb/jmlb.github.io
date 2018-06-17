---
layout: post
title: Insert Tikz objects in Jupyter Notebooks and Jekyll page
meta: tikz Visualization
category: coding
author: "Jean-Marc Beaujour"
img_post: 20160928_tikz.png
summary:
github-link: "na"
---


## Jupyther Notebooks

1. install the extension and load it

{% highlight python linenos%}
%install_ext https://raw.githubusercontent.com/meduz/ipython_magics/master/tikzmagic.py
%load_ext tikzmagic 
{% endhighlight %}

2. Insert a tikz object in a jupyter notebook:

{% highlight python linenos%}
%tikz \draw (0,0) -- (4,0) -- (4,4) -- (0,4) -- (0,0);
%tikz \draw (0,0) parabola (4,4);
%tikz \draw (0,0) .. controls (0,4) and (4,0) .. (4,4);
{% endhighlight %}


{% tikz 20160928_01 %}
  tikz \draw (0,0) -- (4,0) -- (4,4) -- (0,4) -- (0,0);
  tikz \draw (0,0) parabola (4,4);
  tikz \draw (0,0) .. controls (0,4) and (4,0) .. (4,4);
{% endtikz %}



## Jekyll Markdown

1. create a folder *_plugins* if it doees not already exit 

2. copy in *_plugins*, the file: [jekyll-tikz.rb](https://gist.github.com/hack-ghost/6139753)

3. stop *jekyll server* (`CTRL`+`C`)

4. restart *jekyll server* : `jekyll server --port 4455 --watch`:

You can now tstart adding **tikz** in your posts. Note that the tikz are converted to *svg* file and save in: 

 > /img/tikz-and-jupyter/

5. in a **md** file, add the following:

{% highlight python linenos%}
  [scale=.8,auto=left,every node/.style={circle,fill=blue!20}]
  \node (n6) at (1,10) {6};
  \node (n4) at (4,8)  {4};
  \node (n5) at (8,9)  {5};
  \node (n1) at (11,8) {1};
  \node (n2) at (9,6)  {2};
  \node (n3) at (5,5)  {3};

  \foreach \from/\to in {n6/n4,n4/n5,n5/n1,n1/n2,n2/n5,n2/n3,n3/n4}
    \draw (\from) -- (\to);
{% endhighlight %}


{% tikz 20160928_02 %}
  [scale=.8,auto=left,every node/.style={circle,fill=blue!20}]
  \node (n6) at (1,10) {6};
  \node (n4) at (4,8)  {4};
  \node (n5) at (8,9)  {5};
  \node (n1) at (11,8) {1};
  \node (n2) at (9,6)  {2};
  \node (n3) at (5,5)  {3};

  \foreach \from/\to in {n6/n4,n4/n5,n5/n1,n1/n2,n2/n5,n2/n3,n3/n4}
    \draw (\from) -- (\to);
{% endtikz %}

