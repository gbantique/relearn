---
author: George Bantique
date: '2023-03-05T14:47:55+08:00'
guid: https://techtotinker.com/?p=898
id: 898
permalink: /?p=898
title: '003 &#8211; ESP32 MicroPython: General Purpose Input Output | GPIO Pins'
url: /003-esp32-micropython-general-purpose-input-output-gpio-pins657-revision-v1-003-8211-ESP32-MicroPython-General-Purpose-Input-Output-GPIO-Pins
---


**<u>Video Demonstration:</u>**

<div style="clear: both; text-align: left;"><iframe allowfullscreen="" height="260" loading="lazy" src="https://www.youtube.com/embed/9J4YWvHMWf4" width="480" youtube-src-=""></iframe></div>If you like this tutorial and you find it useful, please Share it to your friends. Please consider also supporting me by Subscribing. [Click this to Subscribe to TechToTinker Youtube channel.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

**<u>Source Code:</u>**

**Example 1:**

```
import machine
import time
led = machine.Pin(2, machine.Pin.OUT)
counter = 0
while (counter < 5):
    led.on()
    time.sleep(0.5)
    led.off()
    time.sleep(0.5)
    counter += 1
print('Blinking LED is complete')

```

**Example 2:**

```
import machine
import time
led = machine.Pin(2, machine.Pin.OUT)
def blink_led_ntimes(num, t_on, t_off, msg):
    counter = 0
    while (counter < num):
        led.on()
        time.sleep(t_on)
        led.off()
        time.sleep(t_off)
        counter += 1
    print(msg)

```

**Example 3:**

```
import machine
led = machine.Pin(2, machine.Pin.OUT)
sw = machine.Pin(0, machine.Pin.IN)
while True:
    if (sw.value() == 0):
        led.on()
    else:
        led.off()

```