---
author: George Bantique
date: '2023-03-14T10:04:29+08:00'
excerpt: In this article, we will explore the Reyax LoRa module RYLR896 using MicroPython. For this we will used both a Raspberry Pi Pico and an ESP32.
guid: https://techtotinker.com/?p=1424
id: 1424
permalink: /?p=1424
title: Exploring LoRa RYLR896 with MicroPython
url: /exploring-lora-rylr896-with-micropython614-revision-v1-Exploring-LoRa-RYLR896-with-MicroPython
---


In this article, we will explore the Reyax LoRa module RYLR896 using MicroPython. For this we will used both a Raspberry Pi Pico and an ESP32.

<div> </div>## PINOUT:

From left to right facing front.

1. **VDD** – for the supply voltage. This should be provided with minimum of 2V to 3.6V maximum.
2. **NRST** – for the active-low RESET pin. This could be tied to microcontroller’s ground so that when microcontroller is reset, so the LoRa is also be resetted.
3. **RXD** – for the UART serial receive pin.
4. **TXD** – for the UART serial transmit pin.
5. **N/C** – not connected.
6. **GND** – for the ground pin.

<div> </div>## CIRCUIT DIAGRAM:

### Raspberry Pi Pico with RYLR896:

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/RYRL896_RPico.png)</figure>1. Connect RYLR896 VDD to Pico 3.3V pin.
2. Connect RYLR896 RXD to Pico pin 6.
3. Connect RYLR896 TXD to Pico pin 7.
4. Connect RYLR896 GND to Pico GND pin.

<div> </div>### ESP32 with RYLR896:

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/RYRL896_ESP32.png)</figure>1. Connect RYLR896 VDD to ESP32 3.3V pin.
2. Connect RYLR896 RXD to ESP32 GPIO 26.
3. Connect RYLR896 TXD to ESP32 GPIO 27.
4. Connect RYLR896 GND to ESP32 GND pin.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/RAvn0MW_P-s?feature=oembed" title="Exploring LoRa RYLR896 with MicroPython" width="500"></iframe></div></figure><div> </div>## CALL TO ACTION:

For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. For the Raspberry Pi Pico:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, UART
from time import sleep_ms

class RYLR896:
    def __init__(self, port_num, tx_pin='', rx_pin=''):
        if tx_pin=='' and rx_pin=='':
            self._uart = UART(port_num)
        else:
            self._uart = UART(port_num, tx=tx_pin, rx=rx_pin)
                
    def cmd(self, lora_cmd):
        self._uart.write('{}rn'.format(lora_cmd))
        while(self._uart.any()==0):
            pass
        reply = self._uart.readline()
        print(reply.decode().strip('rn'))
    
    def test(self):
        self._uart.write('ATrn')
        while(self._uart.any()==0):
            pass
        reply = self._uart.readline()
        print(reply.decode().strip('rn'))

    def set_addr(self, addr):
        self._uart.write('AT+ADDRESS={}rn'.format(addr))
        while(self._uart.any()==0):
            pass
        reply = self._uart.readline()
        print(reply.decode().strip('rn'))
        print('Address set to:{}rn'.format(addr))


    def send_msg(self, addr, msg):
        self._uart.write('AT+SEND={},{},{}rn'.format(addr,len(msg),msg))
        while(self._uart.any()==0):
            pass
        reply = self._uart.readline()
        print(reply.decode().strip('rn'))
        
    def read_msg(self):
        if self._uart.any()==0:
            print('Nothing to show.')
        else:
            msg = ''
            while(self._uart.any()):
                msg = msg + self._uart.read(self._uart.any()).decode()
            print(msg.strip('rn'))
    

lora = RYLR896(1) # Sets the UART port to be use
sleep_ms(100)
lora.set_addr(1)  # Sets the LoRa address

```

</div><div> </div>### 2. For the ESP32:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, UART
from time import sleep_ms
                                                                          
class RYLR896:
    def __init__(self, port_num, tx='', rx=''):
        if tx=='' and rx=='':
            self._uart = UART(port_num)
        else:
            self._uart = UART(port_num, tx=tx, rx=rx)

    def cmd(self, lora_cmd):
        self._uart.write('{}rn'.format(lora_cmd))
        while(self._uart.any()==0):
            pass
        reply = self._uart.readline()
        print(reply.decode().strip('rn'))
    
    def test(self):
        self._uart.write('ATrn')
        while(self._uart.any()==0):
            pass
        reply = self._uart.readline()
        print(reply.decode().strip('rn'))

    def set_addr(self, addr):
        self._uart.write('AT+ADDRESS={}rn'.format(addr))
        while(self._uart.any()==0):
            pass
        reply = self._uart.readline()
        print(reply.decode().strip('rn'))
        print('Address set to:{}rn'.format(addr))
              
    def send_msg(self, addr, msg):
        self._uart.write('AT+SEND={},{},{}rn'.format(addr,len(msg),msg))
        while(self._uart.any()==0):
            pass
        reply = self._uart.readline()
        print(reply.decode().strip('rn'))
        
    def read_msg(self):
        if self._uart.any()==0:
            print('Nothing to show.')
        else:
            msg = ''
            while(self._uart.any()):
                msg = msg + self._uart.read(self._uart.any()).decode()
            print(msg.strip('rn'))
            
lora = RYLR896(2,rx=27,tx=26) # Sets the UART port to be use
sleep_ms(100)
lora.set_addr(2)  # Sets the LoRa address

```

</div><div> </div>## REFERENCES AND CREDITS:

<div>1. More details from the official page of RYLR896</div><div> <https://reyax.com/products/rylr896/></div><div> </div><div>2. RYLR896 Datasheet:</div><div> [https://reyax.com/wp-content/uploads/2019/12/RYLR896\_EN-1.pdf](https://reyax.com/wp-content/uploads/2019/12/RYLR896_EN-1.pdf)</div><div> </div><div>3. RYLR896 AT Command Reference:</div><div> [https://reyax.com/wp-content/uploads/2020/01/Lora-AT-Command-RYLR40x\_RYLR89x\_EN.pdf](https://reyax.com/wp-content/uploads/2020/01/Lora-AT-Command-RYLR40x_RYLR89x_EN.pdf)</div>