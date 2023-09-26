---
author: George Bantique
date: '2023-03-13T12:41:41+08:00'
excerpt: In this article, we will learn how to use a DHT11 sensor with ESP32 using MicroPython programming language.
guid: https://techtotinker.com/?p=1339
id: 1339
permalink: /?p=1339
title: '028 &#8211; MicroPython TechNotes: DHT11 Temperature and Humidity Sensor'
url: /028-micropython-technotes-dht11-temperature-and-humidity-sensor598-revision-v1-028-8211-MicroPython-TechNotes-DHT11-Temperature-and-Humidity-Sensor
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/028-DHT11-blog.png)</figure>In this article, we will learn how to use a DHT11 sensor with ESP32 using MicroPython programming language.

<div> </div>## PINOUT:

1. **G** – for the ground pin.
2. **V** – for the supply voltage.
3. **S** – for the signal pin.

<div> </div>## BILL OF MATERIALS:

1. ESP32 development board to serve as the brain of the experiment.
2. GorillaCell ESP32 shield to extend ESP32 pins to pin headers for easy circuit connection.
3. 3-pin female-female dupont jumper wires to attach DHT11 sensor module to ESP32 shield.
4. DHT11 sensor module.

<div> </div>## HARDWARE INSTRUCTION:

1. First, attach the ESP32 on top of the ESP32 shield and make sure that both USB ports are on the same side.
2. Next, attach the dupont wires to DHT11 sensor module by following the color coding which is black for the GND, red for the VCC, and yellow for the SIGNAL pin.
3. Next, attach the other side of the dupont wires to the ESP32 shield by matching the colors of the wires to the colors of the pin headers which is black to black, red to red, and yellow to yellow pin headers. For this experiment, I choose GPIO 23 to serve as the signal pin from DHT11 sensor module.
4. Next, power the ESP32 shield with an external power supply with a type-C USB cable and make sure that the power switch is set to ON state.
5. Lastly, connect the ESP32 to the computer with a micro-USB cable.

<div> </div>## SOFTWARE INSTRUCTION:

1. Copy and paste to Thonny the example source codes.
2. Play with it and adapt it according to your needs.
3. Enjoy learning.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/X8TWhs4e03g?feature=oembed" title="028 - MicroPython TechNotes: DHT11" width="500"></iframe></div></figure><div> </div>For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### **1. Example # 1, basics of using DHT11:**

<div>```

# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from dht import DHT11
d = DHT11(Pin(23))

# # This line of code should be tested using the REPL
# d.measure()
# d.temperature()
# d.humidity()

```

</div><div> </div>### **2. Example # 2, display DHT11 sensor readings to 16×2 LCD:**

<div>```

# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms
from dht import DHT11
from machine import SoftI2C
from i2c_lcd import I2cLcd

d = DHT11(Pin(23))

DEFAULT_I2C_ADDR = 0x20
i2c = SoftI2C(scl=Pin(22), sda=Pin(21), freq=400000) 
lcd = I2cLcd(i2c, DEFAULT_I2C_ADDR, 2, 16)

lcd.clear()
lcd.move_to(0, 0)
lcd.putstr("Temp:    ^C.")
lcd.move_to(0, 1)
lcd.putstr("Humi:    %.")

while True:
    d.measure()
    lcd.move_to(6, 0)
    lcd.putstr(str(d.temperature()))
    lcd.move_to(6, 1)
    lcd.putstr(str(d.humidity()))
    sleep_ms(3000)

```

</div><div> </div>### **3. Example # 3, simple application of DHT11:**

<div>```

# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from time import sleep_ms
from dht import DHT11
from machine import SoftI2C
from i2c_lcd import I2cLcd

d = DHT11(Pin(23))

DEFAULT_I2C_ADDR = 0x20
i2c = SoftI2C(scl=Pin(22), sda=Pin(21), freq=400000) 
lcd = I2cLcd(i2c, DEFAULT_I2C_ADDR, 2, 16)

lcd.clear()
lcd.move_to(0, 0)
lcd.putstr("Temp:    ^C.")
lcd.move_to(0, 1)
lcd.putstr("Humi:    %.")

count = 0

while True:
    d.measure()
    t = d.temperature()
    h = d.humidity()
    lcd.move_to(6, 0)
    lcd.putstr(str(t))
    lcd.move_to(6, 1)
    lcd.putstr(str(h))
    if t >= 30:
        count += 1
        print("Alert! Count {}".format(count))
    else:
        count = 0
        
    sleep_ms(1000)

```

</div><div> </div>## REFERENCES AND CREDITS:

**1. Purchased your Gorillacell ESP32 development kit at:**

 <https://gorillacell.kr>