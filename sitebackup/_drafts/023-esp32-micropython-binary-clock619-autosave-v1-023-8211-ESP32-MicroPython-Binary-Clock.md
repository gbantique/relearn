---
author: George Bantique
date: '2023-03-14T11:15:49+08:00'
excerpt: In this article, I would like to share to you on how to create this simple yet cool project using a single 8×8 dot matrix module to display a binary clock – a clock that is represented using binary numeric system.
guid: https://techtotinker.com/?p=1450
id: 1450
permalink: /?p=1450
title: '023 &#8211; ESP32 MicroPython: Binary Clock'
url: /023-esp32-micropython-binary-clock619-autosave-v1-023-8211-ESP32-MicroPython-Binary-Clock
---


<div style="text-align: left;"></div><figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/023-esp32-micropython-binary-clock.png)</figure>In this article, I would like to share to you on how to create this simple yet cool project using a single 8×8 dot matrix module to display a binary clock – a clock that is represented using binary numeric system.

<div> </div>## BILL OF MATERIALS:

1. An ESP32 development board (or any other development board with MicroPython firmware).
2. An 8×8 Dot Matrix module with SPI interface (if you have I2C interface, you just need to use a different driver library and modify the source code a little bit).
3. And some jumper wires.

<div> </div>## HARDWARE INSTRUCTION:

For the hardware part, it is very easy just follow the circuit diagram below which connects the dot matrix to ESP32 as follows:

1. Dot matrix VCC to 3.3V.
2. Dot matrix GND pin to ESP32 GND pin.
3. Dot matrix DIN pin to ESP32 GPIO 23.
4. Dot matrix CLK pin to ESP32 GPIO 19.
5. Dot matrix CS pin to ESP32 GPIO 18.

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/023-esp32-micropython-binary-clock-diagram.png)</figure><div> </div>## SOFTWARE INSTRUCTION:

1. Copy the max7219 from Jeff Brown github: https://github.com/jgbrown32/ESP8266\_MAX7219 or you may copy it in the SOURCE CODE section below.
2. And save it to ESP32 MicroPython device root directory by clicking the File menu and select Save As.
3. Select MicroPython device and name it as max7219.py and click OK.
4. Copy the example source code below. Please feel free to modify it to your liking.
5. Enjoy.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/ahHNt-RYg2w?feature=oembed" title="023 - ESP32 MicroPython: Binary Clock" width="500"></iframe></div></figure><div> </div>## CALL TO ACTION:

For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker**@gbantiquegmail-com

<div> </div>## SOURCE CODE:

### 1. Binary Clock:

<div>```
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin, SPI
from micropython import const
import framebuf, utime

_DIGIT_0 = const(0x1)

_DECODE_MODE = const(0x9)
_NO_DECODE = const(0x0)

_INTENSITY = const(0xa)
_INTENSITY_MIN = const(0x0)

_SCAN_LIMIT = const(0xb)
_DISPLAY_ALL_DIGITS = const(0x7)

_SHUTDOWN = const(0xc)
_SHUTDOWN_MODE = const(0x0)
_NORMAL_OPERATION = const(0x1)

_DISPLAY_TEST = const(0xf)
_DISPLAY_TEST_NORMAL_OPERATION = const(0x0)

_MATRIX_SIZE = const(8)

# _SCROLL_SPEED_NORMAL is ms to delay (slow) scrolling text.
_SCROLL_SPEED_NORMAL = 100

class Max7219(framebuf.FrameBuffer):
    """
    Driver for MAX7219 8x8 LED matrices
    https://github.com/vrialland/micropython-max7219
    Example for ESP8266 with 2x4 matrices (one on top, one on bottom),
    so we have a 32x16 display area:
    >>> from machine import Pin, SPI
    >>> from max7219 import Max7219
    >>> spi = SPI(1, baudrate=10000000)
    >>> screen = Max7219(32, 16, spi, Pin(15))
    >>> screen.rect(0, 0, 32, 16, 1)  # Draws a frame
    >>> screen.text('Hi!', 4, 4, 1)
    >>> screen.show()
    On some matrices, the display is inverted (rotated 180°), in this case
     you can use `rotate_180=True` in the class constructor.
    """

    def __init__(self, width, height, spi, cs, rotate_180=False):
        # Pins setup
        self.spi = spi
        self.cs = cs
        self.cs.init(Pin.OUT, True)

        # Dimensions
        self.width = width
        self.height = height
        # Guess matrices disposition
        self.cols = width // _MATRIX_SIZE
        self.rows = height // _MATRIX_SIZE
        self.nb_matrices = self.cols * self.rows
        self.rotate_180 = rotate_180
        # 1 bit per pixel (on / off) -> 8 bytes per matrix
        self.buffer = bytearray(width * height // 8)
        format = framebuf.MONO_HLSB if not self.rotate_180 else framebuf.MONO_HMSB
        super().__init__(self.buffer, width, height, format)

        # Init display
        self.init_display()

    def _write_command(self, command, data):
        """Write command on SPI"""
        cmd = bytearray([command, data])
        self.cs(0)
        for matrix in range(self.nb_matrices):
            self.spi.write(cmd)
        self.cs(1)

    def init_display(self):
        """Init hardware"""
        for command, data in (
                (_SHUTDOWN, _SHUTDOWN_MODE),  # Prevent flash during init
                (_DECODE_MODE, _NO_DECODE),
                (_DISPLAY_TEST, _DISPLAY_TEST_NORMAL_OPERATION),
                (_INTENSITY, _INTENSITY_MIN),
                (_SCAN_LIMIT, _DISPLAY_ALL_DIGITS),
                (_SHUTDOWN, _NORMAL_OPERATION),  # Let's go
        ):
            self._write_command(command, data)

        self.fill(0)
        self.show()

    def brightness(self, value):
        # Set display brightness (0 to 15)
        if not 0 <= value < 16:
            raise ValueError('Brightness must be between 0 and 15')
        self._write_command(_INTENSITY, value)

    def marquee(self, message):
        start = 33
        extent = 0 - (len(message) * 8) - 32
        for i in range(start, extent, -1):
            self.fill(0)
            self.text(message, i, 0, 1)
            self.show()
            utime.sleep_ms(_SCROLL_SPEED_NORMAL)

    def show(self):
        """Update display"""
        # Write line per line on the matrices
        for line in range(8):
            self.cs(0)

            for matrix in range(self.nb_matrices):
                # Guess where the matrix is placed
                row, col = divmod(matrix, self.cols)
                # Compute where the data starts
                if not self.rotate_180:
                    offset = row * 8 * self.cols
                    index = col + line * self.cols + offset
                else:
                    offset = 8 * self.cols - row * (8 - line) * self.cols
                    index = (7 - line) * self.cols + col - offset

                self.spi.write(bytearray([_DIGIT_0 + line, self.buffer[index]]))

            self.cs(1)

```

