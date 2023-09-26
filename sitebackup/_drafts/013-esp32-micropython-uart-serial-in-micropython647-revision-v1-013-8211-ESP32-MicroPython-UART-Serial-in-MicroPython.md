---
author: George Bantique
date: '2023-03-05T14:41:23+08:00'
guid: https://techtotinker.com/?p=888
id: 888
permalink: /?p=888
title: '013 &#8211; ESP32 MicroPython: UART Serial in MicroPython'
url: /013-esp32-micropython-uart-serial-in-micropython647-revision-v1-013-8211-ESP32-MicroPython-UART-Serial-in-MicroPython
---


<div style="clear: both; text-align: left;"><span style="text-align: left;">In this tutorial, we will learn to use the UART peripheral of ESP32 in MicroPython. <div style="clear: both;">UART is useful for interfacing devices such as **DF Player Mini** mp3 player module, **SIM800L** gsm module, **HC-06** bluetooth module, and etc which basically needs serial communication as mode of control.</div><div style="clear: both;"></div><div style="clear: both;">ESP32 has 3 hardware uart serial ports available which are Port 0, port 1, and port 2.</div><div style="clear: both;"></div><div style="clear: both;">**Port 0** is connected to GPIO 1 and 3. It is generally use to communicate with the ESP32 for flashing and during the reset event. In MicroPython, it is reserve for the default REPL. In this development board, it is accessible through the general purpose breadboard pins. This is usable when you disabled the default serial REPL and use the WEB REPL instead.</div><div style="clear: both;"></div><div style="clear: both;">**Port 1** is connected to GPIO 9 and 10. It is unused but it is usually used for spi flash memory access. In this version of ESP32 development board, it is not routed to general purpose breadboard pins. </div><div style="clear: both;"></div><div style="clear: both;">**Port 2** is connected to GPIO 16 and 17. It is unused and it is routed to the general purpose breadboard pins.</div><div style="clear: both;"></div><div style="clear: both;">The UARTs Tx and Rx pin can be reassigned to other GPIO pins by setting the rx and tx parameters in UART object creation or in object init initialization.</div></span></div>**<u>Circuit Diagram:</u>**

<div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-fwaLw0au4JI/X31_1u5GkaI/AAAAAAAACEM/56lJbYceNG06NwJ-JWmJ4UBzdO3bPDT7gCLcBGAsYHQ/w400-h340/MP_013_UART.png)](https://1.bp.blogspot.com/-fwaLw0au4JI/X31_1u5GkaI/AAAAAAAACEM/56lJbYceNG06NwJ-JWmJ4UBzdO3bPDT7gCLcBGAsYHQ/s556/MP_013_UART.png)</div>**<u>Instruction:</u>**  
1\. Connect the VCC pin of USB-Serial converter to ESP32 3.3V pin.  
2\. Connect the GND pin of USB-Serial converter to ESP32 GND pin.  
3\. Connect the Tx pin of USB-Serial converter to ESP32 GPIO 16 (Rx2) pin.

<div>4. Connect the Rx pin of USB-Serial converter to ESP32 GPIO 17 (Tx2) pin. **<u>Video Description:</u>**

I hope you learned something from this. If you have any question regarding this tutorial, please do not hesitate to write your question and suggestion in the comment box provided.

You may also like to Share this to your friends, so that it can reach more people who might benefit from this.

Please also consider supporting my Youtube channel by Subscribing. [Click this to Subscribe to TechToTinker Youtube channel.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

Happy tinkering.

**<u>Source Code:</u>**

```
# Example # 1: Simple UART for controlling
# the state of the onboard LED
# Author: George Bantique, TechToTinker
# Date: October 7, 2020

# Load the machine module in order to access the hardware
import machine

# Create the led object in GPIO 2
led = machine.Pin(2, machine.Pin.OUT)
# Create the uart object in port 2
# Rx=GPIO 16, Tx=GPIO 17
uart = machine.UART(2, 115200)

# Create a global variable to hold the receive data in serial
strMsg = ''

# This is the main loop
while True:
	# if there is character in receive serial buffer
    if uart.any() > 0:
    	# Read all the character to strMsg variable
        strMsg = uart.read()
        # Debug print to REPL
        print(strMsg)
        
        # If there is 'on' string in strMsg,
        # Turn on the LED
        if 'on' in strMsg:
            led.on()
            uart.write('Turning on LED')
            print('Turning on LED')
        # If there is 'off' string in strMsg,
        # Turn off the LED
        elif 'off' in strMsg:
            led.off()
            uart.write('Turning off LED')
            print('Turning off LED')
        # Else, invalid command
        else:
            uart.write('Invalid command')
            print('Invalid command')

```

**Solution:**

```

	# This is my solution:
        # decode() function the byte code to string
        # strip() function removes characters found in parameters
	else:
        <span>    </span>uart.write(strMsg.decode().strip('rn'))
            print(strMsg.decode().strip('rn'))

```

</div>