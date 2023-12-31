---
author: George Bantique
categories:
  - ESP32
  - MicroPython
date: '2021-09-29T17:50:00+08:00'
excerpt: 'In this article, I will discuss how you can use a GPS module with ESP32 using MicroPython.

  GPS is an acronym which stands for Global Positioning System. With the help of GPS, we can determine our location called coordinates which is mainly composed of the latitude, the longitude, and the altitude.'
tags:
  - ESP32 GPS
  - ESP8266
  - How to use GPS in MicroPython
  - MicroPython GPS
  - MicroPython project
  - MicroPython tutorials
  - NodeMCU
title: '047 - MicroPython TechNotes: E108 GPS'
url: /2021/09/29/047-micropython-technotes-e108-gps/
---

## **Introduction**

![](/images/047-MicroPython-TechNotes-GPS-TechToTinker.png)

In this article, I will discuss how you can use a GPS module with ESP32 using MicroPython.

GPS is an acronym which stands for Global Positioning System. With the help of GPS, we can determine our location called coordinates which is mainly composed of the latitude, the longitude, and the altitude.

The latitude is the angle formed between the equator and a certain location of the Earth. A positive latitude value when going North while a negative value when going South. North pole is (+)90 degrees, South Pole is -90 degrees, and of course Equator is 0 degrees. Latitude is the first value for each given coordinates.

The longitude is the angle formed between the Greenwich timeline (Greenwich, England) and a certain location of the Earth. A positive longitude value when going to the East while a negative value when going to the West. In the middle of Pacific ocean is either 180 degrees East or -180 degrees West. Longitude is the second value for each given coordinates.

The altitude is the elevation above the horizon. It is a distance measurement above sea level usually measured in meter unit. Altitude is the third value for each given coordinates.

What I have is a GPS module from Gorillacell which is included in the ESP32 development kit. It contains the E108 Integrated Circuit of Chengdu EByte Electronic Technology. The GPS kit also comes with an external antenna for better GPS signal reception.

The E108 GPS module can be connected directly to a computer by connecting it through a type-C USB cable where it appears as a Serial Port device and by using any serial terminal application, you should see a GPS data dump to your app. But it is more beneficial for us to interface it to a microcontroller such as the ESP32 and custom parse the receive data through serial interface.

## **Pinout**

The E108 GPS module has 4 pins namely:
1. **GND** – for the **ground** pin.
2. **VCC** – for the **supply voltage**.
3. **Tx** – for the UART **serial transmit pin**, connect the Rx pin of the microcontroller.
4. **Rx** – for the UART **serial receive pin**, connect the Tx pin of the microcontroller.

## **Bill Of Materials**

1. ESP32 development board.
2. Gorillacell ESP32 shield, optional.
3. Gorillacell GPS module set with antenna.
4. 0.96 OLED for the display or just use the REPL to observe the GPS data.
5. 4-pin dupont wires.

## **Hardware Instruction**

1. Connect the GPS module GND pin to the ESP32 GND pin.
2. Connect the GPS module VCC pin to the ESP32 5V pin.
3. Connect the GPS module Tx pin to the ESP32 GPIO 25.
4. Connect the GPS module Rx pin to the EPS32 GPIO 26.
5. Connect the OLED module GND pin to the ESP32 GND pin.
6. Connect the OLED module VCC pin to the ESP32 5V pin.
7. Connect the OLED module SDA pin to the ESP32 GPIO 21.
8. Connect the OLED module SCL pin to the ESP32 GPIO 22.

## **Software Instruction**

1. Save the micropyGPS.py to your ESP32 MicroPython device root directory. This is the driver library that will handle receiving and parsing GPS data dump to the serial.
2. Save the ssd1306.py to your ESP32 MicroPython device root directory. This is the driver library that will handle for displaying text strings in the OLED display.
3. Copy and paste E108GPS_Example#1.py to Thonny IDE. You may save it to MicroPython device root directory as main.py. This will make the program run after reset or after power ON. Or just save it to your local hard drive, save it the filename you like, and run it clicking the RUN button.

## **Video Demonstration**

