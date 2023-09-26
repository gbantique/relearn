---
author: George Bantique
date: '2023-03-05T14:46:53+08:00'
guid: https://techtotinker.com/?p=896
id: 896
permalink: /?p=896
title: '005 &#8211; ESP32 MicroPython: Pulse Width Modulation'
url: /005-esp32-micropython-pulse-width-modulation655-revision-v1-005-8211-ESP32-MicroPython-Pulse-Width-Modulation
---


In this tutorial, we will learn to use the PWM of ESP32 in MicroPython. PWM stands for Pulse Width Modulation.

**<u>Circuit Diagram:</u>**  
[![](https://1.bp.blogspot.com/-cBMVi23uA8A/X2Fz-gnsdqI/AAAAAAAACCg/WIEimPw2e_crptZIIdSEK5uQUc994ES6wCLcBGAsYHQ/w356-h400/pwm_dcmotor.png)](https://1.bp.blogspot.com/-cBMVi23uA8A/X2Fz-gnsdqI/AAAAAAAACCg/WIEimPw2e_crptZIIdSEK5uQUc994ES6wCLcBGAsYHQ/s618/pwm_dcmotor.png)

**<u>  
</u>**

**<u>Video Demonstration:  
</u><iframe allowfullscreen="" height="260" loading="lazy" src="https://www.youtube.com/embed/0SHE2UMJq6Y" width="480" youtube-src-=""></iframe>**

  
<u>  
</u>**If you find this tutorial helpful, please consider Subscribing to my Youtube channel by [clicking this link to SUBSCRIBE to TechToTinker Youtube channel.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you.

**<u>Source Code:</u>**

```
# DC motor speed control using PWM in MicroPython
# Author: George Bantique, TechToTinker
# Date: September 13, 2020

# Load the machine module which also
# includes the pwm class
import machine

# Create the drives object for the direction
# of rotation of the DC motor
dr1 = machine.Pin(21, machine.Pin.OUT)
dr2 = machine.Pin(19, machine.Pin.OUT)

# Create the en1 object as normal GPIO
en1 = machine.Pin(18, machine.Pin.OUT)

# Create a pwm object and attach it to
# pwm drivers
pwm = machine.PWM(en1)

# Rotates the DC motor clockwise
def cw():
    dr1.value(1)
    dr2.value(0)
    
# Rotates the DC motor counter-clockwise    
def ccw():
    dr1.value(0)
    dr2.value(1)

# Starts the pwm with a definite value
# and start the motor rotating according 
# to the input argument
def start(rotation):
    pwm.init(freq=1, duty=512)
    if (rotation=='cw'):
        cw()
    elif (rotation=='ccw'):
        ccw()

# Stops the pwm using pwm.deinit() function
# which dettach the pwm driver to the en1 object
# We also provide the drives pin a logic low value
def stop():
    pwm.deinit()
    dr1.value(0)
    dr2.value(0)
    

```