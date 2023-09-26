---
author: George Bantique
date: '2023-03-13T17:26:21+08:00'
excerpt: In this article, we will learn how to use Slider Switch with ESP32 using MicroPython programming language. A slider switch is basically a variable resistor that changes resistance according to slider wiper position. With that knowledge, we can used MicroPython's ADC function to interpret slider switch position.
guid: https://techtotinker.com/?p=1373
id: 1373
permalink: /?p=1373
title: '024 &#8211; MicroPython TechNotes: Slider Switch'
url: /024-micropython-technotes-slider-switch604-revision-v1-024-8211-MicroPython-TechNotes-Slider-Switch
---


<div style="clear: both; text-align: center;"></div><figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/024-technotes-Slider-switch-micropython.png)</figure>In this article, we will learn how to use Slider Switch with ESP32 using MicroPython programming language. A slider switch is basically a variable resistor that changes resistance according to slider wiper position. With that knowledge, we can used MicroPython’s ADC function to interpret slider switch position.

<div> </div>## PINOUT:

1. **G** – for the ground pin.
2. **V** – for the supply voltage.
3. **S** – for the signal pin.

<div> </div>## BILL OF MATERIALS:

1. **ESP32 development board** to serve as the brain for the experiment.
2. **ESP32 shield** from Gorillacell ESP32 development kit to extend ESP32 pin to pin headers for easy circuit connection.
3. **3-pin female-female dupont wires** to connect slider switch to ESP32 shield pin headers.
4. **Slider Switch module** from Gorillacell ESP32 development kit.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 on top of the ESP32 shield by making sure that the pins are properly aligned and both USB ports are on the same side.
2. Next, attach the dupont wires to the Slider module by following the color coding which is black for the ground, red for the VCC, and yellow for the signal pin.
3. Next, attach the other end of the dupont wires to the ESP32 shield by matching the colors of the wires to the colors of the pin headers such as black to black, red to red, and yellow to yellow. For this experiment, I choose GPIO 32 to serve as the input signal pin from the Slider module.
4. Next, power the ESP32 shield with external power supply with a type-C USB connector and make sure that the power switch is set to ON state.
5. Lastly, connect the ESP32 to the computer through the USB cable with micro-USB type cable. Demo circuit is now ready.

<div> </div>## SOFTWARE INSTRUCTION:

For this experiment, I prepared to example source code for you to try.

Copy and paste it to Thonny IDE.

Please feel free to modify it and adapt according to your needs.

Happy tinkering.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/P3jzXRALogo?feature=oembed" title="024 - MicroPython TechNotes: Slider Switch" width="500"></iframe></div></figure><div> </div>For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. Example # 1, basics of reading an analog input such as the slider switch:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC
from time import sleep_ms

p32 = Pin(32, Pin.IN)
slider = ADC(p32)
slider.atten(ADC.ATTN_11DB)

while True:
    print(slider.read())
    sleep_ms(300)

```

</div><div> </div>#### How the code works:

<div><div>*<span style="color: red;">from machine import Pin, ADC</span>*</div><div>Imports the Pin class from the machine module in order to access ESP32 pins. It also imports the ADC class from machine module in order to use the ADC in reading the analog values from the slider switch.</div><div> </div><div>*<span style="color: red;">from time import sleep\_ms</span>*</div><div>Imports the sleep_ms class from time module in order to create some delays in milliseconds resolution.</div><div> </div><div>*<span style="color: red;">p32 = Pin(32, Pin.IN)</span>*</div><div>Creates a 300 milliseconds delay that will serve as an interval.</div><div>Creates a Pin object named p32 which is connected to GPIO 32 with a pin direction set as input.</div><div> </div><div>*<span style="color: red;">slider = ADC(p32)</span>*</div><div>Creates an ADC object named slider using the p32 pin object.</div><div> </div><div>*<span style="color: red;">slider.atten(ADC.ATTN\_11DB)</span>*</div><div>Sets the ADC slider object to use 11dB attenuation in order to use 0 to 3.3V voltage range for the ADC.</div><div> </div><div>*<span style="color: red;">while True:</span>*</div><div>Creates an infinite loop to constantly check for the slider analog values</div><div> </div><div>*<span style="color: red;">print(slider.read())</span>*</div><div>Reads the analog values of the slider switch and print it to the REPL.</div><div> </div><div>*<span style="color: red;">sleep\_ms(300)</span>*</div></div><div> </div>### 2. Example # 2, simple application in using a slider switch:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC, PWM
from time import sleep_ms

p32 = Pin(32, Pin.IN)
slider = ADC(p32)
slider.atten(ADC.ATTN_11DB)

boot = Pin(0, Pin.IN)
led = Pin(2, Pin.OUT)

red = PWM(Pin(25, Pin.OUT))
grn = PWM(Pin(26, Pin.OUT))
blu = PWM(Pin(27, Pin.OUT))
red.freq(60)
grn.freq(60)
blu.freq(60)

r = 0
g = 0
b = 0
steps = 0

def map(x, in_min, in_max, out_min, out_max): 
    # This will not handle x value greater than in_max or 
    #                      x value less than in_min 
    return int((x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min) 

while True:      
    if steps==0:
        # Set the RGB from r, g, b values
        red.duty(r)
        grn.duty(g)
        blu.duty(b)
        if boot.value()==0:
            steps = 1 # go to setting red
            print('RGB are {} {} {}'.format(r, g, b))
            
    elif steps==1:
        # Set the red color
        r = map(slider.read(), 0, 4095, 0, 255)
        red.duty(r)
        grn.duty(0)
        blu.duty(0)
        if boot.value()==0:
            steps = 2 # go to setting green
            print('R is set to {}'.format(r))
            
    elif steps==2:
        # Set the green color
        g = map(slider.read(), 0, 4095, 0, 255)
        red.duty(0)
        grn.duty(g)
        blu.duty(0)
        if boot.value()==0:
            steps = 3 # go to setting blue
            print('G is set to {}'.format(g))
            
    elif steps==3:
        # Set the blue color
        b = map(slider.read(), 0, 4095, 0, 255)
        red.duty(0)
        grn.duty(0)
        blu.duty(b)
        if boot.value()==0:
            steps = 0 # write the r,g,b to RGB LED
            print('B is set to {}'.format(b))
    
    sleep_ms(300)
    if steps==1 or steps==2 or steps==3:
        led.value(not led.value())
    else:
        led.value(0)

```

</div><div> </div>## REFERENCES AND CREDITS:

<div> **1. Purchase your Gorillacell ESP32 development kit at:**</div><div> [http://gorillacell.kr](http://gorillacell.kr/)</div>