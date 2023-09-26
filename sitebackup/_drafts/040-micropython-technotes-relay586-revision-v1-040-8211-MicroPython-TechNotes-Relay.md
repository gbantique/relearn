---
author: George Bantique
date: '2023-03-13T08:05:48+08:00'
excerpt: In this article, I will discussed about the RELAY with ESP32 using MicroPython.
guid: https://techtotinker.com/?p=1297
id: 1297
permalink: /?p=1297
title: '040 &#8211; MicroPython TechNotes: Relay'
url: /040-micropython-technotes-relay586-revision-v1-040-8211-MicroPython-TechNotes-Relay
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/07/040-2BMicroPython-2BTechNotes-2BRelay.png)</figure>In this article, I will discussed about the RELAY with ESP32 using MicroPython.

<div>**Relay inactive state: signal value is logic 0.**</div><div>**COM pin is connected to <span style="color: red;">normally-closed</span> NC pin.**</div><figure class="wp-block-image">[![](https://techtotinker.com/wp-content/uploads/2021/07/relay_inactive_state-300x154.png)](https://techtotinker.com/wp-content/uploads/2021/07/relay_inactive_state.png)</figure><figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/07/relay_inactive_state.png)</figure><div>**Relay active state: signal value is logic 1.**</div><div>**COM pin is connected to <span style="color: red;">normally-open</span> NO pin.**</div><figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/07/relay_active_state.png)</figure><div> </div>## PINOUT:

1. **G** – for the the ground.
2. **V** – for the supply voltage.
3. **S** – for the control signal pin.

<div> </div>## BILL OF MATERIALS:

1. ESP32 development board.
2. Gorillacell ESP32 shield.
3. 3-pin F-F Dupont wires.
4. Gorillacell relay module.

<div> </div>## HARDWARE INSTRUCTION:

1. Connect relay G pin to ESP32 GND pin.
2. Connect relay V pin to ESP32 5V pin.
3. Connect relay S pin to ESP32 GPIO 23 pin.

<div> </div>## SOFTWARE INSTRUCTION:

1. Copy example program in the source code section.
2. Please feel to modify according to your needs.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/e2SRPIaOnU0?feature=oembed" title="040 - MicroPython TechNotes: Relay" width="500"></iframe></div></figure><div> </div>If you have any question regarding this video, please write your message in the comment section.

You might also like to support me on my Youtube channel by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Hope you find it useful. Thank you.

Regards,

– George Bantique | tech.to.tinker@gmail.com

<div> </div>## SOURCE CODE:

### 1. Example # 1, exploring the basics:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin

relay = Pin(23, Pin.OUT)


# ******************************************************************
# The following should be explored using the REPL:
# ******************************************************************
# # 1. To activate the relay, set signal pin to logic 1
# relay.value(1)

# # 2. To deactivate the relay, set signal pin to logic 0
# relay.value(0)

```

</div><div> </div>### 2. Example # 2, simple application:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import ticks_ms

relay = Pin(23, Pin.OUT)
sw = Pin(0, Pin.IN)

isActive = False
start = ticks_ms()

while True:
    if ticks_ms()-start>=300:
        if sw.value()==0:
            isActive = not isActive
            relay.value(isActive)
        start=ticks_ms()

```

</div><div> </div>## REFERENCES AND CREDITS:

<div>1. Purchase your Gorillacell ESP32 development kit at:</div><div> [https://gorillacell.kr](https://gorillacell.kr/)</div>