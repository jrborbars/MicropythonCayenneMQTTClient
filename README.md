# MicropythonCayenneMQTTClient, a port of the Python Cayenne MQTT Client to Micropython
## Introduction
Libraries giving easy acces to the MyDevices Cayenne IoT builder are available for many languages:
* C, C++
* Java
* Python
When trying to use the Python code for IoT demos running on Micropython on WeMos D1 mini processors with ESP8266 CPU, I found that the Cayenne MQTT Client code for Python relies on the Eclipse Paho MQTT client, which unfortunately is not available in Micropython. Micropython uses a simpler implementation of the MQTT client called umqtt/simple.py available at 
[umqtt](https://github.com/micropython/micropython-lib/tree/master/umqtt.simple). 
