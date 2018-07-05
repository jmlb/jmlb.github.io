---
layout: post
title: Ros CheatSheet
meta: "ros"
category: "robotics"
subcategory: "ros"
img_post: 20180701-ros-cheatsheet.jpg
summary: 
github-link: "na"
---


<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>




## List of ROS Commands

<table style="text-align: left; font-size: 1rem; font-family: Times New Roman;">
	<tr>
		<th>General</th>
	</tr>
	<tr>
		<th> <b>roscd package_name</b> </th><th><i>list of all running nodes</i></th>
	</tr>
	<tr>
		<th> <b>roslaunch node_launch_fname</b> </th><th><i>launch node(s) - a xml file will specify what nodes to launch</i></th>
	</tr>
	<tr>
		<th> <b>rosrun node_name</b> </th><th><i>run a node</i></th>
	</tr>
	<tr>
		<th>Nodes</th>
	</tr>
	<tr>
		<th> <b>rosnode list</b> </th><th><i>list of all running nodes</i></th>
	</tr>
	<tr>
		<th> <b>rosnode info /node_name</b> </th><th><i></i></th>
	</tr>
	<tr>
		<th>Topic</th>
	</tr>
	<tr>
		<th> <b>rostopic list</b> </th><th></th>
	</tr>
	<tr>
		<th> <b>rostopic info topic_name</b> </th><th></th>
	</tr>
	<tr>
		<th> <b>rostopic echo topic_name</b> </th><th><i>print the data published</i></th>
	</tr>
	<tr>
		<th> <b>rostopic hz topic_name</b> </th><th><i>print the number of iterations per seconds</i></th>
	</tr>
	<tr>
		<th>Bag</th>
	</tr>
	<tr>
		<th> <b>rosbag</b> </th><th>Record data</th>
	</tr>
	<tr>
		<th>Visualization</th>
	</tr>
	<tr>
		<th> <b>rqt</b> </th><th>Show image data - </th>
	</tr>

</table>

