---
author: George Bantique
categories:
  - Arduino
  - Uno
date: '2020-07-17T20:57:00+08:00'
title: 'Tutorial: How to use SIM800L GSM Module using Arduino | Make or Answer Voice Calls'
url: /2020/07/17/tutorial-how-to-use-sim800l-gsm-module-using-arduino-make-or-answer-voice-calls/
---

## **Introduction**
Here we will further our exploration with the SIM800L GSM Module by testing the voice call capability.

**<u>To make a voice call:</u>**  
 1. Send **ATD+ZZxxxxxxxxxx;&lt;CR&gt;&lt;LF&gt;**  
 – this is tell the GSM module to dial the following mobile number  
 **ZZ** – the country code of the mobile number you want to call  
 **xx** – is the 10-digit mobile number  
 **;** – don’t forget semi-colon, it will error without the semi-colon  
 **CR** – Cariage Return, ASCII character 13 or r  
 **LF** – Line Feed, ASCII character 10 or n  
   
**<u>To answer the incoming voice call:</u>**  
 1. Send **ATA&lt;CR&gt;&lt;LF&gt;**  
 – this tells the GSM module to answer the incoming voice call  
 **CR** – Cariage Return, ASCII character 13 or r  
 **LF** – Line Feed, ASCII character 10 or n

**<u>To reject the incoming call or terminate ongoing call:</u>**  
 1. Send **ATH&lt;CR&gt;&lt;LF&gt;**  
 – this tells the GSM module to hang up the call.  
 **CR** – Cariage Return, ASCII character 13 or r  
 **LF** – Line Feed, ASCII character 10 or n

## **Video Demonstration**
{{< youtube id="xMsBFC10s1k" >}}

## **Source Code**

```cpp { lineNos="true" wrap="true" }
#include "SoftwareSerial.h"

SoftwareSerial mySerial(2, 3);

void setup() {
  Serial.begin(9600);
  mySerial.begin(9600);
}

void loop(){
  while (Serial.available()) 
  {
    mySerial.write(Serial.read());//Forward what Serial received to Software Serial Port
  }
  while(mySerial.available()) 
  {
    Serial.write(mySerial.read());//Forward what Software Serial received to Serial Port
  }
}
```

## **Call To Action**
If you found this tutorial helpful, please do Like and Share this to your friends so that many could benefit from it.

Also please do not forget to Subscribe to my Youtube channel:
[Click this to Subscribe](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good day.