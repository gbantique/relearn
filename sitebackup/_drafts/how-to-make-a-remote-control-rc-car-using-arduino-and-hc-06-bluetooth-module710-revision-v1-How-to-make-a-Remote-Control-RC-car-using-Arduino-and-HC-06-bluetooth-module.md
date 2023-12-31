---
author: George Bantique
date: '2023-03-06T13:00:23+08:00'
guid: https://techtotinker.com/?p=1024
id: 1024
permalink: /?p=1024
title: How to make a Remote Control RC car using Arduino and HC-06 bluetooth module
url: /how-to-make-a-remote-control-rc-car-using-arduino-and-hc-06-bluetooth-module710-revision-v1-How-to-make-a-Remote-Control-RC-car-using-Arduino-and-HC-06-bluetooth-module
---


<div dir="ltr" style="text-align: left;"><div style="clear: both; text-align: left;">Growing from a poor family, I am amazed with remote control toys, robot, and radio. I am curious on how those stuffs work? One time when I was a kid, I found an old rusty but still functional DC motor usually found on battery powered toy car. When I got home, I connected it to a battery and I spent the rest of my day thinking and planning on how I am going to use it for my toy boat.</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">My fascination with how stuff works especially in the field of Electronics is my burning passion, that is why I am still tinkering. So today, we are going to convert this battery-powered toy car to a remote control car.</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">This toy car has 3 DC motors inside, 1 motor controlling the right side front and rear wheels, 1 motor controlling the left side front and rear motor, and another 1 motor controlling the shovel arm rising and lowering.</div><div style="clear: both; text-align: left;"></div>| [![](https://1.bp.blogspot.com/-7PRoZ4OMjfc/Xhmc2rCJMqI/AAAAAAAAABw/IdBP6wEo_WgFPxYFGM657ZhP4WXliC13QCEwYBhgL/s400/Right%2BSide%2BView.JPG)](https://1.bp.blogspot.com/-7PRoZ4OMjfc/Xhmc2rCJMqI/AAAAAAAAABw/IdBP6wEo_WgFPxYFGM657ZhP4WXliC13QCEwYBhgL/s1600/Right%2BSide%2BView.JPG) |
|---|
| tech-to-tinker |

<div style="margin-bottom: .0001pt; margin: 0in;"><u>**Materials:**</u>  
1. Battery powered toy car (our subject).  
2. Arduino Uno (or any other microcontroller available)  
3. Motor driver (here I am using 4 motors and 16 servo Arduino Uno shield of doit.am)  
4. HC-06 Bluetooth module  
5. Some jumper wires  
<u>**Video Demonstration:**</u>  
Visit my youtube channel: [tech-to-tinker](https://www.youtube.com/watch?v=-RlkGn6Xl7U)

**<u>SOURCE CODE:</u>**

</div>**<u>  
</u>**<span style="color: #2e74b5; font-family: "arial" , sans-serif; font-size: 12.0pt;"> </span>  
<span style="color: #2e74b5; font-family: "arial" , sans-serif; font-size: 12.0pt;"> </span>

```
#include <softwareserial.h>

// Arduino UNO Rx, Tx
SoftwareSerial hc06(13,12);

String cmd="";

//Motor A or MOTOR 1
int PWMA = 9; //Speed control
int DIRA = 8; //Direction

//Motor B or MOTOR 2
int PWMB = 6; //Speed control
int DIRB = 7; //Direction

//Motor C or MOTOR 3
int PWMC = 5; //Speed control
int DIRC = 4; //Direction

//Motor D or MOTOR 4gggjh
int PWMD = 3; //Speed control
int DIRD = 2; //Direction


void setup() {
  //Initialize Serial Monitor
  Serial.begin(9600);

  //Initialize Bluetooth Serial Port
  hc06.begin(9600);

  pinMode(PWMA, OUTPUT);
  pinMode(DIRA, OUTPUT);

  pinMode(PWMB, OUTPUT);
  pinMode(DIRB, OUTPUT);

  pinMode(PWMC, OUTPUT);
  pinMode(DIRC, OUTPUT);

  pinMode(PWMD, OUTPUT);
  pinMode(DIRD, OUTPUT);

  // Set default at full stop
  stopped();
  movehalt();

  Serial.print("Setup DONE.");
}


void loop() {
 //Read data from HC06
 while(hc06.available()>0){
   cmd+=(char)hc06.read();
 }

 //Select function with cmd
 if(cmd!=""){
   cmd.trim();  // Remove added LF in transmit
   // We expect Command from bluetooth
   if (cmd.equals("F")) {
       Serial.println("Vehicle Forward");
       forward();
   }else if (cmd.equals("B")){
       Serial.println("Vehicle Backward");
       backward();
   }else if(cmd.equals("L")){
       Serial.println("Vehicle Turn Left");  
       turnleft();
   }else if(cmd.equals("R")){
       Serial.println("Vehicle Turn Right");
       turnright();
   }else if(cmd.equals("S")){
       Serial.println("Vehicle Stop");
       stopped();
   }else if(cmd.equals("U")){
       Serial.println("Crane UP");
       moveup();
   }else if(cmd.equals("D")){
       Serial.println("Crane DOWN");
       movedown();   
   }else if(cmd.equals("H")){
       Serial.println("Crane HALT");
       movehalt();      
   }else{
       Serial.println("Command UNKNOWN");
       stopped();
       movehalt();
   }

   Serial.print("Command recieved:");
   Serial.println(cmd);
   cmd=""; //reset cmd
 }

 delay(100);
}



// Function for the vehicle to move FORWARD
void forward() {
  analogWrite(PWMA, 255);
  digitalWrite (DIRA, HIGH);

  analogWrite(PWMB, 255);
  digitalWrite (DIRB, HIGH);
}

// Function for the vehicle to move BACKWARD
void backward() {
  analogWrite(PWMA, 255);
  digitalWrite (DIRA, LOW);
 
  analogWrite(PWMB, 255);
  digitalWrite (DIRB, LOW);
}

// Function for the vehicle to turn RIGHT
void turnright() {
  analogWrite(PWMA, 255);
  digitalWrite (DIRA, HIGH);

  analogWrite(PWMB, 0);
  digitalWrite (DIRB, HIGH);
}

// Function for the vehicle to turn LEFT
void turnleft() {
  analogWrite(PWMA, 0);
  digitalWrite (DIRA, HIGH);

  analogWrite(PWMB, 255);
  digitalWrite (DIRB, HIGH);
}

// Function for the vehicle to STOP
void stopped() {
  analogWrite(PWMA, 0);
  digitalWrite (DIRA, HIGH);

  analogWrite(PWMB, 0);
  digitalWrite (DIRB, HIGH);
}

// Function for the crane to move UP
void moveup() {
  analogWrite(PWMC, 255);
  digitalWrite (DIRC, HIGH);
}

// Function for the crane to move DOWN
void movedown() {
  analogWrite(PWMC, 255);
  digitalWrite (DIRC, LOW);
}

// Function for the crane to HALT
void movehalt() {
  analogWrite(PWMC, 0);
  digitalWrite (DIRC, HIGH);
}

```


<span style="color: #2e74b5; font-family: "arial" , sans-serif; font-size: 12.0pt;"> </span>

<div style="clear: both; text-align: center;"><u></u><sub></sub><sup></sup></div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">If you find this lesson useful, please consider supporting my blogspot and Youtube channel:</div><div style="clear: both; text-align: left;">1. [tech-to-tinker.blogspot.com](https://tech-to-tinker.blogspot.com/)</div><div style="clear: both; text-align: left;">2. [tech-to-tinker Youtube Channel](https://www.youtube.com/channel/UCRxbBqSgqB4LIm6QxLUVlcg)</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">Thank you. Happy tinkering.</div></div>