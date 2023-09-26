---
author: George Bantique
date: '2023-03-10T18:48:31+08:00'
guid: https://techtotinker.com/?p=1176
id: 1176
permalink: /?p=1176
title: How To Play MP3 Files on Arduino from SD Card
url: /how-to-play-mp3-files-on-arduino-from-sd-card701-revision-v1-How-To-Play-MP3-Files-on-Arduino-from-SD-Card
---


<div dir="ltr" style="text-align: left;">After successfully accessing the SD Card with Arduino Uno the crude way (by soldering directly to SD card pins), I thought of making an mp3 player :). As a caveat, this design is not recommended for a daily mp3 player but it does play and I am happy with it.So let’s prepare the following materials:  
1\. Arduino Uno (or any microcontroller you are comfortable with).  
2\. SD card loaded with mp3 file.  
3\. A speaker (optional audio amplifier).  
4\. 3 pieces tactile switch for player control.  
5\. Breadboard.  
6\. Some jumper wires.  
7\. Source code provided below.

So let us begin by:

<div>1. First, make the necessary soldering of jumper wires to the SD card. Please refer to the schematic below. You may also break out the SD card cover to expose its internal parts. This is to make soldering easier. I suggest to make the connection from inside so that you may still connect the SD card to a computer for transferring of files (please refer to what I did).</div><div> </div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-JxHLXM5sGig/Xn10MgzzeqI/AAAAAAAAAD4/8Ou-xqM6ixQegQA1mt0m0v32nfWz_jgmgCLcBGAsYHQ/s1600/SDCard-Solder-For-SPI.png)](https://1.bp.blogspot.com/-JxHLXM5sGig/Xn10MgzzeqI/AAAAAAAAAD4/8Ou-xqM6ixQegQA1mt0m0v32nfWz_jgmgCLcBGAsYHQ/s1600/SDCard-Solder-For-SPI.png)</div><div style="clear: both; text-align: left;">Please take note of the following:</div><div style="clear: both; text-align: left;"> * SD card pin 1 (C/S pin) is connected with (green) wire to Arduino Uno pin 10.</div><div style="clear: both; text-align: left;"> * SD card pin 2 (MOSI pin) is connected with (blue) wire to Arduino Uno pin 11.</div><div style="clear: both; text-align: left;"> * SD card pin 3 and pin 6 (VSS pin) is connected with (violet) wire to Arduino Uno GND pin.</div><div style="clear: both; text-align: left;"> * SD card pin 4 (VDD pin) is connected with (dark gray) wire to Arduino Uno +3.3V pin (but I believe you can also use 5V, but Im not sure with regards to this).</div><div style="clear: both; text-align: left;"> * SD card pin 5 (SCK pin) is connected with (light gray) wire to Arduino Uno pin 13.</div><div style="clear: both; text-align: left;"> * SD card pin 7 (MISO pin) is connected with (black) wire to Arduino Uno pin 12.</div>2\. Connect the speaker to Arduino Uno as follows:  
\* Speaker (+) pin to Arduino Uno pin 9.  
\* Speaker (-) pin to Arduino Uno GND pin.  
3\. Connect the Arduino Uno to a computer.

<div style="clear: both; text-align: left;"> * Run the Arduino IDE.</div><div style="clear: both; text-align: left;"> * Make sure “Arduino Uno” is selected under Tools &gt; Board.</div><div style="clear: both; text-align: left;"> * Check that the correct serial port is selected under Tools &gt; Port.</div>4\. Upload the following sketch. After successfully uploading the sketch to our microcontroller, it should function as follows:  
\* Pressing the first button should play the previous mp3 file.  
\* Pressing the middle button should play / pause the currently playing mp3 file.  
\* Pressing the third button should play the next mp3 file.  
\* Long pressing the first button should decrease the volume.  
\* Long pressing the third button should increase the volume.  
5\. Lastly, feel free to modify the source code for learning and fun.

## Video Demonstration:

</div><div aria-hidden="true" class="wp-block-spacer" style="height:10px"></div><div aria-hidden="true" class="wp-block-spacer" style="height:10px"></div>## Source Code:

<div>```
#include "SD.h"
#include "TMRpcm.h"
#include "SPI.h"


#define SD_ChipSelectPin 10
#define spkrPin 9


TMRpcm tmrpcm;

void setup() {
  
  tmrpcm.speakerPin = spkrPin;

  SD.begin(SD_ChipSelectPin);

  tmrpcm.setVolume(6);
  tmrpcm.play("CannotBe.wav");
  delay(3000);

}

void loop() {
  
  tmrpcm.play("AyeSir.wav");
  delay(3000);  
  
}

```

</div>If you find this lesson helpful, please consider supporting my Youtube channel: tech-to-tinker. You may also leave your comments and suggestions in the comment box below.

Thank you and have a good day.

Happy tinkering!