---
author: George Bantique
date: '2023-03-06T12:40:02+08:00'
guid: https://techtotinker.com/?p=997
id: 997
permalink: /?p=997
title: MIT App Inventor for Arduino
url: /mit-app-inventor-for-arduino686-revision-v1-MIT-App-Inventor-for-Arduino
---


Building an Android application is one option to control your robot project or anything that comes to your mind. And with the use of the MIT App Inventor 2, you can easily create an Android application.

In this tutorial, we will learn to create a simple Android application which can control the state of the LED connected to Arduino Uno board. Bluetooth communication protocol will be use via HC-06 bluetooth module.

In the Arduino Uno, we will create a circuit according to the following diagram:

<div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-DRB-78FT_-4/Xu8ZEw_7E1I/AAAAAAAAAKE/SZaRmBTN1T4c7LE3aLg8idb6s6Gl2XaYACLcBGAsYHQ/s400/For_MIT_App_Inventor_test.png)](https://1.bp.blogspot.com/-DRB-78FT_-4/Xu8ZEw_7E1I/AAAAAAAAAKE/SZaRmBTN1T4c7LE3aLg8idb6s6Gl2XaYACLcBGAsYHQ/s1600/For_MIT_App_Inventor_test.png)</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">**<u>Source Code:</u>**</div>```

#include "SoftwareSerial.h"

#define LED_PIN 13
#define HC06_RX 2
#define HC06_TX 3

SoftwareSerial HC06(HC06_RX,HC06_TX);

bool LED_STATE = false;
String cmd = "";

void setup() {
  // put your setup code here, to run once:
  HC06.begin(9600);

  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LED_STATE);
}

void loop() {
  // put your main code here, to run repeatedly:

  while(HC06.available()>0){
   cmd+=(char)HC06.read();
  }

  if(cmd!=""){
    cmd.trim();  // Remove added LF in transmit
    // We expect Command from bluetooth
    if (cmd.equals("1")) {
      LED_STATE = true;
    } else if (cmd.equals("0")) {
      LED_STATE = false;
    }

    cmd=""; //reset cmd
  }

  digitalWrite(LED_PIN, LED_STATE);
}

```

<div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">**<u>MIT App Inventor:</u>**</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">Follow the following sketch for the MIT App Inventor.</div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-MZBcWCLVBoM/Xu8Z-E3cWaI/AAAAAAAAAKM/szlX6qQuwG0dgj79u_xVQlxZAUGqGOTXACLcBGAsYHQ/w432-h71/MITApp-Setup1.png)](https://1.bp.blogspot.com/-MZBcWCLVBoM/Xu8Z-E3cWaI/AAAAAAAAAKM/szlX6qQuwG0dgj79u_xVQlxZAUGqGOTXACLcBGAsYHQ/s1600/MITApp-Setup1.png)</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-KbNENRf0dJ0/Xu8aR8rJ5bI/AAAAAAAAAKU/GVKe1l_LtPg_cv_1IfVVVm-oL9szMbYgwCLcBGAsYHQ/w470-h175/MITApp-Setup2.png)](https://1.bp.blogspot.com/-KbNENRf0dJ0/Xu8aR8rJ5bI/AAAAAAAAAKU/GVKe1l_LtPg_cv_1IfVVVm-oL9szMbYgwCLcBGAsYHQ/s1600/MITApp-Setup2.png)</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-bxKIuAABQg4/Xu8aZWIja4I/AAAAAAAAAKY/-7lsYHIW-6cCOoKKsiv7vFbjPDn9D68bwCLcBGAsYHQ/w338-h129/MITApp-Disconnect.png)](https://1.bp.blogspot.com/-bxKIuAABQg4/Xu8aZWIja4I/AAAAAAAAAKY/-7lsYHIW-6cCOoKKsiv7vFbjPDn9D68bwCLcBGAsYHQ/s1600/MITApp-Disconnect.png)</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-Gk_zlmYCQ7k/Xu8agzfRVAI/AAAAAAAAAKc/O8i2jxxF8kMLrn7tFWQjBMc-SIqh8UkYACLcBGAsYHQ/w290-h90/MITApp-TurnON.png)](https://1.bp.blogspot.com/-Gk_zlmYCQ7k/Xu8agzfRVAI/AAAAAAAAAKc/O8i2jxxF8kMLrn7tFWQjBMc-SIqh8UkYACLcBGAsYHQ/s1600/MITApp-TurnON.png)</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-RKnyEZagHR0/Xu8am1HFA7I/AAAAAAAAAKk/teh9kt3CvVEeKgCF5fvOaBMsoo7xO2soQCLcBGAsYHQ/w297-h90/MITApp-TurnOFF.png)](https://1.bp.blogspot.com/-RKnyEZagHR0/Xu8am1HFA7I/AAAAAAAAAKk/teh9kt3CvVEeKgCF5fvOaBMsoo7xO2soQCLcBGAsYHQ/s1600/MITApp-TurnOFF.png)</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">You may download the Android application here:</div><div style="clear: both; text-align: left;">[DOWNLOAD THE ANDROID APP](https://drive.google.com/file/d/1B0P0TVVacaOAiNk-2dmj7DjeB3j_-U1k/view?usp=sharing)</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">**<u>Video Demonstration:</u>**</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">**Part 1:**</div><iframe allowfullscreen="" data-thumbnail-src="https://i.ytimg.com/vi/Gb9nCv1b1_I/0.jpg" frameborder="0" height="280" loading="lazy" src="https://www.youtube.com/embed/Gb9nCv1b1_I?feature=player_embedded" width="480"></iframe>

<div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">**Part 2:**</div><iframe allowfullscreen="" data-thumbnail-src="https://i.ytimg.com/vi/kN3Zc-FFaTI/0.jpg" frameborder="0" height="280" loading="lazy" src="https://www.youtube.com/embed/kN3Zc-FFaTI?feature=player_embedded" width="480"></iframe>

<div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">I hope I am able to impart a little information to you, but if you have any question, please do not hesitate to leave your question in the comment box.</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">Thank you and have a good day.</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">Happy tinkering.</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;"></div>