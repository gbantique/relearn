---
author: George Bantique
date: '2023-03-14T08:39:39+08:00'
excerpt: In this article, we will learn on how to read an input device with ESP32 using MicroPython programming language. GorillaCell ESP32 development kit comes with 4 pieces of Button modules with different colors of button caps.
guid: https://techtotinker.com/?p=1405
id: 1405
permalink: /?p=1405
title: '018 &#8211; MicroPython TechNotes: Button | Reading an Input'
url: /018-micropython-technotes-button-reading-an-input611-revision-v1-018-8211-MicroPython-TechNotes-Button-Reading-an-Input
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/018-technotes-buttons.png)</figure>In this article, we will learn on how to read an input device with ESP32 using MicroPython programming language. GorillaCell ESP32 development kit comes with 4 pieces of Button modules with different colors of button caps.

<div> </div>## PINOUT:

1. **G** – for the ground pin.
2. **V** – for the VCC or supply voltage.
3. **S** – for the signal pin.

<div> </div>## BILL OF MATERIALS:

1. **ESP32 development board** flashed with MicroPython firmware. If your ESP32 has no MicroPython firmware, be sure to learn it from: https://techtotinker.blogspot.com/2021/01/001-micropython-technotes-get-started.html . The ESP32 will serve as the brain for this experiment.
2. **ESP32 shield** from GorillaCell ESP32 development kit which will extend the ESP32 pins to the pin headers for easy access and easy connection without confusion.
3. **3-pin female-female dupont wires** which will connects the button modules to the ESP32 shield pin headers.
4. **Button modules** which will serve as an input device.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 on top of the ESP32 shield and make sure that both USB ports are on the same side.
2. Next, attach the dupont wires to the button module by following the color coding which is black wires for the ground, red wires for the VCC, and yellow wires for the signal pin.
3. Next, attach the other end of the dupont wires to the ESP32 shield by matching the colors of the dupont wires to the colors of the pin headers which black to black, red to red, and yellow to yellow. For this experiment I choose GPIO 32 as the input signal pin for the Red button, GPIO 33 for the Green button, and GPIO 34 for the Blue button.
4. Next, power the ESP32 shield with an external power supply with type-C USB cable and make sure that the power switch is set to ON state.
5. Lastly, connect the ESP32 to the computer through the micro-USB cable. Our demo circuit should now be ready. Yehey!

<div> </div>## SOFTWARE INSTRUCTION:

For the button module, I prepared 3 examples you can copy to Thonny for you to try.

Play with it, modify it according to your needs.

Enjoy learning and happy tinkering.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/WbGduiwDAGc?feature=oembed" title="018 - MicroPython TechNotes: Buttons | Reading an Input" width="500"></iframe></div></figure><div> </div>## CALL TO ACTION:

For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. Example # 1, reading an input device:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin

button = Pin(32, Pin.IN)
led = Pin(2, Pin.OUT)

# # The following lines of code can be tested in the REPL:
# button.value() # Reads the current value of the button.
# # Return value is 1 when button is press else,
# # the value is 0 when button is release or not press.

```

</div><div> </div>#### How the code works:

<div>*<span style="color: red;">from machine import Pin</span>*</div><div>Loads the Pin class from machine module to access the pins of ESP32.</div><div> </div><div>*<span style="color: red;">button = Pin(32, Pin.IN)</span>*</div><div>Creates a “button” object which is connected to GPIO 32 with a pin direction set to input.</div><div> </div><div>*<span style="color: red;">led = Pin(2, Pin.OUT)</span>*</div><div>Creates an “led” object which is connected to GPIO 2 with a pin direction set to output.</div><div> </div><div>*<span style="color: red;">button.value()</span>*  
Reads the state of the pin associated to the button object. </div><div> </div>### 2. Example # 2, multiple input device:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms

red_button = Pin(32, Pin.IN)
grn_button = Pin(33, Pin.IN)
blu_button = Pin(34, Pin.IN)
led = Pin(2, Pin.OUT)

while True:
    led.value(not led.value())
    
    if red_button.value()==1:
        print('Red button is press')
    if grn_button.value()==1:
        print('Green button is press')
    if blu_button.value()==1:
        print('Blue button is press')
        
    sleep_ms(200)

```

</div><div> </div>#### How the code works:

<div>*<span style="color: red;">from machine import Pin</span>*</div><div>Loads the Pin class from the machine module to access the pins of ESP32.</div><div> </div><div>*<span style="color: red;">from time import sleep\_ms</span>*</div><div>Loads the sleep_ms from time module to create a blocking delay in milliseconds.</div><div> </div><div>*<span style="color: red;">red\_button = Pin(32, Pin.IN)</span>*</div><div>*<span style="color: red;">grn\_button = Pin(33, Pin.IN)</span>*</div><div>*<span style="color: red;">blu\_button = Pin(34, Pin.IN)</span>*</div><div>Creates a buttons object which are connected on GPIO 32, 33, and 34 with pin directions set to input.</div><div> </div><div>*<span style="color: red;">led = Pin(2, Pin.OUT)</span>*</div><div>Creates an “led” object which is connected to GPIO 2 with a pin direction set to output.</div><div> </div><div>*<span style="color: red;">while True:</span>*</div><div>Creates an infinite loop.</div><div> </div><div>*<span style="color: red;">led.value(not led.value())</span>*</div><div>Toggles the state of the pin associated to led object.</div><div> </div><div>*<span style="color: red;">if red\_button.value()==1:</span>*</div><div><div>*<span style="color: red;"> print(‘Red button is press’)</span>*</div><div>*<span style="color: red;">if grn\_button.value()==1:</span>*</div><div>*<span style="color: red;"> print(‘Green button is press’)</span>*</div><div>*<span style="color: red;">if blu\_button.value()==1:</span>*</div><div>*<span style="color: red;"> print(‘Blue button is press’)</span>*</div></div><div>Checks if the pin associated to the pin has a logic value of 1 which means it is being press. If it is press, send a message accordingly in the REPL.</div><div> </div><div>*<span style="color: red;">sleep\_ms(200)</span>*</div><div>Creates a 200 milliseconds blocking delay.</div><div> </div>### 3. Example # 3, simple application of button modules:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms

