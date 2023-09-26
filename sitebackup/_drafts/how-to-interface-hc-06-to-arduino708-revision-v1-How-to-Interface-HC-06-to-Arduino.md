---
author: George Bantique
date: '2023-03-10T18:40:09+08:00'
guid: https://techtotinker.com/?p=1175
id: 1175
permalink: /?p=1175
title: How to Interface HC-06 to Arduino
url: /how-to-interface-hc-06-to-arduino708-revision-v1-How-to-Interface-HC-06-to-Arduino
---


<div dir="ltr" style="text-align: left;"><div dir="ltr" style="text-align: left;">Electronics projects nowadays usually needs a wireless communication interface. Bluetooth device is one of the most commonly use for this purpose. Today, I am going to show you on how to interface HC-06 bluetooth module to Arduino.

**<u>Materials:</u>**  
1\. HC-06 bluetooth module  
2\. Arduino Uno board  
3\. A couple of jumper wires

<u>**Instruction:**</u>  
1\. Connect the HC-06 VCC pin to +5V pin of Arduino Uno (please refer to the schematic below).  
2\. Connect the HC-06 GND pin to GND pin of Arduino Uno.  
3\. Connect the HC-06 TXD pin to pin 2 of Arduino Uno (functions as Rx).  
4\. Connect the HC-06 RXD pin to pin 3 of Arduino Uno (fucntions as Tx).  
5\. Connect the Arduino Uno to the computer.  
6\. Run the Arduino IDE.  
7\. Make sure Arduino Uno is selected under Tools &gt; Board.  
8\. Select the correct serial port under Tools &gt; Port.  
9\. Upload the sketch. Source code is provided below.

<span style="color: #f1c232;">***This basically echoes the serial data through HC-06 to hardware serial of Arduino Uno and vice-versa.***</span>

**<u>HC-06 Troubleshooting:</u>**  
1\. Connect a jumper wire between HC-06 RXD and TXD.  
2\. Connect +5V supply to VCC.  
3\. Connect the supply ground to GND pin.  
4\. Using a mobile / smart phone connect through a bluetooth to HC-06.  
5\. Enter pin code 1234.

<span style="color: #f1c232;"><span style="color: #0b5394;">***This basically echoes whatever is received by the HC-06 to a bluetooth terminal.***</span></span>

If you find this lesson useful, please kindly support my blogspot and Youtube channel. Links provided below:  
1\. [tech-to-tinker.blogspot.com](https://tech-to-tinker.blogspot.com/)  
2\. [tech-to-tinker Youtube channel.](https://www.youtube.com/channel/UCRxbBqSgqB4LIm6QxLUVlcg)

**<u>Schematic Diagram:</u>**

<div style="clear: both; text-align: center;">[![](https://1.bp.blogspot.com/-8bMqzJBMMu4/XkUOCzqnY1I/AAAAAAAAACw/IhU1lV_Rv5ER_eWWOuQ7FKicsbWx3Gt-wCLcBGAsYHQ/s640/Getting-Started-with-HC-06.png)](https://1.bp.blogspot.com/-8bMqzJBMMu4/XkUOCzqnY1I/AAAAAAAAACw/IhU1lV_Rv5ER_eWWOuQ7FKicsbWx3Gt-wCLcBGAsYHQ/s1600/Getting-Started-with-HC-06.png)</div>**<u>Source Code:</u>**

</div>```
#include <softwareserial.h>

SoftwareSerial hc06(2,3);
String cmd="";

void setup(){
 //Initialize Serial Monitor
 Serial.begin(9600);
 
 //Initialize Bluetooth Serial Port
 hc06.begin(9600);
 
 Serial.print("Setup DONE.");
}

void loop(){

//Write data from HC06 to Serial Monitor
  if (hc06.available()){
    Serial.write(hc06.read());
  }
  
  //Write from Serial Monitor to HC06
  if (Serial.available()){
    hc06.write(Serial.read());
  }   

 delay(100);
}
```

</div>