</div><div> </div>### 2. max7219.py driver library:

<div>```
# Jeff Brown max7219 driver library
from machine import Pin, SPI
from micropython import const
import framebuf, utime

_DIGIT_0 = const(0x1)

_DECODE_MODE = const(0x9)
_NO_DECODE = const(0x0)

_INTENSITY = const(0xa)
_INTENSITY_MIN = const(0x0)

_SCAN_LIMIT = const(0xb)
_DISPLAY_ALL_DIGITS = const(0x7)

_SHUTDOWN = const(0xc)
_SHUTDOWN_MODE = const(0x0)
_NORMAL_OPERATION = const(0x1)

_DISPLAY_TEST = const(0xf)
_DISPLAY_TEST_NORMAL_OPERATION = const(0x0)

_MATRIX_SIZE = const(8)

# _SCROLL_SPEED_NORMAL is ms to delay (slow) scrolling text.
_SCROLL_SPEED_NORMAL = 100

class Max7219(framebuf.FrameBuffer):
    """
    Driver for MAX7219 8x8 LED matrices
    https://github.com/vrialland/micropython-max7219
    Example for ESP8266 with 2x4 matrices (one on top, one on bottom),
    so we have a 32x16 display area:
    >>> from machine import Pin, SPI
    >>> from max7219 import Max7219
    >>> spi = SPI(1, baudrate=10000000)
    >>> screen = Max7219(32, 16, spi, Pin(15))
    >>> screen.rect(0, 0, 32, 16, 1)  # Draws a frame
    >>> screen.text('Hi!', 4, 4, 1)
    >>> screen.show()
    On some matrices, the display is inverted (rotated 180°), in this case
     you can use `rotate_180=True` in the class constructor.
    """

    def __init__(self, width, height, spi, cs, rotate_180=False):
        # Pins setup
        self.spi = spi
        self.cs = cs
        self.cs.init(Pin.OUT, True)

        # Dimensions
        self.width = width
        self.height = height
        # Guess matrices disposition
        self.cols = width // _MATRIX_SIZE
        self.rows = height // _MATRIX_SIZE
        self.nb_matrices = self.cols * self.rows
        self.rotate_180 = rotate_180
        # 1 bit per pixel (on / off) -> 8 bytes per matrix
        self.buffer = bytearray(width * height // 8)
        format = framebuf.MONO_HLSB if not self.rotate_180 else framebuf.MONO_HMSB
        super().__init__(self.buffer, width, height, format)

        # Init display
        self.init_display()

    def _write_command(self, command, data):
        """Write command on SPI"""
        cmd = bytearray([command, data])
        self.cs(0)
        for matrix in range(self.nb_matrices):
            self.spi.write(cmd)
        self.cs(1)

    def init_display(self):
        """Init hardware"""
        for command, data in (
                (_SHUTDOWN, _SHUTDOWN_MODE),  # Prevent flash during init
                (_DECODE_MODE, _NO_DECODE),
                (_DISPLAY_TEST, _DISPLAY_TEST_NORMAL_OPERATION),
                (_INTENSITY, _INTENSITY_MIN),
                (_SCAN_LIMIT, _DISPLAY_ALL_DIGITS),
                (_SHUTDOWN, _NORMAL_OPERATION),  # Let's go
        ):
            self._write_command(command, data)

        self.fill(0)
        self.show()

    def brightness(self, value):
        # Set display brightness (0 to 15)
        if not 0 <= value < 16:
            raise ValueError('Brightness must be between 0 and 15')
        self._write_command(_INTENSITY, value)

    def marquee(self, message):
        start = 33
        extent = 0 - (len(message) * 8) - 32
        for i in range(start, extent, -1):
            self.fill(0)
            self.text(message, i, 0, 1)
            self.show()
            utime.sleep_ms(_SCROLL_SPEED_NORMAL)

    def show(self):
        """Update display"""
        # Write line per line on the matrices
        for line in range(8):
            self.cs(0)

            for matrix in range(self.nb_matrices):
                # Guess where the matrix is placed
                row, col = divmod(matrix, self.cols)
                # Compute where the data starts
                if not self.rotate_180:
                    offset = row * 8 * self.cols
                    index = col + line * self.cols + offset
                else:
                    offset = 8 * self.cols - row * (8 - line) * self.cols
                    index = (7 - line) * self.cols + col - offset

                self.spi.write(bytearray([_DIGIT_0 + line, self.buffer[index]]))

            self.cs(1)

```

</div><div> </div>## REFERENCES AND CREDITS:

<div>**1. Jeff Brown max7219 library:** https://github.com/jgbrown32/ESP8266_MAX7219</div>