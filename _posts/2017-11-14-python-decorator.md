---
layout: post
title: "Intro to Python decorators"
category: coding
img_post: python-decorators.png
author: "Jean-Marc Beaujour"
meta: "python"
summary: "Python decorators enable to dynamically alter the functionality of a function/method/class."
github-link: na
---

## **Examples of functions

In **Python**, functions and classes are both objects:

{% highlight python %}
def tell(msg):
  print(msg)

tell("Hello")
repeat = tell
repeat("Hello")
{% endhighlight %}

Functions can be passed as arguments to another function. Such function that take other functions as arguments are called **higher order functions**:

{% highlight python %}
def inc(x):
  return x+1

def dec(x):
 return x-1 

def operate(func, x):
  result = func(x)
  return result

# run
operate(inc, 3) # returns 4
operate(dec, 3) # returns 2
{% endhighlight %}


## Nested functions

{% highlight python %}
def make_string(func):
  def add_char():
    str = func()
    str += 'a'  
    return str
  return add_char

def my_number():
  return "123456789_"

#Let's run the code
an_instance = make_string(my_number)
an_instance() #returns "123456789_a"
{% endhighlight %}

The function that is nested is said to be decorated. Generally, we decorate a function and assign it with the same name:

{% highlight python %}
my_number = make_string(my_number)
my_number() #returns "123456789_a"
{% endhighlight %}

## Uing `@`:

{% highlight python %}
@make_string #function to be decorated
def my_number():
  return "123456789_"
{% endhighlight %}

which is equivalent to:

{% highlight python %}
@make_string #function to be decorated
def my_number():
  return "123456789_"

my_number = make_string(my_number)
{% endhighlight %}