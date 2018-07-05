---
layout: post
title: How to connect an Accelerometer to a Raspberry Pi
category: "robotics"
subcategory: "hardware"
tags: "accelerometer, rpi, raspberry pi"
img_post: 20180205_accelerometer_rpi.jpg
summary: 
github-link: "na"
---

An accelerometer is a device that measures proper acceleration, i.e the acceleration (or rate of change of velocity) of 
a body in its own instantaneous rest frame. For example, an accelerometer at rest will measure an acceleration 
due to Earth's gravity, straight upwards (by definition) of g ≈ 9.81 m/s2. By contrast, accelerometers in free fall 
(falling toward the center of the Earth at a rate of about 9.81 m/s2) will measure zero.

  * object at rest
$$
a_{true} = \vec{a}_{acc} - \vec{g} = 0
$$

  * falling object
$$
a_{true} = \vec{a}_{acc} - \vec{g} = -\vec{g}
$$ 

The first usage or application of accelerometer in consumer electronics was popularized by Steve Jobs in Apple iPhone. 
Accelerometers have multiple applications in industry and science. Highly sensitive accelerometers are components of 
inertial navigation systems for aircraft and missiles. 

The ADXL34 accelerometer is a multi-axis accelerometer: $$(x, y, z)$$

Single- and multi-axis models of accelerometer are available to detect magnitude and direction of the proper acceleration, as a vector quantity, and can be used to sense orientation (because direction of weight changes), coordinate acceleration, vibration, shock, and falling in a resistive medium (a case where the proper acceleration changes, since it starts at zero, then increases). Micromachined microelectromechanical systems (MEMS) accelerometers are increasingly present in portable electronic devices and video game controllers, to detect the position of the device or provide for game input.



## Connection to Raspberry Pi
  
  * Connections
  
  * GND --> GND

  * 3V --> 3V3
  
  * SDA --> SDA
  
  * SCL --> SCL

ADXL supports both I2C and SPI connections

I use i2C

Here are the steps:

 * 1. Add I2C to the pi config
 `sudo nano /etc/modules`

 and add the followign line:

 `i2c-bcm2708
 i2c-dev`

 * 2 Reboot
 `sudo reboot`

 * 3. Install smbus
 `sudo apt-get install python-smbus i2c-tools git-core`

 *4. Test ADXL345 is found on the `i2c` bus:
 `sudo i2cdetected -y 1`

 You should see a device at address **53**.

 * 5. Clone the repo:
 `https://github.com/pimoroni/adxl345-python 