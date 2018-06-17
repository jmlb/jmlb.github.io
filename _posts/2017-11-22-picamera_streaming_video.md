---
layout: post
title: How to stream at 28fps with Picamera?
category: robotics
img_post: picamera_streaming.png
author: "Jean-Marc Beaujour"
meta: "raspberry_pi, python"
summary: "We look at the performance of streaming mjpeg 
from a raspberry pi to a macbook pro, with the Picamera library (Python). We use a camera module."
github-link: https://github.com/jmlb/RPi_libraries/tree/master/vidstream_picamera
---

I have been looking for ways to get a better video streaming performance from my Raspberry Pi 3. What I mean by better performance is streaming with **high fps and low video latency**. In this article, I explained how to stream video-images with [MJPEG compression](https://en.wikipedia.org/wiki/Motion_JPEG) from a Raspberry Pi to a Laptop over Wifi, using the **Picamera** library. 
The images are recorded by a camera module and streamed in real time from the RPi (the client) to a Laptop (the server). I use:

* a Rapberry Pi 3, with Raspbian OS,
* a [camera module v.2](https://www.raspberrypi.org/products/camera-module-v2/)
* The RPi is loaded with Python3 and the *Picamera* library. 
Picamera* is a library that provides a python interface to the camera. It can be installed with a simple pip command in the Terminal (`pip3 install picamera`).

* The **server** is a Macbook Pro, and is connected to the same Wifi as the RPi.

Let's have a look at the codes we will be using for our little experiments.

### 1. The codes
The code is a modified version of the code provided on the [*Picamera* wikipage](https://picamera.readthedocs.io/en/release-1.13/recipes1.html#capturing-to-a-network-stream). It uses a very simple coomunication protocol where first, the length of the image is sent as a 32-bit integer, and  then the bytes of image data is sent. If the length is 0, this indicates that the connection should be closed as no more images will be forthcoming.
The server script should be run first to ensure there’s a listening socket ready to accept a connection from the client script.


#### 1.1 The server (`server.py` to be saved on the Laptop)
{% highlight ruby linenos%}
# server.py
import io
import socket
import struct
from PIL import Image
import time

if __name__ == '__main__':
    # Start a socket listening for connections on 0.0.0.0:8000 
    # (0.0.0.0 means all interfaces)
    server_socket = socket.socket()
    server_socket.bind(('0.0.0.0', 1308))
    server_socket.listen(0)
    # Accept a single connection and make a file-like object out of it
    connection = server_socket.accept()[0].makefile('rb')
    ##############
    # Parameters
    ##############
    n_recs = 0
    cnt_streamed_imgs = 0
    summary = []
    n_measurements = 100
    avg_img_len = 0
    W, H = 320, 240

    try:
        test = True
        init = time.time()
        while test:
            # Read the length of the image as a 32-bit unsigned int.
            image_len = struct.unpack('<L', connection.read(struct.calcsize('<L')))[0]

            if not image_len:
                break
            # Construct a stream to hold the image data and read the image
            # data from the connection
            image_stream = io.BytesIO()
            try:
                #reading jpeg image
                image_stream.write(connection.read(image_len))
                image = Image.open(image_stream)
            except:
                #if reading raw images: yuv or rgb
                image = Image.frombytes('L', (W, H), image_stream.read())
            # Rewind the stream
            image_stream.seek(0)
            
            avg_img_len += image_len
            elapsed = (time.time() - init)
            cnt_streamed_imgs += 1
            if elapsed > 10 and elapsed < 11:
                #record number of images streamed in about 10secs
                avg_img_len = avg_img_len / cnt_streamed_imgs
                print("{} | Nbr_frames: {} - Elapased Time: {:.2f} | Average img length: {:.1f}]".format(n_recs, cnt_streamed_imgs, elapsed, avg_img_len) )
                summary.append( [cnt_streamed_imgs, elapsed, avg_img_len] )
                n_recs += 1
                #reset counters
                init = time.time()
                cnt_streamed_imgs = 0
                avg_img_len = 0
            if n_recs == n_measurements:
                #Number of measurements
                test = False

            #Write summary
        with open("stream_perf_07.txt", "w") as file:
            file.write("nbr_images, elapsed(sec), avg_img_size\n")
            for record in summary:
                file.write("{}, {}, {}\n".format( record[0], record[1], record[2]))

    finally:
        connection.close()
        server_socket.close()
{% endhighlight %}

The image is not saved locally on the Laptop, but this can be easily done by adding at *line 47* the following:
`image.save('streamed_img.jpeg')`


#### 1.2 The client (`client.py` to be saved on the RPi.)

{% highlight ruby linenos%}
# client.py
import io
import socket
import struct
import time
import picamera
import argparse

############
# arguments
############
parser = argparse.ArgumentParser(description="Client side-RPi streams Motion pictures")
parser.add_argument('-ip', '--ip_last_number', type=int, help='Last ip number of 192.168.0.xxx', default='108')
parser.add_argument('-W', '--img_width', type=int, help='width of captured image', default='320')
parser.add_argument('-H', '--img_height', type=int, help='height of captured image', default='240')
parser.add_argument('-format', '--img_format', type=str, help='Format of the image (jpeg/yuv, rgb)', default='jpeg')
parser.add_argument('-vidport', '--videoport', help='Use video Port True or False', type=int, default=1)
group = parser.add_mutually_exclusive_group()
group.add_argument("-seq", "--sequence", action="store_true", help="use capture_sequence")
group.add_argument("-cont", "--continuous", action="store_true", help="use capture_continuous")
args = parser.parse_args()

#######################################################################################
# Generator yields empty io.BytesIO object to store the captured image from picamera
#######################################################################################
def write_img_to_stream(stream):
    connection.write(struct.pack('<L', stream.tell()))
    connection.flush()
    stream.seek(0)  #seek to location 0 of stream_img
    connection.write(stream.read())  #write to file
    stream.seek(0)
    stream.truncate()


def gen_seq():
    stream = io.BytesIO()
    while True:
        yield stream
        write_img_to_stream(stream)

###########################################################
# setup connection to server
###########################################################
# Connect a client socket to server_ip:8000
client_socket = socket.socket()
ip_address = "192.168.0.{}".format(args.ip_last_number)
client_socket.connect( ( ip_address, 1308 ) )
# Make a file-like object out of the connection
connection = client_socket.makefile('wb')

if __name__ == '__main__':
    try:
        with picamera.PiCamera() as camera:
            if args.img_format != 'jpeg':
                camera.raw_format = args.img_format
            camera.resolution = ( args.img_width, args.img_height ) #default (320, 240)
            # Start a preview and let the camera warm up for 2 seconds
            camera.start_preview()
            time.sleep(2)
            camera.stop_preview()
            print(   args.sequence, args.continuous, args.img_format )
            if args.sequence:
                print("running in sequence")
                camera.capture_sequence(gen_seq(), args.img_format, use_video_port=bool(args.videoport) )
            elif args.continuous:
                print("running in continuous")
                stream = io.BytesIO()
                for img in camera.capture_continuous(stream, args.img_format, use_video_port=bool(args.videoport)):
                    write_img_to_stream(stream)
            else:
                print("running in default")
                camera.capture_sequence(gen_seq(), "jpeg", use_video_port=True)
        connection.write(struct.pack('<L', 0))
    finally:
        connection.close()
        client_socket.close()
{% endhighlight %}

In my case, the ip address of the server has the form `192.168.0.xxx` where xxx is a digit between *100* and *199*. 

### 2. How to run the streaming

* It's important to run the code on the server first, so that the connection is open:

`python server.py`

* Followed by (on the RPi):

`python client.py -W 320 -H 240 -format 'yuv' -vidport 1 -seq`
In this line, we set the image resolution to (*W*idth=320, *H*eight=240)px, the image format is *yuv*, `use_video_port` is set to `True` and `capture_sequence` method is used. 

### 3. The Experiments

Each experiment is made of 100 records, where each record is the number of frames that is received by the laptop after 10 seconds. 


#### 3.1 Continuous versus Sequence

In this first experiment, we will compare 2 streaming methods available with **Picamera**. A stream of images can be captures with either of the following options:

* `capture_continuous()`: Capture images continuously from the camera as an infinite iterator.

command line: `python client.py -W 320 -H 240 -format 'jpeg' -vidport 1 -cont`

* `capture_sequence()`: Capture a sequence of consecutive images from the camera.

command line: `python client.py -W 320 -H 240 -format 'jpeg' -vidport 1 -seq`

In both cases, we use image resolution (320, 240) px, image format: `jpeg` and `use_video_port` set to `True`. 


<img src="/images/{{ page.date | date: "%Y%m%d" }}/seq_cont.png" align="center">

The average fps for `capture_sequence` and `capture_continuous` is fps=28 and fps=23 respectively.

`capture_sequence` results in a larger fps with in average 5 more fps than `capture_continuous`.


#### 3.2 Video Port on/off

`use_video_port` parameter takes 2 values: `True`/`False` and controls whethr to use the video port to capture images.

`use_video_port=True`: `python client.py -W 320 -H 240 -format 'jpeg' -vidport 1 -seq`

`use_video_port=False`: `python client.py -W 320 -H 240 -format 'jpeg' -vidport 0 -seq`

In both cases, we use image resolution (320, 240) px, image format: `jpeg` and `capture_sequence`.

<img src="/images/{{ page.date | date: "%Y%m%d" }}/vid_port.png" align="center">

The average fps when `use_video_port=True` is fps=29, and fps=2 when `use_video_port=False`.

With `use_video_port=True`, we clearly get a boost in fps.


### 3.3 Image format

Now, let's compare the streaming of 3 image formats: `jpeg`, `yuv` and `rgb` with the respective command line:

* `python client.py -W 320 -H 240 -format 'jpeg' -vidport 1 -seq`
* `python client.py -W 320 -H 240 -format 'yuv' -vidport 1 -seq`
* `python client.py -W 320 -H 240 -format 'rgb' -vidport 1 -seq`

<img src="/images/{{ page.date | date: "%Y%m%d" }}/img_format.png" align="center">

The average fps=28 for `jpeg` and `yuv` and fps=19 for `rgb`. Interestingly `yuv` and `jpeg` format results in the same fps, whereas the fps for `rgb` format degrades significantly .

Below, we plot the size of the streamed image for the different formats.

<img src="/images/{{ page.date | date: "%Y%m%d" }}/img_format_sz.png" align="center">

The average image size for `jpeg`, `yuv` and `rgb` is respectively: 67,621 bytes, 115,200 bytes, and 230,400 bytes. The `rgb` format has the largest size, which explains the drop in fps.

With `yuv`, the size of the streamed image is very consistent for the duration of the experiment, however with the `jpeg` format there are noticeable variations in image size.

### Conclusion

In summary, better **MJPEG** streaming performance with the *Picamera library* is achieved with:

* `capture_sequence` 
* `use_video_port=True`. 
* image format, either `YUV` or `JPEG`