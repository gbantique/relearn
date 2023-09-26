---
author: George Bantique
date: '2023-03-06T12:44:07+08:00'
guid: https://techtotinker.com/?p=1004
id: 1004
permalink: /?p=1004
title: 'TUTORIAL: How to use HC-SR04 Ultrasonic Sensor with Arduino'
url: /tutorial-how-to-use-hc-sr04-ultrasonic-sensor-with-arduino692-revision-v1-TUTORIAL-How-to-use-HC-SR04-Ultrasonic-Sensor-with-Arduino
---


This tutorial is about the commonly use HC-SR04 ultrasonic sensor. We will give some details about it, explain how it works, and at the last part we will create an Arduino sketch to test its functionality.

<div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-5fIJq2bwma0/XstGbyuDhaI/AAAAAAAAAHg/srqnr9MSz_QuHxjCJ5xFmLUaSi_4GOzZQCLcBGAsYHQ/w512-h316/HC-SR04-image.png)](https://1.bp.blogspot.com/-5fIJq2bwma0/XstGbyuDhaI/AAAAAAAAAHg/srqnr9MSz_QuHxjCJ5xFmLUaSi_4GOzZQCLcBGAsYHQ/s1600/HC-SR04-image.png)</div>**<u>HOW HC-SR04 WORKS:</u>**  
The HC-SR04 ultrasonic sensor uses a sound frequency to determine distance of an object just like how dolphins and bats senses its surroundings for navigation. Ultrasonic sound frequency can be found just above the upper limit of human hearing capacity (20 Hz to 20 KHz).

<div style="border: 0px; line-height: 1.57143em; margin: 0px; padding: 0px;">Ultrasonic sensors is composed of 2 sound transducers, one for transmitter and another one for receiver. So, how does it work?</div>1. The transmitter sends a signal and when the signal hits an object, the signal is reflected.
2. The reflected signal is then receive by the receiver.

<div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-QSQ1cUjT--s/XtWdACn3vDI/AAAAAAAAAIs/ocoBNKO5EYcTTGRlZCzbiUaI8OaEt-KZQCLcBGAsYHQ/w512-h384/HC-SR04-Illustration.png)](https://1.bp.blogspot.com/-QSQ1cUjT--s/XtWdACn3vDI/AAAAAAAAAIs/ocoBNKO5EYcTTGRlZCzbiUaI8OaEt-KZQCLcBGAsYHQ/s1600/HC-SR04-Illustration.png)</div><div style="clear: both; text-align: left;"></div>The time it takes for the transmitted signal to be reflected back to the sensor is proportionally equivalent to the distance it travels according to the speed of sound.

To determine the distance of an object, we will use the following formula:

 **distance = duration x speed of sound**

<div>But, we need to consider that the signal is sent, hits an object, then reflected back. So taking it in consideration, we will use the following formula instead:</div><div><div style="border: 0px; line-height: 1.57143em; margin: 0px; padding: 0px;"><span style="color: #383838;"><span style="font-size: 14px;"> </span></span>**distance = (duration / 2) x speed of sound**</div><div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div>The speed of sound is **343 m/sec** or **0.0343 cm/µsec** since the returned value of ultrasonic sensor is in micro seconds.

<div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div>**<u>PIN ASSIGNMENTS:</u>**

1. VCC: +5V DC
2. Trig: Trigger pin is an input signal used to activate the sending of ultrasonic signal burst.
3. Echo: Echo pin is an output signal which returns a TTL HIGH equivalent to the duration of travel from transmitting to receiving.
4. GND: Ground pin

The HC-SR04 provides a distance measurement between 2 cm to 400 cm with an accuracy of 3mm at a maximum angle of 15 degrees. For more details, you may check the datasheet (links will be provided n the description).  
\[<https://www.electroschematics.com/wp-content/uploads/2013/07/HCSR04-datasheet-version-1.pdf>\]

<div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div><div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div>**<u>ARDUINO EXAMPLE:</u>**

For this example, we will need the following materials to test its funtionality:

1. An Arduino Uno board as the main controller.
2. The HC-SR04 Ultrasonic sensor.
3. Some jumper wires.

<div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-3lD5MeWgTHQ/XstGajrmP7I/AAAAAAAAAHk/Vdmv1CHZvcMpoBIa74Yf7fLqYlpWxc27wCPcBGAYYCw/w512-h262/HC-SR04-CircuitDiagram.png)](https://1.bp.blogspot.com/-3lD5MeWgTHQ/XstGajrmP7I/AAAAAAAAAHk/Vdmv1CHZvcMpoBIa74Yf7fLqYlpWxc27wCPcBGAYYCw/s1600/HC-SR04-CircuitDiagram.png)</div><div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div>So lets build our circuit,

</div><div>1. Connect the VCC pin of HC-SR04 to 5V pin of Arduino Uno.
2. Connect the Trig pin of HC-SR04 to digital pin 9 of Arduino Uno.
3. Connect the Echo pin of HC-SR04 to digital pin 10 of Arduino Uno.
4. Connect the GND pin of HC-SR04 to GND pin of Arduino Uno.

<div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div>Now, let us prepare our Arduino code,

</div><div>1. Copy and paste the provided Arduino code to your Arduino IDE.
2. Connect the Arduino Uno to your computer and make sure that the correct board is selected under Tools &gt; Board. Also make sure that the correct serial port is selected under Tools &gt; Port.
3. Upload the sketch and if everything is ok, you should be able to see the measured distance of the HC-SR04 ultrasonic sensor in your Serial Monitor.

<div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div><div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div>**<u>VIDEO DEMONSTRATION:</u>**  
**<u>  
</u>**

<div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"><div style="clear: both; text-align: left;"><iframe allowfullscreen="" data-thumbnail-src="https://i.ytimg.com/vi/NwMDtrWotsw/0.jpg" frameborder="0" height="280" loading="lazy" src="https://www.youtube.com/embed/NwMDtrWotsw?feature=player_embedded" width="480"></iframe></div></div><div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div><div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div>**<u>SOURCE CODE:</u>**

<div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div><div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;">```
#define ECHO_PIN 10
#define TRIG_PIN 9

long duration, distance;

void setup() {

  Serial.begin (9600);
  
  pinMode(ECHO_PIN, INPUT);
  pinMode(TRIG_PIN, OUTPUT);
  
  Serial.println("SETUP Complete");
  
}

void loop() {

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
  distance = (duration/2) * 0.0343;     // speed of sound is 0.0343 cm/us

  // Now send it to Serial monitor
  Serial.print("Distance is ");
  Serial.print(distance);
  Serial.println(" cm.");

  delay(1000);

}

```

</div><div style="border: 0px; color: #383838; font-size: 14px; line-height: 1.57143em; margin: 0px; padding: 0px;"></div>If you find this tutorial helpful, please consider helping me by sharing this to your friends. If you have any question, please do not hesitate to leave it in the comments section.

Thank you and have a good day.

Happy tinkering!

</div>