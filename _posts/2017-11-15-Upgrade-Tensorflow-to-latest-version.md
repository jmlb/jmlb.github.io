---
layout: post
title: Upgrade Tensorflow with GPU support
meta: "GPU"
category: ml
author: "Jean-Marc Beaujour"
img_post: install TF_NVIdia.jpg
summary: A step-by-step tutorial on how to install Tensorflow for Python 3.5 with GPU support. The TF library is built from the source, and enables better system compatibility and better performance.
github-link: na
---

summary: "Python decorators enable to dynamically alter the functionality of a function/method/class."
github-link: na
---
In this post, I detail how to install/upgrade Tensorflow (**TF**) to a newest version. The installation is for **Python3.5** with NVidia GPU support for Graphics computing. The installation is made from the source because it enables the build to be specific to the machine architecture. It also enriches TensoFlow with a better system compatibility, and then allowing higher performance.

To update/install TF, one needs to perform the following steps:  

| Steps  |     Details    |
|:----------:|:-------------|
| **1** | Remove tensorflow and protobuf (if previously installed) |
| **2** | Download TF source                                      |
| **3**| Install Bazel                                           |
| **4** | Run TF Configuration script                             |
| **5** | Build TF whl package with Bazel                         |
| **6**| Install TF whl                                          | 
| **7**| Check that TF uses GPU                                        |
|--- |


### **MACHINE SPECS**
<hr>
Below are the details of my machine: a Ubuntu 16.04 LTS Desktop with a **16Gb NVidia TitanX card**, dedicated to computing only (no graphic work), 32 Gb of RAM.

<img src="/images/{{ page.date | date: '%Y%m%d' }}/my_machine.png">


### **STEP1: REMOVE PREVIOUS INSTALL OF PROTOBUF/TF**
<hr>
{% highlight python %}
$ sudo apt-get remove libprotobuf-dev protobuf-compiler
$ sudo apt-get remove libprotobuf-lite8 libprotoc8
$ sudo apt-get remove python-protobuf
$ sudo pip uninstall protobuf
{% endhighlight %}


