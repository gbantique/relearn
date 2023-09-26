---
author: George Bantique
date: '2023-03-11T14:44:38+08:00'
excerpt: 'In this article, I will discussed how to use an Analog Touch Sensor module which is interface to ESP32 with MicroPython programming language.

  Analog Touch Sensor module of Gorillacell ESP32 development kit provides a 4 touch sensor input in a single pin.'
guid: https://techtotinker.com/?p=1261
id: 1261
permalink: /?p=1261
title: '048 &#8211; MicroPython TechNotes: Analog Touch Sensor'
url: /048-micropython-technotes-analog-touch-sensor576-revision-v1-048-8211-MicroPython-TechNotes-Analog-Touch-Sensor
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/048-MicroPython-TechNotes-Analog-Touch-Sensor.jpg)</figure><div>In this article, I will discussed how to use an Analog Touch Sensor module which is interface to ESP32 with MicroPython programming language.</div><div>Analog Touch Sensor module of Gorillacell ESP32 development kit provides a 4 touch sensor input in a single pin. This is achieve because the module provides a different analog value differently for each touch input.</div><div> </div>## PINOUT:

<div>This module has 3 pins which are:</div><div>1. **G** – for the ground pin.</div><div>2. **V** – for the supply voltage.</div><div>3. **S** – for the analog touch sensor signal pin.</div><div> </div>## BILL OF MATERIALS:

<div>In order to follow this lesson, you will need the following:</div><div>1. An ESP32 development board with Micropython firmware.</div><div>2. A Gorillacell ESP32 shield (this is optional, you can use a breadboard instead and just follow the circuit connection).</div><div>3. A 3-pin dupont wires.</div><div>4. And of course the Analog Touch Sensor itself.</div><div>5. You might also need an 8×8 Dot Matrix display with SPI interface for the example # 3.</div><div> </div>## HARDWARE INSTRUCTION:

<div>1. Connect the Analog Touch Sensor G pin to ESP32 GND pin.</div><div>2. Connect the Analog Touch Sensor V pin to ESP32 3.3V pin.</div><div>3. Connect the Analog Touch Sensor S pin to ESP32 GPIO 32. MicroPython implements ADC inputs on GPIO 32 to GPIO 39 only.</div><div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/9YR26NDkx0E?feature=oembed" title="048 - MicroPython TechNotes: Analog Touch Sensor" width="500"></iframe></div></figure><div> </div><div>If you have any concern regarding this lesson, be sure to write your message in the comment section.</div><div>You might also like to consider supporting my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker YT channel.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)</div><div>Thank you,</div><div>– George Bantique | tech.to.tinker@gmail.com</div><div> </div>## SOURCE CODE:

### 1. Example # 1, exploring the basics using the REPL:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from machine import ADC

p32 = Pin(32, Pin.IN)
ats = ADC(p32)
ats.atten(ADC.ATTN_11DB)

print(ats.read())

```

</div><div> </div>### 2. Example # 2, automatic detection of touch sensor key press:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC
from time import sleep

p32 = Pin(32, Pin.IN)
ats = ADC(p32)
ats.atten(ADC.ATTN_11DB)

while True:
    adc_value = ats.read()
    
    if (adc_value>638) and (adc_value<698):
        print('You pressed # 1.')
    elif (adc_value>1483) and (adc_value<1543):
        print('You pressed # 2.')
    elif (adc_value>2303) and (adc_value<2363):
        print('You pressed # 3.')
    elif (adc_value>3169) and (adc_value<3229):
        print('You pressed # 4.')
        
    sleep(0.3)

```

</div><div> </div>### 3. Example # 3, control the pixel movement on Dot Matrix Display using the Analog Touch Sensor:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from machine import ADC
from machine import SPI
from time import sleep
from max7219 import Max7219

spi = SPI(1,
          baudrate=10000000,
          polarity=1,
          phase=0,
          sck=Pin(19),
          mosi=Pin(23))
cs = Pin(18, Pin.OUT)
dot = Max7219(8, 8, spi, cs, True)
dot.brightness(15)

p32 = Pin(32, Pin.IN)
ats = ADC(p32)
ats.atten(ADC.ATTN_11DB)

col = 0
row = 0

while True:
    analog_value = ats.read()
    
    if (analog_value>640) and (analog_value<700):
        if col > 0:
            col = col - 1
    elif (analog_value>1470) and (analog_value<1530):
        if col < 7:
            col = col + 1
    elif (analog_value>2310) and (analog_value<2370):
        if row > 0:
            row = row - 1
    elif (analog_value>3170) and (analog_value<3230):
        if row < 7:
            row = row + 1

    dot.fill(0)
    dot.pixel(col, row, 1)
    dot.show()
    
    sleep(0.1)

```

</div><div> </div>## REFERENCES AND CREDITS:

<div>1. Purchase your Gorillacell ESP32 development kits from:</div><div> <https://gorillacell.kr/></div>