red_button = Pin(32, Pin.IN)
grn_button = Pin(33, Pin.IN)
blu_button = Pin(34, Pin.IN)
led = Pin(2, Pin.OUT)

DEFAULT_COUNTER_VALUE = 0
COUNTER_CHANGE = 1
counter_value = DEFAULT_COUNTER_VALUE
print('counter_value is currently {}.'.format(counter_value)) 

while True:
    led.value(not led.value())
    
    if red_button.value()==1:
        print('Red button is press:')
        counter_value = counter_value + COUNTER_CHANGE
        print('> counter_value incremented to {}.'.format(counter_value))
    if grn_button.value()==1:
        print('Green button is press:')
        counter_value = DEFAULT_COUNTER_VALUE
        print('> counter_value resetted to {}.'.format(counter_value))
    if blu_button.value()==1:
        print('Blue button is press:')
        counter_value = counter_value - COUNTER_CHANGE
        print('> counter_value decremented to {}.'.format(counter_value))
        
    sleep_ms(200)

```

</div><div> </div>#### How the code works:

<div>*<span style="color: red;">from machine import Pin</span>*</div><div>Loads the Pin class from the machine module to access the pins of ESP32.</div><div> </div><div>*<span style="color: red;">from time import sleep\_ms</span>*</div><div>Loads the sleep_ms from time module to create a blocking delay in milliseconds.</div><div> </div><div>*<span style="color: red;">red\_button = Pin(32, Pin.IN)</span>*</div><div>*<span style="color: red;">grn\_button = Pin(33, Pin.IN)</span>*</div><div>*<span style="color: red;">blu\_button = Pin(34, Pin.IN)</span>*</div><div>Creates a buttons object which are connected on GPIO 32, 33, and 34 with pin directions set to input.</div><div> </div><div>*<span style="color: red;">led = Pin(2, Pin.OUT)</span>*</div><div>Creates an “led” object which is connected to GPIO 2 with a pin direction set to output.</div><div> </div><div><div>*<span style="color: red;">DEFAULT\_COUNTER\_VALUE = 0</span>*</div><div>*<span style="color: red;">COUNTER\_CHANGE = 1</span>*</div><div>*<span style="color: red;">counter\_value = DEFAULT\_COUNTER\_VALUE</span>*</div><div>Creates a constants and variables</div><div> </div><div>*<span style="color: red;">print(‘counter\_value is currently {}.’.format(counter\_value)) </span>*</div></div><div>Prints to the REPL.</div><div>{} is used as a place holder.</div><div>.format is used to place the set value to the place holder.</div><div> </div><div>*<span style="color: red;">while True:</span>*</div><div>Creates an infinite loop.</div><div> </div><div>*<span style="color: red;">led.value(not led.value())</span>*</div><div>Toggles the state of the pin associated to led object.</div><div> </div><div><div><span style="color: red;">*if red\_button.value()==1:*</span></div><div><span style="color: red;"> *print(‘Red button is press:’)*</span></div><div><span style="color: red;"> *counter\_value = counter\_value + COUNTER\_CHANGE*</span></div><div><span style="color: red;"> *print(‘&gt; counter\_value incremented to {}.’.format(counter\_value))*</span></div><div><span style="color: red;">*if grn\_button.value()==1:*</span></div><div><span style="color: red;"> *print(‘Green button is press:’)*</span></div><div><span style="color: red;"> *counter\_value = DEFAULT\_COUNTER\_VALUE*</span></div><div><span style="color: red;"> *print(‘&gt; counter\_value resetted to {}.’.format(counter\_value))*</span></div><div><span style="color: red;">*if blu\_button.value()==1:*</span></div><div><span style="color: red;"> *print(‘Blue button is press:’)*</span></div><div><span style="color: red;"> *counter\_value = counter\_value – COUNTER\_CHANGE*</span></div><div><span style="color: red;"> *print(‘&gt; counter\_value decremented to {}.’.format(counter\_value))*</span></div></div><div>Checks if the pin associated to the pin has a logic value of 1 which means it is being press. If it is press, send a message as follows in the REPL:</div><div>1. that the specific button is press then the counter_value is either incremented, decremented, or resetted to the set default value.</div><div>2. then the incremented or decremented value of counter_value.</div><div> </div><div>*<span style="color: red;">sleep\_ms(200)</span>*</div><div>Creates a 200 milliseconds blocking delay.</div><div> </div>## REFERENCES AND CREDITS:

<div>Purchase your Gorillacell ESP32 development kit at:</div><div>[gorillacell.kr](http://gorillacell.kr/)</div>