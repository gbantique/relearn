---
author: George Bantique
date: '2023-03-13T16:59:51+08:00'
excerpt: In this article, we will tackle how to use JOYSTICK with ESP32 using MicroPython programming language.
guid: https://techtotinker.com/?p=1362
id: 1362
permalink: /?p=1362
title: '025 &#8211; MicroPython TechNotes: Joystick'
url: /025-micropython-technotes-joystick602-autosave-v1-025-8211-MicroPython-TechNotes-Joystick
---


<div style="clear: both; text-align: center;"></div><figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/025-joystick-micropython.png)</figure>In this article, we will tackle how to use JOYSTICK with ESP32 using MicroPython programming language.

<div> </div>## PINOUT:

1. **G** – for the ground pin.
2. **V** – for the supply voltage.
3. **x** – for the horizontal potentiometer pin.
4. **y** – for the vertical potentiometer pin.

<div> </div>## BILL OF MATERIALS:

1. **ESP32 development board** to serve as the brain for the experiment.
2. **ESP32 shield** from Gorillacell ESP32 development kit to extend the ESP32 pins to pin headers for easy circuit connection.
3. **4-pin female-female dupont jumper wires** to connect the joystick module to the ESP32 shield.
4. **Joystick module** itself.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 development board on top of the ESP32 shield and make sure that both USB ports are on the same side.
2. Next, attach the dupont wires to the joystick module by following the color coding which is black for the ground, red for the VCC, yellow for the x pin, and white for the y pin.
3. Next, attach the other end of the dupont wires to the ESP32 shield by matching the colors of the wires to the colors of the pin headers which is black to black, red to red, yellow and the following colors to the yellow pin headers.
4. Next, power the ESP32 shield with an external power supply with a type-C USB connector and make sure that the power switch is set to ON state.
5. Lastly, connect the ESP32 to the computer through a micro-USB cable. Our demo circuit should be now ready.

<div> </div>## SOFTWARE INSTRUCTION:

I prepared 3 example source code for this demo.

Copy and paste it to your Thonny IDE.

Play with it, modify and adapt according to your needs.

Enjoy and happy tinkering.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/Wr3ztVSeuts?feature=oembed" title="025 - MicroPython TechNotes: Joystick" width="500"></iframe></div></figure><div> </div>For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. Example # 1, basics of reading an analog input just like the joystick module:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC
from time import sleep_ms

x = ADC(Pin(32, Pin.IN))
y = ADC(Pin(33, Pin.IN))
x.atten(ADC.ATTN_11DB)
y.atten(ADC.ATTN_11DB)

while True:
    x_val = x.read()
    y_val = y.read()
    print('Current position:{},{}'.format(x_val,y_val))
    sleep_ms(300)

```

</div><div> </div>#### How the code works:

<div>*<span style="color: red;">from machine import Pin, ADC</span>*</div><div>Imports Pin class and ADC class from machine module. Pin class is use to access the ESP32 pins. ADC class is use in order to read analog pin.</div><div> </div><div>*<span style="color: red;">from time import sleep\_ms</span>*</div><div>Imports sleep_ms class from time module in order to create delays in milliseconds resolution.</div><div> </div><div>*<span style="color: red;">x = ADC(Pin(32, Pin.IN))</span>*</div><div>*<span style="color: red;">y = ADC(Pin(33, Pin.IN))</span>*</div><div>*<span style="color: red;">x.atten(ADC.ATTN\_11DB)</span>*</div><div>*<span style="color: red;">y.atten(ADC.ATTN\_11DB)</span>*</div><div>Creates ADC objects for the joystick module named x and y. x is connected to GPIO 32 with a pin direction set as input. y is connected to GPIO 33 with a pin direction set as input. For the ADC, we use an attenuation of 11 dB in order to use 0V to 3.3V logic level.</div><div> </div><div>*<span style="color: red;">while True:</span>*</div><div>Creates an infinite loop.</div><div> </div><div>*<span style="color: red;">x\_val = x.read()</span>*</div><div>*<span style="color: red;">y\_val = y.read()</span>*</div><div>print(‘Current position:{},{}’.format(x_val,y_val))</div><div>Reads the analog value of the pins for the x and y and stored it to x_val and y_val variables and prints it to the REPL.</div><div> </div><div>*<span style="color: red;">sleep\_ms(300)</span>*</div><div>Creates a 300 milliseconds interval</div><div> </div>### 2. Example # 2, display joystick position using RGB matrix:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC
from neopixel import NeoPixel
from time import sleep_ms

class RGB_Matrix:
    
    def __init__(self, gpio, width, height):
        self.width = width
        self.height = height
        self.neopixel = NeoPixel(Pin(gpio), width*height)
        
    def pixel_set(self, row, col, r=0, g=0, b=0):
        self.neopixel[row + (col*self.width)] = (r, g, b)
        self.neopixel.write()
            
    def pixel_clr(self, row, col):
        self.neopixel[row + (col*self.width)] = (0, 0, 0)
        self.neopixel.write()
        
    def clear_all(self):
        self.neopixel.fill((0,0,0))
        self.neopixel.write()


np = RGB_Matrix(25, 8, 8)
x = ADC(Pin(32, Pin.IN))
y = ADC(Pin(33, Pin.IN))
x.atten(ADC.ATTN_11DB)
y.atten(ADC.ATTN_11DB)

def map(x, in_min, in_max, out_min, out_max): 
    # This will not handle x value greater than in_max or 
    #                      x value less than in_min 
    return int((x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min)

while True:
    x_pos = map(x.read(), 0, 4095, 0, 7)
    y_pos = map(y.read(), 0, 4095, 7, 0)
    np.clear_all()
    np.pixel_set(x_pos, y_pos, 10,0,0)
    sleep_ms(300)

```

