---
layout: post
title:  "Vibration Motors"
date:   2019-04-30 01:30:13 +0000
categories: Actuators
tags: Vibration Motors
---

Some info about vibration motors!

## Vibrating Mini Motor D

This Adafruit vibration motor can take voltages from 2V up to 5V, where it will draw from 40mA to 100mA. It will reach speeds of 11k RPM at its peak.  If your circuit can provide more than 100mA, be sure to prevent against it with the use of resistors in series (refer to LED tutorial).

![](/docs/assets/res/vibration_1.png)

* Red wire - Vcc;
* Blue wire - Ground;

### Examples

#### PWM control of the vibration motor
In this code example we're going to apply PWM (refer to the LED tutorial) to control the strength of this motor.

##### Schematic

The wires are thin, so make sure they are well connected to the breadboard!

![](/docs/assets/res/vibration_2.png)

Examples of code be found here: 
<a href="https://github.com/datacentricdesign/wheelchair-design-platform/tree/master/examples/actuators/vibration_motors" target="_blank">/examples/actuators/vibration_motors</a>

##### Results

In the end you should see something like this!

![](/docs/assets/res/vibration_1.gif)
