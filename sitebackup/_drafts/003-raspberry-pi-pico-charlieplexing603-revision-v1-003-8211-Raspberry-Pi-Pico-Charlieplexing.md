---
author: George Bantique
date: '2023-03-13T17:12:34+08:00'
excerpt: Have you ever wonder how to control a number of LEDs with less GPIO pins? I actually don’t have idea about Charlieplexing until recently that one of the viewer in my Youtube videos requested about it.
guid: https://techtotinker.com/?p=1367
id: 1367
permalink: /?p=1367
title: '003 &#8211; Raspberry Pi Pico: Charlieplexing'
url: /003-raspberry-pi-pico-charlieplexing603-revision-v1-003-8211-Raspberry-Pi-Pico-Charlieplexing
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/003-pico-charlieplexing.png)</figure>Have you ever wonder how to control a number of LEDs with less GPIO pins? I actually don’t have idea about Charlieplexing until recently that one of the viewer in my Youtube videos requested about it.

So after a couple of searching the internet, I build the test circuit, coded the bare program that I could come up and I feel the necessity to share it also. So, here it is and I hope you like it.

<div> </div>## BILL OF MATERIALS:

1. Raspberry Pi Pico development board which will serve as the brain for the demo circuit.
2. 6 pieces of LEDs preferably with different colors but any will do.
3. A breadboard to hold the circuit in place.
4. Some dupont jumper wires to interconnect the circuit.

<div> </div>## SCHEMATIC DIAGRAM:

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/003-pico-Charlieplexing_SCHEM.png)</figure><div> </div>## HARDWARE INSTRUCTION:

1. First, prepare the needed materials stated above.
2. Next, attach the Raspberry Pi Pico on the breadboard and make sure that the pins are properly aligned.
3. Next, arrange each pair of LEDs with opposite direction (please refer to the circuit diagram).
4. Next, attach GPIO 11 with green dupont wire to one side of the the green LED pairs.
5. Next, attach GPIO 12 with blue dupont wire to one side of the blue LED pairs.
6. Next, attach GPIO 13 with yellow dupont wire to one side of the yellow LED pairs.
7. Next, attach GPIO 11 with green dupont wire to the other side of the yellow LED pairs.
8. Next, attach GPIO 12 with blue dupont wire to the other side of the green LED pairs.
9. Next, attach GPIO 13 with yellow dupont wire to the other side of the blue LED pairs.
10. Please carefully refer to the schematic diagram.
11. Note: I do not use any current limiting resistor for simplicity purposes BUT it is highly advisable to use around 330 Ohms resistor along the Green pin, Blue pin, and Yellow pin.

<div> </div>## CIRCUIT DIAGRAM:

<div>If all goes well, your circuit should look like this.</div><figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/003-pico-Charlieplexing_DIAG.png)</figure><div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/Xg8u_WJjuwM?feature=oembed" title="003 - Raspberry Pi Pico: Charlieplexing" width="500"></iframe></div></figure><div> </div>For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

<div>```
 # More details can be found in TechToTinker.blogspot.com   
 # George Bantique | tech.to.tinker@gmail.com  
 from machine import Pin  
 from time import sleep_ms  
 def charlie(g_val, b_val, y_val):  
   if g_val < 0:  
     g = Pin(11, Pin.IN)  
   else:  
     g = Pin(11, Pin.OUT)  
     g.value(g_val)  
   if b_val < 0:  
     b = Pin(12, Pin.IN)  
   else:  
     b = Pin(12, Pin.OUT)  
     b.value(b_val)  
   if y_val < 0:  
     y = Pin(13, Pin.IN)  
   else:  
     y = Pin(13, Pin.OUT)  
     y.value(y_val)     
 def main():  
   while True:  
     """  
     TRUTH TABLE:  
     |-------------------------------------|  
     | g_val | b_val | y_val | Description |  
     |-------------------------------------|  
     |   1   |  0    | -1    | Green 1     |  
     |   0   |  1    | -1    | Green 2     |  
     |  -1   |  1    |  0    | Blue 1      |  
     |  -1   |  0    |  1    | Blue 2      |  
     |   1   | -1    |  0    | Yellow 1    |  
     |   0   | -1    |  1    | Yellow 2    |  
     |-------------------------------------|  
     """  
     charlie(1,0,-1) # green 1  
     sleep_ms(100)  
     charlie(0,1,-1) # green 2  
     sleep_ms(100)  
     charlie(-1,1,0) # blue 1  
     sleep_ms(100)  
     charlie(-1,0,1) # blue 2  
     sleep_ms(100)  
     charlie(1,-1,0) # yellow 1  
     sleep_ms(100)  
     charlie(0,-1,1) # yellow 2  
     sleep_ms(100)   

```

</div>