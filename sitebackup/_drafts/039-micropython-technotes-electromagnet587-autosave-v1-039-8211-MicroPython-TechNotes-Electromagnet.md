---
author: George Bantique
date: '2023-03-13T08:19:09+08:00'
guid: https://techtotinker.com/?p=1300
id: 1300
permalink: /?p=1300
title: '039 &#8211; MicroPython TechNotes: Electromagnet'
url: /039-micropython-technotes-electromagnet587-autosave-v1-039-8211-MicroPython-TechNotes-Electromagnet
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/06/039-2B-2BMicroPython-2BTechNotes-2BElectromagnet.png)</figure>In this article, we will tackle about on how to control an electromagnet module with ESP32 using MicroPython. **Electromagnet** is an artificial kind of magnet that activates and or deactivates by the use of electricity.

A **logic 1** enables the electromagnet module meaning it can attract metallic objects while a **logic 0** disables the electromagnet module.

<div> </div>## BILL OF MATERIALS:

1. ESP32 development board.
2. Gorillacell ESP32 shield.
3. 3-pin F-F dupont wires.
4. Gorillacell electromagnet module.

<div> </div>## PINOUT:

1. **G** – for the ground.
2. **V** – for the supply voltage of the electromagnet.
3. **S** – for the control signal pin for the electromagnet.

<div> </div>## PIN ASSIGNMENT:

For this lesson, I choose GPIO 23 to serve as the control signal pin for the electromagnet module.

<div> </div>## HARDWARE INSTRUCTION:

1. Attach the ESP32 development board on top of the Gorillacell ESP32 shield and make sure that both USB port are on the same side.
2. Attach a dupont wires to the electromagnet module according to color coding that is black for the ground, red for the VCC, and yellow for the control signal pin.
3. Attach the other end of the dupont wires to the ESP32 shield by matching the colors of the wires to the colors of the pin headers such that black is to black, red is to red, and yellow is to yellow pin headers.
4. Power the ESP32 shield with an external power supply with a type-C USB connector and make sure that the power switch is set to ON state.
5. Connect the ESP32 to the computer with a micro USB cable.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/Tt7ojwS3bAI?feature=oembed" title="039 - MicroPython TechNotes: Electromagnet" width="500"></iframe></div></figure><div> </div>If you have any concern regarding this lesson, please write your message in the comment box provided.

If you found this article helpful, please consider supporting my journey on Youtube by Subscribing on my channel. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Have a good days ahead everyone.

Thanks,

<div> </div>## SOURCE CODE:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin

electromagnet = Pin(23, Pin.OUT)


# ******************************************************************
# The following should be explored using the REPL:
# ******************************************************************
# # 1. To activate the electromagnet, set signal pin to logic 1
# electromagnet.value(1)

# # 2. To deactivate the electromagnet, set signal pin to logic 0
# electromagnet.value(0)


```

</div><div> </div>## REFERENCES AND CREDITS:

<div>**1. Purchase your Gorillacell ESP32 development kit at:**</div><div> [https://gorillacell.kr](https://gorillacell.kr/)</div>