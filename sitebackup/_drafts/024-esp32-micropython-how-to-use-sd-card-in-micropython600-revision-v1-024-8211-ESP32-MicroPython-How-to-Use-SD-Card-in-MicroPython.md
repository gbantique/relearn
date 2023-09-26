---
author: George Bantique
date: '2023-03-13T16:22:29+08:00'
excerpt: In this video, we will learn how to interface and use SD Card with ESP32 using MicroPython programming language.
guid: https://techtotinker.com/?p=1352
id: 1352
permalink: /?p=1352
title: '024 &#8211; ESP32 MicroPython: How to Use SD Card in MicroPython'
url: /024-esp32-micropython-how-to-use-sd-card-in-micropython600-revision-v1-024-8211-ESP32-MicroPython-How-to-Use-SD-Card-in-MicroPython
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/024-ESP32_SD_blog.png)</figure>In this video, we will learn how to interface and use SD Card with ESP32 using MicroPython programming language.

<div> </div>## BILL OF MATERIALS:

1. **ESP32 development board** with MicroPython firmware installed.
2. **Micro/SD Card breakout board / module** with Micro/SD Card inserted.
3. **Breadboard**.
4. Some **dupont wires**.

<div> </div>## PINOUT:

1. **GND** – for the ground pin.
2. **VCC** – for the supply voltage.
3. **MISO** – for the SPI Master Input Slave Output pin.
4. **MOSI** – for the SPI Master Output Slave Input pin.
5. **SCK** – for the SPI Serial Clock pin.
6. **CS** – for the SPI Chip Select pin.

<div> </div>## HARDWARE INSTRUCTION:

1. First, connect the SD module GND pin to ESP32 GND pin.
2. Next, connect the SD module VCC pin to ESP32 VIN pin.
3. Next, connect the SD module **MISO** pin to ESP32 **GPIO 13**.
4. Next, connect the SD module **MOSI** pin to ESP32 **GPIO 12**.
5. Next, connect the SD module **SCK** pin to ESP32 **GPIO 14**.
6. Lastly, connect the SD module **CS** pin to ESP32 **GPIO 27**.

<div> </div>## SOFTWARE INSTRUCTION:

1. Copy the sdcard.py and save it to ESP32 MicroPython root directory by clicking the File menu then select Save As.
2. Click MicroPython Device and save it as “sdcard.py” and click “OK”.
3. Copy and paste the example source code.
4. Play with it, adapt according to your needs and most of all is enjoy what you are learning.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/qL2g5YIVick?feature=oembed" title="024 - ESP32 MicroPython: SD Card" width="500"></iframe></div></figure><div> </div>For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### **1. sdcard.py, SD Card driver library:**

