---
author: George Bantique
date: '2023-03-13T16:41:48+08:00'
excerpt: In this article, we will learn how to use an LDR with ESP32 using MicroPython programming language. LDR stands for Light Dependent Resistor. It is a type of electronic component that changes resistance according to the light intensity.
guid: https://techtotinker.com/?p=1357
id: 1357
permalink: /?p=1357
title: '026 &#8211; MicroPython TechNotes: Light Dependent Resistor (LDR)'
url: /026-micropython-technotes-light-dependent-resistor-ldr601-revision-v1-026-8211-MicroPython-TechNotes-Light-Dependent-Resistor-LDR
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/04/026-LDR-blog.png)</figure>In this article, we will learn how to use an LDR with ESP32 using MicroPython programming language. LDR stands for Light Dependent Resistor. It is a type of electronic component that changes resistance according to the light intensity.

<div> </div>## PINOUT:

1. **G** – for the ground.
2. **V** – for the supply voltage.
3. **S** – for the signal pin.

<div> </div>## BILL OF MATERIALS:

1. An ESP32 development board to serve as the brain for the experiment.
2. Gorillacell ESP32 shield to extend ESP32 pins to pin headers for easy circuit connection.
3. A 3-pin female-female dupont jumper wires to attach the LDR module to the ESP32 shield.
4. An LDR sensor module itself.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 board on top of the ESP32 shield and make sure that both USB port are on the same side.
2. Next, attach the dupont jumper wires to the LDR module by following a color coding which is black for the ground, red for the VCC, and yellow for the signal pin.
3. Next, attach the other end of the dupont wires to the ESP32 shield by matching the colors of the wires to the colors of the pin headers such as black to black, red to red, and yellow to yellow. For this, I choose GPIO 32 to serve as analog input pin from the LDR module.
4. Next, power the ESP32 shield with an external power supply with a type-C USB cable and make sure that the power switch is set to the ON state.
5. Lastly, connect the ESP32 to the computer using a micro-USB cable.

<div> </div>## SOFTWARE INSTRUCTION:

For this experiment, I prepared 3 examples.

Copy and paste it to your Thonny IDE.

Please feel free to modify it according to your needs.

Happy tinkering.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/R0irhLDL4Tc?feature=oembed" title="026 - MicroPython TechNotes: Light Dependent Resistor" width="500"></iframe></div></figure><div> </div>For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. Example # 1, basics of reading an LDR module:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC

LDR = ADC(Pin(32, Pin.IN))
LDR.atten(ADC.ATTN_11DB)

# The following line of codes can be tested using the REPL:
#print(LDR.read())

```

</div><div> </div>#### How the code works:

<div style="text-align: left;"><div>*<span style="color: red;">from machine import Pin, ADC</span>*</div><div>Imports the Pin class and the ADC class from the machine module. Pin class is use in order to access ESP32 pins while ADC class is use to read the LDR sensor module.</div><div> </div><div>*<span style="color: red;">LDR = ADC(Pin(32, Pin.IN))</span>*</div><div>*<span style="color: red;">LDR.atten(ADC.ATTN\_11DB)</span>*</div><div>Creates the ADC object named LDR which is connected on GPIO 32 with a pin direction of input.</div><div> </div><div>*<span style="color: red;">\# The following line of codes can be tested using the REPL:</span>*</div><div>*<span style="color: red;">\#print(LDR.read())</span>*</div><div>Prints the current LDR value by reading the ADC value of the LDR pin in the REPL.</div></div><div> </div>### 2. Example # 2, automatic lights using an LDR:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC
from time import sleep_ms

LDR = ADC(Pin(32, Pin.IN))
LDR.atten(ADC.ATTN_11DB)
led = Pin(2, Pin.OUT)

while True:  
   ldr_value = LDR.read()  
   if ldr_value < 2400:  
     # Its dark  
     led.on()  
   elif ldr_value > 2500:  
     # Its bright  
     led.off()  
   sleep_ms(50)  

```

</div><div> </div>#### How the code works:

