---
author: George Bantique
date: '2023-03-14T11:48:18+08:00'
excerpt: In this article, I would like to share to you the results of my exploration with the basic GPIO of the Raspberry Pi Pico using the MicroPython language.
guid: https://techtotinker.com/?p=1460
id: 1460
permalink: /?p=1460
title: '001 &#8211; Raspberry Pi Pico: GPIO | MicroPython'
url: /001-raspberry-pi-pico-gpio-micropython621-revision-v1-001-8211-Raspberry-Pi-Pico-GPIO-MicroPython
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/001-pico-binary-coded-decimal-micropython.png)</figure>In this article, I would like to share to you the results of my exploration with the basic GPIO of the Raspberry Pi Pico using the MicroPython language.

<div> </div>## BILL OF MATERIALS:

1. Raspberry Pi Pico dev board.
2. Breadboard.
3. 5 pieces of LEDs with its appropriate limiting resistor.
4. 3 pieces of tactile switch.
5. and some jumper wires.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the Raspberry Pi Pico development board to the breadboard and make sure to refer to the circuit diagram below.
2. Next, setup all the LEDs so that its anode pin(long leg) is connected to the limiting resistor in order to prevent damaging the LEDs from over current while its cathode pin to the ground.
3. Next, connect the first LED (red LED in my case) to GPIO 2 (pin 4) of the RPi Pico.
4. Next, connect the second LED (green LED in my case) to GPIO 3 (pin 5) of the RPi Pico.
5. Next, connect the third LED (blue LED in my case) to GPIO 4 (pin 6) of the RPi Pico.
6. Next, connect the fourth LED (yellow LED in my case) to GPIO 5 (pin 7) of the RPi Pico.
7. Next, connect the fifth LED (white LED in my case) to GPIO 6 (pin 9) of the RPi Pico.
8. Next, setup all the tactile switch so that one side of the switch is connected to the ground.
9. Next, connect the first button to GPIO 11 (pin15) of the RPi Pico.
10. Next, connect the second button to GPIO 12 (pin 16) of the RPi Pico.
11. Next, connect the third button to GPIO 13 (pin 17) of the RPi Pico.
12. Lastly, make sure to connect the ground pin of RPi Pico to the ground rail. Our demo circuit should now be ready.

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/001-pico-binary-coded-decimal-micropython-diagram.png)</figure><div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/QmoB_7ihUgo?feature=oembed" title="001 - Raspberry Pi Pico: Displaying Binary Coded Decimal in LED" width="500"></iframe></div></figure><div> </div>## CALL TO ACTION:

For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. Binary Coded Decimal (BCD) using LEDs:

<div>*This code aims to display a Binary Coded Decimal (BCD) using LEDs. BCD value can be decremented using the first button (swA) or incremented using the third button (swC). The BCD value can be reset using the middle button (swB).*</div><div>```
# More details can be found in TechToTinker.blogspot.com   
 # George Bantique | tech.to.tinker@gmail.com  
 from machine import Pin  
 from time import sleep_ms  
 
 # LEDs:  
 r = Pin(2, Pin.OUT)  
 g = Pin(3, Pin.OUT)  
 b = Pin(4, Pin.OUT)  
 y = Pin(5, Pin.OUT)  
 w = Pin(6, Pin.OUT)  
 
 # Buttons:  
 swA = Pin(11, Pin.IN, Pin.PULL_UP)  
 swB = Pin(12, Pin.IN, Pin.PULL_UP)  
 swC = Pin(13, Pin.IN, Pin.PULL_UP) 
 
 # Converts an integer value to equivalent BCD  
 # For more details, visit TechToTinker.blogspot.com  
 def convert_to_bcd(bits, integer):  
   global bcd  
   for i in range(bits-1, -1, -1):  
     # check if the specific bit should be set or clear  
     div = integer//2**i  
     # save the binary value to specific bit in the BCD  
     bcd[i] = div  
     # get the remainder of the integer value  
     # which will be use for the next loop iteration  
     integer = integer%2**i  
     # DEBUG: print the current bit of BCD  
     #    and the current remainder.  
     print(div, integer)  
     
 # constants and global variables  
 NUM_OF_LED = 5  
 led = [w, y, b, g, r]  
 bcd = [0, 0, 0, 0, 0]  
 counter = 0  
 prev_cnt = 0  
 
 while True:  
   if swA.value()==0:  
     # Subtract 1 from counter  
     if counter>0:  
       counter -= 1  
   if swB.value()==0:  
     # Reset counter to 0  
     counter = 0  
   if swC.value()==0:  
     # Add 1 to counter  
     if counter < 31:  
       counter += 1  
       
   # Check if the counter is change  
   if prev_cnt != counter:  
     # convert the counter into BCD  
     convert_to_bcd(NUM_OF_LED, counter)  
     # display the current bcd value  
     for i in range(NUM_OF_LED):  
       led[i].value(bcd[i])  
     # DEBUG: print the current counter  
     print(counter)  
     # store the current counter value  
     prev_cnt = counter 
     
   # soft debounce  
   sleep_ms(250)  

```

</div><div> </div>### 2. Knight Rider v1:

<div>```
from machine import Pin
from time import sleep_ms

r = Pin(2, Pin.OUT)
g = Pin(3, Pin.OUT)
b = Pin(4, Pin.OUT)
y = Pin(5, Pin.OUT)
w = Pin(6, Pin.OUT)
led = [r, g, b, y, w]

swA = Pin(11, Pin.IN, Pin.PULL_UP)
swB = Pin(12, Pin.IN, Pin.PULL_UP)
swC = Pin(13, Pin.IN, Pin.PULL_UP)

delay = 50

while True:
    for i in range(0, 4, 1):
        led[i].value(1)
        sleep_ms(delay)
        led[i].value(0)
        sleep_ms(delay)
    for i in range(4, 0, -1):
        led[i].value(1)
        sleep_ms(delay)
        led[i].value(0)
        sleep_ms(delay)

```

</div><div> </div>### 3. Knight Rider v2:

<div>*This code creates smoother transition.*</div><div>```
from machine import Pin
from time import sleep_ms

r = Pin(2, Pin.OUT)
g = Pin(3, Pin.OUT)
b = Pin(4, Pin.OUT)
y = Pin(5, Pin.OUT)
w = Pin(6, Pin.OUT)
led = [r, g, b, y, w]

swA = Pin(11, Pin.IN, Pin.PULL_UP)
swB = Pin(12, Pin.IN, Pin.PULL_UP)
swC = Pin(13, Pin.IN, Pin.PULL_UP)

delay = 50

while True:
    for i in range(0, 4, 1):
        led[i].value(1)
        sleep_ms(delay)
        led[i+1].value(1)
        sleep_ms(delay)
        led[i].value(0)
        sleep_ms(delay*2)
    for i in range(4, 0, -1):
        led[i].value(1)
        sleep_ms(delay)
        led[i-1].value(1)
        sleep_ms(delay)
        led[i].value(0)
        sleep_ms(delay*2)

```

</div><div> </div>## REFERENCES AND CREDITS:

<div>none so far.</div>