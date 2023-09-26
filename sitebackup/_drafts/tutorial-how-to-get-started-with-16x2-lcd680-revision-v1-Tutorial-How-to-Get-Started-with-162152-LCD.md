---
author: George Bantique
date: '2023-03-06T12:36:10+08:00'
guid: https://techtotinker.com/?p=991
id: 991
permalink: /?p=991
title: 'Tutorial: How to Get Started with 16&#215;2 LCD'
url: /tutorial-how-to-get-started-with-16x2-lcd680-revision-v1-Tutorial-How-to-Get-Started-with-162152-LCD
---


<div>The 16×2 LCD module is very popular to electronics enthusiast and tinkerers because of its availability and its easy to use. Almost all 16×2 LCD are controlled using Hitachi HD44780 LCD controller.</div><div></div><div>5×8 dot matrix</div><div></div><div>could work with 8-bit or 4-bit mode</div><div></div><div>operating voltage is 4.7 to 5.3 V</div><div></div><div>alpha numeric alphabets and numbers</div><div></div><div>16×2 or 16 characters x 2 lines total of 32 characters </div><div></div><div></div><div>**<u>Pin Assignment:</u>**</div><div>**<u>  
</u>**</div><div>| P**in Number** | **Pin Name** | **Description** |
|---|---|---|
| 1 | VSS | Ground voltage |
| 2 | VDD | +5V supply voltage |
| 3 | VEE | Use to control the contrast of the LCD |
| 4 | RS | 0: Data register    1: Command register |
| 5 | R/W | 0: Write operation    1: Read operation |
| 6 | En | 0:     1: to execute LCD Read / Write |
| 7 | D0 | Data line 0 (not use in 4-bit mode) |
| 8 | D1 | Data line 1 (not use in 4-bit mode) |
| 9 | D2 | Data line 2 (not use in 4-bit mode) |
| 10 | D3 | Data line 3 (not use in 4-bit mode) |
| 11 | D4 | Data line 4 |
| 12 | D5 | Data line 5 |
| 13 | D6 | Data line 6 |
| 14 | D7 | Data line 7 |
| 15 | A | Anode, backlight positive terminal |
| 16 | K | Cathode, backlight negative terminal |

<div style="text-align: center;"></div></div><div style="text-align: center;"></div><div style="text-align: left;">**<u>Command Registers:</u>**</div><div style="text-align: left;"></div><div style="text-align: left;">| **Hex code** | **Description** |
|---|---|
| 0x01 | Clear the LCD |
| 0x02 | Return home |
| 0x04 | Decrement cursor position |
| 0x06 | Increment cursor position |
| 0x05 | Shift display right |
| 0x07 | Shift display left |
| 0x0A | Display Off, Cursor Off |
| 0x0C | Display Off, Cursor On |
| 0x0E | Cursor blinking, Display On |
| 0x0F | Cursor blinking, Display On |
| 0x10 | Shift cursor position to left |
| 0x14 | Shift cursor position to right |
| 0x18 | Shift the entire display to left |
| 0x1C | Shift the entire display to right |
| 0x80 | Force cursor to beginning of first line |
| 0xC0 | Force cursor to beginning of second line |

</div><div style="text-align: left;"></div><div style="text-align: left;">We don’t need to re-invent the wheel, so we will use the commonly use LiquidCrystal Arduino Library.</div><div style="text-align: left;"></div><div style="text-align: left;"></div><div style="text-align: left;">**<u>Video Demonstration:</u>**</div><div style="text-align: left;"></div><div style="text-align: left;"></div><div style="text-align: left;"></div><div style="text-align: left;">**<u>Source Codes:</u>**</div><div style="text-align: left;"></div><div style="text-align: left;"></div><div style="text-align: left;"></div><div style="text-align: left;"></div><div style="text-align: left;">If you find this tutorial helpful, please kindly support my Youtube channel TechToTinker</div><div style="text-align: left;"></div><div style="text-align: left;">Thank you and have a good day.</div><div style="text-align: left;"></div><div style="text-align: left;">Happy tinkering.</div>