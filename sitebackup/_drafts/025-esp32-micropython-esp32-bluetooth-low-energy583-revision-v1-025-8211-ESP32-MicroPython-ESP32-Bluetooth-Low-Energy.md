---
author: George Bantique
date: '2023-03-11T19:36:45+08:00'
excerpt: In this article, I will discussed how you can use ESP32 BLE capability using MicroPython. Bluetooth Low Energy or BLE is very popular on most devices today because of its low power consumption. That is achieve by constantly switching to sleep mode then once in a while wakes up to process Bluetooth functions. With its low power requirements, it is best suited for battery operated applications.
guid: https://techtotinker.com/?p=1286
id: 1286
permalink: /?p=1286
title: '025 &#8211; ESP32 MicroPython: ESP32 Bluetooth Low Energy'
url: /025-esp32-micropython-esp32-bluetooth-low-energy583-revision-v1-025-8211-ESP32-MicroPython-ESP32-Bluetooth-Low-Energy
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2021/08/ESP32-2BBLE.png)</figure><div> </div>In this article, I will discussed how you can use ESP32 BLE capability using MicroPython.

Bluetooth Low Energy or BLE is very popular on most devices today because of its low power consumption. That is achieve by constantly switching to sleep mode then once in a while wakes up to process Bluetooth functions. With its low power requirements, it is best suited for battery operated applications.

As of this writing, Bluetooth Classic or the traditional Bluetooth communication is not yet supported by MicroPython community so we will just use the BLE instead. By following the example source code given below, you can implement both direction of communication using BLE.

<div> </div>## BILL OF MATERIALS:

1. ESP32 development board.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/ZR3vgToAHbY?feature=oembed" title="025 - ESP32 MicroPython: Bluetooth Low Energy | BLE" width="500"></iframe></div></figure><div> </div>## SOURCE CODE:

### 1. Example # 1: Demo sending and receiving BLE request:

<div>```
from machine import Pin
from machine import Timer
from time import sleep_ms
import ubluetooth

ble_msg = ""

class ESP32_BLE():
    def __init__(self, name):
        # Create internal objects for the onboard LED
        # blinking when no BLE device is connected
        # stable ON when connected
        self.led = Pin(2, Pin.OUT)
        self.timer1 = Timer(0)
        
        self.name = name
        self.ble = ubluetooth.BLE()
        self.ble.active(True)
        self.disconnected()
        self.ble.irq(self.ble_irq)
        self.register()
        self.advertiser()

    def connected(self):
        self.led.value(1)
        self.timer1.deinit()

    def disconnected(self):        
        self.timer1.init(period=100, mode=Timer.PERIODIC, callback=lambda t: self.led.value(not self.led.value()))

    def ble_irq(self, event, data):
        global ble_msg
        
        if event == 1: #_IRQ_CENTRAL_CONNECT:
                       # A central has connected to this peripheral
            self.connected()

        elif event == 2: #_IRQ_CENTRAL_DISCONNECT:
                         # A central has disconnected from this peripheral.
            self.advertiser()
            self.disconnected()
        
        elif event == 3: #_IRQ_GATTS_WRITE:
                         # A client has written to this characteristic or descriptor.          
            buffer = self.ble.gatts_read(self.rx)
            ble_msg = buffer.decode('UTF-8').strip()
            
    def register(self):        
        # Nordic UART Service (NUS)
        NUS_UUID = '6E400001-B5A3-F393-E0A9-E50E24DCCA9E'
        RX_UUID = '6E400002-B5A3-F393-E0A9-E50E24DCCA9E'
        TX_UUID = '6E400003-B5A3-F393-E0A9-E50E24DCCA9E'
            
        BLE_NUS = ubluetooth.UUID(NUS_UUID)
        BLE_RX = (ubluetooth.UUID(RX_UUID), ubluetooth.FLAG_WRITE)
        BLE_TX = (ubluetooth.UUID(TX_UUID), ubluetooth.FLAG_NOTIFY)
            
        BLE_UART = (BLE_NUS, (BLE_TX, BLE_RX,))
        SERVICES = (BLE_UART, )
        ((self.tx, self.rx,), ) = self.ble.gatts_register_services(SERVICES)

    def send(self, data):
        self.ble.gatts_notify(0, self.tx, data + 'n')

    def advertiser(self):
        name = bytes(self.name, 'UTF-8')
        adv_data = bytearray('x02x01x02') + bytearray((len(name) + 1, 0x09)) + name
        self.ble.gap_advertise(100, adv_data)
        print(adv_data)
        print("rn")
                # adv_data
                # raw: 0x02010209094553503332424C45
                # b'x02x01x02ttESP32BLE'
                #
                # 0x02 - General discoverable mode
                # 0x01 - AD Type = 0x01
                # 0x02 - value = 0x02
                
                # https://jimmywongiot.com/2019/08/13/advertising-payload-format-on-ble/
                # https://docs.silabs.com/bluetooth/latest/general/adv-and-scanning/bluetooth-adv-data-basics



led = Pin(2, Pin.OUT)
but = Pin(0, Pin.IN)
ble = ESP32_BLE("ESP32BLE")

def buttons_irq(pin):
    led.value(not led.value())
    ble.send('LED state will be toggled.')
    print('LED state will be toggled.')   
but.irq(trigger=Pin.IRQ_FALLING, handler=buttons_irq)

while True:
    if ble_msg == 'read_LED':
        print(ble_msg)
        ble_msg = ""
        print('LED is ON.' if led.value() else 'LED is OFF')
        ble.send('LED is ON.' if led.value() else 'LED is OFF')
    sleep_ms(100)

```

