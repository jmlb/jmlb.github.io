---
layout: post
title:  Git cheatsheet
tags: git
category: coding
author: "Jean-Marc Beaujour"
summary: "I generated this cheatsheet of git command after taking the Udacity Course on Git/Github"
category: coding
img_post: git.png
github-link: na
---

This is a **CheatSheet** (or "How to...?") that I wrote after taking the Udacity course : *"How to Use Git and GitHub?"* [(link)](https://www.udacity.com/course/how-to-use-git-and-github--ud775). The course is well built and it gives a solid overview of basic use of *Git Version Control System*, and in particular [GitHub](http://www.github.com).
The course also includes 2 useful videos on how to configure your workspace.

### **1. Some definitions**
<hr>
1. **repository**: a collection of files and folders 
2. **commit**: a user created checkpoint. They are ordered by date 
3. **tip**: last commit on a branch
4. **HEAD**: is the current branch
5. Diagram

<table>
<tr style="border:1px solid"><td><b>working directory</b></td><td></td><td><b>staging area</b> </td><td></td><td><b>repository</b></td></tr>
<tr style="border:1px solid"><td>file1</td><td> &#8594;(add)&#8594; </td><td>file1</td><td> &#8594;(commit)&#8594; </td><td>file1</td></tr>
<tr style="border:1px solid"><td>file2</td><td> &#8594;(add)&#8594; </td><td>file2</td><td> &#8594;(commit)&#8594; </td><td>file2</td></tr>
<tr style="border:1px solid"><td>file3</td><td></td><td>file3 </td><td></td><td>repository</td></tr></table>

### **2. Return info and status**
<hr>
{% highlight python linenos%}
$ git log  {Return lists of all commits with specs: "commit_id/Author/Date/Description"}
$ git diff commit_id1 commit_id2 {Differences between 2 commits}
$ git show commit_id {Show difference of "commit_id" with its parent}
$ git log -n1 {Show a single commit}
$ git status {Show which files has changed since last commit}
$ git diff {Show changes between working directory and staging area (no argument)}
$ git diff --staged {Compare changes between staging area and repository}
$ git branch {Show current branch (no argument)}
$ git checkout new_branch_name {Switch to "new_branch_name"}
$ git log --graph --oneline master branch_name1 {Visualize branches structures of: "master" and<br> "branch_name1", with short description (oneline)}
$ git remote -v {Show url for push/pull actions}
{% endhighlight %}


### **3. Git commands: action**
<hr>
{% highlight python linenos%}
$ git clone repo_url  {Copy entire repository from remote to local}
$ git checkout commit_id {Restoring a previous commit}
{Gives a warning: "you are in detached `HEAD` state".
You can detach the "HEAD" by switching to a previous commit.}

$ git branch new_branch_name {Create a new branch}
$ git remote {Check remote repository}
$ git remote add origin url {Add the remote repository}
$ git add *  {Add to the staging area all files and folders in working dir}
$ git fetch
{% endhighlight %}




### **4. How to ...**
<hr>
#### 4.1. *commit* (locally)

1. **add** files to staging area &#8594; `git add file1` or  `git add *`
2. **check** what's in the staging area  &#8594; `git status `
3. **commit** with message &#8594; `git commit -m "commit_message"`  
The message must be less than 72 chars, explain problems solved and why the change
4. **check** status &#8594; `git status`

Sometimes you might have to **force add** with: "git add -f *"


#### 4.2. Merge/Combine 2 branches

1. **check** active branch label &#8594; `git branch`
2. **merge** &#8594; `git merge master branch_name1`
3. **commit** &#8594; `git commit`
4. **show commit merge** &#8594; `git log`
5. delete label &#8594; `git branch -d branch_name1`


#### 4.3. Initialize a (local) new repository
1. Go to the local folder<br>
2. creates `.git` folder where all repo info are stored &#8594; `git init`
3. how which files has changed since last commit &#8594; `git status` (*s*)


#### 4.4. *Push* repositories (local to remote/Github)
1. Create empty repository on Github (Do not initialize with README if the local repository has commits)
2. Go to the working local directory
3. check remote repository &#8594; `git remote`
4. add the remote repository &#8594; `git remote add origin remote_url`
5. show url for push/pull &#8594; `git remote -v`
6. Push the master branch (starting from the tip) &#8594; `git push remote_name local_branch`
(`remote_name` = `origin` and `local_branch`=`master`)
Commits that are already on remote are not pushed.
7. The branch name will be unchanged on the remote

#### 4.5. *pull*/Update repositories (remote/Github to local)
1. `git pull remote_name branch_name`<br>
Typically, `remote_name`=`origin` and `branch_name`=`master`


#### 4.6. *Fork* a repository (copy the repo of another user to your Github account)
1. Go to another user github page
2. fork repository &#8594;   [ Github (someone) ] ---*fork*--> [my Github / master]
3. clone &#8594;  [my Github / master] <---origin*-- [clone local]
 
#### 4.7. Replace master branch in git, entirely, from another branch?
{% highlight python linenos%}
$ git checkout seotweaks
$ git merge -s ours master
$ git checkout master
$ git merge seotweaks
{% endhighlight %}
