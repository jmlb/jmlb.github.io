---
layout: post
title: Portrait matting on live video stream from the client browser
category: tensorflowjs
tags: DeepLearning lstm rnn keras
author: "Jean-Marc Beaujour"
img_post: 20180624-zoom_premium.png
summary: "Zoom premium is a semantic segmentation app that is tasked to remove or replace the background in webcam live video stream. The Deep Learning model is based on MobileNet and UNet, and is a consequence a small footprint model with about 6 million parameters. This demo shows how inference conducted exclusively on the client browser can be performed even for ressource heavy tasks such as semantic segmentation. Have fun!"
github-link: "na"
---

<div style="text-align: justify; border-left: 1.0rem solid black; padding-left: 20px; margin-top:5rem"><span style="font-size: 1.0em;"> Zoom premium is a semantic segmentation app that is tasked to remove or replace the background in webcam live video stream. The Deep Learning model is based on MobileNet and UNet, and as a result it has a small footprint: about 6 million parameters. This demo shows how inference conducted exclusively on the client browser can be performed even for ressource heavy tasks such as semantic segmentation.
<br>
<br>
For more info about how I build the app check out the post.<br>
Have fun!
</span>
</div>

<div id="status">Loading model...</div>

<div class="controller-panels" id="controller" style="display:none">

  <!-- Big buttons. -->
  
<div style="text-align:center; width: 90%; padding: 0px; border: 2px solid #C0C0C0; margin:0 auto; background-color: #303030">
<div class="panel-row big-buttons" >
  <br>
  <br>
    <span style="font-size: 3.0em; text-shadow: 2px 2px #404040; color: #000; font-weight: bold; width:100%; text-align: center">Zoom <font style="color: #FFD700;"> Premium</font> <br><br></span>
  
  </div><!-- /.panel-row -->

  <!-- container video row -->
  <div style="margin: 0px; padding: 0px; border: 1px solid rgba(255, 255, 255, 0.75); text-align:center; float: center" id="container-vid">
    <!-- container video Original -->
    <!-- menu -->
    <div style="float: left; display: inline-block; margin-left: 1 1 1 1; padding: 0px; border: 1px solid black; text-align: center; color: #E0E0E0; font-size: 0.8rem; padding-top: 5px">
    Settings
    <br>
    <br>
    <span> <button id="show_original" style="background-color: #f44336; width: 100px; border-radius: 2px; font-size: 1.1rem"> OFF </button></span>
    <span><button id="predict" style="background-color: #4CAF50; width: 100px; border-radius: 2px;font-size: 1.1rem; text-align: center"> ON </button></span>
      <br>
      <br>
        <fieldset>
    <legend style="font-size: 0.8rem; text-align: left; color: #E0E0E0;">Custom BCK</legend>
    
    <div class="control">
        <select name="custom_bkg" id="custom_bkg" style="color: black; width: 120px; font-size:1rem" tabindex="1">
        <option value="black" selected="selected">Black</option>
        <option value="/tensorflowjs/zoom_premium/dist/b0b9a8a6f35db0b62e443d7853853e90.jpg">library</option>
        <option value="/tensorflowjs/zoom_premium/dist/d4c0ef68b6db24826922c55f33919665.jpg">office</option>
        <option value="/tensorflowjs/zoom_premium/dist/09f913c68ab75115d26ec227daad16c7.jpeg">beach</option>
        <option value="/tensorflowjs/zoom_premium/dist/80b8bee8f22deddcd2715c4d59c1a485.jpg">nature</option>
</select>
  </div>

  </fieldset>
    </div>

    <div style="float: left; display: inline-block; margin-left: 1 1 1 1; padding: 0px; border: 1px solid rgba(255, 255, 255, 0.50); text-align: center; color:#808080; font-size: 1.4rem">
    Original Video feed<br><br>
        <video autoplay="" playsinline="" muted="" id="webcam" width="224px" height="224px" style="align: center"></video>
    </div>
    <!-- ./container video Original -->

    <!-- container video Matted -->
    <div style="width:240px; margin: 5px; margin-top: 0px; padding: 0 0 0 0; border: 1px solid rgba(255, 255, 255, 0.50); display: inline-block; text-align: center; color:#808080; font-size: 1.4rem">
      Matted frame<br><br>
      <canvas id="combo_class" width="224px" height="224x"></canvas>
    </div>
    <!-- container video Matted -->

    <!-- container video Mask
    <div style="width:240px; margin: 5px; margin-top: 0px; padding: 0 0 0 0; float: right; display: inline-block; text-align: center; border: 1px solid rgba(255, 255, 255, 0.50); color:#808080; font-size: 1.4rem">
    Inference mask<br><br>
      <canvas id="mask_class" width="224px" height="224px; "></canvas>
    </div> -->
    <!-- container video Mask -->

  <br>
  <br>
  FPS: <span id="fps"><span>
  <br>
  <span style="font-size: 0.8em; color: #E0E0E0;">Select a background using the drop down menu. Then, click the [ON] button to show the matted frame.</span>
  <!-- store the background image -->

  </span></span></div>
  <!-- ./container video row -->
</div>

</div><!-- /#controller -->


<!--
https://www.torontopubliclibrary.ca/content/branches/images/dawes-road-library-exterior.jpg
-->
  <script src="/tensorflowjs/zoom_premium/dist/tfjs-examples-webcam-transfer-learning.js"></script>