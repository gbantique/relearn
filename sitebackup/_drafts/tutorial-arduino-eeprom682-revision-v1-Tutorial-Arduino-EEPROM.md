---
author: George Bantique
date: '2023-03-06T12:38:03+08:00'
guid: https://techtotinker.com/?p=993
id: 993
permalink: /?p=993
title: 'Tutorial: Arduino EEPROM'
url: /tutorial-arduino-eeprom682-revision-v1-Tutorial-Arduino-EEPROM
---


Whatever you are doing with the Arduino is lost the moment you pressed the RESET button or you removed the power. The work around that is to implement some data logging to an SD card or through the EEPROM.

So what is EEPROM?  
EEPROM stands for Electrically Erasable Programmable Read-Only Memory. Each EEPROM address can save 1 byte of data. Arduino Uno has 1024 bytes of addressable positions while Arduino Mega has 4096 bytes of addressable positions.

EEPROM is effective solution for saving data but do not abuse the use of it. Why? The manufacturer specified an approximate of 100,000 write cycles for each address position before it gets degrading and giving weird results. Read access is possible as many times as you like without compromising its life expectancy.

**<u>Video Demonstration:</u>**

<div style="clear: both; text-align: left;"><iframe allowfullscreen="" data-thumbnail-src="https://i.ytimg.com/vi/IaU1wvJ8qpE/0.jpg" frameborder="0" height="280" loading="lazy" src="https://www.youtube.com/embed/IaU1wvJ8qpE?feature=player_embedded" width="480"></iframe></div><span></span><span></span>  
**<u>Circuit Diagram:</u>**

<div style="clear: both; text-align: center;">[![](https://1.bp.blogspot.com/-bH9B17AqRrQ/XwO4-iJrYhI/AAAAAAAAAL4/MMrpxuDqlt0bzqw9iFerxF27-EKS00SpQCK4BGAsYHg/w376-h500/Arduino_EEPROM-Diagram.png)](https://1.bp.blogspot.com/-bH9B17AqRrQ/XwO4-iJrYhI/AAAAAAAAAL4/MMrpxuDqlt0bzqw9iFerxF27-EKS00SpQCK4BGAsYHg/s681/Arduino_EEPROM-Diagram.png)</div><div>**<u>  
</u>**

</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>  
</u>**</div><div>**<u>Source Code:</u>**  
**<u>  
</u>**1. Without EEPROM: ```

#define YLW_LED_PIN 10
#define RED_LED_PIN 9
#define YLWBUTTON_PIN  8
#define REDBUTTON_PIN  7

bool YLWledState = LOW;
bool YLWbuttonState;
bool YLWlastButtonState = LOW;
unsigned long YLWlastDebounceTime = 0;
unsigned long YLWdebounceDelay = 50;

bool REDledState = LOW;
bool REDbuttonState;
bool REDlastButtonState = LOW;
unsigned long REDlastDebounceTime = 0;
unsigned long REDdebounceDelay = 50;


void setup() {

  pinMode(YLWBUTTON_PIN, INPUT_PULLUP);
  pinMode(REDBUTTON_PIN, INPUT_PULLUP);
  pinMode(YLW_LED_PIN, OUTPUT);
  pinMode(RED_LED_PIN, OUTPUT);

}

void loop() {

  // Debouncing of the input button
  int YLWreading = digitalRead(YLWBUTTON_PIN);
  int REDreading = digitalRead(REDBUTTON_PIN);

  if (YLWreading != YLWlastButtonState) {
    YLWlastDebounceTime = millis();
  }

  if (REDreading != REDlastButtonState) {
    REDlastDebounceTime = millis();
  }

  if ((millis() - YLWlastDebounceTime) > YLWdebounceDelay) {
    if (YLWreading != YLWbuttonState) {
      YLWbuttonState = YLWreading;
      if (YLWbuttonState == LOW) {
        YLWledState = !YLWledState;
        digitalWrite(YLW_LED_PIN, YLWledState);
      }
    }
  }

  if ((millis() - REDlastDebounceTime) > REDdebounceDelay) {
    if (REDreading != REDbuttonState) {
      REDbuttonState = REDreading;
      if (REDbuttonState == LOW) {
        REDledState = !REDledState;
        digitalWrite(RED_LED_PIN, REDledState);
      }
    }
  }

  YLWlastButtonState = YLWreading;
  REDlastButtonState = REDreading;
}

```

2\. With EEPROM:

```

#include "EEPROM.h"

#define YLW_LED_PIN 10
#define RED_LED_PIN 9
#define YLWBUTTON_PIN  8
#define REDBUTTON_PIN  7

bool YLWledState = LOW;
bool YLWbuttonState;
bool YLWlastButtonState = LOW;
unsigned long YLWlastDebounceTime = 0;
unsigned long YLWdebounceDelay = 50;

bool REDledState = LOW;
bool REDbuttonState;
bool REDlastButtonState = LOW;
unsigned long REDlastDebounceTime = 0;
unsigned long REDdebounceDelay = 50;


void setup() {

  pinMode(YLWBUTTON_PIN, INPUT_PULLUP);
  pinMode(REDBUTTON_PIN, INPUT_PULLUP);
  pinMode(YLW_LED_PIN, OUTPUT);
  pinMode(RED_LED_PIN, OUTPUT);

  getLEDState();
}

void loop() {

  // Debouncing of the input button
  int YLWreading = digitalRead(YLWBUTTON_PIN);
  int REDreading = digitalRead(REDBUTTON_PIN);

  if (YLWreading != YLWlastButtonState) {
    YLWlastDebounceTime = millis();
  }

  if (REDreading != REDlastButtonState) {
    REDlastDebounceTime = millis();
  }

  if ((millis() - YLWlastDebounceTime) > YLWdebounceDelay) {
    if (YLWreading != YLWbuttonState) {
      YLWbuttonState = YLWreading;
      if (YLWbuttonState == LOW) {
        YLWledState = !YLWledState;
        EEPROM.update(0, YLWledState);
        digitalWrite(YLW_LED_PIN, YLWledState);
      }
    }
  }

  if ((millis() - REDlastDebounceTime) > REDdebounceDelay) {
    if (REDreading != REDbuttonState) {
      REDbuttonState = REDreading;
      if (REDbuttonState == LOW) {
        REDledState = !REDledState;
        EEPROM.update(1, REDledState);
        digitalWrite(RED_LED_PIN, REDledState);
      }
    }
  }

  YLWlastButtonState = YLWreading;
  REDlastButtonState = REDreading;
}

void getLEDState() {
  YLWledState = EEPROM.read(0);
  digitalWrite(YLW_LED_PIN, YLWledState);
  REDledState = EEPROM.read(1);
  digitalWrite(RED_LED_PIN, REDledState);
}

```

Hope you find this article useful. Please leave your comments in the comment box.

Thank you and have a good day.

<div style="clear: both; text-align: center;"></div></div>