---
author: George Bantique
date: '2023-03-06T12:41:44+08:00'
guid: https://techtotinker.com/?p=999
id: 999
permalink: /?p=999
title: How to Use LCD Keypad Shield for Arduino
url: /how-to-use-lcd-keypad-shield-for-arduino688-revision-v1-How-to-Use-LCD-Keypad-Shield-for-Arduino
---


In this tutorial, we will learn on how to use the LCD Keypad Shield for Arduino. This shield is compatible for Arduino Uno and Arduino Mega.

<div></div><div>Pin assignments:</div><div></div><div style="clear: both; text-align: center;">[![](https://1.bp.blogspot.com/-WZfO-GoWjxo/XutLkVPQ8hI/AAAAAAAAB6g/8Mj6EFXmok8Mf0KBFgsl5CZkAr1TRnGrwCK4BGAsYHg/w500-h426/LCDKeypadShield.png)](https://1.bp.blogspot.com/-WZfO-GoWjxo/XutLkVPQ8hI/AAAAAAAAB6g/8Mj6EFXmok8Mf0KBFgsl5CZkAr1TRnGrwCK4BGAsYHg/s3396/LCDKeypadShield.png)</div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div>From this pin assignments, we can conclude that the following pins are free and available to be use for other purposes.</div><div> **Digital pins:** 0, 1, 2, 3, 11, 12, and 13.</div><div> **Analog pins:** A1 to A5.</div><div></div><div>**<u>Video Demonstration:</u>**  
**<u>  
</u>**</div><div><div style="clear: both; text-align: left;"><iframe allowfullscreen="" data-thumbnail-src="https://i.ytimg.com/vi/Sf96riu27QI/0.jpg" frameborder="0" height="280" loading="lazy" src="https://www.youtube.com/embed/Sf96riu27QI?feature=player_embedded" width="480"></iframe></div></div><div></div><div>**<u>Source Code:</u>**</div><div>```

#include "LiquidCrystal.h"

LiquidCrystal lcd( 8,  9,  4,  5,  6,  7);

void setup() {
 lcd.begin(16, 2);
 
 lcd.setCursor(0,0);
 lcd.print("  TechToTinker  ");
 
 lcd.setCursor(0,1);
 lcd.print("Press Key:");
 
}
void loop() {
 int x;
 x = analogRead (0);
 
 lcd.setCursor(10,1);
 
 if (x < 60) {
   lcd.print ("Right ");
 }
 else if (x < 200) {
   lcd.print ("Up    ");
 }
 else if (x < 400){
   lcd.print ("Down  ");
 }
 else if (x < 600){
   lcd.print ("Left  ");
 }
 else if (x < 800){
   lcd.print ("Select");
 }
 
} 
```

</div><div></div><div>If you find this tutorial helpful, like and share this to your friends who might benefit from it.</div><div></div><div>Thank you and have a good day.</div>