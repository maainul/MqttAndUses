# MqttAndUses
Important Links :
https://randomnerdtutorials.com/what-is-mqtt-and-how-it-works/
Install Mosquitto in linux<br/>
https://champlnx.blogspot.com/2020/12/installing-mqtt-broker-on-ubuntu-2004.html

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

### Mosquitto
Mosquitto is a really lightweight MQTT broker written in C. We have used it in the past so it came naturally to investigate its capabilities as a broker for a scalable IoT platform. It supports TLS and there are plugins for authorization using a database, but it has some downsides. Unfortunately Mosquitto does not support clustering, it makes scaling a bit difficult. It is using only a single thread so can't take advantage of multi core CPUs. An interesting issue that we had with it when we started to load test it is that above 10 client connections / sec over TLS, connection attempts started to fail due to some errors with the handling of SSL connections using OpenSSL. After searching on the internet for this issue we have found others facing the same and no real solution was found. I'm sure with some tuning on the OS and config levels Mosquitto can do more, but for us it was out of the game.

### RabbitMQ
RabbitMQ is a very popular message broker written in Erlang that has support for MQTT among other protocols through a plugin. TLS support is there, clustering is fine, authorization cannot be done using a database directly but you can create an HTTP REST wrapper over your database and that can be used as an authorization backend. The problem with RabbitMQ is the MQTT support itself. This broker supports the AMQP protocol natively, the MQTT implementation is missing some important features such as QoS2. Quality of Service level 2 ensures that a message is received exactly once. This is very important in some cases, for example when commands are sent from the IoT platform to the devices or actuators. Lost or duplicate commands can cause serious problems in these scenarios so QoS2 is a must. We had to say goodbye to RabbitMQ.

### EMQ
EMQ is another Erlang based broker which was very promising. It is easy to configure and use, scales well, but unfortunately client certificates are not supported in the current version. In SensorHUB we issue x509 client certificates to the devices that we want to connect to the cluster. These certificates provide a secure, passwordless mode of authentication. This is a must for us, if this feature is available EMQ would be a very strong option.

### VerneMQ
VerneMQ is a relatively new MQTT broker written in Erlang (this language is very popular in the message broker world because its distributed and soft real-time capabilities). VerneMQ has all the features that we were looking for. Clustering, TLS 1.2, complete MQTT 3.1.1 implementation and authorization with database. It can be easily extended with Lua scripts, has a relatively nice documentation and a very helpful developer team on GitHub. We ran some performance tests on it and it could easily handle 400 joining clients/sec sending 3000 messages/sec on a 2 core 2 GHz server with 8 gigabytes of ram. At QoS level 1 on both consumer and producer side.

After checking out these and other brokers that fall out of the race even earlier, we went with VerneMQ. Another promising enterprise ready broker is HiveMQ but since it is not open source and free to use we couldn't evaluate it, however they have some very nice and detailed materials on the MQTT protocol, if someone is interested. https://www.hivemq.com/blog/mqtt-essentials/