</div><div> </div>### 2. Example # 2, demo continuous sending of BLE message. In this example is\_ble\_connected global variable is used. This will represent if a device is connected on the BLE device. If you try to send a BLE message when no device is connected, it will throw an error OS Error: -128.:

<div>```
from machine import Pin
from machine import Timer
from time import sleep_ms
import ubluetooth

ble_msg = ""
is_ble_connected = False

class ESP32_BLE():
    def __init__(self, name):
        # Create internal objects for the onboard LED
        # blinking when no BLE device is connected
        # stable ON when connected
        self.led = Pin(2, Pin.OUT)
        self.timer1 = Timer(0)
        
        self.name = name
        self.ble = ubluetooth.BLE()
        self.ble.active(True)
        self.disconnected()
        self.ble.irq(self.ble_irq)
        self.register()
        self.advertiser()

    def connected(self):
        global is_ble_connected
        is_ble_connected = True
        self.led.value(1)
        self.timer1.deinit()

    def disconnected(self):
        global is_ble_connected
        is_ble_connected = False
        self.timer1.init(period=100, mode=Timer.PERIODIC, callback=lambda t: self.led.value(not self.led.value()))

    def ble_irq(self, event, data):
        global ble_msg
        
        if event == 1: #_IRQ_CENTRAL_CONNECT:
                       # A central has connected to this peripheral
            self.connected()

        elif event == 2: #_IRQ_CENTRAL_DISCONNECT:
                         # A central has disconnected from this peripheral.
            self.advertiser()
            self.disconnected()
        
        elif event == 3: #_IRQ_GATTS_WRITE:
                         # A client has written to this characteristic or descriptor.          
            buffer = self.ble.gatts_read(self.rx)
            ble_msg = buffer.decode('UTF-8').strip()
            
    def register(self):        
        # Nordic UART Service (NUS)
        NUS_UUID = '6E400001-B5A3-F393-E0A9-E50E24DCCA9E'
        RX_UUID = '6E400002-B5A3-F393-E0A9-E50E24DCCA9E'
        TX_UUID = '6E400003-B5A3-F393-E0A9-E50E24DCCA9E'
            
        BLE_NUS = ubluetooth.UUID(NUS_UUID)
        BLE_RX = (ubluetooth.UUID(RX_UUID), ubluetooth.FLAG_WRITE)
        BLE_TX = (ubluetooth.UUID(TX_UUID), ubluetooth.FLAG_NOTIFY)
            
        BLE_UART = (BLE_NUS, (BLE_TX, BLE_RX,))
        SERVICES = (BLE_UART, )
        ((self.tx, self.rx,), ) = self.ble.gatts_register_services(SERVICES)

    def send(self, data):
        self.ble.gatts_notify(0, self.tx, data + 'n')

    def advertiser(self):
        name = bytes(self.name, 'UTF-8')
        adv_data = bytearray('x02x01x02') + bytearray((len(name) + 1, 0x09)) + name
        self.ble.gap_advertise(100, adv_data)
        print(adv_data)
        print("rn")
                # adv_data
                # raw: 0x02010209094553503332424C45
                # b'x02x01x02ttESP32BLE'
                #
                # 0x02 - General discoverable mode
                # 0x01 - AD Type = 0x01
                # 0x02 - value = 0x02
                
                # https://jimmywongiot.com/2019/08/13/advertising-payload-format-on-ble/
                # https://docs.silabs.com/bluetooth/latest/general/adv-and-scanning/bluetooth-adv-data-basics


ble = ESP32_BLE("ESP32BLE")

while True:
    if is_ble_connected:
        ble.send('Hello')
    sleep_ms(1000)

```

<div style="text-align: left;">**<span style="color: red;"> </span>**</div>Added Septemter 4, 2021.**

</div><div> </div>## REFERENCES AND CREDITS:

<div style="text-align: left;">1. MicroPython BLE documents:</div><div style="text-align: left;"> <https://docs.micropython.org/en/latest/library/ubluetooth.html></div><div style="text-align: left;"> </div><div style="text-align: left;">2. Micropython BLE examples:</div><div style="text-align: left;"> <https://github.com/micropython/micropython/tree/master/examples/bluetooth></div>