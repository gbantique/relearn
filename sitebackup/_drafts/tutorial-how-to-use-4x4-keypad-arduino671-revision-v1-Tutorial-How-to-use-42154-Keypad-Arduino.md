---
author: George Bantique
date: '2023-03-06T12:00:23+08:00'
guid: https://techtotinker.com/?p=982
id: 982
permalink: /?p=982
title: 'Tutorial: How to use 4&#215;4 Keypad | Arduino'
url: /tutorial-how-to-use-4x4-keypad-arduino671-revision-v1-Tutorial-How-to-use-42154-Keypad-Arduino
---


Keypad is commonly use in devices like ATM machine, microwave oven, safety vault, security door lock, and many more.

In this tutorial we will focus on the most popular to electronics enthusiasts and tinkerers which is a 4×4 matrix keypad. We will discuss how it works and at the last we will provide an example Arduino sketch so you may be able to tinker with it and use it to your project.

**<u>Circuit Diagram:</u>**  
**<u>  
</u>**

<div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-V0M2FlWl9so/XyAONxtI18I/AAAAAAAAAOI/-_j1s9fF0VojIKsXaStP8ESI5vDDzjfKgCLcBGAsYHQ/w452-h594/Keypad.png)](https://1.bp.blogspot.com/-V0M2FlWl9so/XyAONxtI18I/AAAAAAAAAOI/-_j1s9fF0VojIKsXaStP8ESI5vDDzjfKgCLcBGAsYHQ/s1600/Keypad.png)</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">**<u>Hardware:</u>**</div><div style="clear: both; text-align: left;">**<u>  
</u>**</div><div style="clear: both; text-align: left;">1. Arduino Uno or any compatible Arduino microcontroller board.</div><div style="clear: both; text-align: left;">2. 16×2 LCD for the LCD.</div><div style="clear: both; text-align: left;">3. 4×4 or 3×4 Keypad</div><div style="clear: both; text-align: left;">4. Breadboard, jumper wires, resistor, potentiometer, etc.</div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;"></div><div style="clear: both; text-align: left;">**<u>Instruction:</u>**</div><div style="clear: both; text-align: left;">**<u>  
</u>**</div>1\. Connect the LCD pin 1 VSS to the Arduino GND  
2\. Connect the LCD pin 2 VDD to the Arduino 5V  
3\. Connect the potentiometer pin 1 and pin 3 to Arduino 5V and GND respectively, and the potentiometer center pin 2 to LCD pin 3 VEE/Vo  
4\. Connect the LCD pin 4 RS to the Arduino digital pin 13  
5\. Connect the LCD pin 5 RW to the Arduino GND because we only need to write to the LCD (no reading required)  
6\. Connect the LCD pin 6 En to the Arduino digital pin 12  
7\. Leave the LCD pin 7 D0 to pin 10 D3 not connected because we will use 4-bit mode of the LCD.  
8\. Connect the LCD pin 11 D4 to the Arduino digital pin 11  
9\. Connect the LCD pin 12 D5 to the Arduino digital pin 10  
10\. Connect the LCD pin 13 D6 to the Arduino digital pin 9  
11\. Connect the LCD pin 14 D7 to the Arduino digital pin 8  
12\. Connect the LCD pin 15 Anode to the Arduino 5V via current limiting resistor (220 ohms).  
13\. Connect the LCD pin 16 Cathode to the Arduino GND  
14\. Connect the keypad Row1 (left-most) to the Arduino Uno digital pin D7.  
15\. Connect the keypad Row2 to the Arduino Uno digital pin D6.  
16\. Connect the keypad Row3 to the Arduino Uno digital pin D5.  
17\. Connect the keypad Row4 to the Arduino Uno digital pin D4.  
18\. Connect the keypad Column1 to the Arduino Uno digital pin D3.  
19\. Connect the keypad Column2 to the Arduino Uno digital pin D2.  
20\. Connect the keypad Column3 to the Arduino Uno digital pin D1.  
21\. Connect the keypad Column4 (right-most) to the Arduino Uno digital pin D0.  
22\. Upload the provided sketch making sure that the correct board and serial comm port is selected under the Tools menu of Arduino IDE.  
23\. If it works, modify and experiment with it, and enjoy learning.

**<u>4×4 Keypad Illustration:</u>**  
**<u>  
</u>**

<div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-hwFhmRg-4Oc/XyAVYLAWMxI/AAAAAAAAAOU/fcGP0p7dTqcAol5pMx3TpcyO3pmyWrbmQCLcBGAsYHQ/w410-h307/4x4%2BKeypad%2Billustration.jpg)](https://1.bp.blogspot.com/-hwFhmRg-4Oc/XyAVYLAWMxI/AAAAAAAAAOU/fcGP0p7dTqcAol5pMx3TpcyO3pmyWrbmQCLcBGAsYHQ/s1600/4x4%2BKeypad%2Billustration.jpg)</div>**<u>Video Demonstration:</u>**

<div style="clear: both; text-align: left;"><iframe allowfullscreen="" data-thumbnail-src="https://i.ytimg.com/vi/Vkt5h3nyIGU/0.jpg" frameborder="0" height="260" loading="lazy" src="https://www.youtube.com/embed/Vkt5h3nyIGU?feature=player_embedded" width="480"></iframe></div>I hope you find this tutorial as helpful. Please consider supporting me in Youtube by Subscribing.

Thank you and have a good day.

**<u>Source Code:</u>**  
**<u>  
</u>1. 4×4 Keypad using no library for keypad**

```
/*
 * 4x4 Keypad to Arduino Uno with LCD display
 * Author: George Bantique, TechToTinker (July 28, 2020)
 * 
 *  - In this tutorial, we need the following:
 *    1. Arduino Uno or any compatible Arduino board
 *    2. 16x2 LCD for the display
 *    3. 4x4 keypad module
 *    
 */

#include "LiquidCrystal.h"
LiquidCrystal lcd(13, 12, 11, 10, 9, 8);

#define ROW   4   // we have 4 rows
#define COL   4   // by 4 columns of keys
char keys[ROW][COL] =
{
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};
uint8_t row_line[ROW] = {7, 6, 5, 4};
uint8_t col_line[COL] = {3, 2, 1, 0};
char keyPressed = ' ';

void setup() {

  // initialized the LCD as 16x2
  lcd.begin(16, 2); 
  
  // set the row lines as output then
  // initialized all the line as HIGH
  for (int r = 0; r < ROW; r++) {
    pinMode(row_line[r], OUTPUT);
    digitalWrite(row_line[r], HIGH);
  }

  // set the column lines as input,
  // use INPUT_PULLUP to avoid additional hardware 
  // by taking advantage of the internal pullup resistors
  for (int c = 0; c < COL; c++) {
    pinMode(col_line[c], INPUT_PULLUP);
  }
  
}

void loop() {
  
  // Process the key press here
  keyPressed = getKey();
  lcd.setCursor(0,0);                   // if the key is press
  lcd.print(keyPressed);                // display it to the LCD

  // Do other stuff
  
}

char getKey() {
  char key_temp = keyPressed;
  
  // scan the keypad
  for (int r = 0; r < ROW; r++) {             // for traversing the row lines
    digitalWrite(row_line[r], LOW);           // enable the specific row line (one by one)
    for (int c = 0; c < COL; c++) {           // for traversing the column lines
      if (digitalRead(col_line[c]) == LOW) {  // check if the specific column is press (one by one)
         key_temp = keys[r][c];               // store it to key_temp
      }                                       // if no key is press, check next column
    }
    digitalWrite(row_line[r], HIGH);          // disable the specific row line, then next row lines
  }

  return key_temp;
}

```

**<u>2. 4×4 Keypad using Keypad library:</u>**

```

#include "Keypad.h"
#include "LiquidCrystal.h"
LiquidCrystal lcd(13, 12, 11, 10, 9, 8);

#define ROW   4
#define COL   4
char key[ROW][COL] =
{
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};
uint8_t row_line[ROW] = {7, 6, 5, 4};
uint8_t col_line[COL] = {3, 2, 1, 0};
Keypad keypad = Keypad( makeKeymap(key), row_line, col_line, ROW, COL );

void setup() {
  lcd.begin(16, 2); // initialized the LCD as 16x2
}

void loop() {
  char keypress = keypad.getKey();
  if (keypress){
    lcd.clear();
    lcd.print(keypress);
  }
}

```

**<u>3. 4×4 Keypad for password entry:</u>**

```

#include <keypad .h="">
#include 	<liquidcrystal .h="">
LiquidCrystal lcd(13, 12, 11, 10, 9, 8);

#define ROW   4
#define COL   4
char key[ROW][COL] =
{
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};
uint8_t row_line[ROW] = {7, 6, 5, 4};
uint8_t col_line[COL] = {3, 2, 1, 0};
Keypad keypad = Keypad( makeKeymap(key), row_line, col_line, ROW, COL );
const String password = "1234"; // change your password here
String input_password;

void setup() {

  lcd.begin(16, 2); // initialized the LCD as 16x2
  lcd.setCursor(0,0);
  lcd.print("-Enter password-");
  lcd.setCursor(0,1);
  lcd.print("Password: ");
}

void loop() {
  char key = keypad.getKey();
  
  if (key){

    if(key == '*') {
      input_password = ""; // reset imput password
      lcd.clear();
      lcd.print("Enter password");
      lcd.setCursor(0,1);
      lcd.print("Password: ");
    } else if(key == '#') {
      if(password == input_password) {
        lcd.clear();
        lcd.print("Granted, welcome");
        // DO YOUR WORK HERE
        
      } else {
        lcd.clear();
        lcd.print("Access denied");
      }

      input_password = ""; // reset imput password
    } else {
      input_password += key; // append new character to input password string
      lcd.setCursor(10, 1);
      lcd.print(input_password);
    }
  }
}</liquidcrystal></keypad>
```

**<u>4. 4×4 Keypad for password entry (password replaced with \*):</u>**

```

#include "Keypad.h"
#include "LiquidCrystal.h"
LiquidCrystal lcd(13, 12, 11, 10, 9, 8);

#define ROW   4
#define COL   4
char key[ROW][COL] =
{
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};
uint8_t row_line[ROW] = {7, 6, 5, 4};
uint8_t col_line[COL] = {3, 2, 1, 0};
Keypad keypad = Keypad( makeKeymap(key), row_line, col_line, ROW, COL );
const String password = "1234"; // change your password here
String input_password;
int char_cnt = 0;

void setup() {

  lcd.begin(16, 2); // initialized the LCD as 16x2
  lcd.setCursor(0,0);
  lcd.print("-Enter password-");
  lcd.setCursor(0,1);
  lcd.print("Password: ");
}

void loop() {

  char key = keypad.getKey();
  
  if (key){

    if(key == '*') {
      input_password = ""; // reset imput password
      char_cnt = 0;
      lcd.clear();
      lcd.print("Enter password");
      lcd.setCursor(0,1);
      lcd.print("Password: ");
    } else if(key == '#') {
      if(password == input_password) {
        lcd.clear();
        lcd.print("Granted, welcome");
        // DO YOUR WORK HERE
        
      } else {
        lcd.clear();
        lcd.print("Access denied");
      }

      input_password = ""; // reset imput password
    } else {
      input_password += key; // append new character to input password string
      char_cnt++;
      lcd.setCursor(10, 1);
      for (int i = 0; i < char_cnt; i++) {
        lcd.print("*");
      };
    }
  }
}

```