<div style="text-align: left;"><div><div>*<span style="color: red;">from machine import Pin, ADC</span>*</div><div>Imports the Pin class and the ADC class from the machine module. Pin class is use in order to access ESP32 pins while ADC class is use to read the LDR sensor module.</div><div> </div></div><div>*<span style="color: red;">from time import sleep\_ms</span>*</div><div>Imports the sleep_ms class from the time module. Sleep_ms is in milliseconds resolution.</div><div> </div><div><div>*<span style="color: red;">LDR = ADC(Pin(32, Pin.IN))</span>*</div><div>*<span style="color: red;">LDR.atten(ADC.ATTN\_11DB)</span>*</div><div>Creates the ADC object named LDR which is connected on GPIO 32 with a pin direction of input.</div><div> </div></div><div>*<span style="color: red;">led = Pin(2, Pin.OUT)</span>*</div><div>Creates a pin class named led which is connected to GPIO 2 (on board LED) with a pin direction set as output.</div><div> </div><div>*<span style="color: red;">while True:</span>*</div><div>Creates an infinite loop.</div><div> </div><div><div>*<span style="color: red;">ldr\_value = LDR.read()</span>*</div><div>Reads the LDR value by reading it through an ADC and stores it to ldr_value variable.</div><div> </div><div>*<span style="color: red;">if ldr\_value &lt; 2400:</span>*</div><div>*<span style="color: red;"> # Its dark</span>*</div><div>*<span style="color: red;"> led.on()</span>*</div><div>*<span style="color: red;">elif ldr\_value &gt; 2500:</span>*</div><div>*<span style="color: red;"> # Its bright</span>*</div><div>*<span style="color: red;"> led.off()</span>*</div><div>Checks if the LDR value signifies dark, then turn ON the LED else if its bright, turn OFF the LED.</div><div> </div><div>*<span style="color: red;">sleep\_ms(50)</span>*</div><div>Creates a 50 milliseconds interval in between the LDR readings.</div></div></div><div> </div>### 3. Example # 3, Intruder Detection Security System:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, ADC
from time import sleep_ms

LDR = ADC(Pin(32, Pin.IN))
LDR.atten(ADC.ATTN_11DB)

laser = Pin(25, Pin.OUT)
laser.on()

led = Pin(2, Pin.OUT)

isAlert = False
print(LDR.read())

while True:
    if LDR.read() < 3000:
        isAlert = True
    else:
        isAlert = False
        
    if isAlert:
        led.value(not led.value())
        sleep_ms(100)

```

</div><div> </div>#### How the code works:

<div style="text-align: left;"><div><div><div>*<span style="color: red;">from machine import Pin, ADC</span>*</div><div>Imports the Pin class and the ADC class from the machine module. Pin class is use in order to access ESP32 pins while ADC class is use to read the LDR sensor module.</div><div> </div></div><div>*<span style="color: red;">from time import sleep\_ms</span>*</div><div>Imports the sleep_ms class from the time module. Sleep_ms is in milliseconds resolution.</div><div> </div><div><div>*<span style="color: red;">LDR = ADC(Pin(32, Pin.IN))</span>*</div><div>*<span style="color: red;">LDR.atten(ADC.ATTN\_11DB)</span>*</div><div>Creates the ADC object named LDR which is connected on GPIO 32 with a pin direction of input.</div><div> </div></div><div>*<span style="color: red;">led = Pin(2, Pin.OUT)</span>*</div><div>Creates a pin class named led which is connected to GPIO 2 (on board LED) with a pin direction set as output.</div><div> </div><div>*<span style="color: red;">while True:</span>*</div><div>Creates an infinite loop.</div></div><div> </div><div>*<span style="color: red;">isAlert = False</span>*</div><div>Initialize the isAlert variable to False (means no intruder currently detected)</div><div> </div><div>*<span style="color: red;">while True:</span>*</div><div>Creates an infinite loop.</div><div> </div><div><div>*<span style="color: red;">if LDR.read() &lt; 3000:</span>*</div><div>*<span style="color: red;"> isAlert = True</span>*</div><div>*<span style="color: red;">else:</span>*</div><div>*<span style="color: red;"> isAlert = False</span>*</div><div>Checks if LDR value is below the threshold (if the laser light is obstructed), if yes, set the isAlert variable to True (which means that the laser light is obstructed or cut, an intruder is detected) else set the isAlert variable to False.</div><div> </div><div>*<span style="color: red;">if isAlert:</span>*</div><div>*<span style="color: red;"> led.value(not led.value())</span>*</div><div>*<span style="color: red;"> sleep\_ms(100)</span>*</div><div>If an intruder is detected, blinks the onboard LED every 100 milliseconds. You can also add audible alarm or send an alert message to the owner of the establishment.</div></div></div><div> </div>## REFERENCES AND CREDITS:

<div style="text-align: left;">**1. Purchase your Gorillacell ESP32 development kit at:**</div><div style="text-align: left;"> **[gorillacell.kr](http://gorillacell.kr/)**</div>