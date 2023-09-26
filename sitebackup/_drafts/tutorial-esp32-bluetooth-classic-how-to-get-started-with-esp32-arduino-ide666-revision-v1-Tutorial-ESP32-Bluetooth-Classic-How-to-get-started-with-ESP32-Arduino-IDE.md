---
author: George Bantique
date: '2023-03-06T11:53:54+08:00'
guid: https://techtotinker.com/?p=976
id: 976
permalink: /?p=976
title: 'Tutorial: ESP32 Bluetooth Classic | How to get started with ESP32 | Arduino IDE'
url: /tutorial-esp32-bluetooth-classic-how-to-get-started-with-esp32-arduino-ide666-revision-v1-Tutorial-ESP32-Bluetooth-Classic-How-to-get-started-with-ESP32-Arduino-IDE
---


ESP32 features a builtin WiFi and Bluetooth capabilities. In this tutorial, we will focus to the Bluetooth classic because Bluetooth LE deserves a separate tutorial.

**<u>Circuit Diagram:</u>**

<div style="clear: both; text-align: center;">[![](https://1.bp.blogspot.com/-9chq16NamVE/XzNWlo7UFpI/AAAAAAAACAA/1YBewTetQiwNhK0fyveKlGhW1rSNZNS4QCPcBGAYYCw/s0/ESP32_WebServer_AP.png)](https://1.bp.blogspot.com/-9chq16NamVE/XzNWlo7UFpI/AAAAAAAACAA/1YBewTetQiwNhK0fyveKlGhW1rSNZNS4QCPcBGAYYCw/s640/ESP32_WebServer_AP.png)</div><div style="clear: both; text-align: center;"></div>**<u>Video Demonstration:</u>**

<div style="clear: both; text-align: left;"><iframe allowfullscreen="" height="280" loading="lazy" src="https://www.youtube.com/embed/m2bcOhgjUts" width="480" youtube-src-=""></iframe></div>If you find this tutorial as helpful, please consider supporting me by Subscribing to my Youtube channel.

Thank you and have a good day.

**<u>Source Code:</u>**

```
 #include <BluetoothSerial.h>  
   
 // Pin assignments  
 #define BLUE_LED  25  
 #define GREEN_LED  26  
 #define RED_LED   27  
   
 // Instantiate a bluetooth serial  
 BluetoothSerial SerialBT;  
   
 // Variable for LED states  
 bool bluLedState = false;  
 bool grnLedState = false;  
 bool redLedState = false;  
   
 // Character buffer for incoming bluetooth data  
 char bufferData[8];   // holds the char data, 
                       // 	increase this if you will use longer commands  
 char bufferIndex = 0; // holds the position of the char in the buffer  
   
 void setup() {  
  // Create the Bluetooth device name  
  SerialBT.begin("ESP32-BT");  
   
  // Set the pin directions  
  pinMode(BLUE_LED, OUTPUT);  
  pinMode(GREEN_LED, OUTPUT);  
  pinMode(RED_LED, OUTPUT);  
 }  
   
 void loop() {  
  manageBT();  
 }  
   
 void manageBT() {  
   
  // Manage the incoming bluetooth data  
  if (SerialBT.available()) {  
   // read a single character from BT and store it in a char variable inchar  
   char inchar = (char) SerialBT.read();  
     
   if (inchar != 'n') {                       // checks if character is not Carriage Return  
    bufferData[bufferIndex++] = inchar;        //  if not, store it in our buffer  
   } else {                                    //  else,  
    bufferIndex = 0;                           //  reset our buffer index  
    memset(bufferData, 0, sizeof(bufferData)); //   and our buffer  
   }  
  }  
   
  // Execute a specific task according to bluetooth command  
  // String compare function basically compares the 2 string  
  //  and if characters are the same, returns 0  
  if        (strcmp(bufferData, "Red:1") == 0) {  
   redLedState = true;  
   SerialBT.println("Red LED On");  
  } else if (strcmp(bufferData, "Red:0") == 0) {  
   redLedState = false;  
   SerialBT.println("Red LED Off");  
  } else if (strcmp(bufferData, "Grn:1") == 0) {  
   grnLedState = true;  
   SerialBT.println("Green LED On");  
  } else if (strcmp(bufferData, "Grn:0") == 0) {  
   grnLedState = false;  
   SerialBT.println("Green LED Off");  
  } else if (strcmp(bufferData, "Blu:1") == 0) {  
   bluLedState = true;  
   SerialBT.println("Blue LED On");  
  } else if (strcmp(bufferData, "Blu:0") == 0) {  
   bluLedState = false;  
   SerialBT.println("Blue LED Off");  
  }  
   
  // Update the LED state  
  digitalWrite(BLUE_LED, bluLedState);  
  digitalWrite(GREEN_LED, grnLedState);  
  digitalWrite(RED_LED, redLedState);  
 }
 

```