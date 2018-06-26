---
layout: post
title: Install NVidia GeForce TitanX card for Deep Learning GPU computing
category: ml
img_post: gpu.jpg
author: "Jean-Marc Beaujour"
meta: "hardware"
summary: "Here is a step by step on how to setup/install a NVidia GPU card on a Linux machine as a secondary graphic card for Deep Learning computation. That includes some tricks during the installation of the driver to prevent some known issues such as infinite login loop. "
github-link: na
---


Then define your chart like this:


The following steps are good only if the NVIdia card is used for GPU acceleration computing only, not for display. My machine has 2 graphic cards: a Rodeon ATI (not built-in) for display and the **NVIdia GForce** for computing. Before starting, make sure to save all your work.


step0:  Download the driver from <a href="http://www.nvidia.com/Download/index.aspx">NVIdia website</a>.**
<br>Make note of the directory where the file is saved (usually it is in the folder <i>Downloads/</i>)

step1. Get into the text-only console (TTY)

step2: Enter login and password <br>

step3 **4. kill the current X-server session**<br>

step4: **5. Optional: purge all NVidia drivers (there is no need to do that but in case you want a fresh start)** <br>
step 5:  Enter runlevel 3**<br>
step6: Run the install. Go to the directory where the file NVIDIA&lowast;.run file is saved. If you have 1 install file, type:
It is important to include the option `no-opengl--files` in the command. If it is not included, the install would overwrite the opengl files used by the Graphic Card for Display. And you would end up with a machine stucked into a login loop.

**8. If some error messages prompt up, dismiss them **<br>

**9. Do not update Xconfig**<br>
You will be asked if you wish to update Xconfig: select **NO**. If you choose YES, the NVIdia Card will take over the display work and that will create conflicts with the other graphic card.
Otherwise, replace NVI&lowast;.run by the full name of the file.

**10. Start X-server session**<br>
**11. Exit the TTY mode**<br>


{% highlight python linenos%}
`"CTRL"` + `"ALT"` + `"F1"`
#Enter login and passwrod
sudo service lightdm stop
sudo apt-get purge nvidia-*
sudo init 3
cd {location_of_nvidia_file}
`sudo sh NVI*.run --no-opengl-files`
{ Follow the instructions }
`sudo service lightdm start`
`"CTRL"` + `"ALT"` + `"F7"` 

{% endhighlight %}


Voila! You are all set with the installation of the driver!


The next and final step is to make sure that TensorFlow detects the GPU. Here is a short script:

<script src="https://gist.github.com/jmlb/5e10a6df497b0c777b891d4c790fb4c5.js"></script>

The script calculates the matrix multiplication: $$c = a \times b$$, where *a* is a (2,1) and *b* is a (1,2) matrix.

If the GPU is detected by TF, you should see something like this:

<img src="/images/20161129/gpu.png" width="1200em" align="center">


