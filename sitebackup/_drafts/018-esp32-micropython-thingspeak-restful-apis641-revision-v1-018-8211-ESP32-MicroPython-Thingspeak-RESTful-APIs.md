---
author: George Bantique
date: '2023-03-17T11:28:50+08:00'
excerpt: REST API is one of the most popular API types. REST stands for REpresentational State Transfer which is also known as RESTful APIs. It is designed to take advantage of existing protocol. It can be used over any existing protocol, but it is typically used over HTTP when used on web applications. A RESTful API is an application program interface that uses HTTP requests to GET, PUT, POST and DELETE data.
guid: https://techtotinker.com/?p=1587
id: 1587
permalink: /?p=1587
title: '018 &#8211; ESP32 MicroPython: Thingspeak | RESTful APIs'
url: /018-esp32-micropython-thingspeak-restful-apis641-revision-v1-018-8211-ESP32-MicroPython-Thingspeak-RESTful-APIs
---


<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/018-esp32-rest-api-micropython-techtotinker.png)</figure>REST API is one of the most popular API types. REST stands for **REpresentational State Transfer** which is also known as RESTful APIs.

It is designed to take advantage of existing protocol. It can be used over any existing protocol, but it is typically used over HTTP when used on web applications.

A **RESTful API** is an application program interface that uses **HTTP requests** to **GET**, **PUT**, **POST** and **DELETE** data.

