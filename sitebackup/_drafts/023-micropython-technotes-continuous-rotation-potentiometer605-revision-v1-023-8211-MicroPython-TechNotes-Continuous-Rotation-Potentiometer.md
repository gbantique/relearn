---
author: George Bantique
date: '2023-03-13T17:41:29+08:00'
excerpt: In this article, we will tackle on how to use CONTINUOUS ROTATION POTENTIOMETER with ESP32 using MicroPython programming language.
guid: https://techtotinker.com/?p=1381
id: 1381
permalink: /?p=1381
title: '023 &#8211; MicroPython TechNotes: Continuous Rotation Potentiometer'
url: /023-micropython-technotes-continuous-rotation-potentiometer605-revision-v1-023-8211-MicroPython-TechNotes-Continuous-Rotation-Potentiometer
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/023-technotes-continuous-rotation-potentiometer.png)</figure>In this article, we will tackle on how to use CONTINUOUS ROTATION POTENTIOMETER with ESP32 using MicroPython programming language.

<div> </div>## PINOUT:

1. **GND** – for the ground.
2. **VCC** – for the supply voltage.
3. **SIG** – for the signal pin.

<div> </div>## BILL OF MATERIALS:

1. ESP32 development board.
2. Gorillacell ESP32 shield.
3. 3-pin female-female dupont wires.
4. Continuous Rotation Potentiometer module.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 dev board on top of the ESP32 shield and make sure that both USB port are on the same side.
2. Next, attach the dupont wires to the Continuous Rotation Potentiometer module by following the color coding which is black for the ground, red for the VCC, and yellow for the signal pin.
3. Next, attach the other end of the dupont wires to the ESP32 shield by matching the colors of the wires to the colors of the pin headers which is black to black, red to red, and yellow to yellow.
4. Next, power the ESP32 shield with an external power supply with a type-C USB cable and make sure that the power switch is set to ON state.
5. Lastly, connect ESP32 to the computer with a micro USB cable. Our demo circuit should now be ready.

<div> </div>## SOFTWARE INSTRUCTION:

I prepared 2 example source code for this demonstration.

Copy and paste it to Thonny IDE.

Modify and adapt it according to your needs, and most of all enjoy learning.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/LRHXe90JJtQ?feature=oembed" title="023 - MicroPython TechNotes: Continuous Rotation Potentiometer" width="500"></iframe></div></figure><div> </div>For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. Example # 1, basics of reading an analog input pin:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC
from time import sleep_ms

p32 = Pin(32, Pin.IN)
crpot = ADC(p32)
crpot.atten(ADC.ATTN_11DB)

while True:
    print(crpot.read())
    sleep_ms(300)

```

</div><div> </div>### 2. Example # 2, application of using Continuous Rotation Potentiometer module:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC
from neopixel import NeoPixel
from time import sleep_ms

p32 = Pin(32, Pin.IN)
crpot = ADC(p32)
crpot.atten(ADC.ATTN_11DB)

NUM_OF_LED = 16
np = NeoPixel(Pin(25), NUM_OF_LED)
np.fill((0,0,0))

def map(x, in_min, in_max, out_min, out_max):
    return int((x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min)

while True:
    crpot_value = crpot.read()
    np.fill((0,0,0))
    np_addr = map(crpot_value, 0, 4095, 0, 15)
    np[np_addr] = (10, 0, 0)
    np.write()
    sleep_ms(300)

```

</div><div> </div>## REFERENCES AND CREDITS:

<div style="text-align: left;">**1. Purchase your Gorillacell ESP32 development kit:**</div><div style="text-align: left;">[**gorillacell.kr**](http://gorillacell.kr/)</div>