---
author: George Bantique
date: '2023-03-06T12:42:19+08:00'
guid: https://techtotinker.com/?p=1000
id: 1000
permalink: /?p=1000
title: How to Use Arduino Interrupts
url: /how-to-use-arduino-interrupts689-revision-v1-How-to-Use-Arduino-Interrupts
---


<div>Interrupt is extremely useful when dealing with all kinds of processes that needs attention at unexpected time or when expecting a certain event or signal at indefinite and unknown time.</div><div></div><div>For example you send a chat message to your crush. You are very excited but also nervous. So every second you are checking your messenger if she already replied. This is tiring and it takes most of your time and energy. You will not be able to do other important tasks and responsibilities. In microcontroller jargon, it is called **<u>polling</u>**.</div><div></div><div>What you can do is to turn the notification and go on with your normal life, do your chores and routines. When someone send you a chat message, you will hear a notification sound. That is the time for you to check your mobile phone.</div><div></div><div>The notification sound serves as **<u>interrupt</u>** call and checking of mobile phone is the **<u>Interrupt Service Routine</u>**. Efficient, right?</div><div></div><div><div>**So how does the interrupt really works?**</div><div></div><div>When an event occurs which triggers the interrupt, the current process will be put on hold and the ISR or Interrupt Service Routine will be executed. All other interrupt will be disabled.</div><div></div><div>When the ISR finish executing, the previous process before the interrupt will continue and all other interrupts will be re-enabled.</div></div><div></div><div><div>It is highly recommended to make the ISR as short as possible because an ISR that takes lets say 5 seconds to executes means a 5 seconds of uncertainty, WHY? Because all other processes are disabled during ISR execution, right? As a rule of thumb, just set variable flag or save a sensor value inside the ISR and do the other remaining processes inside the main loop.</div><div></div><div>**There are two kinds of interrupt:**</div><div>**Internal interrupt – or software interrupt** is triggered by internal peripherals like timers.</div><div></div><div>**External interrupt – or hardware interrupt** is triggered by an external device.</div><div></div><div>**External Interrupt pins:**  
  
</div><div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-oH8RcqJfVX8/XutD-q2ERjI/AAAAAAAAAJs/tOBeW0kiKfs1qCULMGsjvCVBYt-2LMsxACLcBGAsYHQ/s400/Arduino%2BInterrupt%2BPins.png)](https://1.bp.blogspot.com/-oH8RcqJfVX8/XutD-q2ERjI/AAAAAAAAAJs/tOBeW0kiKfs1qCULMGsjvCVBYt-2LMsxACLcBGAsYHQ/s1600/Arduino%2BInterrupt%2BPins.png)</div></div><div></div><div></div><div>**How to set interrupt:**</div></div><div></div><div><div>**attachInterrupt(INTERRUPT\_NUM, ISR\_FUNCTION, TRIGGER\_MODE)**</div><div></div><div>**INTERRUPT\_NUM** – is the interrupt number associated to interrupt pin.</div><div></div><div>**ISR\_FUNCTION** – is the ISR function to be executed when the interrupt is detected</div><div></div><div>**TRIGGER\_MODE** – is the type of detection of interrupt. It could be a RISING, FALLING, CHANGE, or LOW. </div><div>**RISING** – triggers when the pin goes from low to high</div><div>**FALLING** – triggers when the pin goes from high to low</div><div>**CHANGE** – triggers whenever the pin changes value</div><div>**LOW** – triggers when the pin is low.</div><div></div><div>Keep in mind the following rules when using interrupt:</div>1. ***Keep it short.*** As I already discussed earlier.
2. ***Do not use delay*** functions as per the first rule said, keep interrupt short while delay function prolong the execution of the function.
3. ***Do not use serial.*** Serial use and trigger interrupts.
4. ***Make global variable volatile.*** This is as per C standard, variable should be declared volatile when there is chance that the variable will be use outside the normal execution flow (outside the main loop, which is the ISR function). Volatile disables the variable optimization.

<div></div></div><div>**<u>Video Demonstration:</u>**</div><div><div style="clear: both; text-align: left;"><iframe allowfullscreen="" data-thumbnail-src="https://i.ytimg.com/vi/lD7o0lySAIs/0.jpg" frameborder="0" height="280" loading="lazy" src="https://www.youtube.com/embed/lD7o0lySAIs?feature=player_embedded" width="480"></iframe></div></div><div></div><div>**<u>Source Code:</u>**</div><div>```

#define INTERRUPT_PIN 2
#define INDICATOR_PIN 3

volatile int LED_state = 1;

void setup() {
  // put your setup code here, to run once:

  pinMode(INTERRUPT_PIN, INPUT);
  pinMode(INDICATOR_PIN, OUTPUT);

  attachInterrupt(digitalPinToInterrupt(INTERRUPT_PIN), Button_ISR, RISING);
}

void loop() {
  // put your main code here, to run repeatedly:
}

void Button_ISR () {
  LED_state = !LED_state;
  digitalWrite(INDICATOR_PIN, LED_state);
}

```

</div><div>If you found this tutorial useful, kindly share this to your friends.</div><div></div><div>Thank you and have a good day.</div><div></div><div></div>