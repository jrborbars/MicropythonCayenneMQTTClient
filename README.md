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
* cayenne username
* cayenne password
* cayenne client_id
This information you will find on the Cayenne page for your device.

Finally you must create a CayenneMQTTClient object:

client = cayenne.client.CayenneMQTTClient()
Calling the *begin* method of this class will connect the program to your WiFi network and then to the Cayenne MQTT broker at mqtt.mydevices.com. Once the connection is established you can send your data. See Example-01-SendData.py for an example.
## The Oled display
For the WeMos D1 mini you can get a small Oled display which is connected to the CPU through the I2C bus. The Cayenne MQTT client will check if this module is connected and it will print connection information (IP number and a message that connection to Cayenne has been established). If the Oled module is not connected the Client will work nevertheless.
Some WeMos D1 shields may use the D1 or D2 lines (GPIO 4 or 5) for other purpose than as I2C SDA and SCL lines. In this case we cannot use the Oled display and the cayenne client object should be created with: 

client = cayenne.client.CayenneMQTTClient(testOled=False)

## Client methods
Here is a list of methods supplied by the client class:
* begin(username, password, clientid, ssid=SSID, wifiPassword=PASSWORD,
        hostname='mqtt.mydevices.com', port=1883,
        logname=LOG_NAME, loglevel=logging.WARNING):
        starts the connection to Cayenne
* celsiusWrite(channel,value): sends a temperature value in °Celsius
* fahrenheitWrite(channel,value): sends a temperature value in Fahrenheit
* kelvinWrite(channel,value): sends a temperature value in Kelvin
* humidityWrite(channel,value): send a relative humidity value in percent
* luxWite(channel,value): sends a light intensity value in lux
* pascalWrite(channel,value): sends a barometric pressure value in Pascal
* hectoPascalWrite(channel,value): sends a barometric pressure value in hecto Pascal
* volatageWrite(channel,value): sends a voltage value in mV
* digitalWrite(channel,value): sends a digital value (0/1)
## Receiving and treating commands
In order to receive commands from Cayenne you must subscribe to a command topic. This is done automatically after establishing the connection to Cayenne. In order to treat command messages you must create a callback function which takes a list (topic,payload) as parameter as register it with the Cayenne MQTT client:

client.on_message = yourCallback

It is then your job to find out from which channel the command came from and which value was sent.
## The CayenneMessage class
In order to help you parsing the received command message, you create a CayenneMessage object passing it the received topic and payload:

msg=CayenneMessage(topic,payload)

* msg.client_id contains the client_id having sent the message
* msg.channel contains the channel number
* msg.msg_id contains the message id
* msg.value contains the value

Please have a look at Example_02_ReceiveData.py how this is done
