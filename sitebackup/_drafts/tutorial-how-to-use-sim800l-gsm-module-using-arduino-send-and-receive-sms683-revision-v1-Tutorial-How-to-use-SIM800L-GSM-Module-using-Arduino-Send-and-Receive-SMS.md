---
author: George Bantique
date: '2023-03-06T12:38:27+08:00'
guid: https://techtotinker.com/?p=994
id: 994
permalink: /?p=994
title: 'Tutorial: How to use SIM800L GSM Module using Arduino | Send and Receive SMS'
url: /tutorial-how-to-use-sim800l-gsm-module-using-arduino-send-and-receive-sms683-revision-v1-Tutorial-How-to-use-SIM800L-GSM-Module-using-Arduino-Send-and-Receive-SMS
---


There are actually a lot of GSM module available in the market but I personally recommend and prefer SIM800L due to its size and simplicity. It has a small footprint so it occupies a very small space. This is because SIM800L only provides the basic core of GSM. Meaning to say, you need to provide additional circuitry to use it but don’t worry I will show you the simplest way to use it.

**<u>To send SMS:</u>**  
 1. Send **AT+CMGF=1 &lt;CR&gt;&lt;LF&gt;**  
 – this is to configure the GSM module to text mode  
 **CR** – Cariage Return, ASCII character 13 or r  
 **LF** – Line Feed, ASCII character 10 or n  
 2. Sent **AT+CMGS=”+ZZxxxxxxxxxx”&lt;CR&gt;&lt;LF&gt;**  
 – this is for entering the sim number  
 **ZZ** – is the country code  
 **xx** – is the 10-digit mobile number  
 **CR** – Cariage Return, ASCII character 13 or r  
 **LF** – Line Feed, ASCII character 10 or n  
 3. Wait for the ‘&gt;’ reply from SIM800L before sending the next  
 4. When the GSM replied with ‘&gt;’, this is the time to send your message such as:  
 **“Hello from SIM800L”&lt;CR&gt;&lt;LF&gt;**  
 **Hello from SIM800L** – is the SMS you want to send.  
 **CR** – Cariage Return, ASCII character 13 or r  
 **LF** – Line Feed, ASCII character 10 or n  
 5. Send a **CTRL+Z** character or **ASCII character 26**.

**<u>To receive SMS:</u>**  
 1. Send **AT+CMGF=1 &lt;CR&gt;&lt;LF&gt;**  
 – this is to configure the GSM module to text mode  
 **CR** – Cariage Return, ASCII character 13 or r  
 **LF** – Line Feed, ASCII character 10 or n  
 2. Send **AT+CNMI=1,2,0,0,0&lt;CR&gt;&lt;LF&gt;**  
 – this is to configure the GSM module to forward via serial when SMS is receive.  
 **CR** – Cariage Return, ASCII character 13 or r  
 **LF** – Line Feed, ASCII character 10 or n

**<u>Video Demonstration:</u>**  
**<u>  
</u>**

<div style="clear: both; text-align: left;"><iframe allowfullscreen="" data-thumbnail-src="https://i.ytimg.com/vi/Xz73S-mrv3Y/0.jpg" frameborder="0" height="280" loading="lazy" src="https://www.youtube.com/embed/Xz73S-mrv3Y?feature=player_embedded" width="480"></iframe></div>**<u>Source Code:</u>**

```

#include "SoftwareSerial.h"

SoftwareSerial mySerial(2, 3);

String cmd = "";

void setup()
{
  mySerial.begin(9600);
  Serial.begin(9600);
  Serial.println("Initializing...");
  delay(1000);

  mySerial.println("AT");                 // Sends an ATTENTION command, reply should be OK
  updateSerial();
  mySerial.println("AT+CMGF=1");          // Configuration for sending SMS
  updateSerial();
  mySerial.println("AT+CNMI=1,2,0,0,0");  // Configuration for receiving SMS
  updateSerial();
}

void loop()
{
  updateSerial();
}

void updateSerial()
{
  delay(500);
  while (Serial.available()) 
  {

    cmd+=(char)Serial.read();
 
    if(cmd!=""){
      cmd.trim();  // Remove added LF in transmit
      if (cmd.equals("S")) {
        sendSMS();
      } else {
        mySerial.print(cmd);
        mySerial.println("");
      }
    }
  }
  
  while(mySerial.available()) 
  {
    Serial.write(mySerial.read());//Forward what Software Serial received to Serial Port
  }
}

void sendSMS(){
  mySerial.println("AT+CMGF=1");
  delay(500);
  mySerial.println("AT+CMGS="+639175291539"r");
  delay(500);
  mySerial.print("Hi! TechToTinker!");
  delay(500);
  mySerial.write(26);
}

```

If you find article helpful, please kindly Subscribe to my channel.

Thank you and have a good day.