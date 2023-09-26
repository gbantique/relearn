---
author: George Bantique
date: '2023-03-06T12:52:15+08:00'
guid: https://techtotinker.com/?p=1015
id: 1015
permalink: /?p=1015
title: What is Arduino Software Serial
url: /what-is-arduino-software-serial702-revision-v1-What-is-Arduino-Software-Serial
---


I feel obligated to post this tutorial after seeing beginners assigning two serial devices to Arduino Uno hardware serial pin which is digital pin 0 (Rx) and digital pin 1 (Tx). Connecting two serial devices results to undefined behavior due to conflicting signals. Arduino Uno, Arduino Nano, and Arduino Mini has only 1 serial port. If you need to connect more serial devices, you have the option to use their big brother; the Arduino Mega which has four serial ports and more pins available. But using the Arduino Mega results to additional cost. The other option is to use the Arduino Software Serial library.

<div></div><div>Arduino Software Serial library basically mimics or copy the behavior of the hardware serial. We are going to use the library of PaulStoffregen from <https://github.com/PaulStoffregen/SoftwareSerial>. With SoftwareSerial, we can assign another set of pins for serial devices.</div><div></div><div>Please download the library then add it by clicking the Sketch &gt; Include Library &gt; Add ZIP Library and select the downloaded SoftwareSerial library.</div><div></div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-WTmUQ9XBYJQ/XoakNoJendI/AAAAAAAAAEk/Gp3oAg-RDHMCTLbrbM2G6gqpCFl6stg4ACLcBGAsYHQ/s1600/AddingLibrary.png)](https://1.bp.blogspot.com/-WTmUQ9XBYJQ/XoakNoJendI/AAAAAAAAAEk/Gp3oAg-RDHMCTLbrbM2G6gqpCFl6stg4ACLcBGAsYHQ/s1600/AddingLibrary.png)</div><div></div><div>**<u>  
</u>**</div><div>**<u>Instruction:</u>**</div><div style="clear: both; text-align: left;">[![](https://1.bp.blogspot.com/-QMNpduV0Qrc/XoatkUMXz2I/AAAAAAAAAEw/nfgQtZD7vnMNMsacdRzwu_KjemmbUHrRgCLcBGAsYHQ/s1600/SoftwareSerial-Schematic.png)](https://1.bp.blogspot.com/-QMNpduV0Qrc/XoatkUMXz2I/AAAAAAAAAEw/nfgQtZD7vnMNMsacdRzwu_KjemmbUHrRgCLcBGAsYHQ/s1600/SoftwareSerial-Schematic.png)</div><div>**<u>  
</u>**</div><div>1. Connect the Arduino Uno pin 2 to USB-Serial converter Tx pin.</div><div> Connect the Arduino Uno pin 3 to USB-Serial converter Rx pin.</div><div>2. Connect the supply voltage to USB-Serial converter.</div><div>3. Upload the sketch provided below in the source code section. This sketch should function as follows:</div><div> * Data sent from hardware serial port should be receive in software serial port.</div><div> * Data sent from software serial port should be receive in hardware serial port.</div><div>4. Please feel free to modify and experiment with the source code to maximize learning.</div><div></div><div>**<u>Video Demonstration:</u>**  
**<u>  
</u>**</div><div><div style="clear: both; text-align: left;"><iframe allowfullscreen="" data-thumbnail-src="https://i.ytimg.com/vi/_LF2ySZ_omk/0.jpg" frameborder="0" height="320" loading="lazy" src="https://www.youtube.com/embed/_LF2ySZ_omk?feature=player_embedded" width="480"></iframe></div></div><div></div><div>**<u>Source Code:</u>**</div><div>```

#include "SoftwareSerial.h"

SoftwareSerial swSerial(2,3);


void setup(){
 //Initialize HARDWARE serial port
 Serial.begin(9600);
 
 //Initialize SOFTWARE serial port
 swSerial.begin(9600);
 
 Serial.print("Setup DONE.");
}

void loop(){

  // If there is data from software serial
  if (swSerial.available()){
    Serial.write(swSerial.read());  // Write it to hardware serial
  }
  
  // If there is data from hardware serial
  if (Serial.available()){
    swSerial.write(Serial.read());  // Write it to software serial
  }   

}

```

</div><div></div><div>If you find this tutorial helpful, please consider supporting my Youtube channel tech-to-tinker. Please leave your comments and suggestions in the comment box.</div><div></div><div>Thank you and have a good day. Happy tinkering.</div>