---
author: George Bantique
date: '2023-03-05T14:44:51+08:00'
guid: https://techtotinker.com/?p=893
id: 893
permalink: /?p=893
title: '008 &#8211; ESP32 MicroPython: Hardware Timer Interrupts'
url: /008-esp32-micropython-hardware-timer-interrupts652-revision-v1-008-8211-ESP32-MicroPython-Hardware-Timer-Interrupts
---


In this tutorial, we will learn how to use the hardware Timer Interrupts of ESP32 in MicroPython. Timer in essence is basically a counter that either counts up or counts down. Timers can be use to trigger an interrupt event when an overflow happened or a certain count value is reached.

<u style="font-weight: bold;">  
Circuit Diagram:</u>

[![](https://1.bp.blogspot.com/-b-0wMRPDxjY/X26dU-s3CAI/AAAAAAAACDI/2fNriDTwwlcBlWT9A6RnsnJCfEI4ID_vQCLcBGAsYHQ/w373-h400/MP_008_servo.png)](https://1.bp.blogspot.com/-b-0wMRPDxjY/X26dU-s3CAI/AAAAAAAACDI/2fNriDTwwlcBlWT9A6RnsnJCfEI4ID_vQCLcBGAsYHQ/s657/MP_008_servo.png)

<u style="font-weight: bold;">Video Description:</u>

<div style="clear: both; text-align: left;"><iframe allowfullscreen="" height="260" loading="lazy" src="https://www.youtube.com/embed/lV7Rju9Cvo0" width="480" youtube-src-=""></iframe></div>If you have any question or suggestion, please do not hesitate to write it in the comment box provided.

<div><div>If you find this article as helpful, please kindly consider supporting my Youtube channel by Subscribing. [Please click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)**<u>Source Code:</u>**

**Example # 1 Simple Timer:**

</div>```
import machine

led = machine.Pin(2, machine.Pin.OUT)

tim0 = machine.Timer(0)

def handle_callback(timer):
    led.value( not led.value() )

# tim0.init(period=2000, mode=machine.Timer.ONE_SHOT, callback=lambda t: led.value(not led.value()))
tim0.init(period=50, mode=machine.Timer.PERIODIC, callback=handle_callback)


```

 **Example # 2 Hardware Timer Interrupts use for multithreading:**

```
# Load the machine module
import machine

# 1. Create the GPIO object as
#    normal GPIO pin assignment 
p23 = machine.Pin(23, machine.Pin.OUT)
p22 = machine.Pin(22, machine.Pin.OUT)

# 2. Create the servo object and
#    attach the pwm driver to the GPIO pin
b_servo = machine.PWM(p23)
s_servo = machine.PWM(p22)
#    Initialize the servo object with
#    20ms pulse interval and set the
#    angle to 0 degree
b_servo.init(freq=50, duty=20)
s_servo.init(freq=50, duty=20)

# 3. Create a timer object for both servo
timB = machine.Timer(0)
timS = machine.Timer(1)
#    Functions to handle the Timer callbacks
def handle_timB(timer):
    global is_timB
    is_timB = True
def handle_timS(timer):
    global is_timS
    is_timS = True
#    and initialize the timer objects
timB.init(period=200, mode=machine.Timer.PERIODIC, callback=handle_timB)
timS.init(period=100, mode=machine.Timer.PERIODIC, callback=handle_timS)

# Create a global variables
is_timB = False
is_timS = False
b_angle = 0
s_angle = 0

# Functions:
# Converts a value using map function of Arduino
# this basically functions like by ratio and proportion
def map(x, in_min, in_max, out_min, out_max):
    return int((x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min)
# Function to convert an angle to a duty value
def servo(pin, angle):
    pin.duty(map(angle, 0, 180, 20, 120))
    
# Main loop
while True:
    # Checks if the Timer interrupt sets the global variable
    if is_timB==True:
        is_timB = False         # disable the flag
        servo(b_servo, b_angle) # rotate the servo
        if b_angle < 180:       # if the angle is less than 180 
            b_angle += 10        # increment the angle
    if is_timS==True:
        is_timS = False
        servo(s_servo, s_angle)
        if s_angle < 180:
            s_angle += 5


```

</div>