<figure class="wp-block-image size-large">![](https://techtotinker.com/wp-content/uploads/2023/03/018-esp32-rest-api-micropython-techtotinker-thingspeak-1024x498.png)</figure><div> </div>## BILL OF MATERIALS:

1. <span style="font-family: "Open Sans", sans-serif; white-space: normal;">ESP32 or ESP8266 development board.</span>
2. <span style="font-family: "Open Sans", sans-serif; white-space: normal;">DHT22 temperature and humidity sensor, or DHT11.</span>

<div> </div>## HARDWARE INSTRUCTION:

1. Connect the DHT22 VCC pin to ESP32 3.3V pin.
2. Connect the DHT22 Data pin to ESP32 D23 pin.
3. Connect the DHT22 GND pin to ESP32 GND pin.

<div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/F0WtIg6fRs4?feature=oembed" title="018 - ESP32 MicroPython: Thingspeak | RESTful APIs" width="500"></iframe></div></figure><div> </div>## CALL TO ACTION:

For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## SOURCE CODE:

### 1. Example # 1:

```
<pre class="wp-block-code">```
# **************************************#
# Author: George Bantique               #
#         TechToTinker Youtube Channel  #
#         TechToTinker.blogspot.com     #
#         tech.to.tinker@gmail.com      #
# Date: Nov.13, 2020                    #
# Please feel free to modify the code   #
# according to your needs.              #
# **************************************#

# **************************************
# Load necessary libraries
import machine
import network 
import wifi_credentials 
import urequests 
import dht 
import time 

# **************************************
# Create objects:
led = machine.Pin(2,machine.Pin.OUT) 
d = dht.DHT22(machine.Pin(23)) 

# *************************************
# Configure the ESP32 wifi as STAtion
sta = network.WLAN(network.STA_IF)
if not sta.isconnected(): 
  print('connecting to network...') 
  sta.active(True) 
  #sta.connect('your wifi ssid', 'your wifi password') 
  sta.connect(wifi_credentials.ssid, wifi_credentials.password) 
  while not sta.isconnected(): 
    pass 
print('network config:', sta.ifconfig()) 

# **************************************
# Constants and variables:
HTTP_HEADERS = {'Content-Type': 'application/json'} 
THINGSPEAK_WRITE_API_KEY = 'W6ANWJGN2T7507GW' 
UPDATE_TIME_INTERVAL = 5000  # in ms 
last_update = time.ticks_ms() 
# initially there would be some delays 
# before submitting the first update 
# but should be enough to stabilize the 
# the DHT sensor. 

# **************************************
# Main loop:
while True: 
    if time.ticks_ms() - last_update >= UPDATE_TIME_INTERVAL: 
        d.measure() 
        t = d.temperature() 
        h = d.humidity() 
         
        dht_readings = {'field1':t, 'field2':h} 
        request = urequests.post( 
          'http://api.thingspeak.com/update?api_key=' +
          THINGSPEAK_WRITE_API_KEY, 
          json = dht_readings, 
          headers = HTTP_HEADERS )  
        request.close() 
        print(dht_readings) 
         
        led.value(not led.value()) 
        last_update = time.ticks_ms()
```
```

<div> </div>### 2. wifi\_credentials.py :

```
<pre class="wp-block-code">```
ssid = "your wifi ssid"
password = "your wifi password"
```
```

<div> </div>### 3. ***With MicroPython ESP32 v1.15, it seems like that the urequests module is removed. With that, you may try to save the following to MicroPython root directory by clicking the File menu, select Save As, select MicroPython Device and save it as urequests.py. (PLEASE DO NOT FORGET THE .py as extension):***

<div>```
import usocket

class Response:

    def __init__(self, f):
        self.raw = f
        self.encoding = "utf-8"
        self._cached = None

    def close(self):
        if self.raw:
            self.raw.close()
            self.raw = None
        self._cached = None

    @property
    def content(self):
        if self._cached is None:
            try:
                self._cached = self.raw.read()
            finally:
                self.raw.close()
                self.raw = None
        return self._cached

    @property
    def text(self):
        return str(self.content, self.encoding)

    def json(self):
        import ujson
        return ujson.loads(self.content)


def request(method, url, data=None, json=None, headers={}, stream=None):
    try:
        proto, dummy, host, path = url.split("/", 3)
    except ValueError:
        proto, dummy, host = url.split("/", 2)
        path = ""
    if proto == "http:":
        port = 80
    elif proto == "https:":
        import ussl
        port = 443
    else:
        raise ValueError("Unsupported protocol: " + proto)

    if ":" in host:
        host, port = host.split(":", 1)
        port = int(port)

    ai = usocket.getaddrinfo(host, port, 0, usocket.SOCK_STREAM)
    ai = ai[0]

    s = usocket.socket(ai[0], ai[1], ai[2])
    try:
        s.connect(ai[-1])
        if proto == "https:":
            s = ussl.wrap_socket(s, server_hostname=host)
        s.write(b"%s /%s HTTP/1.0rn" % (method, path))
        if not "Host" in headers:
            s.write(b"Host: %srn" % host)
        # Iterate over keys to avoid tuple alloc
        for k in headers:
            s.write(k)
            s.write(b": ")
            s.write(headers[k])
            s.write(b"rn")
        if json is not None:
            assert data is None
            import ujson
            data = ujson.dumps(json)
            s.write(b"Content-Type: application/jsonrn")
        if data:
            s.write(b"Content-Length: %drn" % len(data))
        s.write(b"rn")
        if data:
            s.write(data)

        l = s.readline()
        #print(l)
        l = l.split(None, 2)
        status = int(l[1])
        reason = ""
        if len(l) > 2:
            reason = l[2].rstrip()
        while True:
            l = s.readline()
            if not l or l == b"rn":
                break
            #print(l)
            if l.startswith(b"Transfer-Encoding:"):
                if b"chunked" in l:
                    raise ValueError("Unsupported " + l)
            elif l.startswith(b"Location:") and not 200 <= status <= 299:
                raise NotImplementedError("Redirects not yet supported")
    except OSError:
        s.close()
        raise

    resp = Response(s)
    resp.status_code = status
    resp.reason = reason
    return resp


def head(url, **kw):
    return request("HEAD", url, **kw)

def get(url, **kw):
    return request("GET", url, **kw)

def post(url, **kw):
    return request("POST", url, **kw)

def put(url, **kw):
    return request("PUT", url, **kw)

def patch(url, **kw):
    return request("PATCH", url, **kw)

def delete(url, **kw):
    return request("DELETE", url, **kw)

```

</div><div> </div>## REFERENCES AND CREDITS:

**1. Thingspeak website:**  [http://www.thingspeak.com](http://www.thingspeak.com/)  
**2. Mathworks register:**   
 <https://www.mathworks.com/mwaccount/register>

**3. urequests.py library:**   
 <https://github.com/micropython/micropython-lib/blob/master/urequests/urequests.py>