---
author: George Bantique
date: '2023-03-13T18:48:05+08:00'
excerpt: In this article, we will learn how to use potentiometer with ESP32 using MicroPython programming langauge.
guid: https://techtotinker.com/?p=1385
id: 1385
permalink: /?p=1385
title: '022 &#8211; MicroPython TechNotes: Potentiometer | Reading an Analog Input'
url: /022-micropython-technotes-potentiometer-reading-an-analog-input606-autosave-v1-022-8211-MicroPython-TechNotes-Potentiometer-Reading-an-Analog-Input
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/022-technotes-potentiometer.png)</figure>In this article, we will learn how to use potentiometer with ESP32 using MicroPython programming langauge.

<div> </div>## PINOUT:

1. **G** – for the ground.
2. **V** – for the supply voltage.
3. **S** – for the signal pin.

<div> </div>## BILL OF MATERIALS:

1. **ESP32 development board** that will serve as the brain of this experiment.
2. **ESP32 shield** from Gorillacell ESP32 development kit to extend ESP32 pins to pin headers for easy circuit connection.
3. **3-pin female-female dupont wires** to attach the potentiometer module to ESP32 shield.
4. **Potentiometer module**.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 board on top of the ESP32 shield making sure that all pins are properly aligned and both USB port are on the same side.
2. Next, attach the dupont wires to potentiometer module by following the color coding which is black for the ground, red for the VCC, and yellow for the signal pin.
3. Next, attach the other end of the dupont wires to the ESP32 shield by matching the colors of the wires to the colors of the pin headers which is black to black, red to red, and yellow to yellow. For this, I choose GPIO 32 to serve as the analog input pin from the potentiometer.
4. Next, power the ESP32 shield with an external power supply with a type-C USB connector cable and make sure that the power switch is set to ON state.
5. Next, connect the ESP32 shield to the computer through a micro-USB cable. The demo circuit should now be ready.

<div> </div>## SOFTWARE INSTRUCTION

I prepared 2 example source code for this experiment. Copy each one-by-one.

Modify it according to what you want.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/-gHXofJfegs?feature=oembed" title="022 - MicroPython TechNotes: Potentiometer" width="500"></iframe></div></figure><div> </div>For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. Example # 1, basics of reading an analog input:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC

p32 = Pin(32, Pin.IN)
pot = ADC(p32)
pot.atten(ADC.ATTN_11DB)

# The following line of code should be tested in the REPL:
# This will read the analog values of pot object and
# print it to the REPL
print(pot.read())

```

</div><div> </div>### 2. Example # 2, simple application in using a potentiometer module:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC, PWM
from time import sleep_ms

p32 = Pin(32, Pin.IN)
pot = ADC(p32)
pot.atten(ADC.ATTN_11DB)

p2 = Pin(2, Pin.OUT)
led = PWM(p2)
led.freq(60)

def map(x, in_min, in_max, out_min, out_max):
    return int((x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min)

while True:
    pot_value = pot.read() # 0 - 4095
    pwm_value = map(pot_value, 0, 4095, 0, 1023)
    led.duty(pwm_value)    # 0 - 1023
    sleep_ms(500)

```

</div><div> </div>## REFERENCES AND CREDITS:

<div>**1. Purchase your Gorillacell ESP32 development kit from:**</div><div>[gorillacell.kr](http://gorillacell.kr/)</div><div> </div><div>**2. ESP32 Quick Reference:**</div><div><https://docs.micropython.org/en/latest/esp32/quickref.html></div>