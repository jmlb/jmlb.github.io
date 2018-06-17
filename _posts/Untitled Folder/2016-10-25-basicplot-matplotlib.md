---
layout: post
title: A few basic plots with matplotlib
comments: true
date: 2016-11-05
tags: plot python matplotlib basics 
categories: MachineDeepLearning
img_post: matplotlib_logo.svg.png
github-link: na
---



A few eample of pyplot

from numpy import *
import matplotlib.pyplot as plt
x = arange(0.,10.,0.1)
y = sin(x)
xl = plt.xlabel('horizontal axis')
yl = plt.ylabel('vertical axis')
ttl = plt.title('sine function')
ll = plt.plot(x,y, 'r--',label='sine')
lc = plt.legend(loc='upper left')
txt = plt.text(0,1.3,'here is some text')
plt.show()