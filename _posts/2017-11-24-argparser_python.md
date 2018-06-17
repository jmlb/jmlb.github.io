---
layout: post
title: How to use the argument parser - argparse (Python)?
category: flashcards
subcategory: coding
tags: python argparser
author: "Jean-Marc Beaujour"
summary: "Argparse is a module that allows for option and argument parsing for python programs"
img_post: argparse_20171124.jpg
github-link: na
---


[Argparse](https://docs.python.org/2/howto/argparse.html) is a module that allows for neat parsing of options and arguments for python programs.


#### **1. Positional arguments**

A positional argument is an argument that is not supplied as a `key=value pair`.
The example below shows how to use the argument parser with **positional arguments**.

{% highlight python linenos%}
#################
#substract.py
#################
import argparse

def substract(a, b):
    print(a - b)

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="this program substracts 2 elements")
    #Create arguments: add_argument(name_of_argument, help_text, type_of_argument)
    parser.add_argument("a", help="1st element", type=int)
    parser.add_argument("b", help="2nd element", type=int)
    #Get arguments from the parser
    args =  parser.parse_args()
    substract(args.a, args.b)
{% endhighlight %}

We now run the file from the terminal:

{% highlight ruby linenos%}
$ python substract.py 2 3
$ -1
{% endhighlight %}

Note that the order of the argument matters: 
* `python substract.py 2 3` returns `-1`
* `python substract.py 3 2` returns `1`

If a Positional argument is not specified, it is automatically given the value `None`.


### **2. Optional arguments**

Optional arguments are, as the name suggests, *optional*: they are not needed to run the program. For example, `-h` *helper* is a (built-in) optional argument.

{% highlight python linenos%}
#################
#substract.py
#################
import argparse

def substract(a, b):
    print(a - b)

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="this program substracts 2 elements")
    # add_argument("short name", "long name", action="action to be done when the option is called ")
    # action="store_true" ==> set -v = True
    parser.add_argument("-v", "--verbose", help="print out", action="store_true")
    parser.add_argument("a", help="help text", type=int)
    parser.add_argument("b", help="help text", type=int)
    #Get arguments from the parser
    args =  parser.parse_args()

    if args.verbose:
        print("Substraction: {}-{}".format(args.a, args.b))
    add(args.a, args.b)
{% endhighlight %}

We then run the file from the terminal:

{% highlight ruby linenos%}
$ python substract.py 2 3 -v
$ -1
{% endhighlight %}

We can write the flag in any order we want. The argument `-v` can be called at any position. The following calls are equivalent: 

* `python substract.py 2 3 -v`
* `python substract.py 2 -v 3`
* `python substract.py -v 2 3`

`store_true`: the default is `False` but when the flag is called, it is given the value `True`.

An optional argument can be requested to be called by using the attribute `required=True`:

`parser.add_argument("-v", "--verbose", help="help text", action="store_true", required=True)`




### **3. Mutually exclusive argument**

**Mutually exclusive arguments** are arguments that cannot be called more than one at a time.
In the example below, we will add 2 mutually exclusive arguments: `quiet` and `verbose`.

{% highlight python linenos%}
#################
#substract.py
#################
import argparse

def substract(a, b):
    return a - b

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="describe what the program does")
    parser.add_argument("a", help="help text", type=int)
    parser.add_argument("b", help="help text", type=int)
    #create a container
    group = parser.add_mutually_exclusive_group()
    # '-q': short hand notation, '--quiet': long hand notation
    group.add_argument("-q", "--quiet", action="store_true", help="print quiet")
    group.add_argument("-v", "--verbose", action="store_true", help="print verbose")
    args =  parser.parse_args()

    if args.verbose:
        print("Substraction: {}-{} = {}".format(args.a, args.b, substract(args.a, args.b)))
    elif args.quiet:
        print("{}".format( substract(args.a, args.b) ))
    else:
        print("{}-{}={}".format(args.a, args.b, substract(args.a, args.b)))
{% endhighlight %}

Let's run the code:

{% highlight ruby linenos%}
$ python substract.py 2 3
$ 2 - 3 = -1

$ python substract.py 2 3 -v
$ Substraction: 2 - 3 = -1

$ python substract.py 2 3 -q
$ -1
{% endhighlight %} 