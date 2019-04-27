# MicropythonCayenneMQTTClient, a port of the Python Cayenne MQTT Client to Micropython
## Introduction
Libraries giving easy acces to the MyDevices Cayenne IoT builder are available for many languages:
* C, C++
* Java
* Python

When developing IoT demonstration programs running on Micropython on WeMos D1 mini processors with ESP8266 CPU, I found that the [Cayenne MQTT Client](https://github.com/myDevicesIoT/Cayenne-MQTT-Python) code for Python relies on the Eclipse Paho MQTT client, which unfortunately is not available in Micropython. Micropython uses a simpler implementation of the MQTT client called umqtt/simple.py available at 
[umqtt](https://github.com/micropython/micropython-lib/tree/master/umqtt.simple) or in 
[micropython-lib](https://github.com/micropython/micropython-lib)
I therefore ported the standard Cayenne MQTT Client Python library to Micropython leaving the API unchanged. Like this the example programs available in the original code can be used unchanged.
## Testing the library
The Cayenne MQTT Client for Micropython depends on
* umqtt/simple.py
* logging

Befor installing the cayenne library you must modify thee WiFi SSID and wifiPassword parameters in 
The logging code can be found in micropython-lib. In order to test the library you may create a directory "lib" on your Micropython using uPyCraft with sub-directories umqtt and cayenne. The logging code (logging.y from micropython-lib) goes into lib, simple.py goes into lib/umqtt and client.py from this repository goes into cayenne. With the libraries now available you may try to run the example codes
## Installation of the library
Once you manage to run the example codes you may want to permanenty install the libraries in Micropython. Download the [Micropython sources](https://github.com/micropython/micropython), and change directory to ???micropythonsSource???/ports/esp8266/modules. Copy logging.py to the modules directory. Create the directories umqtt and cayenne and copy simple.py and client.py respectlively.
Recompile Micropython and flash onto your ESP8266 CPU card. The libraries are now available for use.
## Using the library
To use the library you must 

import cayenne.client

import logging

After this, define the Cayenne credentials:
* username
* password
