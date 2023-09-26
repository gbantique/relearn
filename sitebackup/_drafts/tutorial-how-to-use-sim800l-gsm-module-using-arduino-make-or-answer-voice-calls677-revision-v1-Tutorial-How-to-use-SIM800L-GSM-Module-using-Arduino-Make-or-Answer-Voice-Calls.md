---
author: George Bantique
date: '2023-04-10T12:12:08+08:00'
guid: https://techtotinker.com/?p=1609
id: 1609
permalink: /?p=1609
title: 'Tutorial: How to use SIM800L GSM Module using Arduino | Make or Answer Voice Calls'
url: /tutorial-how-to-use-sim800l-gsm-module-using-arduino-make-or-answer-voice-calls677-revision-v1-Tutorial-How-to-use-SIM800L-GSM-Module-using-Arduino-Make-or-Answer-Voice-Calls
---


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

**<u>Video Demonstration:</u>**  
**<u>  
</u>**

<div style="clear: both; text-align: left;"><iframe allowfullscreen="allowfullscreen" data-thumbnail-src="https://i.ytimg.com/vi/xMsBFC10s1k/0.jpg" frameborder="0" height="260" loading="lazy" src="https://www.youtube.com/embed/xMsBFC10s1k?feature=player_embedded" width="480"></iframe></div><div style="clear: both; text-align: left;"> </div><div style="clear: both; text-align: left;"> </div><div style="clear: both; text-align: left;">**<u>Source Code:</u>**</div><div style="clear: both; text-align: left;"> </div>```
<pre class="wp-block-code">```
#include "SoftwareSerial.h"

SoftwareSerial mySerial(2, 3);

void setup(){

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
```

<div style="clear: both; text-align: left;"> </div><div style="clear: both; text-align: left;"> </div><div style="clear: both; text-align: left;">If you found this tutorial helpful, please do Like and Share this to your friends so that many could benefit from it. </div><div style="clear: both; text-align: left;"> </div><div style="clear: both; text-align: left;">Also please do not forget to Subscribe to my Youtube channel:</div><div style="clear: both; text-align: left;">[Click this to Subscribe](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)</div>Thank you and have a good day.