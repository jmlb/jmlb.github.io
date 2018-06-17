---
layout: post
title: How to connect an Accelerometer to a Raspberry Pi
meta: "deep, learning NLP"
category: flashcards
subcategory: robots
author: "Jean-Marc Beaujour"
img_post: 20180405_word_embedding.jpg
summary: 
github-link: "na"
---

COnnections
GND --> GND
3V --> 3V3
SDA --> SDA
SCL --> SCL

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