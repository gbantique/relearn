---
author: George Bantique
date: '2023-03-05T14:48:26+08:00'
guid: https://techtotinker.com/?p=899
id: 899
permalink: /?p=899
title: '001 &#8211; ESP32 MicroPython: What is MicroPython'
url: /001-esp32-micropython-what-is-micropython658-revision-v1-001-8211-ESP32-MicroPython-What-is-MicroPython
---


 MicroPython is essentially a Python created for microcontrollers.

MicroPython is a light-weight and compact implementation of Python Programming Language. It is stripped and trimmed down in order to work with embedded devices which basically has limited resources.

And with the help of machine modules, we can easily communicate with the hardware input and output devices.

<div style="clear: both; text-align: center;">[![](https://1.bp.blogspot.com/-4sGkpapRT_s/X1XVsdMSalI/AAAAAAAACBk/wduFHnYTEEMVR6d1BeCJtMQQMhGYq7dugCLcBGAsYHQ/w500-h250/MicroPythonVSCpp.png)](https://1.bp.blogspot.com/-4sGkpapRT_s/X1XVsdMSalI/AAAAAAAACBk/wduFHnYTEEMVR6d1BeCJtMQQMhGYq7dugCLcBGAsYHQ/s718/MicroPythonVSCpp.png)</div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div><div>So how does the MicroPython works? as you may asked.</div><div></div><div></div><div>1. In MicroPython, the source code is created and/or edited in the computer using an Editor like Thonny Python which is the same as in Arduino traditional programming using an Arduino IDE.</div><div></div><div>2. In MicroPython, the code is stored inside the microcontrollers flash memory while in Arduino the code is stored in your computer.</div><div></div><div>3. In MicroPython, the source code compilation happens inside the microcontroller or much better to call it source code interpretation. The Python interpreter converts the source code into byte code and store it in the Random Access Memory and consequently executes the program which all happens in runtime while in Arduino, the source code compilation happens in computer. The Cpp compiler converts code in to machine code. The machine code is transfered to microcontroller through serial interface and written to flash memory. Flash memory is overwritten everytime source code is uploaded.</div><div></div><div>4. In MicroPython, the time it takes from code compilation until execution is very minimal and virtually executes immediately. And because MicroPython is an interpreted language, when you modified your source code it doesn’t need to compile everything it just need to modify a portion according to change in source code while in Arduino, everytime you modified your source code, you also need to recompile everything in order for the modification to take effect.</div><div></div><div>But since MicroPython is interpreted during the execution of the code, speed performance is not efficient while traditional programming has the advantage in using machine code level.</div><div></div><div>Another cool feature of MicroPython is the addition of REPL. REPL stands for Read-Evaluate-Print-Loop. REPL allows you to connect to development board and be able to test a code without any need of compiling.</div><div></div><div>REPL take advantage of the UART serial interface which is commonly included to almost all embedded devices available in the market. **Video Demonstration:**

</div><div style="clear: both; text-align: left;"><iframe allowfullscreen="" height="260" loading="lazy" src="https://www.youtube.com/embed/eHf76lzPbdU" width="480" youtube-src-=""></iframe>Thank you.

</div></div><div></div>