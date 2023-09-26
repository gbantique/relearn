---
author: George Bantique
date: '2023-03-06T12:39:33+08:00'
guid: https://techtotinker.com/?p=996
id: 996
permalink: /?p=996
title: 'Tutorial: Arduino GPIO | How to use Arduino Pins'
url: /tutorial-arduino-gpio-how-to-use-arduino-pins685-revision-v1-Tutorial-Arduino-GPIO-How-to-use-Arduino-Pins
---


<div></div><div style="margin-left: 1em; margin-right: 1em;"></div><div><div>When I am starting to learn the Arduino microcontroller, I started searching on how I am able to use it immediately because I believe that the best way of learning something is the experience of using it.</div><div></div><div>In this tutorial, we will learn the basics of Arduino GPIO or the General Purpose Input Output or in the simplest term Arduino pins.</div></div><div></div><div>GPIO are physical pins found in microcontrollers which can be configured either as input or output.</div><div></div><div>When a certain pin is configured as input, it can be used to read the state of a switch or the value of the sensor.</div><div></div><div>When a certain pin is configured as output, it can be used to write such as lights ON or lights OFF or control the rotation of DC motors. </div><div></div><div></div><div>Now let us discuss functions commonly used in Arduino:</div><div></div><div style="clear: both; text-align: center;">[![](https://1.bp.blogspot.com/-zESOMgrLSOs/XvW6xixKVXI/AAAAAAAAB7c/pEbVHHhhSEU2EnJYd7mSi7pLyRHdQcS1ACK4BGAsYHg/pinMode.png)](https://1.bp.blogspot.com/-zESOMgrLSOs/XvW6xixKVXI/AAAAAAAAB7c/pEbVHHhhSEU2EnJYd7mSi7pLyRHdQcS1ACK4BGAsYHg/s281/pinMode.png)</div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div>pinMode(PIN, MODE)</div><div>This function is use to set the direction of the PIN either as input or output. </div><div>MODE could be OUTPUT, INPUT, or INPUT_PULLUP</div><div></div><div>Example:</div><div>pinMode(13, OUTPUT)</div><div>This configures digital pin 13 as output.</div><div></div><div></div><div></div><div>**OUTPUT** mode configures the PIN output which can drive LEDs, LCD, control signals, and etcetera.</div><div></div><div>**INPUT** mode configures the PIN as input which can read state of switch or read sensor data. An external pull-down or pull-up resistor is needed when the input pin has unknown state which also known as tri-state.</div><div></div><div>**INPUT\_PULLUP** mode configures the PIN as input and also connects the PIN to a pullup resistor internally. This is especially useful when external pullup or pulldown resistor is not available.</div><div></div><div>**Pull-up Resistor** – is a resistor connected between the VCC and the specific pin. Pullup resistor makes the specific pin to a default value of logic HIGH. The RESET button of Arduino Uno is configured using a pullup resistor, so the default value is logic HIGH but when the RESET button is pressed, the RESET button will be connected to the GND which has logic LOW value.</div><div></div><div>*(insert pullup vs pulldown resistor)*</div><div></div><div>**Pull-down Resistor** – is a resistor connected between the GND and the specific pin. It function similar to pull-up resistor but opposite logic functions.</div><div></div><div>*(show the benefit of pull-up resistor by showing the state of a pin when floating and when pullup.)*</div><div></div><div></div><div style="clear: both; text-align: center;">[  ](https://1.bp.blogspot.com/-YpMDtLPm4Ko/XvW68Wr7NMI/AAAAAAAAB7o/u3U9fXx-w6kF1DYFXtZ72RUyKWM-Qnm2ACK4BGAsYHg/s423/digitalWrite.png)[![](https://1.bp.blogspot.com/-YpMDtLPm4Ko/XvW68Wr7NMI/AAAAAAAAB7o/u3U9fXx-w6kF1DYFXtZ72RUyKWM-Qnm2ACK4BGAsYHg/w400-h65/digitalWrite.png)](https://1.bp.blogspot.com/-YpMDtLPm4Ko/XvW68Wr7NMI/AAAAAAAAB7o/u3U9fXx-w6kF1DYFXtZ72RUyKWM-Qnm2ACK4BGAsYHg/s423/digitalWrite.png)</div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div>digitalWrite(PIN, HIGH/LOW)</div><div>This function drives the PIN either a logic HIGH or logic LOW.</div><div></div><div></div><div></div><div></div><div style="clear: both; text-align: center;">[![](https://1.bp.blogspot.com/-JvaGNbxKioQ/XvW7D-kReJI/AAAAAAAAB74/m28vj3SnJXYjZoBFSol02Z-2atXm3xRsQCK4BGAsYHg/w400-h65/digitalRead.png)](https://1.bp.blogspot.com/-JvaGNbxKioQ/XvW7D-kReJI/AAAAAAAAB74/m28vj3SnJXYjZoBFSol02Z-2atXm3xRsQCK4BGAsYHg/s423/digitalRead.png)</div><div></div><div></div><div></div><div></div><div></div><div></div><div>digitalRead(PIN)</div><div>This function reads the state of the PIN and return either a logic HIGH or logic LOW.</div><div></div><div></div><div></div><div></div><div style="clear: both; text-align: center;">[![](https://1.bp.blogspot.com/-3wXD5CyHMRg/XvW7KRksEbI/AAAAAAAAB8E/1I301U1sCi4xmcL5mSBCOupzoLCRdFRqgCK4BGAsYHg/w500-h195/analogWrite.png)](https://1.bp.blogspot.com/-3wXD5CyHMRg/XvW7KRksEbI/AAAAAAAAB8E/1I301U1sCi4xmcL5mSBCOupzoLCRdFRqgCK4BGAsYHg/s652/analogWrite.png)</div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div>analogWrite(PIN, VALUE)</div><div>This function writes an analog value (in PWM square wave) to a pin.</div><div></div><div></div><div></div><div></div><div style="clear: both; text-align: center;">[![](https://1.bp.blogspot.com/-pFJ_x5IHcRs/XvW7RK6rmzI/AAAAAAAAB8Q/DL0sHT9I5gsW8JZl975mmqj-ycbawjPFwCK4BGAsYHg/w500-h195/analogRead.png)](https://1.bp.blogspot.com/-pFJ_x5IHcRs/XvW7RK6rmzI/AAAAAAAAB8Q/DL0sHT9I5gsW8JZl975mmqj-ycbawjPFwCK4BGAsYHg/s652/analogRead.png)</div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div>analogRead(PIN)</div><div>This function reads an analog value from a specified analog pins.</div><div></div><div></div><div>**<u>Circuit Diagram:</u>**  
*(insert circuit diagram here)*  
**<u>  
</u><u>  
</u><u>Video Demonstration:</u>**<div style="clear: both; text-align: left;"><iframe allowfullscreen="" data-thumbnail-src="https://i.ytimg.com/vi/3nAjmWvTSGU/0.jpg" frameborder="0" height="280" loading="lazy" src="https://www.youtube.com/embed/3nAjmWvTSGU?feature=player_embedded" width="480"></iframe></div></div><div></div><div></div><div>**<u>Source Code:</u>**</div><div>```

#include "Servo.h"
#include "LiquidCrystal.h"

#define RED_LED_PIN 11
#define YEL_LED_PIN 12
#define SERVO_PIN   A5

#define JS_YL_PIN   A1  // Joystick vertical position in the left
#define JS_XL_PIN   A2  // Joystick horizontal position in the left
#define JS_YR_PIN   A3  // Joystick vertical position in the right
#define JS_XR_PIN   A4  // Joystick horizontal position in the right

#define JS_SL_PIN   3   // Joystick tactile switch in the left
#define JS_SR_PIN   2   // Joystick tactile switch in the right

Servo myservo;
LiquidCrystal lcd(8, 9, 4, 5, 6, 7);

void setup() {

  // Setting of the pin direction
  pinMode(RED_LED_PIN, OUTPUT);
  pinMode(YEL_LED_PIN, OUTPUT);
  pinMode(SERVO_PIN, OUTPUT);

  pinMode(JS_YL_PIN, INPUT);
  pinMode(JS_XL_PIN, INPUT);
  pinMode(JS_YR_PIN, INPUT);
  pinMode(JS_XR_PIN, INPUT);

  pinMode(JS_SL_PIN, INPUT_PULLUP);
  pinMode(JS_SR_PIN, INPUT_PULLUP);

  // ##########################
  myservo.attach(SERVO_PIN);  // Attach the servo to the assigned pin
  lcd.begin(16,2);            // Initialize the LCD as 16 columns and 2 rows
  lcd.clear();                // Clear any garbage in the LCD EPROM
  
}

void loop() {

  //#######################################
  // Using the horizontal (X) analog value
  int js_xr_value = analogRead(JS_XR_PIN);
  byte servo_angle = map (js_xr_value, 0, 1023, 0, 180);
  myservo.write(servo_angle);
  lcd.setCursor(0,0);
  lcd.print("Servo angle: ");
  if (servo_angle < 100) {  // if value is 0 to 99, add a space before printing
    lcd.print(" ");         // this is to make sure that 3 digits is occupied always
  }
  lcd.print(servo_angle);

  //#######################################
  // Using the vertical (Y) analog value
  int js_yr_value = analogRead(JS_YR_PIN);
  byte led_pwm = map ( js_yr_value, 0, 1023, 255, 0);
  analogWrite(RED_LED_PIN, led_pwm);

  lcd.setCursor(0,1);
  lcd.print("LED pwm: ");
  if (led_pwm < 100) {  // if value is 0 to 99, add space before printing
    lcd.print(" ");     // this is to make sure that 3 digits is occupied always.
  }
  lcd.print(led_pwm);

  //#################################################
  // Using the push button switch (S) digital value
  digitalWrite(YEL_LED_PIN, digitalRead(JS_SR_PIN));
  
}

```

</div><div></div><div></div><div></div><div></div><div>That’s all for now, I hope this tutorial helps. Please kindly LIKE and SHARE this video to your friends who may benefit from it.</div><div></div><div>SUBSCRIBE and leave your comments and suggestions in the comment box.</div><div></div><div>Thank you and have a good day.</div><div></div><div>George signing off, bye.</div><div></div>