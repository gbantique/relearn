---
author: George Bantique
date: '2023-03-11T19:50:54+08:00'
excerpt: In this article, I will show you on how we can use a Bluetooth Low Energy module with ESP32 using MicroPython. Bluetooth modules are useful for controlling applications such as in robotics or for displaying sensor values in IOT projects. BLE stands for Bluetooth Low Energy. It provides low power consumption because it stays in sleep mode most of the time.
guid: https://techtotinker.com/?p=1288
id: 1288
permalink: /?p=1288
title: '042 &#8211; MicroPython TechNotes: JDY-32 | Bluetooth Low Energy BLE'
url: /042-micropython-technotes-jdy-32-bluetooth-low-energy-ble584-revision-v1-042-8211-MicroPython-TechNotes-JDY-32-Bluetooth-Low-Energy-BLE
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/08/042-BLE-2Bblogs.png)</figure><div> </div>In this article, I will show you on how we can use a Bluetooth Low Energy module with ESP32 using MicroPython. Bluetooth modules are useful for controlling applications such as in robotics or for displaying sensor values in IOT projects. BLE stands for Bluetooth Low Energy. It provides low power consumption because it stays in sleep mode most of the time.

<div> </div>## BILL OF MATERIALS:

1. ESP32 development board.
2. Gorillacell ESP32 shield.
3. 4-pin female-female dupont wires.
4. Gorillacell Bluetooth 4.0 BLE module.

<div> </div>## PINOUT:

1. GND – for the grounds.
2. VCC – for the supply voltage.
3. TX – Serial UART transmit pin.
4. RX – Serial UART receive pin.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 development board on top of Gorillacell ESP32 shield and make sure that both USB port are on the same side.
2. Next, attach the dupont wires to Bluetooth 4.0 module by following the color coding that is black for the ground, red for the VCC, yellow for the TX pin, and white for the RX pin.
3. Next, attach the other end of the dupont wires to the ESP32 shield by matching the colors of the wires to the colors of the pin headers such that black is to black, red is to red, and yellow and following colors to yellow pin headers.
4. Next, power the Gorillacell ESP32 shield with an external power supply by connecting a type-C USB cable. Make sure that the power switch is set to ON state.
5. Lastly, connect the ESP32 to the computer by attaching a micro USB cable.

<div> </div>## SOFTWARE INSTRUCTION:

1. Copy the sample source code from the SOURCE CODE section and paste it to your Thonny IDE.
2. Please feel free to modify it according to your needs.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/p5JzDTXKByE?feature=oembed" title="042 - MicroPython TechNotes: JDY-32 | Bluetooth Low Energy | BLE" width="500"></iframe></div></figure><div> </div><div>If you have any concern regarding this video, please write your question in the comment box.</div><div> </div><div>You might also liked to support my journey on Youtube by subscribing on my channel, TechToTinker. [Click this to Subscribe.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)</div><div> </div><div>Thank you and have a good days ahead.</div><div> </div><div>See you,</div><div>– George Bantique | tech.to.tinker@gmail.com</div><div> </div>## SOURCE CODE:

### 1. Example # 1, exploring the basics of BLE:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import UART

ble = UART(2, baudrate=9600, tx=25, rx=23)

# The following lines of code can be tested using the REPL:
# 1. To check if there is available serial data
# ble.any()
# 2. To read all data available:
# ble.read()
# 3. Or you can input the number of bytes as parameter
#    you want to read from uart
# ble.read(num_here)
# 4. Or you can read 1 line
# ble.readline()
# 5. Now to write / transmit uart data
# ble.write(your_data)

```

</div><div> </div>### 2. Example # 2, demonstrating sending and transmitting BLE messages:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from machine import UART
from machine import Timer
from time import ticks_ms

ble = UART(2, baudrate=9600, tx=25, rx=23)
led = Pin(2, Pin.OUT)
sw = Pin(0, Pin.IN)
tim0 = Timer(0)
t_start = ticks_ms()

while True:
    if ble.any()!=0:
        msg = ble.read(ble.any()).decode()
        if "ON" in msg:
            led.value(1)
            tim0.deinit()
            print('LED is ON')
        elif "OFF" in msg:
            led.value(0)
            tim0.deinit()
            print('LED is OFF')
        elif "BLINK" in msg:
            tim0.init(period=250, mode=Timer.PERIODIC, callback=lambda t: led.value(not led.value()))
            print('LED is blinking')
        else:
            print(msg.strip('rn'))
            
    if ticks_ms()-t_start >= 300:
        if sw.value()==0: # BOOT button is pressed
            ble.write('Boot button is pressed.rn')
            print('Sending "Boot button is pressed."')
        t_start=ticks_ms()

```

</div><div> </div>## REFERENCES AND CREDITS:

<div style="text-align: left;">**1. Purchase your Gorillacell ESP32 development kits:**</div><div style="text-align: left;"> <http://gorillacell.kr/></div><div> </div><div style="text-align: left;">2. [**JDY-32 Datasheet**](http://myosuploads3.banggood.com/products/20190517/20190517042519JDY32.pdf)</div>