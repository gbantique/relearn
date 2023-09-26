---
author: George Bantique
date: '2023-03-13T19:07:59+08:00'
excerpt: In this article, we will learn how to use a color touch sensor module with ESP32 using MicroPython programming language.
guid: https://techtotinker.com/?p=1390
id: 1390
permalink: /?p=1390
title: '021 &#8211; MicroPython TechNotes: Color Touch Sensor'
url: /021-micropython-technotes-color-touch-sensor607-revision-v1-021-8211-MicroPython-TechNotes-Color-Touch-Sensor
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/021-technotes-color-touch-sensor.png)</figure>In this article, we will learn how to use a color touch sensor module with ESP32 using MicroPython programming language.

<div> </div>## PINOUT:

1. **G** – for the ground.
2. **V** – for the supply voltage.
3. **S** – for the signal pin.

<div> </div>## BILL OF MATERIALS:

1. An **ESP32 development board** that will serve as the brain for this experiment.
2. An **ESP32 shield from Gorillacell ESP32 development kit** to extend the pins to pin headers for easy circuit connection.
3. A **3-pin female-female dupont jumper wires** to attach the color touch sensor module to the ESP32 shield pin headers.
4. And of course the **color touch sensor modules** itself.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 board on top of the ESP32 shield and make sure that both USB port are on the same side.
2. Next, attach the dupont wires to the color touch sensor by following the color coding which is black for the ground, red for the VCC, and yellow for the signal pin.
3. Next, attach the other end of the dupont wires to the ESP32 shield by matching the colors of the wires to the colors of the pin headers which is black to black, red to red, and yellow to yellow. For this experiment I choose GPIO 32, GPIO 33, GPIO 34, and GPIO 35 for the red, green, blue, and rainbow color touch sensor respectively.
4. Next, power the ESP32 shield with an external power supply with a type-C USB connector and make sure that the power switch is set to ON state.
5. Lastly, connect the ESP32 to the computer through the micro USB cable. Our demo circuit should now be ready.

<div> </div>## SOFTWARE INSTRUCTION:

I prepared 5 examples below which you can copy and paste to your Thonny IDE.

Play with it and modify it to adapt according to your needs.

Hope you enjoy it by learning. Happy tinkering!

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/zzdSyYwfuP4?feature=oembed" title="021 - MicroPython TechNotes: Color Touch Sensor" width="500"></iframe></div></figure><div> </div>For any concern, write your message in the comment section.

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

cts = Pin(32, Pin.IN)

# # The following lines of code can be tested in the REPL:
# cts.value() # Reads the current value of the color touch sensor.
# # Return value is 1 when color touch sensor is touch,
# # the value is 0 when color touch sensor is not being touch.

```

</div><div> </div>#### How the code works:

<div><span style="color: red;">*from machine import Pin*</span></div><div>Imports the Pin class from machine module in order to access ESP32 pins.</div><div> </div><div>*<span style="color: red;">cts = Pin(32, Pin.IN)</span>*</div><div>Creates a Pin object named cts (short for **c**olor **t**ouch **s**ensor) which is connected on GPIO 32 with pin direction set as input.</div><div> </div><div>*<span style="color: red;">cts.value()</span>*</div><div>Reads the digital value of the pin associated to cts object, in our case GPIO 32. Returned value is either 1 when the touch sensor is currently press else returned value is 0.</div><div> </div>### 2. Example # 2, multiple color touch sensor:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms

red_cts = Pin(32, Pin.IN)
grn_cts = Pin(33, Pin.IN)
blu_cts = Pin(34, Pin.IN)
rbw_cts = Pin(35, Pin.IN)
isRed = red_cts.value()
isGrn = grn_cts.value()
isBlu = blu_cts.value()
isRbw = rbw_cts.value()


while True:
    if red_cts.value()==1:
        if isRed==False:
            isRed = True
            print('Red color touch sensor is activated.')
    else:
        isRed = False
    
    if grn_cts.value()==1:
        if isGrn==False:
            isGrn = True
            print('Green color touch sensor is activated.')
    else:
        isGrn = False
        
    if blu_cts.value()==1:
        if isBlu==False:
            isBlu = True
            print('Blue color touch sensor is activated.')
    else:
        isBlu = False
        
    if rbw_cts.value()==1:
        if isRbw==False:
            isRbw = True
            print('Rainbow color touch sensor is activated.')
    else:
        isRbw = False
        
    sleep_ms(300)

```

