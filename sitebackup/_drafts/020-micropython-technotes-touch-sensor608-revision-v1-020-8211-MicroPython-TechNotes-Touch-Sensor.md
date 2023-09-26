---
author: George Bantique
date: '2023-03-13T19:17:27+08:00'
excerpt: In this article, we will learn how to use a touch sensor module with ESP32 using MicroPython programming language.
guid: https://techtotinker.com/?p=1394
id: 1394
permalink: /?p=1394
title: '020 &#8211; MicroPython TechNotes: Touch Sensor'
url: /020-micropython-technotes-touch-sensor608-revision-v1-020-8211-MicroPython-TechNotes-Touch-Sensor
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/020-technotes-touch-sensor.png)</figure>In this article, we will learn how to use a touch sensor module with ESP32 using MicroPython programming language.

<div> </div>## PINOUT:

1. **G** – for the ground.
2. **V** – for the supply voltage.
3. **S** – for the signal pin.

<div> </div>## BILL OF MATERIALS:

1. **ESP32 development board** which will serve as the brain for this experiment.
2. **ESP32 shield from Gorillacell ESP32 development kit** which will extend the pins of ESP32 to ESP32 shield pin headers for easy circuit connection.
3. **3-pin female-female dupont wires** to attach the touch sensor module to ESP32 shield.
4. **Touch sensor module**.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 development board on top of the ESP32 shield and make sure that both USB port are on the same side.
2. Next, attach the dupont wires to the touch sensor module by following a color coding which is black for the ground, red for the supply voltage, and yellow for the signal pin.
3. Next, attach the other end of the dupont wires by matching the colors of the dupont wires to the colors of the pin headers. For this experiment, I choose GPIO 32 as the input pin from the touch sensor module.
4. Next, power the ESP32 shield with an external power supply with a type-C USB connector and make sure that the power switch is set to ON state.
5. Next, connect the ESP32 to the computer through a micro USB cable. Our demo circuit is now ready.

<div> </div>## SOFTWARE INSTRUCTION:

For the software part, I prepared 4 example source code for the demonstration.

Copy and paste it to Thonny IDE and play with it. Modify and adapt it according to your needs.

Enjoy and happy tinkering.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/5hqweVbNaho?feature=oembed" title="020 - MicroPython TechNotes: Touch Sensor" width="500"></iframe></div></figure><div> </div>For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. Example # 1, exploring the basics:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin

ts = Pin(32, Pin.IN)

# # The following lines of code can be tested in the REPL:
# ts.value() # Reads the current value of the touch sensor.
# # Return value is 1 when touch sensor is touch,
# # the value is 0 when touch sensor is not being touch.

```

</div><div> </div>### 2. Example # 2, simple application using the touch sensor:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms

ts = Pin(32, Pin.IN)
led = Pin(2, Pin.OUT)

while True:
    if ts.value()==1:
        # Touch sensor detects a touch
        # Lets toggle the state of the LED
        # to create a blinking LED.
        led.value(not led.value())
        sleep_ms(300)
    else:
        # Touch sensor has no touch detected
        # Lets turn OFF the LED.
        led.value(0)

```

</div><div> </div>### 3. Example # 3, touch sensor toggles a state:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms

ts = Pin(32, Pin.IN)
led = Pin(2, Pin.OUT)
isTouch = False

while True:
    if ts.value()==1:
        isTouch = not isTouch
        sleep_ms(300)
        if isTouch:
            print('LED blinking ON')
        else:
            print('LED blinking OFF')
    
    if isTouch:
        # Lets toggle the state of the LED
        # to create a blinking LED.
        led.value(not led.value())
        sleep_ms(5000)
    else:
        # Lets turn OFF the LED.
        led.value(0)

```

</div><div> </div>### 4. Example # 4, touch sensor non-blocking code:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms
from time import ticks_ms

ts = Pin(32, Pin.IN)
led = Pin(2, Pin.OUT)
isTouch = False
startTime = 0

while True:
    if ts.value()==1:
        isTouch = not isTouch
        sleep_ms(300)
        if isTouch:
            print('LED blinking ON')
            startTime = ticks_ms()
        else:
            print('LED blinking OFF')
    
    if isTouch:
        # Lets toggle the state of the LED
        # to create a blinking LED.
        if ticks_ms() - startTime >= 300:
            led.value(not led.value())
            startTime = ticks_ms()
    else:
        # Lets turn OFF the LED.
        led.value(0)

```

</div><div> </div>## CREDITS AND REFERENCES:

<div>Purchase your Gorillacell ESP32 development kit:</div><div>[gorillacell.kr](http://gorillacell.kr/)</div>