<div>```

 """  
 MicroPython driver for SD cards using SPI bus.  
 Requires an SPI bus and a CS pin. Provides readblocks and writeblocks  
 methods so the device can be mounted as a filesystem.  
 Example usage on pyboard:  
   import pyb, sdcard, os  
   sd = sdcard.SDCard(pyb.SPI(1), pyb.Pin.board.X5)  
   pyb.mount(sd, '/sd2')  
   os.listdir('/')  
 Example usage on ESP8266:  
   import machine, sdcard, os  
   sd = sdcard.SDCard(machine.SPI(1), machine.Pin(15))  
   os.mount(sd, '/sd')  
   os.listdir('/')  
 """  
 from micropython import const  
 import time  
 _CMD_TIMEOUT = const(100)  
 _R1_IDLE_STATE = const(1 << 0)  
 # R1_ERASE_RESET = const(1 << 1)  
 _R1_ILLEGAL_COMMAND = const(1 << 2)  
 # R1_COM_CRC_ERROR = const(1 << 3)  
 # R1_ERASE_SEQUENCE_ERROR = const(1 << 4)  
 # R1_ADDRESS_ERROR = const(1 << 5)  
 # R1_PARAMETER_ERROR = const(1 << 6)  
 _TOKEN_CMD25 = const(0xFC)  
 _TOKEN_STOP_TRAN = const(0xFD)  
 _TOKEN_DATA = const(0xFE)  
 class SDCard:  
   def __init__(self, spi, cs):  
     self.spi = spi  
     self.cs = cs  
     self.cmdbuf = bytearray(6)  
     self.dummybuf = bytearray(512)  
     self.tokenbuf = bytearray(1)  
     for i in range(512):  
       self.dummybuf[i] = 0xFF  
     self.dummybuf_memoryview = memoryview(self.dummybuf)  
     # initialise the card  
     self.init_card()  
   def init_spi(self, baudrate):  
     try:  
       master = self.spi.MASTER  
     except AttributeError:  
       # on ESP8266  
       self.spi.init(baudrate=baudrate, phase=0, polarity=0)  
     else:  
       # on pyboard  
       self.spi.init(master, baudrate=baudrate, phase=0, polarity=0)  
   def init_card(self):  
     # init CS pin  
     self.cs.init(self.cs.OUT, value=1)  
     # init SPI bus; use low data rate for initialisation  
     self.init_spi(100000)  
     # clock card at least 100 cycles with cs high  
     for i in range(16):  
       self.spi.write(b"xff")  
     # CMD0: init card; should return _R1_IDLE_STATE (allow 5 attempts)  
     for _ in range(5):  
       if self.cmd(0, 0, 0x95) == _R1_IDLE_STATE:  
         break  
     else:  
       raise OSError("no SD card")  
     # CMD8: determine card version  
     r = self.cmd(8, 0x01AA, 0x87, 4)  
     if r == _R1_IDLE_STATE:  
       self.init_card_v2()  
     elif r == (_R1_IDLE_STATE | _R1_ILLEGAL_COMMAND):  
       self.init_card_v1()  
     else:  
       raise OSError("couldn't determine SD card version")  
     # get the number of sectors  
     # CMD9: response R2 (R1 byte + 16-byte block read)  
     if self.cmd(9, 0, 0, 0, False) != 0:  
       raise OSError("no response from SD card")  
     csd = bytearray(16)  
     self.readinto(csd)  
     if csd[0] & 0xC0 == 0x40: # CSD version 2.0  
       self.sectors = ((csd[8] << 8 | csd[9]) + 1) * 1024  
     elif csd[0] & 0xC0 == 0x00: # CSD version 1.0 (old, <=2GB)  
       c_size = csd[6] & 0b11 | csd[7] << 2 | (csd[8] & 0b11000000) << 4  
       c_size_mult = ((csd[9] & 0b11) << 1) | csd[10] >> 7  
       self.sectors = (c_size + 1) * (2 ** (c_size_mult + 2))  
     else:  
       raise OSError("SD card CSD format not supported")  
     # print('sectors', self.sectors)  
     # CMD16: set block length to 512 bytes  
     if self.cmd(16, 512, 0) != 0:  
       raise OSError("can't set 512 block size")  
     # set to high data rate now that it's initialised  
     self.init_spi(1320000)  
   def init_card_v1(self):  
     for i in range(_CMD_TIMEOUT):  
       self.cmd(55, 0, 0)  
       if self.cmd(41, 0, 0) == 0:  
         self.cdv = 512  
         # print("[SDCard] v1 card")  
         return  
     raise OSError("timeout waiting for v1 card")  
   def init_card_v2(self):  
     for i in range(_CMD_TIMEOUT):  
       time.sleep_ms(50)  
       self.cmd(58, 0, 0, 4)  
       self.cmd(55, 0, 0)  
       if self.cmd(41, 0x40000000, 0) == 0:  
         self.cmd(58, 0, 0, 4)  
         self.cdv = 1  
         # print("[SDCard] v2 card")  
         return  
     raise OSError("timeout waiting for v2 card")  
   def cmd(self, cmd, arg, crc, final=0, release=True, skip1=False):  
     self.cs(0)  
     # create and send the command  
     buf = self.cmdbuf  
     buf[0] = 0x40 | cmd  
     buf[1] = arg >> 24  
     buf[2] = arg >> 16  
     buf[3] = arg >> 8  
     buf[4] = arg  
     buf[5] = crc  
     self.spi.write(buf)  
     if skip1:  
       self.spi.readinto(self.tokenbuf, 0xFF)  
     # wait for the response (response[7] == 0)  
     for i in range(_CMD_TIMEOUT):  
       self.spi.readinto(self.tokenbuf, 0xFF)  
       response = self.tokenbuf[0]  
       if not (response & 0x80):  
         # this could be a big-endian integer that we are getting here  
         for j in range(final):  
           self.spi.write(b"xff")  
         if release:  
           self.cs(1)  
           self.spi.write(b"xff")  
         return response  
     # timeout  
     self.cs(1)  
     self.spi.write(b"xff")  
     return -1  
   def readinto(self, buf):  
     self.cs(0)  
     # read until start byte (0xff)  
     for i in range(_CMD_TIMEOUT):  
       self.spi.readinto(self.tokenbuf, 0xFF)  
       if self.tokenbuf[0] == _TOKEN_DATA:  
         break  
     else:  
       self.cs(1)  
       raise OSError("timeout waiting for response")  
     # read data  
     mv = self.dummybuf_memoryview  
     if len(buf) != len(mv):  
       mv = mv[: len(buf)]  
     self.spi.write_readinto(mv, buf)  
     # read checksum  
     self.spi.write(b"xff")  
     self.spi.write(b"xff")  
     self.cs(1)  
     self.spi.write(b"xff")  
   def write(self, token, buf):  
     self.cs(0)  
     # send: start of block, data, checksum  
     self.spi.read(1, token)  
     self.spi.write(buf)  
     self.spi.write(b"xff")  
     self.spi.write(b"xff")  
     # check the response  
     if (self.spi.read(1, 0xFF)[0] & 0x1F) != 0x05:  
       self.cs(1)  
       self.spi.write(b"xff")  
       return  
     # wait for write to finish  
     while self.spi.read(1, 0xFF)[0] == 0:  
       pass  
     self.cs(1)  
     self.spi.write(b"xff")  
   def write_token(self, token):  
     self.cs(0)  
     self.spi.read(1, token)  
     self.spi.write(b"xff")  
     # wait for write to finish  
     while self.spi.read(1, 0xFF)[0] == 0x00:  
       pass  
     self.cs(1)  
     self.spi.write(b"xff")  
   def readblocks(self, block_num, buf):  
     nblocks = len(buf) // 512  
     assert nblocks and not len(buf) % 512, "Buffer length is invalid"  
     if nblocks == 1:  
       # CMD17: set read address for single block  
       if self.cmd(17, block_num * self.cdv, 0, release=False) != 0:  
         # release the card  
         self.cs(1)  
         raise OSError(5) # EIO  
       # receive the data and release card  
       self.readinto(buf)  
     else:  
       # CMD18: set read address for multiple blocks  
       if self.cmd(18, block_num * self.cdv, 0, release=False) != 0:  
         # release the card  
         self.cs(1)  
         raise OSError(5) # EIO  
       offset = 0  
       mv = memoryview(buf)  
       while nblocks:  
         # receive the data and release card  
         self.readinto(mv[offset : offset + 512])  
         offset += 512  
         nblocks -= 1  
       if self.cmd(12, 0, 0xFF, skip1=True):  
         raise OSError(5) # EIO  
   def writeblocks(self, block_num, buf):  
     nblocks, err = divmod(len(buf), 512)  
     assert nblocks and not err, "Buffer length is invalid"  
     if nblocks == 1:  
       # CMD24: set write address for single block  
       if self.cmd(24, block_num * self.cdv, 0) != 0:  
         raise OSError(5) # EIO  
       # send the data  
       self.write(_TOKEN_DATA, buf)  
     else:  
       # CMD25: set write address for first block  
       if self.cmd(25, block_num * self.cdv, 0) != 0:  
         raise OSError(5) # EIO  
       # send the data  
       offset = 0  
       mv = memoryview(buf)  
       while nblocks:  
         self.write(_TOKEN_CMD25, mv[offset : offset + 512])  
         offset += 512  
         nblocks -= 1  
       self.write_token(_TOKEN_STOP_TRAN)  
   def ioctl(self, op, arg):  
     if op == 4: # get number of blocks  
       return self.sectors  

```

