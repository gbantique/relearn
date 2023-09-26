---
author: George Bantique
date: '2023-03-05T14:10:06+08:00'
guid: https://techtotinker.com/?p=848
id: 848
permalink: /?p=848
title: '002 &#8211; ESP32 MicroPython: Fundamental'
url: /609-revision-v1-002-8211-ESP32-MicroPython-Fundamental
---


<div style="text-align: left;">In this article, we will discuss some basics of MicroPython. This will not teach you the complete Python fundamentals but will help you to easily get started in using MicroPython – a Python for Microcontrollers.</div><div style="text-align: left;"> </div><div style="text-align: left;">There are 3 ways to test a MicroPython program using Thonny IDE – a beginner-friendly Python IDE:</div><div style="text-align: left;">1. Through a shell terminal using the REPL.</div><div style="text-align: left;">2. Through the main editor.</div><div style="text-align: left;">3. Through MicroPython device itself by writing it to ESP32 MicroPython root directory by storing it as main.py.  
   
Now lets tackle some fundamental, these are some of the basics we need to get started with our uP journey.   
   
disclaimer:   
this tutorials aimed to teach uP beginners to easily jump in and start coding for microcontrollers and if you need to deepen your knowledge, there are a lot of information available in internet.   
\#1. modules, these are the modules available included in uP firmware,   
// show the help(‘modules’)   
machine and time modules are commonly use   
uP modules works similar to Arduino libraries   
and to use it, we need to import the module lets say import machine   
// show   
machine module enable us to access the hardware pins or GPIO as an input or as an output   
\#2. variables, one of the feature of uP is we dont need to declare data types. a variable lets say x could be an integer   
// show x = 3, then print x   
it could become a float   
// show x = 3.14, then print   
it could become a string variable   
// show x = ‘hello’   
or anything you could think of   
\#3. function:   
function in Python is easily created by using a def keyword   
lets create a function lets say to print a certain nick_name when called   
first, lets create the function def print_nick_name and lets pass a single argument which is the nick name and hit enter   
// def print_nick_name(nick_name):   
notice the use of colon character and the indentation, in MicroPython there is no curly braces like in C programming language. The indention dictates and tells the MicroPython interpreter that a certain statement in code is included in the function   
   
now, inside the function, lets print a string “My name is” and lets append the input argument nick_name   
// print(‘My nick name is’ + nick_name)   
and then to exit the indention, lets hit the enter 2 times.   
now we can call the print_nick_name function and lets pass a nick name like John   
// print_nick_name(‘John’)   
using functions in any programming language, makes the code speak by itself. It means the code is easy to understand especially to the eyes of others who will use the source code.   
and we can call again the function print_nick_name and pass a different argument, a different nick name lets say Dave.   
// print_nick_name(‘Dave’)   
another advantage of using function is reusability. Reusability means we can reuse a function as many times as we like just like   
print_nick_name(‘TechToTinker’)   
Function could also return a value, lets say def sum(a, b):   
return a+b   
lets use the sum function and lets pass an argument and let save the return value to x   
x = sum(3, 4)   
now, lets see the value of x   
print(x)   
notice that print is also a function that prints an input parameters   
   
\#4. Conditional statements and code flow control. There are 3 commonly use conditional statements that we should be familiar with.   
\#1 if statements,   
lets say you have an x = 3   
// x = 3   
and you want to check if x = 2   
// if (x == 2) :   
// print (‘x is 2’)   
// else:   
// print(‘x is ‘ + x)   
and if you need to check additional condition, lets say if x is 0 print that’s zero. we can add   
elif (x == 0):   
print(‘that’s zero’)   
\#2 while loop   
commonly use while loop statement is, while True: which creates a continous loop in eternal.   
another while loop statements could be lets say, I want to print Hello MicroPython string 5 times   
x = 0   
while (x &lt; 5):   
print(‘Hello MicroPyhon’)   
x += 1   
\#3 for loop   
let say you want to print a number from 0 to 10, that is   
for x in range (0, 11):   
print(x)   
what if you need to increment by 2, that is   
for x in range(0, 11, 2):   
print(x)   
what if you need to decrement by 2, that is   
for x in range(10, -1, -2):   
print(x)   
Simple, right?   
In the next video, we will explore the General Purpose Input and Output. That should enable us to drive an output device such as an LED and read an input device like a switch.   
In any ways you have some confusion, you may write it out in the comment box provided.   
And if you enjoy this video, please give me thumbs up by clicking the LIKE button   
and please share this to your friends by doing that it can reach more people who might benefit from this   
and please do SUBSCRIBE for more videos like this.   
You may also like to checkout the companion blog post for this tutorial at:   
TechToTinker.blogspot.com   
I am posting more details in that blog post for your reference.   
Thank you for watching, and I will see you in the next video.   
Stay safe everyone….   
Bye </div>