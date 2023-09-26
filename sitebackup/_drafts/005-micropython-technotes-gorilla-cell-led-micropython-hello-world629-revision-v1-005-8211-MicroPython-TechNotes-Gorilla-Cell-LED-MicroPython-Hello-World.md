---
author: George Bantique
date: '2023-03-15T16:47:51+08:00'
excerpt: In this article, we will look at LED. We will learn on how to control it by turning it ON and OFF. LED stands for Light-Emitting Diode. It is a type of electronic of component that emits light when a sufficient voltage is applied on its terminals.
guid: https://techtotinker.com/?p=1506
id: 1506
permalink: /?p=1506
title: '005 &#8211; MicroPython TechNotes: Gorilla Cell LED | MicroPython Hello World'
url: /005-micropython-technotes-gorilla-cell-led-micropython-hello-world629-revision-v1-005-8211-MicroPython-TechNotes-Gorilla-Cell-LED-MicroPython-Hello-World
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/005-technotes-led-micropython-techtotinker.png)</figure>In this article, we will look at LED. We will learn on how to control it by turning it ON and OFF. **LED** stands for **L**ight-**E**mitting **D**iode. It is a type of electronic of component that emits light when a sufficient voltage is applied on its terminals.

<div> </div>## PINOUT:

1. **G** – for the ground pin.
2. **V** – for the supply voltage pin.
3. **S** – for the control signal for the LED. Applying a logic 1 on this pin will turn ON the LED and a logic 0 turn it OFF.

<div> </div>## BILL OF MATERIALS:

1. LED module.
2. ESP32 Development board.
3. Gorilla ESP32 shield.
4. 3-pin female-to-female dupont jump wires.

<div> </div>## HARDWARE INSTRUCTION:

1. Connect ESP32 dev board at the top of ESP32 shield observing correct orientation and alignment.
2. Attach a dupont to LED module observing proper color-coding which black wire for the ground, red wire for the VCC, and yellow wire for other signal pins.
3. Attach the other side of dupont to ESP32 shield by matching the color-coding of the dupont and the pin headers which black to black, red to red, and other colors to yellow.
4. Power up the ESP32 shield by attaching the USB type-C cable.
5. Connect the micro USB cable to ESP32 dev board.

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/005-technotes-led-micropython-techtotinker-diagram.png)</figure><div> </div>## SOFTWARE INSTRUCTION:

1. Open Thonny Python IDE.
2. Click “New” to create a new file.
3. Copy and paste one of the example source code.
4. Save it to your computer in your preferred location. I recommend to create a single folder location for easy reference.
5. Click the “Stop” button in Thonny. The triple greater than sign should appear &gt;&gt;&gt;.
6. Click the “Run” button to execute the code.
7. Enjoy.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/CsMkJr3qhH4?feature=oembed" title="005 - MicroPython TechNotes: LED | MicroPython Hello World" width="500"></iframe></div></figure><div> </div>## CALL TO ACTION:

For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. LED Example # 1, turning ON or OFF:

<div>```
from machine import Pin

red = Pin(16, Pin.OUT)

# The following codes should be tested
# using the REPL
red.on()  # Turn ON LED
red.off() # Turn OFF LED

red.value(1) # Turn ON LED
red.value(0) # Turn OFF LED

red.value(True)  # Turn ON LED
red.value(False) # Turn OFF LED

```

</div>#### How the code works:

<div style="text-align: left;">*<span style="color: red;">from machine import Pin</span>*</div><div style="text-align: left;">This imports or loads the Pin class from the machine module to enable access to the ESP32 pins. </div><div style="text-align: left;">*<span style="color: red;">red = Pin(16, Pin.OUT)</span>*</div><div style="text-align: left;">Creates an object named “red” for the red LED which connected on GPIO 16 denoted by the number “16” and the pin is set as an output by “Pin.OUT”. </div><div style="text-align: left;">The red LED can be turned ON by calling any of the following:</div><div style="text-align: left;">*<span style="color: red;">red.on()</span>*</div><div style="text-align: left;">*<span style="color: red;">red.value(1)</span>*</div><div style="text-align: left;">**<span style="color: red;">red.value(True)</span>**</div><div style="text-align: left;">or turn it OFF by calling any of the following:</div><div style="text-align: left;"><div>*<span style="color: red;">red.off()</span>*</div><div>*<span style="color: red;">red.value(0)</span>*</div><div>*<span style="color: red;">red.value(False)</span>*</div></div><div> </div>### 2. LED Example#2, blinking all LED:

<div>```
from machine import Pin
from time import sleep

r = Pin(16, Pin.OUT)
g = Pin(17, Pin.OUT)
b = Pin(18, Pin.OUT)
y = Pin(19, Pin.OUT)

while True:
    # Turn ON 'all' LEDs
    r.on()
    g.on()
    b.on()
    y.on()
    sleep(1)
    
    # Turn OFF 'all' LEDs
    r.off()
    g.off()
    b.off()
    y.off()
    sleep(1)
```

</div>#### How the code works:

<div style="text-align: left;">*<span style="color: red;">from machine import Pin  
from time import sleep</span>*</div><div style="text-align: left;">This imports the Pin class from the machine library and the sleep class from the time library. Pin class enables access to the pins of ESP32 while sleep class enables the use of sleep() function which is equivalent to delay. *<span style="color: red;">r = Pin(16, Pin.OUT)</span>*

</div><div style="text-align: left;">*<span style="color: red;">g = Pin(17, Pin.OUT)</span>*</div><div style="text-align: left;">*<span style="color: red;">b = Pin(18, Pin.OUT)</span>*</div><div style="text-align: left;">*<span style="color: red;">y = Pin(19, Pin.OUT)</span>*</div><div style="text-align: left;">Creates objects for the LEDs namely “r” for the red LED, “g” for the green LED, “b” for the blue LED, and “y” for the yellow LED. *<span style="color: red;">while True:</span>*

</div><div style="text-align: left;">*<span style="color: red;"> r.on()  
</span>*</div><div style="text-align: left;">*<span style="color: red;"> g.on()  
</span>*</div><div style="text-align: left;">*<span style="color: red;"> b.on()  
</span>*</div><div style="text-align: left;">*<span style="color: red;"> y.on()  
</span>*</div><div style="text-align: left;">*<span style="color: red;"> sleep(1)  
</span>*</div><div style="text-align: left;">*<span style="color: red;"> </span>*</div><div style="text-align: left;">*<span style="color: red;"> r.off()  
</span>*</div><div style="text-align: left;">*<span style="color: red;"> g.off()  
</span>*</div><div style="text-align: left;">*<span style="color: red;"> b.off()  
</span>*</div><div style="text-align: left;">*<span style="color: red;"> y.off()  
</span>*</div><div style="text-align: left;">*<span style="color: red;"> sleep(1)</span>*</div><div style="text-align: left;">Creates the main loop of the program. </div><div style="text-align: left;">“while True:”makes the loop to loop forever or indefinitely. Any code inside will be executed one by one until the end then the cycle will be repeated.</div><div style="text-align: left;"> </div><div style="text-align: left;">Inside the main loop, all the LEDs are turned ON then it will wait for 1 second before turning it OFF then wait for another 1 second then the program will be repeated.</div><div> </div>### 3. LED Example#3, running LED:

<div>```
from machine import Pin
from time import sleep

r = Pin(16, Pin.OUT)
g = Pin(17, Pin.OUT)
b = Pin(18, Pin.OUT)
y = Pin(19, Pin.OUT)

# Create the list of led objects
led = [r, g, b, y]
while True:
    # Create a loop that will iterate from 0 to 3
    for x in range(4):
        print(x)       # Print the current value of 'x'
        led[x].on()    # Turn ON current x led
        led[x-1].off() # Turn OFF previous x led
        sleep(0.5)     # Stay in the LED value for a definite time
```

</div>#### How the code works:

<div style="text-align: left;">The works similar to example # 2 but this time, it is turned ON one-by-one to create the running-LED-effect. </div><div style="text-align: left;">*<span style="color: red;">led = \[r, g, b, y\]</span>*</div><div style="text-align: left;">Creates the list of LED objects which works similar to an array.</div><div style="text-align: left;"> </div><div style="text-align: left;">Inside the main loop, the LEDs are turned on one-by-one by using the for loop and turned on the LED according to current loop index. </div><div> </div>## REFERENCES AND CREDITS:

<div>1. [gorillacell.kr/](http://gorillacell.kr/)</div>