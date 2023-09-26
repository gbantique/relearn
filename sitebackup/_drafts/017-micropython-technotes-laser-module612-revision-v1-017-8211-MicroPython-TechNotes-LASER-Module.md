---
author: George Bantique
date: '2023-03-14T08:56:43+08:00'
excerpt: In this article, we will learn on how to use a LASER module with ESP32 using MicroPython programming language.
guid: https://techtotinker.com/?p=1411
id: 1411
permalink: /?p=1411
title: '017 &#8211; MicroPython TechNotes: LASER Module'
url: /017-micropython-technotes-laser-module612-revision-v1-017-8211-MicroPython-TechNotes-LASER-Module
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/017-technotes-LASER.png)</figure>In this article, we will learn on how to use a LASER module with ESP32 using MicroPython programming language.

<div> </div>## PINOUT:

1. **G** – for the ground pin.
2. **V** – for the supply voltage.
3. **S** – for the control signal pin.

<div> </div>## BILL OF MATERIALS:

1. ESP32 development board with MicroPython firmware that will serve as the main microcontroller.
2. ESP32 shield from Gorillacell ESP32 development kit to extend the pins of ESP32 and to provide easy circuit connection.
3. 3-pin female-female dupont jumper wires.
4. And of course the LASER module itself.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 development board on top of the ESP32 shield and make sure that both USB port are on the same side.
2. Next, attach the dupont wires to the LASER module by following the color coding which black for the ground, red for the VCC, and yellow for the control signal pin.
3. Next, attach the other side of the dupont jumper wires to the ESP32 shield by matching the colors of the wires to the colors of the pin headers which is black to black, red to red, and yellow to yellow. For this experiment, I choose GPIO 25 because this ESP32 shield provides 5V on GPIO 25.
4. Next, power the ESP32 shield with an external power supply with type-C USB connector. Make sure that the power switch is set to ON state.
5. Next, connect the ESP32 to the computer through a micro-USB cable. The demo circuit should be ready.

<div> </div>## SOFTWARE INSTRUCTION:

For this experiment, I prepared 2 example source code. Just copy and paste it in Thonny. Enjoy.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/yq0LTKFgReo?feature=oembed" title="017 - MicroPython TechNotes: LASER" width="500"></iframe></div></figure><div> </div>## CALL TO ACTION:

For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. Example # 1, basic and simple control of LASER module:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin

laser = Pin(25, Pin.OUT)

# # The following source should be tested using the REPL:
# laser.on()  # Turns ON the laser.
# laser.off() # Turns OFF the laser.


```

</div><div> </div>#### How the code works:

<div style="text-align: left;">*<span style="color: red;">from machine import Pin</span>*</div><div style="text-align: left;">Loads or imports the necessary library, in this case the Pin class from the machine module. This is in order to access the pins of ESP32.</div><div style="text-align: left;"> </div><div style="text-align: left;">*<span style="color: red;">laser = Pin(25, Pin.OUT)</span>*</div><div style="text-align: left;">This line of code creates the object named laser which is connected on GPIO 25 with a pin direction set as an output.</div><div style="text-align: left;"> </div><div style="text-align: left;">*<span style="color: red;">laser.on()</span>*</div><div style="text-align: left;">This will set the pin to logic HIGH which will turn ON the laser module.</div><div style="text-align: left;"> </div><div style="text-align: left;">*<span style="color: red;">laser.off()</span>*</div><div style="text-align: left;">This will clear the pin to logic LOW which will turn OFF the laser module.</div><div> </div>### 2. Example # 2, SOS using the LASER module:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import ticks_ms

laser = Pin(25, Pin.OUT)
switch = Pin(0, Pin.IN, Pin.PULL_UP)

DOTS_INTERVAL = 250
DASH_INTERVAL = 750
LOWS_INTERVAL = 500
BETW_INTERVAL = 700

isSOS = False
startTime = ticks_ms()
steps = 0

while True:
    if isSOS:
        if steps == 1 or steps == 3 or steps == 5:
            laser.on()
            if ticks_ms() - startTime >= DOTS_INTERVAL :
                steps += 1
                startTime = ticks_ms()
        elif steps == 7 or steps == 9 or steps == 11:
            laser.on()
            if ticks_ms() - startTime >= DASH_INTERVAL:
                steps += 1
                startTime = ticks_ms()
        elif steps == 0 or steps == 6 or steps == 12:
            laser.off()
            if ticks_ms() - startTime >= BETW_INTERVAL:
                steps += 1
                startTime = ticks_ms()
        else:
            laser.off()
            if ticks_ms() - startTime >= LOWS_INTERVAL:
                steps += 1
                startTime = ticks_ms()
         
        if steps > 12:
             steps = 0
        
    if switch.value() == 0:
        laser.off()
        #print('press')
        isSOS = not isSOS
        steps = 0
        sleep_ms(300)

```

</div><div> </div>#### How the code works:

<div style="text-align: left;">*<span style="color: red;">from machine import Pin</span>*</div><div style="text-align: left;">*<span style="color: red;">from time import ticks\_ms</span>*</div><div style="text-align: left;">Loads the necessary libraries needed. ticks_ms class from time module will return a time value in milliseconds that ticks every 1 milliseconds, hence its name.</div><div style="text-align: left;"> </div><div style="text-align: left;"><span style="color: red;">*laser = Pin(25, Pin.OUT)*</span></div><div style="text-align: left;"><span style="color: red;">*switch = Pin(2, Pin.IN, Pin.PULL\_UP)*</span></div><div style="text-align: left;">Creates the objects for the laser and the switch.</div><div style="text-align: left;">The switch object is connected to GPIO 2 with pin direction set as input and the internal pull-up resistor is enabled. The pull-up resistor will prevent tri-state or unknown state (it will make sure that the pin can be read with a known digital value, either 1 or 0). An input pin with pull-up resistor will have default value of (logic) 1 when the button is not press else the value is (logic) 0.</div><div style="text-align: left;"> </div><div style="text-align: left;"><div><div><span style="color: red;">*DOTS\_INTERVAL = 250*</span></div><div><span style="color: red;">*DASH\_INTERVAL = 750*</span></div><div><span style="color: red;">*LOWS\_INTERVAL = 500*</span></div><div><span style="color: red;">*BETW\_INTERVAL = 700*</span></div><div>This are constants that will be use as an individual interval for the SOS signal</div><div> </div><div><div>*<span style="color: red;">isSOS = False</span>*</div><div>*<span style="color: red;">startTime = ticks\_ms()</span>*</div><div>*<span style="color: red;">steps = 0</span>*</div><div>This are variables:</div><div>**isSOS** flag will determine if the sending of SOS signal should be started. </div><div>**startTime** captures the beginning of the timer counter.</div><div>**steps** variable holds the current steps in SOS signal.</div><div> </div><div><span style="color: red;">*while True:*</span></div><div>Creates the infinite loop for the program.</div><div> </div><div><div><div>*<span style="color: red;">if isSOS:</span>*</div><div>*<span style="color: red;"> if steps == 1 or steps == 3 or steps == 5:</span>*</div><div>*<span style="color: red;"> laser.on()</span>*</div><div>*<span style="color: red;"> if ticks\_ms() – startTime &gt;= DOTS\_INTERVAL :</span>*</div><div>*<span style="color: red;"> steps += 1</span>*</div><div>*<span style="color: red;"> startTime = ticks\_ms()</span>*</div><div>*<span style="color: red;"> elif steps == 7 or steps == 9 or steps == 11:</span>*</div><div>*<span style="color: red;"> laser.on()</span>*</div><div>*<span style="color: red;"> if ticks\_ms() – startTime &gt;= DASH\_INTERVAL:</span>*</div><div>*<span style="color: red;"> steps += 1</span>*</div><div>*<span style="color: red;"> startTime = ticks\_ms()</span>*</div><div>*<span style="color: red;"> elif steps == 0 or steps == 6 or steps == 12:</span>*</div><div>*<span style="color: red;"> laser.off()</span>*</div><div>*<span style="color: red;"> if ticks\_ms() – startTime &gt;= BETW\_INTERVAL:</span>*</div><div>*<span style="color: red;"> steps += 1</span>*</div><div>*<span style="color: red;"> startTime = ticks\_ms()</span>*</div><div>*<span style="color: red;"> else:</span>*</div><div>*<span style="color: red;"> laser.off()</span>*</div><div>*<span style="color: red;"> if ticks\_ms() – startTime &gt;= LOWS\_INTERVAL:</span>*</div><div>*<span style="color: red;"> steps += 1</span>*</div><div>*<span style="color: red;"> startTime = ticks\_ms()</span>*</div><div>This lines of code creates the SOS signal.</div><div> </div><div><div><span style="color: red;">*if steps &gt; 12:*</span></div><div><span style="color: red;"> *steps = 0*</span></div></div><div>This makes sure that SOS signal repeats itself.</div><div> </div><div><div>*<span style="color: red;">if switch.value() == 0:</span>*</div><div>*<span style="color: red;"> laser.off()</span>*</div><div>*<span style="color: red;"> #print(‘press’)</span>*</div><div>*<span style="color: red;"> isSOS = not isSOS</span>*</div><div>*<span style="color: red;"> steps = 0</span>*</div><div>*<span style="color: red;"> sleep\_ms(300)</span>*</div></div><div>This if statement reads the current state of the pin associated to switch object which is connected on GPIO 2. If the read value is 0, it means that the switch is press.</div><div>If the switch is press, turns OFF the laser module, toggled the isSOS flag, reset the steps variable to 0, and a delay of 300 milliseconds to prevent multiple detection of switch press.</div></div></div></div></div><div> </div></div><div> </div>## REFERENCES AND CREDITS:

<div style="text-align: left;">Purchased your Gorillacell ESP32 development kit at:</div><div style="text-align: left;">[gorillacell.kr](http://gorillacell.kr/)</div>