{{< youtube id="330qdOZpIcQ" >}}

## **Call To Action**

If you have any concern regarding this video, please do not hesitate to leave your message in the comment section.
You might also like to support my journey on Youtube by subscribing to my channel. [Click this link to Subscribe to TechToTinker Youtube channel.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)
As always, thank you for visiting.
Regards,
    – George Bantique | tech.to.tinker@gmail.com

## **Source Code**

### 1. Example # 1:

```py { lineNos="true" wrap="true" }
# More details can be found in TechToTinker.blogspot.com 
# George Bantique | tech.to.tinker@gmail.com

from machine import Pin
from machine import SoftI2C
from machine import UART
from micropyGPS import MicropyGPS
from ssd1306 import SSD1306_I2C 

def main():
    uart = UART(1, rx=25, tx=26, baudrate=9600, bits=8, parity=None, stop=1, timeout=5000, rxbuf=1024)
    gps = MicropyGPS()
    i2c = SoftI2C(scl=Pin(22), sda=Pin(21), freq=400000) 
    oled = SSD1306_I2C(128, 64, i2c, addr=0x3C)
    led = Pin(2, Pin.OUT)
    
    while True:
      buf = uart.readline()

      for char in buf:
        gps.update(chr(char))  # Note the conversion to to chr, UART outputs ints normally

      print('UTC Timestamp:', gps.timestamp)
      print('Date:', gps.date_string('long'))
      print('Satellites:', gps.satellites_in_use)
      print('Altitude:', gps.altitude)
      print('Latitude:', gps.latitude)
      print('Longitude:', gps.longitude_string())
      print('Horizontal Dilution of Precision:', gps.hdop)
      print()
      
      oled.fill(0)
      oled.text('Date:{}'.format(gps.date_string('s_mdy')),0,0)
      oled.text('Sat:{}'.format(gps.satellites_in_use),0,10)
      oled.text('Alt:{}'.format(gps.altitude),0,20)
      oled.text('{}'.format(gps.latitude_string()),0,35)
      oled.text('{}'.format(gps.longitude_string()),0,45)
      oled.show()
      
      led.value(not led.value())
        
# def startGPSthread():
#     _thread.start_new_thread(main, ())

if __name__ == "__main__":
  print('...running main, GPS testing')
  main()

```

### 2. microGPS.py