### **STEP2: DOWNLOAD TF SOURCE**
<hr>
[link](https://www.tensorflow.org/install/install_sources)
To clone the latest TensorFlow repository, issue the following command:

{% highlight python %}
$ cd Documents
$ git clone https://github.com/tensorflow/tensorflow
$ cd tensorflow
$ git checkout Branch # where Branch is the desired branch
{% endhighlight %}
To install the latest version of TF, skip the last line (*git checkout ...*).

A Directory *tensorflow* is then created.

### **STEP3: INSTALL BAZEL**
<hr>
The updated list of releases are available [here](https://github.com/bazelbuild/bazel/releases).
Note that we will not be installing the latest version of Bazel, but the Release 0.5.4 (2017-08-25). Indeed, with the newest versions of Bazel, there is an error when building the TF library. 
First, we need to download the [sh file](https://github.com/bazelbuild/bazel/releases/download/0.5.4/bazel-0.5.4-installer-linux-x86_64.sh), and then install bazel:

{% highlight python %}
$ chmod +x bazel-0.5.4-installer-linux-x86_64.sh
$ ./bazel-0.5.4-installer-linux-x86_64.sh --user
{% endhighlight %}


### **STEP4: RUN TF CONFIGURATION SCRIPT**
<hr>
Now, we need to configure the installation: the flags of the configuration are of great importance because they determine how well and compatible the TensorFlow will be installed!! At first we have to go to the TensorFlow root:

{% highlight python %}
$ cd tensorflow
{% endhighlight %}

You should see a screen as shown below, and a series of [Y/N] flags to configure the install.

<img src="/images/{{ page.date | date: '%Y%m%d' }}/bazel_install.png">

Before starting to answer the flags, make sure you know the version of Python you want TF to be build for:

{% highlight python %}
$which python
  /usr/bin/python3.5
  /usr/lib/python3/dist-packages
{% endhighlight %}

<img src="/images/{{ page.date | date: '%Y%m%d' }}/python_version.png">


Then run the configure script as follows:

{% highlight python %}
$ ./configure

Please specify the location of python. [Default is /usr/bin/python]: [enter]
Do you wish to build TensorFlow with Google Cloud Platform support? [y/N] n
No Google Cloud Platform support will be enabled for TensorFlow
Do you wish to build TensorFlow with GPU support? [y/N] y
GPU support will be enabled for TensorFlow
Please specify which gcc nvcc should use as the host compiler. [Default is /usr/bin/gcc]: [enter]
Please specify the Cuda SDK version you want to use, e.g. 7.0. [Leave empty to use system default]: 8.0
Please specify the location where CUDA 8.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: [enter]
Please specify the Cudnn version you want to use. [Leave empty to use system default]: [enter]
Please specify the location where cuDNN 5 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: [enter]
Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size.
[Default is: "3.5,5.2"]: 5.2,6.1 [see https://developer.nvidia.com/cuda-gpus]
Setting up Cuda include
Setting up Cuda lib64
Setting up Cuda bin
Setting up Cuda nvvm
Setting up CUPTI include
Setting up CUPTI lib64
Configuration finished
{% endhighlight %}


### **STEP5: BUILD TF .WHL PACKAGE WITH BAZEL**
<hr>
{% highlight python %}
$ bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
$ bazel-bin/tensorflow/tools/pip_package/build_pip_package ~/tensorflow_package
$ bazel build -c opt --copt=-mavx --copt=-mavx2 --copt=-mfma --copt=-mfpmath=both --copt=-msse4.2 --config=cuda //tensorflow/tools/pip_package:build_pip_package
$ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
{% endhighlight %}

The bazel build command builds a script named **build_pip_package**. Running the following script build a .whl file within the ~/tensorflow_package directory:


### **STEP6: INSTALL TF WHL**
<hr>
{% highlight python %}
$ sudo pip install --upgrade /tmp/tensorflow_pkg/tensorflow-*.whl
{% endhighlight %}

### **STEP7: CONFIRM GPU USAGE BY TF**
<hr>
Before running any of the steps below, make sure you are not anymore in the *tensorflow* folder or in a folder where *tensorflow* is a direct child folder.

#### **7.1 Let's check the version of tensorflow**

{% highlight python %}
~ $ python
Python 3.5.2 (default, Sep 14 2017, 22:51:06) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
>>> tf.__version__
'1.1.0-rc0'
{% endhighlight %}


#### **7.2. Method 1: TF uses GPU ?**
[stackoverflow](https://stackoverflow.com/questions/38009682/how-to-tell-if-tensorflow-is-using-gpu-acceleration-from-inside-python-shell
)

{% highlight python %}
$ python 
sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
{% endhighlight %}

and returns:

<img src="/images/{{ page.date | date: '%Y%m%d' }}/tf_with_gpu.png">
The Graphic card is detected by TF: *TitanX*

#### **7.3. Method 2: TF uses GPU ?**
Another method consists in performing a tensor operation using GPU device:

{% highlight python %}
import tensorflow as tf
with tf.device('/gpu:0'):
    a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a')
    b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')
    c = tf.matmul(a, b)

with tf.Session() as sess:
    print (sess.run(c))
{% endhighlight %}


### **COMMENTS**
<hr>
Sometimes the TF session might failed to be created. 

{% highlight python %}
>>> sess = tf.Session()
2017-11-15 11:04:54.020997: E tensorflow/core/common_runtime/direct_session.cc:137] Internal: failed initializing StreamExecutor for CUDA device ordinal 0: Internal: failed call to cuDevicePrimaryCtxRetain: CUDA_ERROR_OUT_OF_MEMORY; total memory reported: 12782075904
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/client/session.py", line 1193, in __init__
    super(Session, self).__init__(target, graph, config=config)
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/client/session.py", line 554, in __init__
    self._session = tf_session.TF_NewDeprecatedSession(opts, status)
  File "/usr/lib/python3.5/contextlib.py", line 66, in __exit__
    next(self.gen)
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/framework/errors_impl.py", line 466, in raise_exception_on_not_ok_status
    pywrap_tensorflow.TF_GetCode(status))
tensorflow.python.framework.errors_impl.InternalError: Failed to create session.
{% endhighlight %}

It is apparently a known behaviour ( [link](https://github.com/tensorflow/tensorflow/issues/7983) )