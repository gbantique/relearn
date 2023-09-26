---
author: George Bantique
date: '2023-03-13T07:53:01+08:00'
excerpt: In this article, I will  talk about the (external) BLUETOOTH module with ESP32 using MicroPython.
guid: https://techtotinker.com/?p=1294
id: 1294
permalink: /?p=1294
title: '041 &#8211; MicroPython TechNotes: Bluetooth HC-06'
url: /041-micropython-technotes-bluetooth-hc-06585-autosave-v1-041-8211-MicroPython-TechNotes-Bluetooth-HC-06
---


<div class="wp-block-group has-global-padding is-layout-constrained"><figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/07/041-2B-2BMicroPython-2BTechNotes-2BHC06-2BBluetooth.png)</figure>In this article, I will talk about the (external) BLUETOOTH module with ESP32 using MicroPython.

<div> </div>## BILL OF MATERIALS:

*To follow this lesson you will need the following:*

1. ESP32 development board.
2. Gorillacell ESP32 shield.
3. 4-pin female-female dupont wires.
4. Gorillacell Bluetooth module.

<div> </div><figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/07/HC06-2BBluetooth-2BGorillacell-2Bpinout.png)</figure><div> </div>## PINOUT:

1. **GND** – for the ground pin.
2. **VCC** – for the supply voltage.
3. **TX** – for the UART transmit pin.
4. **RX** – for the UART receive pin.

<div> </div>## HARDWARE INSTRUCTION

1. Connect the Bluetooth module GND pin to ESP32 GND pin.
2. Connect the Bluetooth module VCC pin to ESP32 3.3V pin.
3. Connect the Bluetooth module TX pin to ESP32 GPIO 23 pin.
4. Connect the Bluetooth module RX pin to ESP32 GPIO 25 pin.

<div> </div>## SOFTWARE INSTRUCTION:

1. Copy the sample source code below and paste it to your Thonny IDE.
2. Run it then modify according to your needs.
3. Enjoy and happy learning.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/Tp7s5xOMiTU?feature=oembed" title="041 - MicroPython TechNotes: HC-06 Bluetooth" width="500"></iframe></div></figure>For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. Example # 1, exploring the basics:

<div>```
# More details can be found in TechToTinker.blogspot.com <br></br># George Bantique | tech.to.tinker@gmail.com<br></br><br></br>from machine import UART<br></br><br></br>bt = UART(2, baudrate=9600, tx=25, rx=23)<br></br><br></br># The following lines of code can be tested using the REPL:<br></br># 1. To check if there is available serial data<br></br># bt.any()<br></br># 2. To read all data available:<br></br># bt.read()<br></br># 3. Or you can input the number of bytes as parameter<br></br>#    you want to read from uart<br></br># bt.read(num_here)<br></br># 4. Or you can read 1 line<br></br># bt.readline()<br></br># 5. Now to write / transmit uart data<br></br># bt.write(your_data)<br></br>
```

</div><div> </div>### 2. Example # 2, controlling an output:

<div>```
# More details can be found in TechToTinker.blogspot.com <br></br># George Bantique | tech.to.tinker@gmail.com<br></br><br></br>from machine import Pin<br></br>from machine import UART<br></br><br></br>bt = UART(2, baudrate=9600, tx=25, rx=23)<br></br>led = Pin(2, Pin.OUT)<br></br><br></br>while True:<br></br>    if bt.any()!=0:<br></br>        msg = bt.read(bt.any()).decode().strip('rn')<br></br>        print(msg)<br></br>        if "ON" in msg:<br></br>            led.value(1)<br></br>        if "OFF" in msg:<br></br>            led.value(0)<br></br>
```

</div><div> </div>### 3. Example # 3, reading an input:

<div>```
# More details can be found in TechToTinker.blogspot.com <br></br># George Bantique | tech.to.tinker@gmail.com<br></br><br></br>from machine import Pin<br></br>from machine import UART<br></br>from machine import Timer<br></br>from time import ticks_ms<br></br><br></br>bt = UART(2, baudrate=9600, tx=25, rx=23)<br></br>led = Pin(2, Pin.OUT)<br></br>sw = Pin(0, Pin.IN)<br></br>tim0 = Timer(0)<br></br>t_start = ticks_ms()<br></br><br></br>while True:<br></br>    if bt.any()!=0:<br></br>        #msg = bt.read(bt.any()).decode().strip('rn')<br></br>        #print(msg)<br></br>        msg = bt.read(bt.any()).decode()<br></br>        if "ON" in msg:<br></br>            led.value(1)<br></br>            tim0.deinit()<br></br>            print('LED is ON')<br></br>        elif "OFF" in msg:<br></br>            led.value(0)<br></br>            tim0.deinit()<br></br>            print('LED is OFF')<br></br>        elif "BLINK" in msg:<br></br>            tim0.init(period=250, mode=Timer.PERIODIC, callback=lambda t: led.value(not led.value()))<br></br>            print('LED is blinking')<br></br>        else:<br></br>            print(msg.strip('rn'))<br></br>            <br></br>    if ticks_ms()-t_start >= 300:<br></br>        if sw.value()==0: # BOOT button is pressed<br></br>            bt.write('Boot button is pressed.rn')<br></br>            print('Sending "Boot button is pressed."')<br></br>        t_start=ticks_ms()<br></br>
```

</div><div> </div>## REFERENCES AND CREDITS:

<div>1. Purchased your Gorillacell ESP32 development kit at:</div><div> <https://gorillacell.kr/></div></div>