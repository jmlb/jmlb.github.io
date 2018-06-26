---
layout: post
title: Installation of a IMU
meta: self-driving, autonomous, raspberry-pi, rpi, arduino, imu, sonic
category: robotics
author: "Jean-Marc Beaujour"
img_post: install_imu.jpg
summary: 
github-link: "na"
---

The next step in building **SONIC** is the installation of a **Inertial Measurement Unit** (IMU). The IMU will enable to determine the Roll, Pitch and Heading/Yaw angles. The details on how to calculate those angles from IMU data is the subject of [another post](/2017-07-02-roll_pitch_yaw_angles.md). 
In this post, I focus on the installation of the IMU and on setting up the libraries and the Arduino sketch to collect the raw data.


## Initial Measurement Unit

<div style="-webkit-column-count: 2; -moz-column-count: 2; column-count: 2; -webkit-column-rule: 1px dotted #e0e0e0; -moz-column-rule: 1px dotted #e0e0e0; column-rule: 1px dotted #e0e0e0;">
    <div style="display: inline-block; float:left;">
       The IMU includes 3 different sensors:<br>
       1. <b>Gyroscope</b>: the gyroscope measures the angular velocity (in deg/sec or revolution/sec) about the 3 axis X, Y, Z. The gyroscope has a vibrating structure to determine the rate of rotation. To keep track of the angles, we need to sum all of the rates of change over a given period of time. This is basically the integrale of the gyro data<br>
       [dessin]

       omega_tilt = omega + b + eta

       omega = true angular velocity, b=bias (temperature depednent and not static) and eta = zero mean Gaussian Noise

From Gyro measurement to orientation using Taylor expansion:

theta(t + delta t) ~ theta(t) + theta_dot(t) * delta_t + o(delta t^2)

the last term is the approximation error
theta_dot is the gyro angular velocity

Pros: accurate in short term, but not reliable in long term due to drift. Bias and noise variance can be estimated, other sensor measurementsw used to correct for drift (sensor fusion)
Big challenge with gyro alone: drift over time due to integration error (mostly).
<br>

       2. <b>Accelerometer</b>: measures linear acceleration in m/s^2 or g-unit. The acceleration is differente of coordinate acceleration. The most significant of the sensed acceleration is caused by gravity. With that information, we can calculate what direction the object is facing<br> 
[dessin]
The linear acceleration: a = a_g + a_i
without motion: a = a_g pointing down ||a|| = 9.81 m/s2=1g
With motion: a_g + a_i

Pros: accurate in long term because no drift (earth gravity does not change)
Problem: noisy measurement, unreliable in short run due to motion.


<br>
       3. <b>Magnetometer</b>: it measures magnetic field strength in Gauss or Tesla. The chip is used as a digital compass to sense the angle from magnetic north (not the true north) in degrees. Use earth magnetic field in Gauss or Tesla. Actual direction depdens on lattitude longitude. Distorsion due to metal/electronics.
       Difficult to work with magnetometer without proper calibration
       Pros: magnetometer complementary of accelerometer gives heading.
       Cons: affected by metal, 

    </div>
    <div style="display: inline-block; ">
        <i>To learn more about IMU, check out this video:</i>
        <a href="https://www.youtube.com/watch?v=eqZgxR6eRjo"><img src="/images/20170630/youtube_screenshot.png" width ="500px" alt="Hall sensor explained"></a>
    </div>
</div>

<br>


## IMU part

<div style="-webkit-column-count: 2; -moz-column-count: 2; column-count: 2; -webkit-column-rule: 1px dotted #e0e0e0; -moz-column-rule: 1px dotted #e0e0e0; column-rule: 1px dotted #e0e0e0;">
    <div style="display: inline-block; float:left;">
       The IMU is a <a href="https://www.adafruit.com/product/1714?gclid=CjwKEAjws-LKBRDCk9v6_cnBgjISJAADkzXeNPktEisTTU6cSD_evsHz24OzJgVzMmk8G-Euk25JgBoCTRjw_wcB">Adafruit 9-DOF IMU Breakout (L3GD20H + LSM303)</a>. It costs about $19. The shipping fee from <a href=-"https://www.adafruit.com"> Adafruit</a> is insane, so I ended up ordering the part from another vendor (<a href="www.arrow.com">arrow.com</a>). They offer free shipping and I receive the IMU, only 2 days after placing the order. <br>
       This IMU includes a 3 axes accelerometer, 3 axis gyroscope and 3 axis magnetometer, resulting in 9 axes of data.

    </div>
    <div style="display: inline-block; ">
        <img src="/images/20170630/imu01.png" width="150px">
        <img src="/images/20170630/imu02.png" width="150px">
    </div>
</div>

<br>
Here are the specs:

- L3GD20H 3-axis gyroscope: ±250, ±500, or ±2000 degree-per-second scale
- LSM303 3-axis compass: ±1.3 to ±8.1 gauss magnetic field scale
- LSM303 3-axis accelerometer: ±2g/±4g/±8g/±16g selectable scale



## Where to place the IMU and arduino connections?

The best is to place the IMU far from any of source of magnetic field that could interfere with the Magnetometer. Magnetic material such as Iron, steel can also distort field lines. At the back of the car, we have the motor which uses a strong permanent magnet, so that's a no-no. A good position for the IMU is at the front of the car, centered with the 2 front wheels. The signal pins to be connected are SDA and SDL which are plugged into the Analog Pin 4 (A4) and Pin 5 (A5) respectively.

<img src="/images/20170630/install_imu01.jpg" width="300px"> | <img src="/images/20170630/connect.png" width="400px">


## Software
Adafruit did a good job detailing how to get started with the IMU (check out their [tutorial](https://learn.adafruit.com/adafruit-9-dof-imu-breakout/software). We need to download 4 zip files:

[Adafruit Sensor Library](https://github.com/adafruit/Adafruit_Sensor/archive/master.zip) | [Adafruit LSM303 Library](https://github.com/adafruit/Adafruit_LSM303DLHC/archive/master.zip) | [Adafruit L3GD20 Library](https://github.com/adafruit/Adafruit_L3GD20_U/archive/master.zip) | [Adafruit 9DoF Library](https://github.com/adafruit/Adafruit_9DOF/archive/master.zip)

and then unzip them in the **library** folder of the Arduino.

<script src="https://gist.github.com/jmlb/464f88be70fb9444cb822003ca6cccae.js"></script>

<img src="/images/20170630/serial_monitor.png" width="600">

