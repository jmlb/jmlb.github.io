---
layout: post
title: args/kwargs Python
tags: arguments Python
category: coding
summary: ""
img_post: 20180122_args_kwargs.jpg
github-link: na
---

In this post, we look at examples of the use of `*args` and `*kwargs`.


Consider a function that takes 2 positional arguments and adds them:

```
def sum(x, y):
    print (x + y)
```
```
sum(5, 4)
Output 9
```

## Using `*args`
Can we modify the function `sum()` so that it can take any number of arguments?
That's the purpose of `*args`:

```
def sum(*args):
    s = 0
    for  in args:
        s += i
    print(s)
```

```
sum(4, 5)
sum(2, 3, 4)
sum(3, 5, 10, 6)
```

`*args` enables to send a argument list of variable-length in to thr function.


## Using `**kwargs`


`**kwargs` (note the double asterix) is used to pass a keyworded argument 
dictionary of variable-length, to a function.

Let's look at the example below:

```
def concat(**kwargs):
    m = ""
    for k, val in kwargs.items():
        m += val
    return m
```

```
input = {"name_1": "A", "name_2": "N", "name_3": "D", "name_4": "D", "name_5": "R", "name_6": "O", "name_7": "I", "name_8": "D"}
concat(**input)

output: "'ONDAIDDR"
```

Weird, we would expect the output to be `ANDROID`. So why are the letters shuffled like that?

The reason is that Python does not preserve the order in which the keyword arguments were passed to the function in a dictionaries: the keys may not be ordered.

With `**kargs`, we can pass a longer or shorter `dict()` to the function, and therefore there is the flexibility to use any argument length.


