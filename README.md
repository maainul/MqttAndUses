# MqttAndUses
Important Links :
https://randomnerdtutorials.com/what-is-mqtt-and-how-it-works/

## What is MQTT Protocol?
MQTT is a lightweight publish/subscribe messaging protocol designed for M2M (machine to machine) telemetry in low bandwidth environments.<br/>
It was designed by Andy Stanford-Clark (IBM) and Arlen Nipper in 1999 for connecting Oil Pipeline telemetry systems over satellite.

Although it started as a proprietary protocol it was released Royalty free in 2010 and became an OASIS standard in 2014.<br/>
MQTT stands for MQ Telemetry Transport but previously was known as Message Queuing Telemetry Transport.

MQTT is fast becoming one of the main protocols for IOT (internet of things) deployments

### MQTT Versions

There are two different variants of MQTT and several versions.

- MQTT v3.1.0 –
- MQTT v3.1.1 – In Common Use
- MQTT v5 – Currently Limited use
- MQTT-SN – See notes later

The original MQTT which was designed in 1999 and has been in use for many years and is designed for TCP/IP networks.

MQTTv3.1.1 is version in common use.

![1_PO5_87H8ZHRGWQpeOAFXMA](https://user-images.githubusercontent.com/37740006/114506108-26141100-9c53-11eb-8718-8ff634a708fc.jpeg)
![MQTT_Ov1](https://user-images.githubusercontent.com/37740006/114506127-2d3b1f00-9c53-11eb-9170-b8ae030cdc6b.png)

## MQTT Basic Concepts
In MQTT there are a few basic concepts that you need to understand:

- Publish/Subscribe
- Messages
- Topics
- Broke

###  MQTT – Publish/Subscribe

The first concept is the publish and subscribe system. In a publish and subscribe system, a device can publish a message on a topic, or it can be subscribed to a particular topic to receive messages

![Screenshot from 2021-04-13 12-39-47](https://user-images.githubusercontent.com/37740006/114507831-7f7d3f80-9c55-11eb-83fb-b08b8d4b129c.png)

For example Device 1 publishes on a topic.
Device 2 is subscribed to the same topic as device 1 is publishing in.
So, device 2 receives the message.

### MQTT – Messages
Messages are the information that you want to exchange between your devices. Whether it’s a command or data.

## MQTT – Topics
Another important concept are the topics. Topics are the way you register interest for incoming messages or how you specify where you want to publish the message.
Topics are represented with strings separated by a forward slash. Each forward slash indicates a topic level. Here’s an example on how you would create a topic for a lamp in your home office:
Note: topics are case-sensitive, which makes these two topics different:

![Screenshot from 2021-04-13 12-40-14](https://user-images.githubusercontent.com/37740006/114507870-8c9a2e80-9c55-11eb-9240-0fd22cae3ef6.png)

If you would like to turn on a lamp in your home office using MQTT you can imagine the following scenario:
![Screenshot from 2021-04-13 12-40-25](https://user-images.githubusercontent.com/37740006/114507953-a89dd000-9c55-11eb-9fb7-147ffa60513e.png)

You have a device that publishes “on” and “off” messages on the home/office/lamp topic.<br/>
You have a device that controls a lamp (it can be an ESP32, ESP8266, or any other board). The ESP32 that controls your lamp, is subscribed to that topic: home/office/lamp.<br/>
So, when a new message is published on that topic, the ESP32 receives the “on” or “off” message and turns the lamp on or off.<br/>
This first device, can be an ESP32, an ESP8266, or an Home Automation controller platform like Node-RED, Home Assistant, Domoticz, or OpenHAB, for example.<br/>

mqtt-device

### MQTT – Broker
At last, you also need to be aware of the term broker.

The broker is primarily responsible for receiving all messages, filtering the messages, decide who is interested in them and then publishing the message to all subscribed clients.

mqtt-broker

There are several brokers you can use. In our home automation projects we use the Mosquitto broker which can be installed in the Raspberry Pi. Alternatively, you can use a cloud MQTT broker.
![Screenshot from 2021-04-13 12-40-44](https://user-images.githubusercontent.com/37740006/114508003-b7848280-9c55-11eb-968e-29be7338f275.png)
