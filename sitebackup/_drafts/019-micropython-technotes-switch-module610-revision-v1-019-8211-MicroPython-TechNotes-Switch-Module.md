---
author: George Bantique
date: '2023-03-14T08:13:42+08:00'
excerpt: In this article, we will learn on how to use switch module with ESP32 using MicroPython programming language.
guid: https://techtotinker.com/?p=1400
id: 1400
permalink: /?p=1400
title: '019 &#8211; MicroPython TechNotes: Switch Module'
url: /019-micropython-technotes-switch-module610-revision-v1-019-8211-MicroPython-TechNotes-Switch-Module
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/019-technotes-switch.png)</figure>In this article, we will learn on how to use switch module with ESP32 using MicroPython programming language.

<div> </div>## PINOUT:

1. **G** – for the ground pin.
2. **V** – for the supply voltage.
3. **S** – for the signal pin.

<div> </div>## BILL OF MATERIALS:

1. **ESP32 development board** which will serve as the brain for this experiment.
2. **ESP32 shield from Gorillacell** which will extend the ESP32 pins to shield’s pin headers for easy circuit connection.
3. **3-pin Female-Female dupont wires** to attach the switch module to ESP32 shield.
4. **Switch module**.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 on top of ESP32 shield and make sure that both USB port are on the same side.
2. Next, attach the dupont wires to switch module according to color coding which is black for the ground, red for the VCC, and yellow for the signal pin.
3. Next, attach the other end of the dupont wires to ESP32 shield by matching the colors of the wires to the colors of the pin headers. For this experiment, I choose GPIO 32 as the signal pin.
4. Next, power the ESP32 shield with an external power supply with a type-C USB cable.
5. Lastly, connect the ESP32 to the computer through a micro USB cable. Our circuit should now be ready.

<div> </div>## SOFTWARE INSTRUCTION:

For this, I prepared 2 example source code for you to try. Copy and paste it to Thonny IDE.

Play with it and adapt it according to your needs.

Enjoy.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/XZqDXWd_W8Q?feature=oembed" title="019 - MicroPython TechNotes: Switch" width="500"></iframe></div></figure><div> </div>For any concern, write your message in the comment section.

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

switch = Pin(32, Pin.IN)

# # The following lines of code can be tested in the REPL:
# switch.value() # Reads the current value of the switch.
# # Return value is 1 when switch is set to ON state,
# # the value is 0 when switch is set to OFF state.

```

</div>#### How the code works:

<div><span style="color: red;">*from machine import Pin*</span></div><div>Loads the Pin class from the machine module in order to access the pins of ESP32.</div><div> </div><div>*<span style="color: red;">switch = Pin(32, Pin.IN)</span>*</div><div>Creates the “switch” object which is connected to GPIO 32 with a pin direction as input.</div><div> </div><div>*<span style="color: red;">switch.value()</span>*</div><div>Reads the value of the pin associated to “switch” object, in this case, its GPIO 32.</div><div> </div>### 2. Example # 2, simple application for the switch module:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms

switch = Pin(32, Pin.IN)
led = Pin(2, Pin.OUT)

while True:
    if switch.value()==1:
        # Switch is set to ON state
        # Lets toggle the state of the LED
        # to create a blinking LED.
        led.value(not led.value())
        sleep_ms(300)
    else:
        # Switch is set to OFF state
        # Lets turn OFF the LED.
        led.value(0)

```

</div><div> </div>#### How the code works:

<div>*<span style="color: red;">from machine import Pin</span>*</div><div>*<span style="color: red;">from time import sleep\_ms</span>*</div><div>Loads the sleep_ms class from the time module. sleep_ms produces a blocking delay with a resolution in milliseconds.</div><div> </div><div><span style="color: red;">*switch = Pin(32, Pin.IN)*</span></div><div><span style="color: red;">*led = Pin(2, Pin.OUT)*</span></div><div>Creates the “led” object which is connected to GPIO 2 with a pin direction set to output.</div><div> </div><div>*<span style="color: red;">while True:</span>*</div><div>Creates an infinite loop in order to continuously check for the switch.</div><div> </div><div><span style="color: red;">*if switch.value()==1:*</span></div><div>This if statement checks if the switch is set to ON by reading the value of the pin associated with the switch.</div><div> </div><div>*<span style="color: red;">led.value(not led.value())</span>*</div><div>This line of code toggles the state of the pin associated with the led. If the state of led is 1, then make it as 0 or if the state is 0, then make it as 1.</div><div> </div><div>*<span style="color: red;">sleep\_ms(300)</span>*</div><div>This creates a 300 milliseconds blocking delay.</div><div> </div><div>*<span style="color: red;">led.value(0)</span>*</div><div>Sets the pin associated to led to 0.</div><div> </div>## REFERENCES AND CREDITS:

<div>Purchase your Gorillacell ESP32 development kit at:</div><div>[**gorillacell.kr**](http://gorillacell.kr/)</div>