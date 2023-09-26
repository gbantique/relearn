---
author: George Bantique
date: '2023-03-05T14:47:21+08:00'
guid: https://techtotinker.com/?p=897
id: 897
permalink: /?p=897
title: '004 &#8211; ESP32 MicroPython: External Interrupts'
url: /004-esp32-micropython-external-interrupts656-revision-v1-004-8211-ESP32-MicroPython-External-Interrupts
---


**<u> Circuit Diagram for Example 3:</u>**

<div style="clear: both; text-align: center;">[![](https://1.bp.blogspot.com/-roaiYuUfiWU/X1ne2ImRuPI/AAAAAAAACBw/3jYSuGSE9WoNCSjT3p_nbO5VqRL61QwfgCLcBGAsYHQ/w400-h355/uP_interrupt_bb.png)](https://1.bp.blogspot.com/-roaiYuUfiWU/X1ne2ImRuPI/AAAAAAAACBw/3jYSuGSE9WoNCSjT3p_nbO5VqRL61QwfgCLcBGAsYHQ/s1269/uP_interrupt_bb.png)</div>**<u>  
</u>**

**<u>  
</u>**

**<u>  
</u>**

**<u>  
</u>**

**<u>  
</u>**

**<u>  
</u>**

**<u>  
</u>**

**<u>  
</u>**

<u style="font-weight: bold;">Video Demonstration:</u>

<div style="clear: both; text-align: left;"><iframe allowfullscreen="" height="260" loading="lazy" src="https://www.youtube.com/embed/HhoQdoQMNWo" width="480" youtube-src-=""></iframe></div>**<u>  
Source Code:</u>**

**Example 1, Simple Polling:**

```
"""
*** Simple Polling Method of reading an input ***

	Author: George V. Bantique, TechToTinker
	Date: September 10, 2020
	Description: The switch or the BOOT switch is
		polled until such time that the switch
		is detected at logic LOW
		When the switch is press, it will blink
		the onboard LED on GPIO2 for 7 times then
		will send a 'Done.' message before it exit
"""
import machine
import time

led = machine.Pin(2, machine.Pin.OUT)
sw = machine.Pin(0, machine.Pin.IN)

def blink_led_ntimes(num, t_on, t_off, msg):
    counter = 0
    while (counter < num):
        led.on()
        time.sleep(t_on)
        led.off()
        time.sleep(t_off)
        counter += 1
    print (msg)

while True:
    if (sw.value() == 0):
        blink_led_ntimes(7, 0.25, 0.50, 'Done.')



```

**Example 2, Simple Interrupt:**

```
"""
*** Simple Interrupt Method of reading an input ***

	Author: George V. Bantique, TechToTinker
	Date: September 10, 2020
	Description: The switch or the BOOT switch is
		attached to interrupt for efficiency.
		When the interrupt is triggered, it will
		toggle the state of the onboard LED
		on GPIO2.
"""
import machine

led = machine.Pin(2, machine.Pin.OUT)
sw = machine.Pin(0, machine.Pin.IN)

def handle_interrupt(pin):
    led.value(not led.value())
    
sw.irq(trigger=machine.Pin.IRQ_FALLING, handler=handle_interrupt)

```

**Example 3, DC motor with limit switch using interrupts:**

```
""" 
*** DC motor with limit switch using interrupts ***
	Author: George V. Bantique, TechToTinker
	Date: September 10, 2020
	Description: The direction of rotation of the DC motor
		is controlled by the limit switch in the
		left or right side. L298N motor driver module
		is use to isolate and protect the ESP32.
"""

import machine

sw1 = machine.Pin(15, machine.Pin.IN, machine.Pin.PULL_UP)
sw2 = machine.Pin(21, machine.Pin.IN, machine.Pin.PULL_UP)
dr1 = machine.Pin(22, machine.Pin.OUT)
dr2 = machine.Pin(23, machine.Pin.OUT)

press = False
irq_pin = 0

def handle_interrupt(pin):
    global press
    press = True
    global irq_pin
    irq_pin = int(str(pin)[4:-1])

sw1.irq(trigger=machine.Pin.IRQ_FALLING, handler=handle_interrupt)
sw2.irq(trigger=machine.Pin.IRQ_FALLING, handler=handle_interrupt)

while True:
    if press:
        print(irq_pin)
        press = False
        
        if irq_pin == 15:
            dr1.value(0)
            dr2.value(1)
            print('counter')
        elif irq_pin == 21:
            dr1.value(1)
            dr2.value(0)
            print('clockwise')
        else:
            pass

```