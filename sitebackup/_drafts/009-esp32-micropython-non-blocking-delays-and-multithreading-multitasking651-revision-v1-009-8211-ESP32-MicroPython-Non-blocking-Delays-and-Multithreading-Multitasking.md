---
author: George Bantique
date: '2023-03-05T14:44:10+08:00'
guid: https://techtotinker.com/?p=892
id: 892
permalink: /?p=892
title: '009 &#8211; ESP32 MicroPython: Non-blocking Delays and Multithreading | Multitasking'
url: /009-esp32-micropython-non-blocking-delays-and-multithreading-multitasking651-revision-v1-009-8211-ESP32-MicroPython-Non-blocking-Delays-and-Multithreading-Multitasking
---


<div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"><span style="font-family: verdana;">In previous tutorials we learned to use the hardware timer interrupt which is very useful when we need to execute a task at specific interval period. We also use hardware timer interrupts for executing timed threaded task which is similar to multitasking. Using the hardware timer in that manner, sooner or later, we will runout of resources because ESP32 have 4 hardware timer only namely Timer0 to Timer3.</span><span style="font-family: times;"> </span></div>In this video, we will try to explore alternative ways of doing multiple task at almost the same time using the time module.

<div>**<u>  
Circuit Diagram:</u>**</div>![](https://1.bp.blogspot.com/-rYdcCBcd2K0/X26oBPSJclI/AAAAAAAACDc/jXve0NeUYHcXtYScTmdbod5hMptYgfJnwCLcBGAsYHQ/w640-h322/MP_009_Time.png)

**<u>Instruction:</u>**  
1\. Connect the ESP32 development board to the breadboard.  
2\. Connect the cathode lead of the red LED to the ground while the anode pin through limiting resistor to GPIO D27 of ESP32.  
3\. Connect the cathode lead of the green LED to the ground while the anode pin through limiting resistor to GPIO D26 of ESP32.  
4\. Connect the cathode lead of the blue LED to the ground while the anode pin through limiting resistor to GPIO D25 of ESP32.  
5\. Connect the ‘mode’ tactile switch with one pin to the ground while the other to GPIO D33 of ESP32.  
6\. Connect the ‘left’ tactile switch with one pin to the ground while the other to GPIO D32 of ESP32.  
7\. Connect the ‘rght’ tactile switch with one pin to the ground through a 10K resistor while the other to GPIO D35 of ESP32.  
8\. Connect the ‘entr’ tactile switch with one pin to the ground through a 10K resistor while the other to GPIO D34 of ESP32.

***Pins D32 and D33 of ESP32 has internal pull-up resistor so take advantage of it while for pins D34 and D35 the internal pull-ups is NOT PRESENT so we use external pull-up resistor of 10 kilo Ohms which works exactly the same. The button switches are configured as ACTIVE-LOW, meaning the value when press is LOW or 0.***

**<u>Video Description:</u>**

<div style="clear: both; text-align: left;"><iframe allowfullscreen="" height="260" loading="lazy" src="https://www.youtube.com/embed/DaRYiKsfdZk" width="480" youtube-src-=""></iframe></div>If you have any question regarding this tutorial, please do not hesitate to write it in the comment box provided.

You may also like to Subscribe to my Youtube Channel. [Please click this to Subscribe to TechToTinker Youtube Channel.  ](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)  
Thank you and have a good days ahead.

**<u>Source Code:</u>**

**Example # 1 Blinking an LED, Blocking Code:**

```

import machine
import time

led = machine.Pin(2, machine.Pin.OUT)

while True:
<span>    </span>led.on()
    time.sleep(0.5)
    led.off()
    time.sleep(0.5)

```

**Example # 2 Blinking an LED, Non-Blocking Code:**

```

import machine
import time

led = machine.Pin(2, machine.Pin.OUT)

start_time = time.ticks_ms()
interval = 500      # 500 ms interval
led_state = 0

while True:
  if ( time.ticks_ms() - start_time > interval ):
    if led_state == 1:
      led_state = 0
    else:
      led_state = 1
    led.value(led_state)
    start_time = time.ticks_ms()


```

**Example # 3 Multi-threading or doing multiple task at almost the same time:**

```

import machine
import time

red = machine.Pin(27, machine.Pin.OUT)
grn = machine.Pin(26, machine.Pin.OUT)
blu = machine.Pin(25, machine.Pin.OUT)

mode = machine.Pin(33, machine.Pin.IN, machine.Pin.PULL_UP)
left = machine.Pin(32, machine.Pin.IN, machine.Pin.PULL_UP)
rght = machine.Pin(35, machine.Pin.IN)
entr = machine.Pin(34, machine.Pin.IN)

r_start = time.ticks_ms()
g_start = time.ticks_ms()
b_start = time.ticks_ms()
k_start = time.ticks_ms()

r_interval = 300
g_interval = 500
b_interval = 700
k_interval = 200

state = 0
EDIT_RESOLUTION = 10
reset = 0

print('**************************')
print('  DEFAULT Interval Values ')
print('--------------------------')
print('Red interval:', r_interval, 'ms')
print('Grn interval:', g_interval, 'ms')
print('Blu interval:', b_interval, 'ms')
print('**************************')
            
while True:
    if time.ticks_ms() - r_start >= r_interval:
        red.value( not red.value() )
        r_start = time.ticks_ms()
    if time.ticks_ms() - g_start >= g_interval:
        grn.value( not grn.value() )
        g_start = time.ticks_ms()
    if time.ticks_ms() - b_start >= b_interval:
        blu.value( not blu.value() )
        b_start = time.ticks_ms()
    if time.ticks_ms() - k_start >= k_interval:
        k_start = time.ticks_ms()
        
        if mode.value()==0:
            if state==0: # idle mode
                state = 1
                print()
                print('*************')
                print('Red edit mode')
                print('-------------')
            elif state==1: # red edit mode
                state = 2
                print()
                print('*************')
                print('Grn edit mode')
                print('-------------')
            elif state==2: # grn edit mode
                state = 3
                print()
                print('*************')
                print('Blu edit mode')
                print('-------------')
            elif state==3: # blu edit mode
                state = 0
                print()
                print('*************')
                print('Idle mode')
                print('-------------')
                
        if left.value()==0:
            if   state==1: # red edit mode
                if r_interval - EDIT_RESOLUTION > 0:
                    r_interval -= EDIT_RESOLUTION
                print('Red interval:', r_interval, 'ms')
            elif state==2: # grn edit mode
                if g_interval - EDIT_RESOLUTION > 0:
                    g_interval -= EDIT_RESOLUTION
                print('Grn interval:', g_interval, 'ms')
            elif state==3: # blu edit mode
                if b_interval - EDIT_RESOLUTION > 0:
                    b_interval -= EDIT_RESOLUTION
                print('Blu interval:', b_interval, 'ms')
                    
        if rght.value()==0:
            if   state==1: # red edit mode
                r_interval += EDIT_RESOLUTION
                print('Red interval:', r_interval, 'ms')
            elif state==2: # grn edit mode
                g_interval += EDIT_RESOLUTION
                print('Grn interval:', g_interval, 'ms')
            elif state==3: # blu edit mode
                b_interval += EDIT_RESOLUTION
                print('Blu interval:', b_interval, 'ms')
        
        if entr.value()==0:
            r_interval = 300
            g_interval = 500
            b_interval = 700
            print()
            print('**************************')
            print('Values RESETTED to DEFAULT')
            print('--------------------------')
            print('Red interval:', r_interval, 'ms')
            print('Grn interval:', g_interval, 'ms')
            print('Blu interval:', b_interval, 'ms')
            print('**************************')


```