```py { lineNos="true" wrap="true" }
"""
# MicropyGPS - a GPS NMEA sentence parser for Micropython/Python 3.X
# Copyright (c) 2017 Michael Calvin McCoy (calvin.mccoy@protonmail.com)
# The MIT License (MIT) - see LICENSE file
"""
# TODO:
# Time Since First Fix
# Distance/Time to Target
# More Helper Functions
# Dynamically limit sentences types to parse

from math import floor, modf

# Import utime or time for fix time handling
try:
    # Assume running on MicroPython
    import utime
except ImportError:
    # Otherwise default to time module for non-embedded implementations
    # Should still support millisecond resolution.
    import time

class MicropyGPS(object):
    """GPS NMEA Sentence Parser. Creates object that stores all relevant GPS data and statistics.
    Parses sentences one character at a time using update(). """

    # Max Number of Characters a valid sentence can be (based on GGA sentence)
    SENTENCE_LIMIT = 90
    __HEMISPHERES = ('N', 'S', 'E', 'W')
    __NO_FIX = 1
    __FIX_2D = 2
    __FIX_3D = 3
    __DIRECTIONS = ('N', 'NNE', 'NE', 'ENE', 'E', 'ESE', 'SE', 'SSE', 'S', 'SSW', 'SW', 'WSW', 'W',
                    'WNW', 'NW', 'NNW')
    __MONTHS = ('January', 'February', 'March', 'April', 'May',
                'June', 'July', 'August', 'September', 'October',
                'November', 'December')

    def __init__(self, local_offset=0, location_formatting='ddm'):
        """
        Setup GPS Object Status Flags, Internal Data Registers, etc
            local_offset (int): Timzone Difference to UTC
            location_formatting (str): Style For Presenting Longitude/Latitude:
                                       Decimal Degree Minute (ddm) - 40° 26.767′ N
                                       Degrees Minutes Seconds (dms) - 40° 26′ 46″ N
                                       Decimal Degrees (dd) - 40.446° N
        """

        #####################
        # Object Status Flags
        self.sentence_active = False
        self.active_segment = 0
        self.process_crc = False
        self.gps_segments = []
        self.crc_xor = 0
        self.char_count = 0
        self.fix_time = 0

        #####################
        # Sentence Statistics
        self.crc_fails = 0
        self.clean_sentences = 0
        self.parsed_sentences = 0

        #####################
        # Logging Related
        self.log_handle = None
        self.log_en = False

        #####################
        # Data From Sentences
        # Time
        self.timestamp = [0, 0, 0]
        self.date = [0, 0, 0]
        self.local_offset = local_offset

        # Position/Motion
        self._latitude = [0, 0.0, 'N']
        self._longitude = [0, 0.0, 'W']
        self.coord_format = location_formatting
        self.speed = [0.0, 0.0, 0.0]
        self.course = 0.0
        self.altitude = 0.0
        self.geoid_height = 0.0

        # GPS Info
        self.satellites_in_view = 0
        self.satellites_in_use = 0
        self.satellites_used = []
        self.last_sv_sentence = 0
        self.total_sv_sentences = 0
        self.satellite_data = dict()
        self.hdop = 0.0
        self.pdop = 0.0
        self.vdop = 0.0
        self.valid = False
        self.fix_stat = 0
        self.fix_type = 1

    ########################################
    # Coordinates Translation Functions
    ########################################
    @property
    def latitude(self):
        """Format Latitude Data Correctly"""
        if self.coord_format == 'dd':
            decimal_degrees = self._latitude[0] + (self._latitude[1] / 60)
            return [decimal_degrees, self._latitude[2]]
        elif self.coord_format == 'dms':
            minute_parts = modf(self._latitude[1])
            seconds = round(minute_parts[0] * 60)
            return [self._latitude[0], int(minute_parts[1]), seconds, self._latitude[2]]
        else:
            return self._latitude

    @property
    def longitude(self):
        """Format Longitude Data Correctly"""
        if self.coord_format == 'dd':
            decimal_degrees = self._longitude[0] + (self._longitude[1] / 60)
            return [decimal_degrees, self._longitude[2]]
        elif self.coord_format == 'dms':
            minute_parts = modf(self._longitude[1])
            seconds = round(minute_parts[0] * 60)
            return [self._longitude[0], int(minute_parts[1]), seconds, self._longitude[2]]
        else:
            return self._longitude

    ########################################
    # Logging Related Functions
    ########################################
    def start_logging(self, target_file, mode="append"):
        """
        Create GPS data log object
        """
        # Set Write Mode Overwrite or Append
        mode_code = 'w' if mode == 'new' else 'a'

        try:
            self.log_handle = open(target_file, mode_code)
        except AttributeError:
            print("Invalid FileName")
            return False

        self.log_en = True
        return True

    def stop_logging(self):
        """
        Closes the log file handler and disables further logging
        """
        try:
            self.log_handle.close()
        except AttributeError:
            print("Invalid Handle")
            return False

        self.log_en = False
        return True

    def write_log(self, log_string):
        """Attempts to write the last valid NMEA sentence character to the active file handler
        """
        try:
            self.log_handle.write(log_string)
        except TypeError:
            return False
        return True

    ########################################
    # Sentence Parsers
    ########################################
    def gprmc(self):
        """Parse Recommended Minimum Specific GPS/Transit data (RMC)Sentence.
        Updates UTC timestamp, latitude, longitude, Course, Speed, Date, and fix status
        """

        # UTC Timestamp
        try:
            utc_string = self.gps_segments[1]

            if utc_string:  # Possible timestamp found
                hours = (int(utc_string[0:2]) + self.local_offset) % 24
                minutes = int(utc_string[2:4])
                seconds = float(utc_string[4:])
                self.timestamp = (hours, minutes, seconds)
            else:  # No Time stamp yet
                self.timestamp = (0, 0, 0)

        except ValueError:  # Bad Timestamp value present
            return False

        # Date stamp
        try:
            date_string = self.gps_segments[9]

            # Date string printer function assumes to be year >=2000,
            # date_string() must be supplied with the correct century argument to display correctly
            if date_string:  # Possible date stamp found
                day = int(date_string[0:2])
                month = int(date_string[2:4])
                year = int(date_string[4:6])
                self.date = (day, month, year)
            else:  # No Date stamp yet
                self.date = (0, 0, 0)

        except ValueError:  # Bad Date stamp value present
            return False

        # Check Receiver Data Valid Flag
        if self.gps_segments[2] == 'A':  # Data from Receiver is Valid/Has Fix

            # Longitude / Latitude
            try:
                # Latitude
                l_string = self.gps_segments[3]
                lat_degs = int(l_string[0:2])
                lat_mins = float(l_string[2:])
                lat_hemi = self.gps_segments[4]

                # Longitude
                l_string = self.gps_segments[5]
                lon_degs = int(l_string[0:3])
                lon_mins = float(l_string[3:])
                lon_hemi = self.gps_segments[6]
            except ValueError:
                return False

            if lat_hemi not in self.__HEMISPHERES:
                return False

            if lon_hemi not in self.__HEMISPHERES:
                return False

            # Speed
            try:
                spd_knt = float(self.gps_segments[7])
            except ValueError:
                return False

            # Course
            try:
                if self.gps_segments[8]:
                    course = float(self.gps_segments[8])
                else:
                    course = 0.0
            except ValueError:
                return False

            # TODO - Add Magnetic Variation

            # Update Object Data
            self._latitude = [lat_degs, lat_mins, lat_hemi]
            self._longitude = [lon_degs, lon_mins, lon_hemi]
            # Include mph and hm/h
            self.speed = [spd_knt, spd_knt * 1.151, spd_knt * 1.852]
            self.course = course
            self.valid = True

            # Update Last Fix Time
            self.new_fix_time()

        else:  # Clear Position Data if Sentence is 'Invalid'
            self._latitude = [0, 0.0, 'N']
            self._longitude = [0, 0.0, 'W']
            self.speed = [0.0, 0.0, 0.0]
            self.course = 0.0
            self.valid = False

        return True

    def gpgll(self):
        """Parse Geographic Latitude and Longitude (GLL)Sentence. Updates UTC timestamp, latitude,
        longitude, and fix status"""

        # UTC Timestamp
        try:
            utc_string = self.gps_segments[5]

            if utc_string:  # Possible timestamp found
                hours = (int(utc_string[0:2]) + self.local_offset) % 24
                minutes = int(utc_string[2:4])
                seconds = float(utc_string[4:])
                self.timestamp = (hours, minutes, seconds)
            else:  # No Time stamp yet
                self.timestamp = (0, 0, 0)

        except ValueError:  # Bad Timestamp value present
            return False

        # Check Receiver Data Valid Flag
        if self.gps_segments[6] == 'A':  # Data from Receiver is Valid/Has Fix

            # Longitude / Latitude
            try:
                # Latitude
                l_string = self.gps_segments[1]
                lat_degs = int(l_string[0:2])
                lat_mins = float(l_string[2:])
                lat_hemi = self.gps_segments[2]

                # Longitude
                l_string = self.gps_segments[3]
                lon_degs = int(l_string[0:3])
                lon_mins = float(l_string[3:])
                lon_hemi = self.gps_segments[4]
            except ValueError:
                return False

            if lat_hemi not in self.__HEMISPHERES:
                return False

            if lon_hemi not in self.__HEMISPHERES:
                return False

            # Update Object Data
            self._latitude = [lat_degs, lat_mins, lat_hemi]
            self._longitude = [lon_degs, lon_mins, lon_hemi]
            self.valid = True

            # Update Last Fix Time
            self.new_fix_time()

        else:  # Clear Position Data if Sentence is 'Invalid'
            self._latitude = [0, 0.0, 'N']
            self._longitude = [0, 0.0, 'W']
            self.valid = False

        return True

    def gpvtg(self):
        """Parse Track Made Good and Ground Speed (VTG) Sentence. Updates speed and course"""
        try:
            course = float(self.gps_segments[1])
            spd_knt = float(self.gps_segments[5])
        except ValueError:
            return False

        # Include mph and km/h
        self.speed = (spd_knt, spd_knt * 1.151, spd_knt * 1.852)
        self.course = course
        return True

    def gpgga(self):
        """Parse Global Positioning System Fix Data (GGA) Sentence. Updates UTC timestamp, latitude, longitude,
        fix status, satellites in use, Horizontal Dilution of Precision (HDOP), altitude, geoid height and fix status"""

        try:
            # UTC Timestamp
            utc_string = self.gps_segments[1]

            # Skip timestamp if receiver doesn't have on yet
            if utc_string:
                hours = (int(utc_string[0:2]) + self.local_offset) % 24
                minutes = int(utc_string[2:4])
                seconds = float(utc_string[4:])
            else:
                hours = 0
                minutes = 0
                seconds = 0.0

            # Number of Satellites in Use
            satellites_in_use = int(self.gps_segments[7])

            # Get Fix Status
            fix_stat = int(self.gps_segments[6])

        except (ValueError, IndexError):
            return False

        try:
            # Horizontal Dilution of Precision
            hdop = float(self.gps_segments[8])
        except (ValueError, IndexError):
            hdop = 0.0

        # Process Location and Speed Data if Fix is GOOD
        if fix_stat:

            # Longitude / Latitude
            try:
                # Latitude
                l_string = self.gps_segments[2]
                lat_degs = int(l_string[0:2])
                lat_mins = float(l_string[2:])
                lat_hemi = self.gps_segments[3]

                # Longitude
                l_string = self.gps_segments[4]
                lon_degs = int(l_string[0:3])
                lon_mins = float(l_string[3:])
                lon_hemi = self.gps_segments[5]
            except ValueError:
                return False

            if lat_hemi not in self.__HEMISPHERES:
                return False

            if lon_hemi not in self.__HEMISPHERES:
                return False

            # Altitude / Height Above Geoid
            try:
                altitude = float(self.gps_segments[9])
                geoid_height = float(self.gps_segments[11])
            except ValueError:
                altitude = 0
                geoid_height = 0

            # Update Object Data
            self._latitude = [lat_degs, lat_mins, lat_hemi]
            self._longitude = [lon_degs, lon_mins, lon_hemi]
            self.altitude = altitude
            self.geoid_height = geoid_height

        # Update Object Data
        self.timestamp = [hours, minutes, seconds]
        self.satellites_in_use = satellites_in_use
        self.hdop = hdop
        self.fix_stat = fix_stat

        # If Fix is GOOD, update fix timestamp
        if fix_stat:
            self.new_fix_time()

        return True

    def gpgsa(self):
        """Parse GNSS DOP and Active Satellites (GSA) sentence. Updates GPS fix type, list of satellites used in
        fix calculation, Position Dilution of Precision (PDOP), Horizontal Dilution of Precision (HDOP), Vertical
        Dilution of Precision, and fix status"""

        # Fix Type (None,2D or 3D)
        try:
            fix_type = int(self.gps_segments[2])
        except ValueError:
            return False

        # Read All (up to 12) Available PRN Satellite Numbers
        sats_used = []
        for sats in range(12):
            sat_number_str = self.gps_segments[3 + sats]
            if sat_number_str:
                try:
                    sat_number = int(sat_number_str)
                    sats_used.append(sat_number)
                except ValueError:
                    return False
            else:
                break

        # PDOP,HDOP,VDOP
        try:
            pdop = float(self.gps_segments[15])
            hdop = float(self.gps_segments[16])
            vdop = float(self.gps_segments[17])
        except ValueError:
            return False

        # Update Object Data
        self.fix_type = fix_type

        # If Fix is GOOD, update fix timestamp
        if fix_type > self.__NO_FIX:
            self.new_fix_time()

        self.satellites_used = sats_used
        self.hdop = hdop
        self.vdop = vdop
        self.pdop = pdop

        return True

    def gpgsv(self):
        """Parse Satellites in View (GSV) sentence. Updates number of SV Sentences,the number of the last SV sentence
        parsed, and data on each satellite present in the sentence"""
        try:
            num_sv_sentences = int(self.gps_segments[1])
            current_sv_sentence = int(self.gps_segments[2])
            sats_in_view = int(self.gps_segments[3])
        except ValueError:
            return False

        # Create a blank dict to store all the satellite data from this sentence in:
        # satellite PRN is key, tuple containing telemetry is value
        satellite_dict = dict()

        # Calculate  Number of Satelites to pull data for and thus how many segment positions to read
        if num_sv_sentences == current_sv_sentence:
            # Last sentence may have 1-4 satellites; 5 - 20 positions
            sat_segment_limit = (sats_in_view - ((num_sv_sentences - 1) * 4)) * 5
        else:
            sat_segment_limit = 20  # Non-last sentences have 4 satellites and thus read up to position 20

        # Try to recover data for up to 4 satellites in sentence
        for sats in range(4, sat_segment_limit, 4):

            # If a PRN is present, grab satellite data
            if self.gps_segments[sats]:
                try:
                    sat_id = int(self.gps_segments[sats])
                except (ValueError,IndexError):
                    return False

                try:  # elevation can be null (no value) when not tracking
                    elevation = int(self.gps_segments[sats+1])
                except (ValueError,IndexError):
                    elevation = None

                try:  # azimuth can be null (no value) when not tracking
                    azimuth = int(self.gps_segments[sats+2])
                except (ValueError,IndexError):
                    azimuth = None

                try:  # SNR can be null (no value) when not tracking
                    snr = int(self.gps_segments[sats+3])
                except (ValueError,IndexError):
                    snr = None
            # If no PRN is found, then the sentence has no more satellites to read
            else:
                break

            # Add Satellite Data to Sentence Dict
            satellite_dict[sat_id] = (elevation, azimuth, snr)

        # Update Object Data
        self.total_sv_sentences = num_sv_sentences
        self.last_sv_sentence = current_sv_sentence
        self.satellites_in_view = sats_in_view

        # For a new set of sentences, we either clear out the existing sat data or
        # update it as additional SV sentences are parsed
        if current_sv_sentence == 1:
            self.satellite_data = satellite_dict
        else:
            self.satellite_data.update(satellite_dict)

        return True

    ##########################################
    # Data Stream Handler Functions
    ##########################################

    def new_sentence(self):
        """Adjust Object Flags in Preparation for a New Sentence"""
        self.gps_segments = ['']
        self.active_segment = 0
        self.crc_xor = 0
        self.sentence_active = True
        self.process_crc = True
        self.char_count = 0

    def update(self, new_char):
        """Process a new input char and updates GPS object if necessary based on special characters ('

```

### 3. SSD1306.py

Copy it from: <https://techtotinker.com/010-micropython-technotes-0-96-oled-display/>


## **References And Credits**
1. Purchase your Gorillacell ESP32 development kit at:
<https://gorillacell.kr/>
Or email them at: <alphaco@hanmail.net>

2. microGPS.py Github repository:
<https://github.com/inmcm/micropyGPS>

3. Convert degrees minutes seconds to decimal degrees by visiting:
<https://www.rapidtables.com/convert/number/degrees-minutes-seconds-to-degrees.html>

## **Additional Information**

In case you want to plot the GPS coordinates on the map, you may achieve it by:
1. Parse the latitude and convert it as decimal degrees.
2. Parse the longitude and convert it as decimal degrees.
3. Parse the altitude in meters.
4. Concatenate the coordinates to the following internet link:
`https://www.google.com/maps/@&lt;latitude&gt;,&lt;longitude&gt;,&lt;altitude&gt;`
    - **For example:**
    `https://www.google.com/maps/@7.360303,125.7525,14z`
5. The links in the example should open a google maps in the location provided by the GPS module.</div><div> </div>


