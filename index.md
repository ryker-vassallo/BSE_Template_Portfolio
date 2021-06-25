
﻿# Phone Controlled Tilt Maze
This project utilizes the gyroscope in a phone and multiple servo motors to control a marble tilting maze.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Ryker V | The Nueva School | Mechanical Engineering | Incoming Junior

  
# Final Milestone
FILL
# Second Milestone
FILL
# First Milestone
[![Video1](https://res.cloudinary.com/marcomontalbano/image/upload/v1624641303/video_to_markdown/images/youtube--xaeBMSuE92U-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=xaeBMSuE92U&feature=youtu.be&scrlybrkr=7484f1f7&ab_channel=BlueStampEng "Video1")
My first milestone in this project was designing my tilt maze and my servo circuit. Although my initial plan was to create the maze out of cardboard and other low quality materials, I decided to attempt to construct it with legos. This provided me with both additional stability while building, and unique parts that helped immensely with the design. Along with the maze itself, I also designed a circuit similar to the one I will eventually use. I utilized the sample "knob" code for controlling a servo with a potentiometer and adapted it to two servos with two potentiometers. Understanding this relationship is vital, as my project also requires two servos working together.
# Photos
![circuit](https://user-images.githubusercontent.com/86121632/123455891-14c27b80-d597-11eb-95dd-bb2e9d7f4ce4.png)

# Code
<pre>
<font color="#434f54">&#47;&#47;This example code is in the Public Domain (or CC0 licensed, at your option.)</font>
<font color="#434f54">&#47;&#47;By Evandro Copercini - 2018</font>
<font color="#434f54">&#47;&#47;</font>
<font color="#434f54">&#47;&#47;This example creates a bridge between Serial and Classical Bluetooth (SPP)</font>
<font color="#434f54">&#47;&#47;and also demonstrate that SerialBT have the same functionalities of a normal Serial</font>

<font color="#5e6d03">#include</font> <font color="#005c5f">&#34;BluetoothSerial.h&#34;</font>
<font color="#5e6d03">#include</font> <font color="#434f54">&lt;</font><font color="#000000">ESP32Servo</font><font color="#434f54">.</font><font color="#000000">h</font><font color="#434f54">&gt;</font> 

<font color="#5e6d03">#if</font> <font color="#434f54">!</font><font color="#000000">defined</font><font color="#000000">(</font><font color="#000000">CONFIG_BT_ENABLED</font><font color="#000000">)</font> <font color="#434f54">||</font> <font color="#434f54">!</font><font color="#000000">defined</font><font color="#000000">(</font><font color="#000000">CONFIG_BLUEDROID_ENABLED</font><font color="#000000">)</font>
<font color="#5e6d03">#error</font> <font color="#000000">Bluetooth</font> <font color="#000000">is</font> <font color="#5e6d03">not</font> <font color="#000000">enabled</font><font color="#434f54">!</font> <font color="#000000">Please</font> <font color="#d35400">run</font> <font color="#000000">`make</font> <font color="#000000">menuconfig`</font> <font color="#000000">to</font> <font color="#5e6d03">and</font> <font color="#000000">enable</font> <font color="#000000">it</font>
<font color="#5e6d03">#endif</font>

<b><font color="#d35400">BluetoothSerial</font></b> <font color="#d35400">SerialBT</font><font color="#000000">;</font>
<font color="#00979c">const</font> <font color="#00979c">int</font> <font color="#000000">servoPinRoll</font> <font color="#434f54">=</font> <font color="#000000">18</font><font color="#000000">;</font>
<font color="#00979c">const</font> <font color="#00979c">int</font> <font color="#000000">servoPinPitch</font> <font color="#434f54">=</font> <font color="#000000">19</font><font color="#000000">;</font>
<font color="#00979c">int</font> <font color="#000000">receivedInt</font><font color="#000000">;</font>
<font color="#00979c">int</font> <font color="#000000">updatedInt</font><font color="#000000">;</font>
<b><font color="#d35400">Servo</font></b> <font color="#000000">servoPitch</font><font color="#000000">;</font>
<b><font color="#d35400">Servo</font></b> <font color="#000000">servoRoll</font><font color="#000000">;</font>

<font color="#00979c">void</font> <font color="#5e6d03">setup</font><font color="#000000">(</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#000000">115200</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">SerialBT</font><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#005c5f">&#34;ESP32test&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#434f54">&#47;&#47;Bluetooth device name</font>
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;The device started, now you can pair it with bluetooth!&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">ESP32PWM</font><font color="#434f54">:</font><font color="#434f54">:</font><font color="#000000">allocateTimer</font><font color="#000000">(</font><font color="#000000">0</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">ESP32PWM</font><font color="#434f54">:</font><font color="#434f54">:</font><font color="#000000">allocateTimer</font><font color="#000000">(</font><font color="#000000">1</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">ESP32PWM</font><font color="#434f54">:</font><font color="#434f54">:</font><font color="#000000">allocateTimer</font><font color="#000000">(</font><font color="#000000">2</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">ESP32PWM</font><font color="#434f54">:</font><font color="#434f54">:</font><font color="#000000">allocateTimer</font><font color="#000000">(</font><font color="#000000">3</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">servoRoll</font><font color="#434f54">.</font><font color="#000000">setPeriodHertz</font><font color="#000000">(</font><font color="#000000">50</font><font color="#000000">)</font><font color="#000000">;</font><font color="#434f54">&#47;&#47; Standard 50hz servo</font>
 &nbsp;<font color="#000000">servoPitch</font><font color="#434f54">.</font><font color="#000000">setPeriodHertz</font><font color="#000000">(</font><font color="#000000">50</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">servoRoll</font><font color="#434f54">.</font><font color="#d35400">attach</font><font color="#000000">(</font><font color="#000000">servoPinRoll</font><font color="#434f54">,</font> <font color="#000000">500</font><font color="#434f54">,</font> <font color="#000000">2400</font><font color="#000000">)</font><font color="#000000">;</font> 
 &nbsp;<font color="#000000">servoPitch</font><font color="#434f54">.</font><font color="#d35400">attach</font><font color="#000000">(</font><font color="#000000">servoPinPitch</font><font color="#434f54">,</font> <font color="#000000">500</font><font color="#434f54">,</font> <font color="#000000">2400</font><font color="#000000">)</font><font color="#000000">;</font> 
<font color="#000000">}</font>

<font color="#00979c">void</font> <font color="#5e6d03">loop</font><font color="#000000">(</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;<font color="#000000">receivedInt</font> <font color="#434f54">=</font> <font color="#000000">(</font><font color="#00979c">int</font><font color="#000000">)</font><font color="#d35400">SerialBT</font><font color="#434f54">.</font><font color="#d35400">read</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">available</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#d35400">SerialBT</font><font color="#434f54">.</font><font color="#d35400">write</font><font color="#000000">(</font><b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">read</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#d35400">SerialBT</font><font color="#434f54">.</font><font color="#d35400">available</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#d35400">SerialBT</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34;Received:&#34;</font><font color="#000000">)</font><font color="#000000">;</font><font color="#434f54">&#47;&#47; write on BT app</font>
 &nbsp;&nbsp;&nbsp;<font color="#d35400">SerialBT</font><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#000000">receivedInt</font><font color="#000000">)</font><font color="#000000">;</font><font color="#434f54">&#47;&#47; write on BT app &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font> <font color="#000000">(</font><font color="#005c5f">&#34;Received:&#34;</font><font color="#000000">)</font><font color="#000000">;</font><font color="#434f54">&#47;&#47;print on serial monitor</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#000000">receivedInt</font><font color="#000000">)</font><font color="#000000">;</font><font color="#434f54">&#47;&#47;print on serial monitor &nbsp;&nbsp;&nbsp;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">receivedInt</font> <font color="#434f54">&gt;</font> <font color="#000000">1000</font><font color="#000000">)</font><font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">updatedInt</font> <font color="#434f54">=</font> <font color="#000000">receivedInt</font> <font color="#434f54">-</font> <font color="#000000">1000</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">servoRoll</font><font color="#434f54">.</font><font color="#d35400">write</font><font color="#000000">(</font><font color="#000000">updatedInt</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">}</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">receivedInt</font> <font color="#434f54">&lt;</font> <font color="#000000">1000</font><font color="#000000">)</font><font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">servoPitch</font><font color="#434f54">.</font><font color="#d35400">write</font><font color="#000000">(</font><font color="#000000">receivedInt</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;
<font color="#000000">}</font>

</pre>
