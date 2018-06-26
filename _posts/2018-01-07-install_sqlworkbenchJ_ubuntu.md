---
layout: post
title: Install SQLWorkbenchJ on Ubuntu
meta: SQL workbenchJ
category: ds
author: "Jean-Marc Beaujour"
img_post: 20180107-sqlworkbenchJ.jpg
summary: SQL Workbench/J is a SQL query tool, written in Java. In this post, I will walk you through how to install SQLWorkbenchJ. 
github-link: "na"
---

This is a step-by-step walk-through to run *sqlWorkbenchJ* on Ubuntu machine.


**1. Go to the website**

**2. Click on the link *download***
[http://www.sql-workbench.net/](http://www.sql-workbench.net/)

<img src="/images/20180107/webpage.png" width="1000em">

**3. Download the generic package**

<img src="/images/20180107/download.png" width="1000em">

**4. Unzip the downloaded *zip file**
The package should contain the following files:

<img src="/images/20180107/files.png" width="600em">

For the *Ubuntu* install, we are interested in the file: *sqlworkbench.sh*.

**5. Enable the file to be executable**

Right click on the file *sqlworkbench.sh* and open tab *Permission*, and check the box:
*Enable executing file as program*

<img src="/images/20180107/executable.png" width="600em">

**6. Check if Java is installed**

<img src="/images/20180107/java_version.png" width="600em">

**7. Run *sqlworkbench.sh***

Open a Terminal, and run the file *sqlworkbench.sh*:
{% highlight python %}
sh sqlworkbench.sh
{% endhighlight %}
<img src="/images/20180107/sqlworkbenchJ.png" width="600em">