</div><div> </div>#### How the code works:

<div>*<span style="color: red;">from machine import Pin</span>*</div><div>Loads the Pin class from machine module in order to access ESP32 pins.</div><div> </div><div>*<span style="color: red;">from time import sleep\_ms</span>*</div><div>Imports sleep_ms class from time module for creating a time delay with milliseconds resolution.</div><div> </div><div><div>red_cts = Pin(32, Pin.IN)</div><div>grn_cts = Pin(33, Pin.IN)</div><div>blu_cts = Pin(34, Pin.IN)</div><div>rbw_cts = Pin(35, Pin.IN)</div></div><div>Creates the Pin objects for all the available color touch sensor which are connected to GPIO 32, 33, 34, and 35 respectively.</div><div> </div><div><div><span style="color: red;">*isRed = red\_cts.value()*</span></div><div><span style="color: red;">*isGrn = grn\_cts.value()*</span></div><div><span style="color: red;">*isBlu = blu\_cts.value()*</span></div><div><span style="color: red;">*isRbw = rbw\_cts.value()*</span></div></div><div>Creates a variable flags for each color touch sensor and initialized it by reading the digital state of the associated pin.</div><div> </div><div>*<span style="color: red;">while True:</span>*</div><div>Creates an infinite to constantly checks for the color touch sensor state.</div><div> </div><div><div>*<span style="color: red;">if red\_cts.value()==1:</span>*</div><div>*<span style="color: red;"> if isRed==False:</span>*</div><div>*<span style="color: red;"> isRed = True</span>*</div><div>*<span style="color: red;"> print(‘Red color touch sensor is activated.’)</span>*</div><div>*<span style="color: red;">else:</span>*</div><div>*<span style="color: red;"> isRed = False</span>*</div></div><div>Checks if the specific color touch sensor is currently active. If yes, check if it is previously inactive. This works similar to “rising edge” detection. If both condition is satisfied, set the variable flag to True then send a print message in REPL. Else (if the color touch sensor is inactive), then set the variable flag to False. This applies to all other color touch sensor.</div><div> </div><div>*<span style="color: red;">sleep\_ms(300)</span>*</div><div>Creates a 300 milliseconds interval.</div><div> </div>### 3. Example # 3, introducing pin interrupt:

<div>```

# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin

red_cts = Pin(32, Pin.IN)
led = Pin(2, Pin.OUT)

def handle_interrupt(pin):
    led.value(not led.value())
    print('LED is toggled.')

# Use Pin.IRQ_RISING to set interrupt trigger to rising edge.
# Use Pin.IRQ_FALLING to set interrupt trigger to falling edge.
# Use Pin.IRQ_RISING | Pin.IRQ_FALLING to set interrupt trigger to both edge.
# Interrupt with rising edge detects signal change from logic LOW to logic HIGH.
# Interrupt with falling edge detects signal change from logic HIGH to logic LOW.
red_cts.irq(trigger=Pin.IRQ_RISING, handler=handle_interrupt)

```

</div><div> </div>#### How the code works:

<div>*<span style="color: red;">from machine import Pin</span>*</div><div>Loads the Pin class from machine module in order to access the ESP32 GPIO pins.</div><div> </div><div>*<span style="color: red;">red\_cts = Pin(32, Pin.IN)</span>*</div><div>Creates a Pin object named red_cts which is connected to GPIO 32 with pin direction set as an input.</div><div> </div><div>*<span style="color: red;">led = Pin(2, Pin.OUT)</span>*</div><div>Creates a Pin object named led which is the onboard LED on GPIO 2 with pin direction set as output.</div><div> </div><div><div>*<span style="color: red;">def handle\_interrupt(pin):</span>*</div><div>*<span style="color: red;"> led.value(not led.value())</span>*</div><div>*<span style="color: red;"> print(‘LED is toggled.’)</span>*</div></div><div>Creates a function that will be called when the interrupt is triggered. This will trigger the state of the led pin and send a message in the REPL.</div><div> </div><div>*<span style="color: red;">red\_cts.irq(trigger=Pin.IRQ\_RISING, handler=handle\_interrupt)</span>*</div><div>Attaches the red_cts pin to interrupt using the “.irq” method. </div><div>**trigger** parameter sets the what edge detection to be use. It can be either rising-edge detection or falling-edge detection or both using the logic OR.</div><div>**handler** parameter sets what callback function to be called when the interrupt occurs.</div><div> </div>### 4. Example # 4, multiple pin interrupt:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin

red_cts = Pin(32, Pin.IN)
grn_cts = Pin(33, Pin.IN)
blu_cts = Pin(34, Pin.IN)
rbw_cts = Pin(35, Pin.IN)

def handle_interrupt(pin):
    print(pin)

# Use Pin.IRQ_RISING to set interrupt trigger to rising edge.
# Use Pin.IRQ_FALLING to set interrupt trigger to falling edge.
# Use Pin.IRQ_RISING | Pin.IRQ_FALLING to set interrupt trigger to both edge.
# Interrupt with rising edge detects signal change from logic LOW to logic HIGH.
# Interrupt with falling edge detects signal change from logic HIGH to logic LOW.
red_cts.irq(trigger=Pin.IRQ_RISING | Pin.IRQ_FALLING, handler=handle_interrupt)
grn_cts.irq(trigger=Pin.IRQ_RISING | Pin.IRQ_FALLING, handler=handle_interrupt)
blu_cts.irq(trigger=Pin.IRQ_RISING | Pin.IRQ_FALLING, handler=handle_interrupt)
rbw_cts.irq(trigger=Pin.IRQ_RISING | Pin.IRQ_FALLING, handler=handle_interrupt)

```

</div><div>This just shows how to use multiple pin interrupts by expanding the Example # 3.</div><div> </div>### 5. Example # 5, simple pin interrupt example:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms

red_cts = Pin(32, Pin.IN)
grn_cts = Pin(33, Pin.IN)
blu_cts = Pin(34, Pin.IN)
rbw_cts = Pin(35, Pin.IN)
led = Pin(2, Pin.OUT)

press = False
irq_pin = 0

def handle_interrupt(pin):
    global press
    press = True
    global irq_pin
    irq_pin = int(str(pin)[4:-1])
"""  
   String slicing [<start position>,<end position>]  
   positive value - starts counting from left-hand side  
   negative value - starts counting from right-hand side  
   For example:  
     If you press red color touch sensor it will return:  
       Pin(32)  
     Now to get only the integer value, we can slice the string  
     by removing the "Pin(" and that is 4 position from left.  
     We also need to remove the ")" and that is 1 position from right  
     (use negative "-" to indicate slice from right).  
     Which leaves a string "32".  
     And to make it as an integer value, we can use an int() casting.        
   """  

red_cts.irq(trigger=Pin.IRQ_RISING, handler=handle_interrupt)
grn_cts.irq(trigger=Pin.IRQ_RISING, handler=handle_interrupt)
blu_cts.irq(trigger=Pin.IRQ_RISING, handler=handle_interrupt)
rbw_cts.irq(trigger=Pin.IRQ_RISING, handler=handle_interrupt)

while True:
    if press:
        press = False
        if irq_pin == 32:
            print('Red color touch sensor triggered.')
        if irq_pin == 33:
            print('Green color touch sensor triggered.')
        if irq_pin == 34:
            print('Blue color touch sensor triggered.')
        if irq_pin == 35:
            print('Rainbow color touch sensor triggered.')

```

</div><div>This code works still the same as Example # 3 and Example # 4.</div><div> </div>## REFERENCES AND CREDITS:

<div>**<u>REFERENCES and CREDITS:</u>**</div><div>Purchase your Gorillacell ESP32 development kit at:</div><div>[gorillacell.kr](http://gorillacell.kr/)</div>