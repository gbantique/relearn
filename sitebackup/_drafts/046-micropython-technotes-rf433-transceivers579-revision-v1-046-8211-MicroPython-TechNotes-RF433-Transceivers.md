---
author: George Bantique
date: '2023-03-11T18:35:44+08:00'
excerpt: "In this article, I will tackle how you can use an RF433 transceiver modules with ESP32 using MicroPython.\n \nRF433 transmitter and receiver modules uses a radio frequency of 433 MHz which is under the Industrial, Scientific, and Medical bandwidth or ISM band for short."
guid: https://techtotinker.com/?p=1277
id: 1277
permalink: /?p=1277
title: '046 &#8211; MicroPython TechNotes: RF433 Transceivers'
url: /046-micropython-technotes-rf433-transceivers579-revision-v1-046-8211-MicroPython-TechNotes-RF433-Transceivers
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/09/046-2BMicroPython-2BTechNotes-2BRF433-2BTransceivers.png)</figure><div> </div><div style="text-align: left;">In this article, I will tackle how you can use an RF433 transceiver modules with ESP32 using MicroPython.</div><div style="text-align: left;"> </div><div style="text-align: left;">RF433 transmitter and receiver modules uses a radio frequency of 433 MHz which is under the Industrial, Scientific, and Medical bandwidth or ISM band for short.</div><div> </div><div>**<u>How does it works?</u>**</div><div>The RF433 transmitter and receiver module both have 3 pins which are (1) G for the ground pin, (2) V for the supply voltage, and (3) S – for the signal pin or data pin. In receiver module, S pin serves as the receive data pin while in transmitter module, S pin serves as the transmitter data pin.</div><div> </div><div>**<u>What you will need?</u>**</div><div>To follow this lesson, you will need the following:</div><div>1. An ESP32 development board, 1 for the transmitter side and another 1 for the receiver side.</div><div>2. A Gorillacell ESP32 shield, 2 pieces for both side of the circuit. This component is optional, you may use a breadboard instead if you want. Gorillacell ESP32 shield simplifies circuit connection.</div><div>3. A 3-pin dupont jumper wires, 2 pieces for both side of the circuit.</div><div>4. The RF433 transceivers, the receiver and the transmitter module.</div><div> </div><div>**<u>Side-trip:</u>**</div><div>Since I don’t have prior knowledge in using an RF433 transceiver modules so I did some quick research in the internet. I found this video <https://www.youtube.com/watch?v=98tYLEnevfA> where he demonstrates the use of the module without using any microcontroller. You might want to jump to 2:23 timestamp to just watch the demonstration.</div><div> </div><figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/09/RF433-2BReceiver-2BTransmitter-2Bwithout-2BMicrocontroller.png)</figure><div> </div><div>I have the exact components, so I followed the circuit but it is a bit clunky. It easily picks interference and it is not working properly.</div><div> </div><figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/09/Gorillacell-2BRF433-2BTransceivers-2Bwithout-2BMicrocontroller.png)</figure>I have a spare components from Gorillacell for RF433 transceivers which produces a functional circuit.

<div> </div>## HARDWARE INSTRUCTION:

1\. Attach the RF433 transceiver module to ESP32 as follows:

 RF433 Rx/Tx G pin to ESP32 ground pin.

 RF433 Rx/Tx V pin to ESP32 5V pin.

 RF433 Rx/Tx S pin to ESP32 GPIO 23 pin.

2\. (Optional) Power the ESP32 shield with an external power supply through the type-C USB cable connector.

3\. Connect the ESP32 to the computer through a micro USB cable.

<div> </div>## SOFTWARE INSTRUCTION:

1\. Copy the source code and run source code to the assigned ESP32, 1 for the transmitter side and another 1 for the receiver side circuit.

2\. Feel free to modify the source code and please let me know if you made progress or you have any concern.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/BkmIUqccwDw?feature=oembed" title="046 - MicroPython TechNotes: RF433 Transceiver" width="500"></iframe></div></figure><div> </div><div>If you have any concern regarding this video, please write your question in the comment box.</div><div> </div><div>You might also liked to support my journey on Youtube by subscribing on my channel, TechToTinker. [Click this to Subscribe.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)</div><div> </div><div>Thank you and have a good days ahead.</div><div> </div><div>See you,</div><div>– George Bantique | tech.to.tinker@gmail.com</div><div> </div>## SOURCE CODE:

### 1. Example # 1, blinking an LED, transmitter:

<div style="text-align: left;">```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms

transmit = Pin(23, Pin.OUT)
sw = Pin(0, Pin.IN)

while True:
    transmit.value(not transmit.value())
    sleep_ms(50)

```

</div><div> </div>### 2. Example # 1, blinking an LED, receiver:

<div style="text-align: left;">```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms

receive = Pin(23, Pin.IN)
led = Pin(2, Pin.OUT)

while True:
    led.value(receive.value())
    sleep_ms(50)

```

</div><div> </div>### 3. Example # 2, sending message through RF433 modulated serial UART:

<div style="text-align: left;">```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from machine import UART

tx = UART(2, baudrate=9600, tx=23, rx=25)

# tx.write('This is a test messagern')

```

</div><div> </div>### 4. Example # 2, receiving message through RF433 modulated serial UART:

<div style="text-align: left;">```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from machine import UART

rx = UART(2, baudrate=9600, tx=25, rx=23)

# print(rx.read())
while True:
    if rx.any():
        print(rx.read())

```

</div><div> </div>### 5. Modified # 1, toggling the state of the receiver on-board LED, transmitter:

<div style="text-align: left;">```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms

transmit = Pin(23, Pin.OUT)
sw = Pin(0, Pin.IN)
var_sw = 1

while True:
    if sw.value()==0:       # means, sw is pressed!
        var_sw = not var_sw # toggle the state of variable
    transmit.value(var_sw)  # send it to the radio
    sleep_ms(50)

```

</div><div> </div>### 6. Modified # 1, toggling the state of the receiver on-board LED, transmitter:

<div style="text-align: center;">*<span style="color: red;">**(use the same source code in # 2)**</span>*</div><div> </div>## REFERENCES AND CREDITS:

<div style="text-align: left;">1. Purchase your Gorillacell ESP32 development kits:</div><div style="text-align: left;"> <https://gorillacell.kr/></div><div style="text-align: left;"> </div><div style="text-align: left;">2. Video showing the use of RF433 transceiver modules without an MCU:</div><div style="text-align: left;"> <https://www.youtube.com/watch?v=98tYLEnevfA/></div>