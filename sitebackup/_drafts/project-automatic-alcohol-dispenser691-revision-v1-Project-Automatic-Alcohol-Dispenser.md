---
author: George Bantique
date: '2023-03-06T12:43:35+08:00'
guid: https://techtotinker.com/?p=1003
id: 1003
permalink: /?p=1003
title: 'Project: Automatic Alcohol Dispenser'
url: /project-automatic-alcohol-dispenser691-revision-v1-Project-Automatic-Alcohol-Dispenser
---


This is my own version of automatic (No touch / touchless) alcohol / sanitizer dispenser. This is especially useful during this COVID19 pandemic. In here, we will be using an ultrasonic sensor to detect the presence of hand near the dispenser. When the ultrasonic sensor detects an object less than the set distance, it will rotate the attached servo motors to push the plunger of the dispenser.

<div></div><div>**<u>Materials:</u>**</div><div>1. Arduino Uno</div><div>2. HC-SR04 ultrasonic sensor</div><div>3. MG996R servo motor</div><div>4. Some wires, dispenser, some piece of wood.</div><div></div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-xe1-pc6XOlU/XtWuRJ40KSI/AAAAAAAAAJE/w__JfulAhNIAhZZW0zVLTgdUz_YlaLpPwCLcBGAsYHQ/s400/AlcoholDispenser.png)](https://1.bp.blogspot.com/-xe1-pc6XOlU/XtWuRJ40KSI/AAAAAAAAAJE/w__JfulAhNIAhZZW0zVLTgdUz_YlaLpPwCLcBGAsYHQ/s1600/AlcoholDispenser.png)</div><div></div><div>**<u>Video Demonstration:</u>**</div><div></div><div style="clear: both; text-align: left;"><iframe allowfullscreen="" height="280" loading="lazy" src="https://www.youtube.com/embed/YDNmXjx8tbA" width="480" youtube-src-=""></iframe></div><div></div><div></div><div>**<u>Source Code:</u>**</div><div>```

#include "Servo.h"

Servo myservo; // create servo object to control a servo
   
#define ECHO_PIN 6
#define TRIG_PIN 7

#define SERVO_PIN 9

long duration, cm;


void setup() {
  // put your setup code here, to run once:
  
  Serial.begin (9600);

  pinMode(ECHO_PIN, INPUT);
  pinMode(TRIG_PIN, OUTPUT);

  myservo.attach(SERVO_PIN); // attaches the servo on pin 9 to the servo object
  myservo.write(60);
  delay(1000);

  Serial.println("SETUP Complete");
}

void loop() {
  // put your main code here, to run repeatedly:

  // The sensor is triggered by a HIGH pulse of 10 or more microseconds.
  // Give a short LOW pulse beforehand to ensure a clean HIGH pulse:
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(5);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
 
  // Read the signal from the sensor: a HIGH pulse whose
  // duration is the time (in microseconds) from the sending
  // of the ping to the reception of its echo off of an object.
  duration = pulseIn(ECHO_PIN, HIGH);
 
  // Convert the time into a distance
  cm = (duration/2) *0.0343;     // Speed of sound 0.0343 cm/usec

  myservo.write(60);
  
  if (cm <= 10) {
    myservo.write(120); // sets the servo position
  }

  delay(400);
}

```

</div><div></div><div>If you have any question, please do not hesitate to leave it in the comment box. </div><div></div><div>Please Subscribe to my Youtube channel so that you will not missed interesting tutorials like this.</div><div></div><div>Thank you and have a good day.</div><div></div><div>Happy tinkering.</div>