**<u>How the code works:</u>**

</div><div><div>*<span style="color: red;">x\_pos = map(x.read(), 0, 4095, 0, 7)</span>*</div><div>*<span style="color: red;">y\_pos = map(y.read(), 0, 4095, 7, 0)</span>*</div><div>Reads the analog values from the joystick module which have a value ranging from 0 to 4095 and convert it to 0 to 7 by ratio and proportion by calling the map() function. Please be noted that y position is rotated.</div><div> </div><div>*<span style="color: red;">np.clear\_all()</span>*</div><div>Clears all the RGB LED pixels.</div><div> </div><div>*<span style="color: red;">np.pixel\_set(x\_pos, y\_pos, 10,0,0)</span>*</div><div>Set specific RGB LED according to the converted x and y position.</div></div><div> </div>### 3. Example # 3, control a pixel in RGB matrix using the joystick module:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC
from neopixel import NeoPixel
from time import sleep_ms

class RGB_Matrix:
    
    def __init__(self, gpio, width, height):
        self.width = width
        self.height = height
        self.neopixel = NeoPixel(Pin(gpio), width*height)
        
    def pixel_set(self, row, col, r=0, g=0, b=0):
        self.neopixel[row + (col*self.width)] = (r, g, b)
        self.neopixel.write()
            
    def pixel_clr(self, row, col):
        self.neopixel[row + (col*self.width)] = (0, 0, 0)
        self.neopixel.write()
        
    def clear_all(self):
        self.neopixel.fill((0,0,0))
        self.neopixel.write()


np = RGB_Matrix(25, 8, 8)
x = ADC(Pin(32, Pin.IN))
y = ADC(Pin(33, Pin.IN))
x.atten(ADC.ATTN_11DB)
y.atten(ADC.ATTN_11DB)

curr_x = 3
curr_y = 3

while True:
    x_val = x.read()
    y_val = y.read()
    
    if x_val < 1850:  
      if curr_x > 0:  
        curr_x = curr_x - 1  
    elif x_val > 1930:  
      if curr_x < 7:  
        curr_x = curr_x + 1  
    if y_val < 1890:  
      if curr_y < 7:  
        curr_y = curr_y + 1  
    elif y_val > 1970:  
      if curr_y > 0:  
        curr_y = curr_y - 1  
    print(curr_x, curr_y)  
    
    np.clear_all()
    np.pixel_set(curr_x, curr_y, 10, 0, 0)
    sleep_ms(100)

```

</div><div> </div>#### How the code works:

<div><div>*<span style="color: red;">if x\_val &lt; 1850:</span>*</div><div>*<span style="color: red;"> if curr\_x &gt; 0:</span>*</div><div>*<span style="color: red;"> curr\_x = curr\_x – 1</span>*</div><div>*<span style="color: red;">elif x\_val &gt; 1930:</span>*</div><div>*<span style="color: red;"> if curr\_x &lt; 7:</span>*</div><div>*<span style="color: red;"> curr\_x = curr\_x + 1</span>*</div><div>*<span style="color: red;"> </span>*</div><div>*<span style="color: red;">if y\_val &lt; 1890:</span>*</div><div>*<span style="color: red;"> if curr\_y &lt; 7:</span>*</div><div>*<span style="color: red;"> curr\_y = curr\_y + 1</span>*</div><div>*<span style="color: red;">elif y\_val &gt; 1970:</span>*</div><div>*<span style="color: red;"> if curr\_y &gt; 0:</span>*</div><div>*<span style="color: red;"> curr\_y = curr\_y – 1</span>*</div><div>*<span style="color: red;">print(curr\_x, curr\_y)</span>*</div><div>Checks if joystick has been move to any direction. If yes, also check if the current position is not in the border line before adding or subtracting from the current position.</div><div> </div><div>*<span style="color: red;">np.clear\_all()</span>*</div><div>Clears all the pixel in the RGB LED by turning it OFF.</div><div> </div><div>*<span style="color: red;">np.pixel\_set(curr\_x, curr\_y, 10, 0, 0)</span>*</div><div>Then set the specific RGB pixel according to the current position set by the joystick module.</div></div><div> </div>## REFERENCES AND CREDITS:

<div style="text-align: left;">1. Purchase your Gorillacell ESP32 development kit at:</div><div style="text-align: left;"> **[gorillacell.kr](http://gorillacell.kr/)**</div>