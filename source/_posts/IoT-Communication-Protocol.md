---
title: IoT Communication Protocol
summary: IoT applications are complex and comprehensive systems that require you to understand the features and limitations of different network protocols and make appropriate choices for different parts of the system. 
date: 2022-02-21 10:25:28
keywords: IOT,MQTT，AMQP，CoAP，HTTP，LwM2M，Communication protocol，MQ
categories: IOT，Protocol
tags:
  - Protocol
  - MQTT
  - AMQP
  - HTTP
  - CoAP
  - LwM2M
---


### Publish-Subscribe Protocol

#### MQTT

The MQTT (MQ Telemetry Transport) protocol is a lightweight network protocol developed by IBM in 1999. It adopts a publish-subscribe communication model. It has three main features:


1. The binary message content encoding format is adopted, so the payload content such as binary data, JSON and pictures can be easily transmitted.
2. The protocol header is very compact, and the protocol interaction is also simple, which ensures that the network transmission traffic is small.
3. Supports 3 QoS (Quality of Service) levels, which is convenient for applications to choose flexibly according to different scenarios.

What is QoS? It refers to the negotiation between the communicating parties about the reliability of message transmission.

QoS level 0, the message is only sent once, and the message may be lost;
QoS level 1, the sender will receive feedback to ensure the delivery of the message, but the message may be repeated.
QoS level 2, through multiple interactions between sender and receiver, it is guaranteed that there is one and only one message.
The MQTT protocol is very suitable for remote devices with limited computing power, low network bandwidth, and unstable signals, so it has become the de facto network protocol standard for IoT systems.

#### AMQP

In addition to the MQTT protocol, there are other network protocols that use a publish-subscribe model, such as the AMQP protocol. Although the AMQP protocol has a huge feature set and is relatively heavy, it is not suitable for IoT devices with limited computing resources and strict power consumption requirements, but it can meet the reliability and scalability requirements of the background system. Therefore, it is widely used in the platform system of IOT. For example, the RabbitMQ message middleware widely used in distributed systems is based on AMQP.
Like MQTT, AMQP is also based on the TCP protocol, uses a binary message format, and supports 3 QoS levels as well. The two versions, AMQP 1.0 and AMQP 0.9.1, which are widely used now, are very different in design. When you query data or apply the two versions of the AMQP protocol, you must pay attention to the version to avoid using the wrong one.

### Request-response Pattern

Request-response Pattern
There are many benefits of the publish-subscribe model, but there are exceptions to everything, and there are some IoT use cases that are not suitable for this model. For example, there are smart express cabinets in the community now. After you enter the pickup code, the server will send an opening instruction to the corresponding cabinet door. In a publish-subscribe model, the server knows that the command was sent successfully, but it has no way of knowing if the cabinet door was opened or not. In the cases, you need to enable the cabinet door to feedback the execution result of the command to the server. Of course, you can also ask the server to subscribe to a "door closed" topic message, and then wait for the door to publish this message. But this is very cumbersome and not direct enough. In this scenario, another communication pattern comes in handy, and that is the request-response pattern.
The request-response pattern has two roles, one is the client (Client) and the other is the server (Server). A client is a party that requests data or services. The server is used to receive client requests and provide corresponding data or services. After the server receives the request, it will obtain the data, process the resource data (such as the database), prepare the response, and then return it to the client.
The request-response pattern is a stateless way of communication where each complete request-response is independent of each other. If further subdivided, it can also be divided into two types: synchronous and asynchronous. You can see this picture below.

![Request-response Protocol](Request-response-Protocol.png)

#### HTTP

HTTP is a typical representative of the request-response pattern. The HTTP/2 protocol also introduces an asynchronous request-response mode, where the client can set different priorities for requests, and the server can decide which request to respond to first according to the priority. Although the message format of the HTTP protocol is very heavy, the message header alone can reach the size of KB, which is not suitable for embedded devices with limited resources. But on some IoT devices with sufficient computing resources and network resources, the HTTP protocol is still an option. Moreover, it is compatible with existing Web systems and can utilize existing Web server resources.

#### CoAP

CoAP (Constrained Application Protocol) protocol, like HTTP protocol, CoAP protocol also has GET, POST, PUT, DELETE and other methods and response status codes, and uses URI instead of Topic to identify resources as well. For example, we need to access the resource bedroom/temp under the server iotdemo.com. The complete resource address is:

```bash
coap://iotdemo.com:5683/bedroom/temp 
```

CoAP messages are in binary format and support two QoS levels of acknowledgment and unacknowledged messages. Confirmable message (Confirmable Message) is similar to QoS 1 of MQTT protocol, non-confirmable message (Non-confirmable Message) corresponds to QoS 0 level of MQTT protocol.
In addition, the transport layer protocol based on the CoAP protocol is UDP, not the TCP protocol of HTTP and MQTT protocols, so the computing resource requirements for the device are lower. Sensor devices generally only need to upload data and do not need to receive control commands from the server at any time, which shows that the CoAP protocol is suitable for battery-powered sensor devices.

#### LwM2M

LwM2M (Lightweight M2M) protocol. The LwM2M protocol is defined on top of the CoAP protocol, but it goes further than message transmission. Because it standardizes the device model based on IPSO (IP-base Smart Object), it provides a set of lightweight device management and interaction interface protocols.
Currently, the main implementations of the LwM2M protocol are Wakaama in C language and Leshan in Java language, which are relatively few in application. The application scenarios of the CoAP protocol are also suitable for the LwM2M protocol. If you want to implement device management more conveniently based on the CoAP protocol, you can consider the LwM2M protocol.

### Coexistence of Communication Modes

MQTT 5.0 adds new features of the request-response pattern; AMQP version 1.0 also defines the request-response pattern. As for the CoAP protocol, the new draft version (Draft) also adds a publish-subscribe model feature. The coexistence of communication modes in this network protocol greatly facilitates the application in specific scenarios compared to the design of a single mode, and represents the development direction of a network protocol.

### Summary

IoT applications are complex and comprehensive systems that require you to understand the features and limitations of different network protocols and make appropriate choices for different parts of the system. These selection principles, I summarize into the following three points:

1. IoT devices usually need to run in an environment where the network is not very reliable, and there are also many limitations in terms of power consumption, volume, and computing resources, so we can consider using MQTT and CoAP protocols in the development of the device.
2. Fast and reliable message forwarding is required between the various services of the cloud platform. In this case, the AMQP protocol can be selected.
3. Some applications require a RESTful architecture compatible with the Web system, such as opening the resource capabilities in IOT through REST for other applications to call. In this case, HTTP and CoAP protocols are suitable choices.

![Summary of Network Protocols](Summary-of-Network-Protocols.png)