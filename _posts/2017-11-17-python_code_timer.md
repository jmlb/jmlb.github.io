---
layout: post
title: Analysis of Python code time efficiency
category: coding
img_post: c-profile_python.jpg
author: "Jean-Marc Beaujour"
meta: "python"
summary: "A set of tools to debug the efficiency of python code: check the speed of the "
github-link: na
---


# 1. c-Profile
{% highlight python %}
pip install line-profiler
{% endhighlight %}

# 2. Use line profile in jupyter notebook
{% highlight python %}
%load_ext line_profiler
{% endhighlight %}


# 3. Run the code to get timing

{% highlight python %}
%lprun -f function_name function_name(value)
%lprun %Time -f function_name function_name(value) #Gte time at each line
%lprun %Time -T timings.text -f function_name function_name(value) #save data in text fil
{% endhighlight %}