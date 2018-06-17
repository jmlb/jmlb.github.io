---
layout: post
title: Setup a Docker container to play ROS bag data
comments: true
date: 2016-10-28
tags: SDCND Self-Driving-Car-Nanodegree Udacity 
categories: MachineDeepLearning SDCND
img_post: post_dockerRos_title.png
github-link: "https://github.com/jmlb/MacOS-Container4Ros-SDCND"
---


For the first challenges of the Self-Driving Car Nanodegree (SDC-ND), Udacity Team released a big bunch of driving data. The data is stored in ROS bags, and require the **[ROS](http://wwww.ROS.org)** package. 
My first attempt to install ROS directly on my MacBook Pro was a total failure. During the install, I would get a lot of warnings related to missing dependencies. In the course of fixing those, I ended up altering a few other packages that were previously installed. So, after a lot of frustrations and a few headaches, I decided to go the **countainer**'s route. In this post, I will explain how I setup a **[Docker](https://www.docker.com/)** image on MacOS (Sienna) for the purpose of: 1) extracting data, and 2) playing the videos in a container.

The container runs *Ubuntu 14.04* and *ROS Indigo*, and is loaded with a few more packages. For conveniency, I store the *rosbag* file on an external Volume (named "*backup*"). The rest of the files are kept on the primary Hard Drive.


So let's dive in! There are 4 main steps: 

1. Install Docker.

2. Create an Image (filesystem and parameters to use at runtime) with the required packages: Ubuntu, ROS Indigo etc...

3. Run a container (an instance of the image).

4. Run a python script to show the images from the *rosbag*.



# 1. Install Docker for Mac
Go to the [webpage](http://www.docker.com) and follow the detailed [instructions](https://docs.docker.com/docker-for-mac/#/what-to-know-before-you-install) on the website.
One important step is to add the appropriate folders that will be shared with the container: click on the icone "*Docker*" (top-right of the screen) >> on the tab "*File Sharing*", and add the folder you want to share with the container.

Below are a few commands that I used quite a lot when experimenting with Docker:

+ list of all containers:  

```python
docker ps -a 
```

+ Delete all containers:  

```python
docker rm $(docker ps -a -q)
```

+ Delete all images:

```python
docker rmi $(docker images -q)
```

# 2. Create an image
Docker has a good [tutorial](https://docs.docker.com/engine/getstarted/step_four/) on how to build an Image.

At first, we create a new folder "*sdc_docker*". That's where we will store all the *.sh* files.

+ in  "*~/Documents/*", we create a new folder: `mkdir sdc_docker` 

+ Move to the new directory: `cd sdc_docker`

+ In "*~/Documents/sdc_docker/*", we create the file "*Dockerfile*":  `touch Dockerfile`

+ open "*Dockfile*" with a text editor (at this point the file should be empty). Then, copy/paste all the lines below:

<script src="https://gist.github.com/jmlb/f7e6555fd23ca251b3df1d9a094c9db8.js"></script>

+ Save "*Dockerfile*" in the directory  "*~/Documents/sdc_docker/*"

+ The next step is to generate an Image that we will call "*sdc-u14*". Go back to the Terminal window, and make sure to be in the directory "*/sdc_docker/*", where the file "*Dockerfile*" should be. Run the following line (don't forget the "dot" at the end of the command):

```python
docker build -t sdc-u14 .
```

/!\ The install will take some time.


+ Once the install is done, we can check if we have generated the Image using the command:

```python
docker images
```


You should see a list similar to the screnshot below (the name of our Image is "*sdc-u14*" and its "*id*" is a string of letter and digits):

![png]({{ site.baseurl }}/pics/posts_pics/list_of_docker_image.png?style=centerme){:width="800rem"}


# 3. Run the container

With the line below, a container is instantiated :

```python
docker run -i -t sdc-u14 /bin/bash
```

which prints out the following:

![png]({{ site.baseurl }}/pics/posts_pics/screenshot_docker_container.png?style=centerme){:width="500rem"}

The symbol "*#*" means that we are now in the container.

The command line above basically tells Docker to create a container ("*docker run*"), in interactive mode ("*-i*"), attach the Terminal to Docker ("*-t*") and load the Image "*sdc-u14*" into the container.

A more elegant way to generate a container is by calling a "*.sh*" file that would contain the command lines above. We name that file "*Docker-run.sh*" and save it in "*~/Documents/sdc_docker/*". Here is the content of the file "*Docker-run.sh*":

<script src="https://gist.github.com/jmlb/4c0d0f6eeb5f249d2ac89d113854a014.js"></script>

The IP address on *line 3* is followed by "*:0*". This part tells the container where to find the environmental variable **DISPLAY**. 

We obtained the IP after running:

```python
echo "show State:/Network/Global/IPv4" | scutil | grep PrimaryInterface | awk '{print $3}' | xargs ifconfig | grep inet | grep -v inet6 | awk '{print $2}'
```

+ To get inside the container, run:

```python
sh docker-run.sh
```


+ To check that the ROS package is properly installed in the container, we launch:

```python
roscore
```

You should then see something like that:
![png]({{ site.baseurl }}/pics/posts_pics/roscore_container.png?style=centerme){:width="500rem"}


# 4. Run a python script to display the images
Hey, this is our final step...oofff...but it requires a few sub-steps :-(

## 4.1. Additional tools:
We need to install 2 more packages: **SOCAT** and **XQuartz** on MacOS.

+ Install **SOCAT** with: `brew install socat`

+ **XQuartz** : can be installed from the dmg file available [here](https://www.xquartz.org/).
You might need to logout after the install.

+ *view_rosbag_video.py* : this python file allows to play the rosbag file. It is available [here](https://github.com/diyjac/SDC-Udacity-Challenge-2) for download. !!! We will copy the file in the same directory where we have the rosbag file.

+ `attach-docker-container.sh`: this "*sh*" file allows to attach the container to more Terminal. The file can be downloaded [here](https://github.com/gtarobotics/self-driving-car). We save that file with the other *.sh* files, in "*~/documents/sdc-dockers/*".


## 4.2. Set up
First we need to run *socat* in background. Type the following in a MacOS shell:

```python
socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\" `
```

We then launch *XQuartz*:

```python
open -a XQuartz
```

Now, things start to get more interesting!
We are going to open 3 *XQuartz* Terminals (in *Quartz* app, go to *Applications>>Terminals*), and follow the instructions from Marius S. ([here](https://github.com/gtarobotics/self-driving-car)). Basically, it involves having 3 *XQuartz* Terminals attached to the same container.

+ *XQuartz* Terminal1: go to the folder "*~/Documents/sdc-docker/*"" and get in the container with:

```python
sh docker-run.sh
```

+ *XQuartz* Terminal2: again, make sure to be in the right directory: "*~/Documents/src-dockers/*". Then run:

```python
sh attach-container.sh <id_value_of_src-u14>
```

Now, Terminal2 is also in the container. We finally run those 2 lines:

`source /opt/ros/indigo/setup.bash`
`roscore` 

+ *XQuartz* Terminal3: Get into the container using same step as for *Xquartz* Terminal2. We go to the directory where we store the rosbag file ("*dataset.bag*"), and run :

```python
rosbag play -s 120 dataset.bag
```

After about 6 minutes, the data from the *rosbag* start to be printed out.

+ Go back to *XQuartz* Terminal1, and move to the folder where the dataset is stored, and run:

```python
python view_rosbag_video.py
``` 

And, voila! After several minutes, you should see 3 videos, each recorded by the front cameras of the Udacity car:

![png]({{ site.baseurl }}/pics/posts_pics/screenshot_vid1.png?style=centerme){:width="500rem"}

Hopefully, you were successful at getting your container running!


My next step is to modify the python script ("*python view_rosbag_video.py*") so it can simultaneously plays the videos, saves the images and saves other data like steering wheel angle. I can't wait to start working on the problem of **lane detection**...stay tuned!

