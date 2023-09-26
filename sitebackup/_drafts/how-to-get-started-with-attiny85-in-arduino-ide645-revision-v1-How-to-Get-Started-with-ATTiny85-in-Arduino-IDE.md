---
author: George Bantique
date: '2023-03-05T14:39:36+08:00'
guid: https://techtotinker.com/?p=886
id: 886
permalink: /?p=886
title: How to Get Started with ATTiny85 in Arduino IDE
url: /how-to-get-started-with-attiny85-in-arduino-ide645-revision-v1-How-to-Get-Started-with-ATTiny85-in-Arduino-IDE
---


I have this little tiny microcontroller lying around for some time in my bins. I purchased this out of curiosity but not able find time to try. Yesterday, I saw it again and I decided to give it a try. So here it is.

For the datasheet you may refer to:  
[https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-2586-AVR-8-bit-Microcontroller-ATtiny25-ATtiny45-ATtiny85\_Datasheet.pdf](https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-2586-AVR-8-bit-Microcontroller-ATtiny25-ATtiny45-ATtiny85_Datasheet.pdf)

For the schematic and pinouts, you may refer to:  
<https://s3.amazonaws.com/digispark/DigisparkSchematicFinal.pdf>

And for more details, you may refer to the Digistump wiki:  
<https://digistump.com/wiki/digispark?redirect=1>

**<u>Instruction:</u>**

**1. USB Driver Installation for Digispark ATTiny85 Development Board:**  
a. Install the USB driver from:  
<https://github.com/digistump/DigistumpArduino/releases>  
and look for Digistump.Drivers.zip and download it  
b. Uncompressed the downloaded Digistump.Drivers.zip  
c. Look for DPinst (if you are in Windows 32-bit) or DPinst64 (if you have 64-bit Windows) and install it.  
**2. Arduino IDE Preparation for ATTiny85:**  
a. You may follow the Digistump wiki in:  
<https://digistump.com/wiki/digispark/tutorials/connecting>  
or you may follow me by opening the Arduino IDE.  
b. Click File menu  
c. Then select Preferences  
d. And in the “Additional Boards Manager URL:” add the following:  
[http://digistump.com/package\_digistump\_index.json](http://digistump.com/package_digistump_index.json)  
e. Click OK.  
3\. Install the Digistump Board Manager  
a. Click the “Tools” menu  
b. Select “Board”  
c. Then click the “Boards Manager”  
d. In the search box type “digistump”  
e. Install the Digistump AVR Boards

Now the Arduino IDE should be ready for the ATTiny85 coding.

**<u>Video Demonstration:</u>**  
<iframe allowfullscreen="allowfullscreen" height="260" loading="lazy" src="https://www.youtube.com/embed/TarVJQiSxs8" width="480"></iframe>

If you have any concern regarding this article, be sure to leave your message in the comment box below. You may also write me in the Contact Form in the left-hand-side of this article.

Thank you and have a good days ahead.

Happy tinkering.

**<u>Source Code:</u>**

**Example 1**, Blink example from Arduino IDE &gt; File Menu &gt; Examples &gt; Basics &gt; Click Blink

**Example 2**, Blink the onboard LED using direct port manipulation

<div style="clear: both; text-align: left;"></div>```
// the setup function runs once when you press reset or power the board
void setup() {
  // initialize digital pin LED as an output.
  //pinMode(LED_PIN, OUTPUT);
  DDRB |= 0B00000010;
}

// the loop function runs over and over again forever
void loop() {
  //digitalWrite(LED_PIN, HIGH);   // turn the LED on (HIGH is the voltage level)
  PORTB |= 0B00000010;	           // turn the LED on using port manipulation
  delay(50);                       // wait for 50ms
  //digitalWrite(LED_PIN, LOW);    // turn the LED off by making the voltage LOW
  PORTB &= 0B11111101;	           // turn the LED off using port manipulation
  delay(50);                       // wait for 50ms
}

```