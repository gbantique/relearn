---
author: George Bantique
date: '2023-03-14T17:05:23+08:00'
excerpt: 'I just received my Raspberry Pi Pico which I purchased in our local online store and I am so excited to try it out the moment I received it.


  And just after a minute or two, I am able to make the most popular Hello world of embedded electronics which is the blinking of the LED. So, I decided to create a tutorial on how easy it is to get started with Raspberry Pi Pico using MicroPython language. I divided this tutorial into 3 simple steps. So, without further delays, lets get started.'
guid: https://techtotinker.com/?p=1483
id: 1483
permalink: /?p=1483
title: '000 &#8211; Raspberry Pi Pico: How to get started | MicroPython'
url: /000-raspberry-pi-pico-how-to-get-started-micropython625-revision-v1-000-8211-Raspberry-Pi-Pico-How-to-get-started-MicroPython
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/000-pico-get-started-micropython.png)</figure>I just received my Raspberry Pi Pico which I purchased in our local online store and I am so excited to try it out the moment I received it.

And just after a minute or two, I am able to make the most popular Hello world of embedded electronics which is the blinking of the LED. So, I decided to create a tutorial on how easy it is to get started with Raspberry Pi Pico using MicroPython language. I divided this tutorial into 3 simple steps. So, without further delays, lets get started.

<div> </div>## BILL OF MATERIALS:

1. Raspberry Pi Pico

<div> </div>## 3 Simple Steps to Get Started with MicroPython Using Raspberry Pi Pico:

1. Configure Raspberry Pi Pico as MicroPython device. 
    - First, download the MicroPython firmware from the official webpage of Raspberry Pi Pico by pressing and holding the BOOTSEL button on the Pico while attaching the micro USB cable. This will make your Pico recognized as storage device.
    - Next, open a file explorer, look for Raspberry drive. Inside it, double click the index.htm which will redirect you to the Raspberry Pi Pico official page.
    - Next, scroll down and look for “Getting Started with MicroPython”.
    - Next, download the MicroPython UF2 file that will be the new firmware for the Pico.
    - After the download, drag and drop it to the Raspberry drive. This time, your Pico is now configured and will function as a MicroPython device.
2. Install a Python IDE. 
    - First, go to Thonny.org.
    - Next, download the installer according to your operating system.
    - After the download, install it just like installing a normal application.
    - After the installation, open the Thonny.
    - Next, configure the Thonny for Raspberry Pi Pico by clicking the Tools menu and select Options.
    - Next, click the Interpreter tab and under the device, select MicroPython(Raspberry Pi Pico). Under the Port, select the appropriate serial port for your Pico. In my case, its COMM10.
    - Click OK.
3. Test the MicroPython firmware 
    
    
    - Try typing the following in your Thonny IDE console each line followed by pressing ENTER key in your keyboard. First it should turn ON the on-board LED then turn OFF and lastly, turn ON again.

<div>```
from machine import Pin 
led = Pin(25, Pin.OUT)
led.value(1)
led.value(0)
led.toggle()
```

</div>
<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/wSmcCE1Lv3M?feature=oembed" title="000 - Raspberry Pi Pico: How to get started | MicroPython" width="500"></iframe></div></figure><div> </div>## CALL TO ACTION:

For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## REFERENCES AND CREDITS:

<div>1. Raspberry Pi Pico official page:   
<https://www.raspberrypi.org/documentation/pico/getting-started/>  
</div><div> </div><div>2. Thonny Python IDE: [thonny.org](http://thonny.org/)</div>