</div><div> </div>### 2. Example demo in using / accessing an SD Card:

<div>```

# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

import os
from machine import Pin, SoftSPI
from sdcard import SDCard

# Pin assignment:
# MISO -> GPIO 13
# MOSI -> GPIO 12
# SCK  -> GPIO 14
# CS   -> GPIO 27
spisd = SoftSPI(-1, miso=Pin(13), mosi=Pin(12), sck=Pin(14))
sd = SDCard(spisd, Pin(27))


print('Root directory:{}'.format(os.listdir()))
vfs = os.VfsFat(sd)
os.mount(vfs, '/sd')
print('Root directory:{}'.format(os.listdir()))
os.chdir('sd')
print('SD Card contains:{}'.format(os.listdir()))


# 1. To read file from the root directory:
# f = open('sample.txt', 'r')
# print(f.read())
# f.close()

# 2. To create a new file for writing:
# f = open('sample2.txt', 'w')
# f.write('Some text for sample 2')
# f.close()

# 3. To append some text in existing file:
# f = open('sample3.txt', 'a')
# f.write('Some text for sample 3')
# f.close()

# 4. To delete a file:
# os.remove('file to delete')

# 5. To list all directories and files:
# os.listdir()

# 6. To create a new folder:
# os.mkdir('sample folder')

# 7. To change directory:
# os.chdir('directory you want to open')

# 8. To delete a folder:
# os.rmdir('folder to delete')

# 9.  To rename a file or a folder:
# os.rename('current name', 'desired name')

```

</div><div> </div>## REFERENCES AND CREDITS:

### 1. sdcard.py:

<div> <https://github.com/micropython/micropython/blob/master/drivers/sdcard/sdcard.py> </div>