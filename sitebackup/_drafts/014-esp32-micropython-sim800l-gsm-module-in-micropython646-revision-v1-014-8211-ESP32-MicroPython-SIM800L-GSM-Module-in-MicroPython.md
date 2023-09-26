---
author: George Bantique
date: '2023-03-05T14:40:44+08:00'
guid: https://techtotinker.com/?p=887
id: 887
permalink: /?p=887
title: '014 &#8211; ESP32 MicroPython: SIM800L GSM Module in MicroPython'
url: /014-esp32-micropython-sim800l-gsm-module-in-micropython646-revision-v1-014-8211-ESP32-MicroPython-SIM800L-GSM-Module-in-MicroPython
---


In this tutorial, we will tackle on how to interface SIM800L GSM module to ESP32 in MicroPython.

**<u>Circuit Diagram:</u>**

<div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-2Nb5xCGaXws/X4G7pSyf6mI/AAAAAAAACEc/Evpku9h-pRMMSxfkdw5F-jSzgwD5AGKtACLcBGAsYHQ/w400-h356/sim800l_intro_mp.png)](https://1.bp.blogspot.com/-2Nb5xCGaXws/X4G7pSyf6mI/AAAAAAAACEc/Evpku9h-pRMMSxfkdw5F-jSzgwD5AGKtACLcBGAsYHQ/s551/sim800l_intro_mp.png)</div>**<u>Instruction:</u>**  
1\. Connect SIM800L Rx pin to ESP32 GPIO 16 (Tx2).

<div>2. Connect SIM800L Tx pin to ESP32 GPIO 17 (Rx2).  
3. Connect the SIM800L VCC pin to external positive terminal. In this tutorial, I use external power supply through 1N4001 diode.  
4. Common ground all the ground pins. **Refer to this manual for the AT Commands:**

</div><div>[https://www.elecrow.com/wiki/images/2/20/SIM800\_Series\_AT\_Command\_Manual\_V1.09.pdf](https://www.elecrow.com/wiki/images/2/20/SIM800_Series_AT_Command_Manual_V1.09.pdf)</div>**<u>Video Demonstration:</u>**

**<u>  
</u><u></u>**

<div style="clear: both; text-align: left;"><iframe allowfullscreen="" height="260" loading="lazy" src="https://www.youtube.com/embed/uKGD9oKQJSs" width="480" youtube-src-=""></iframe></div>If you find this tutorial as helpful, please do Subscribe to my Youtube channel by clicking the following link: [Click this to Subscribe to TechToTinker Youtube channel.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead,  
George Bantique, TechToTinker

**<u>Source Code:</u>**

```
```
import machine
gsm = machine.UART(2, 115200)

#1. Check for the signal strength
gsm.write('AT+CSQr')

#2. Get the list of available network operators
gsm.write('AT+COPS=?r')

#3. Determine the network operators the sim800l is currently registered
gsm.write('AT+COPS?r')

#4. Determine if the sim800l is currently connected
gsm.write('AT+CREG?r')

#5. Force it to connect
gsm.write('AT+CREG=1r')

#6. Get the current battery level
gsm.write('AT+CBCr')

#7. Turn off the echo of commands, 

```# use ATE0 to turnoff and ATE1 to turnon.
gsm.write('ATE0r')
```