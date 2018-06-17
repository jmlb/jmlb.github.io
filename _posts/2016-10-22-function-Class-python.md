---
layout: post
title: function( ) and Class( ) in Python
meta: python
category: coding
author: "Jean-Marc Beaujour"
img_post: 2016-10-22_python_function.png
summary: 
github-link: "na"
---

A few days ago, I started the Udacity course: ["**Programming Foundations with Python**"](https://classroom.udacity.com/courses/ud036/lessons/993510820/concepts/9991989300923) for the sole purpose of having it completed before I apply for the [A.I Nanodegree (AIND)](https://www.udacity.com/ai). Indeed, one of the requirements to get into the ND is to show some credentials in programming. 
The Python course is for beginners. It focuses on **classes** and **functions**, and it has multiple examples that shows how when to use functions and classes. The instructor is really good: he is concise and clear. After watching the videos and working on the problems, concepts like *inheritance* were much clearer to me. In this post, I will review how to define and work with **functions** and **classes** in **python**. I wrote this post mainly as an occasion to review the material of the course.


# 1. Functions
We will write a function that renames all the files in a directory. In our case, the function will remove all the digits that are contained in the original file name.

<script src="https://gist.github.com/jmlb/0f44a2c9b95291d1d2145a3fcb8214a2.js"></script>

In `listdir(r "C:/myfolder/")`, the letter  `r`  stands for *raw*: it tells **python** that it must not interpret the characters in the string `C:/myfolder/`.


+ **Abstraction**: means to give names to things, so that the name captures the core of what a function or a whole program does.


# 2. Class

+ a **class** defines a template, a blueprint.

+ The name of a class starts with a capital letter. It is meant to differentiate with variables.

## 2.1. How to use an existing class: `Turtle()`
Before we start making our own class, let's see how to use an existing class. We create a function `draw_square`, that calls a class named `Turtle()`.The goal of the function is to draw a square on a red background window.

<script src="https://gist.github.com/jmlb/ff90b5fd4db6b44abe93dc3cd30bb135.js"></script>


+ With the line `brad = turtle.Turtle()`, we are saying to **python** that there is a file called `turtle` and inside that file, there is a class named `Turtle()`. 

+ The empty brackets `( )` is to call the funtion `__init__( )` of the class `Turtle()`. 

+ `__init__()` function is called the **constructor**: it initializes/creates the space in memory for a new instance, a new object. In our case, `__init__()` instantiates a new object named `brad`.

+ The underscore in `__init__()` tells us that the name `init` is reserved in python: it is a special function / method. 

+ In the `draw_square()` function, we call several **instance methods** of `Turtle()`: `forward` and `right`.



## 2.2. Create a Class: `Movie()`

We have seen how to call a class and create new instances. Now, let's build our own class from scrath.

+ In the "*media.py*" file, we create a class `Movie()`

+ The 1st function in the class, is the constructor `__init__()`.

<script src="https://gist.github.com/jmlb/34d71a31de736793a87047a79de2b4a7.js"></script>

<script src="https://gist.github.com/jmlb/b3b7df98a3ac124d858f6f7d191e43e3.js"></script>

We first import the file `media`.

+ `toy_story = media.Movie()`, tells python to go to the file `media` and get the class `Movie()`, then call the function `__init__()`.

+ the function `__init__()` is there to instantiate (initialize), and creates space in memory for the instance `toy_story`. Anytime an object is created, it looks for an init function and calls whatever is in there.

+ `__init__()` takes `self` as argument.   `self` points to the instance/object `toy_story`.


### Instance Variables

Typically, a movie has the following characteristics: a title, a storyline, a trailer and a poster image.

+ We are going to pass those values to the class as **instance variables**: `self.movie_title`, etc... These variables will be associated with every instance created.

+ we need to let python knows that when we create an object `Movie( )`, the object will have the following attributes: `movie_title`, `storyline`, etc... 

<script src="https://gist.github.com/jmlb/be277e24629d20b79eebb591c229160a.js"></script>

+ Initialize the object:  `__init__()` is going to receive several arguments: `movie_title`, `strotyline`, `poster_image_url`, `trailer_youtube_url`.

<script src="https://gist.github.com/jmlb/bd37476a3f6a9f9d2dad312fc4e7bae7.js"></script>

+ Now, we can create a new instance of `Movie()`: `toy_story`, and that has several arguments

<script src="https://gist.github.com/jmlb/bd294f67b27c81d3e2ea8614f25f7b3a.js"></script>

### Class variables

**Class variables** are used when instances to share variables.

+ the name of the class variable is typically written in upper case `VALID_RATINGS`.

<script src="https://gist.github.com/jmlb/97bea44c2cff38c5613c1e4628742274.js"></script>

+ The class variable can be called with:

<script src="https://gist.github.com/jmlb/5bddddbaa3cf021ab8142368e4ac81a3.js"></script>

### Instance method

**An instance method** is a function that is defined inside a class and is associated with an instance.

+ The first argument of an instance method must be `self` [for example, `def show_trailer(self)`].
That tells python to run the function on this instance.

<script src="https://gist.github.com/jmlb/781e103eece7e3becb55131eaeec724c.js"></script>

<script src="https://gist.github.com/jmlb/f101e2670fc2d4871f1839e7bbf61481.js"></script>

### Special class variable: `__doc__()`

+ `__doc__()` is used to print the documentation related to the Class

<script src="https://gist.github.com/jmlb/316897707dd9495bfde0eb642515e1a0.js"></script>

## 2.2. Class Inheritance : parent/child

So far, we have considered one type of videos, but let's say we want now to create a class `TVShow()`.

The attributes of the classes are:

+ **class Movie( )** : [title / duration / storyline / poster_image / youtube_trailer]
+ **class TVShow( ):** : [title / duration / season / episode / tv_station]

There are several items that are shared by the 2 classes. We can create a new class `Video( )` that will have `title` and `duration` as instance variables.

<script src="https://gist.github.com/jmlb/9c57dc31dc12e3180cb4c0fa00de5a82.js"></script>

+ we want `Movie( )` (the child) to inherite from `Video( )` (the parent): `Movie( )` will reuse everything that is available in `Video( )`, i.e `Movie( )` will inherite `title` and `duration`. For this to happen, we need to call `Video( )` as an argument of `Movie( )`.

<script src="https://gist.github.com/jmlb/efcd9b4cd7ef5ef36915bbac2a5390f2.js"></script>

+ we initialize the instance variables of `Video( )` by using :  `Video.__init__(self, movie_title, movie_duration)`

<script src="https://gist.github.com/jmlb/34f05adace596f812282666ed789ec89.js"></script>


### Method overriding

If an instance method has the same name in the child and in the parent, then the instance method in child will override parent instance method.
The child instance method will be ran in priority.

<script src="https://gist.github.com/jmlb/64f94efb133b5dd9bc21d4b0d0667b56.js"></script>

<script src="https://gist.github.com/jmlb/e5cf9443736fa6c1ef2a9073a7d296c8.js"></script>

That will output:

`Movie Title from Child: Toy Story`