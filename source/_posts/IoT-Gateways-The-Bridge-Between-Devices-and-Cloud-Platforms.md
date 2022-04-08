---
title: 'IoT Gateways: The Bridge Between Devices and Cloud Platforms'
summary: 'The IoT gateway is the bridge between the device and the cloud platform. The gateway has various southbound and northbound interfaces.'
date: 2022-02-22 10:43:45
keywords: IOT,Gateway, Gateways,IoT Gateways,LoRa, BLE,ZigBee, NB-IoT
categories: IOT
tags:
  - Gateway
  - LoRa
  - BLE
  - ZigBee
  - NB-IoT
---



#### IoT Gateways: The Bridge Between Devices and Cloud Platforms

For example, a cold storage temperature and humidity monitoring system has been designed, which is based on `LoRa` communication technology. We know that the environment of cold storage is very complex.

Generally speaking, the cellular network signal inside the warehouse is very poor, and even often there is no network signal at all, for two reasons. First, in order to enhance the thermal insulation performance, the warehouse walls are made thicker than ordinary walls; the second is that the warehouses are usually located in the suburbs of the city, which are relatively remote.
In addition, due to various situations such as full warehouses and empty warehouses, the signal environment of the warehouse is changeable. Therefore, if the monitoring equipment deployed inside the cold storage is directly connected to the cellular network, reliable and stable communication cannot be achieved at all.
In this case, the solution you can choose is to deploy an IoT gateway that has a stable network signal, so that the monitoring equipment in the cold storage can use `LoRa`, a technology with strong penetration ability and a communication range of several kilometers in an open environment. Communicate with the gateway to indirectly realize the networking of devices.

![Cold storage IoT gateway](Cold-storage-IoT-gateway.png)

The IoT gateway is becoming an indispensable role in the entire IoT system. It is becoming more and more important as a bridge between IoT devices and cloud platforms.

#### Protocol conversion: the key to building bridges

When `BLE`, `ZigBee,` and `LoRa` devices communicate with the gateway, the gateway needs to parse out data based on open or internal private protocols; then the gateway uses the connection protocol with the cloud platform to organize data and complete data transmission.
This process naturally requires the gateway device to be able to support different communication technologies. A schematic diagram of the structure of a gateway communication interface is given below.

![Gateway Protocol Conversion](Gateway-Protocol-Conversion.png)

The upper part is called the "northbound interface" and the lower part is called the "southbound interface". This is a common saying in the industry.
**The northbound interface** needs to be connected to the Internet, so the usual choices are `RJ45 Ethernet` port, optical fiber interface, `Wi-Fi` and `4G`, `NB-IoT,` and other cellular network modules.
**The southbound interface** is used to connect IoT devices. In addition to the wireless technology interfaces such as `BLE`, `ZigBee`, `LoRa`, and `Wi-Fi` just mentioned, `RJ45 Ethernet`, `RS232`, `RS485,` and other wired interfaces are also commonly used on industrial personal computers (Industrial Personal Computer).

It should be noted here that the interface type and the number of each gateway device are not fixed, because gateway products generally determine several different specifications and models according to application scenarios. Different types of gateways need to support different types of protocols and the conversion of different numbers of protocols, so the protocol conversion function of the gateway generally adopts the software architecture of plug-ins.
The secondary development capability of the plug-in mechanism is very important. On the one hand, it allows us to dynamically and flexibly configure the protocol conversion function according to the interface; on the other hand, it can also facilitate us to develop the parsing function of private protocols.
For example, devices that communicate with gateways through `BLE`, `ZigBee,` or `LoRa` technology usually use private application layer protocols, which requires us to write parsing codes based on the private protocols defined during device architecture design.
As for IoT devices connected to the gateway using `RJ45` network port or `Wi-Fi`, in addition to using proprietary protocols based on `TCP` or `UDP`, standard protocols such as `MQTT` or `CoAP` may also be used. At this time, we need to deal with the format of these protocols.
In addition, standard protocols often used in industrial computers and PLCs (Programmable Logic Controllers) include `Modbus`, `ProfiBus`, `OPC UA`, and `BACnet`.


#### Other functions of the gateway

After protocol conversion, the gateway obtains data in a common format. For these data, the gateway also needs to persist and temporarily store the data.
The storage function of the gateway can prevent the loss of device data due to temporary network failures and other reasons. In addition, the configuration information of the gateway and the device also needs to be stored in the gateway, so that the device can be quickly read during the operation.
At the same time, data security is also very important. The IoT gateway needs to do the following things:

1. Perfect local authentication. In this way, the software or data of the gateway device can be prevented from being arbitrarily modified.
2. The gateway ensures encrypted transmission of data. Because many IoT devices have the very weak computing power and do not have the ability to encrypt data, gateways are needed to ensure the encryption and decryption of data or control commands.
3. The gateway can support operator private network access, or support VPN (Virtual Private Network, virtual private network) technology. The advantage of VPN technology is to establish an encrypted channel based on the Internet network. This not only ensures the safety and reliability of data transmission but also lowers the cost of establishing a dedicated line. Commonly used VPN protocols include IPsec, OpenVPN, etc.

In addition to the protocol conversion, storage function, and security management mentioned above, IoT gateways generally have functional modules such as device management, gateway configuration, and over-the-air upgrades.

#### Data analysis on the gateway

The industry has begun to try to sink some computing services on the cloud platform to "edge devices" close to where the data occurs. This is the origin of edge computing, and the IoT gateway is the key to the most lightweight solution in edge computing.
Edge devices, such as IoT gateways, complete preliminary data processing and computing tasks that require timely response; while cloud platforms are responsible for data analysis that requires large-scale data and complex computing, as well as overall coordination and control.
Specifically, the cloud platform migrates some computing tasks of the original cloud computing model to network edge devices; network edge devices (such as routers, mobile network base stations, etc.) perform data processing and data analysis tasks near the data source. In this way, the computing load of the cloud computing center is reduced, the pressure of IOT on network bandwidth is reduced, and the efficiency of data processing is improved.
Benefits of the edge computing model:

1. **Low latency**. The data only needs to be transmitted from the generation device to the edge device, and does not need to go through other networks. Due to the short transmission distance, the network latency is low.
2. **Save the backbone network bandwidth**. Alleviate network congestion caused by massive data transmission. Especially like the proprietary networks of some banks, their bandwidth is very limited and can only be used to transmit critical data.
3. **Computational availability is good**. The path length of data in the network is significantly shortened, and the unavailability of computing services caused by network fluctuations will be reduced.
4. **Better privacy**. Since the edge device is close to the user, the user's private data no longer needs to be uploaded to the cloud platform. Therefore, in the edge computing scenario, the user's privacy can also be better protected.

##### The Impact of Edge Computing on Gateways

Changes in the technical architecture generally place new demands on the system. The impact of edge computing on gateways is mainly reflected in three aspects:

1. First, in order to better run edge computing tasks in the gateway, the gateway needs to support virtualization technology, which is usually implemented by container technology in current practice. Containers are naturally lightweight and portable, making them ideal for developers to quickly test applications, and for maintainers to deploy and update applications on a large scale on the gateway. In addition, container technology is also more conducive to gateways using container automatic operation and maintenance platform technology, such as `Kubernetes`, to implement functions such as application orchestration.
2. Secondly, due to the uncertainty of the network environment, the gateway needs to have a certain autonomy. When the communication between the gateway and the cloud platform is interrupted, this situation should not affect the computing task of the gateway processing data, and the management of IoT devices.
3. Finally, because of the introduction of edge computing, we need to implement the task of data analysis and processing on the gateway side. The hardware resources of the gateway are very diverse, and the business requirements are also very different, so it becomes more important to provide a unified development framework on the gateway. A development framework can provide developers with a consistent API and interoperability of components. In this way, developers can implement business functions more efficiently, and it is easier to collaborate with other vendors.

Gateway architecture diagram after adding edge computing capabilities:

![Gateway Architecture](Gateway-Architecture.png)

#### How to realize edge computing?

First, let's talk about the Organization for Standardization. At present, there are three main standards organizations promoting edge computing, namely:

1.  [LF Edge](https://www.lfedge.org/) (Linux Foundation Edge) under Linux Foundation and [CNCF](https://www.cncf.io/) (Cloud Native Computing Foundation) dedicated to promoting Cloud Native
2. [Eclipse IoT](https://iot.eclipse.org/) project under Eclipse Foundation.
3. [OpenStack Foundation](https://www.openstack.org/)

**Container technology**: There is mainly the [KubeEdge](https://kubeedge.io/zh/). Its main idea is to extend Kubernetes from the cloud to edge devices to facilitate the orchestration and scheduling of applications on edge gateways, and to realize collaborative data processing between cloud and edge devices.
**Development framework**: The more well-known [EdgeX Foundry](https://cn.edgexfoundry.org/) is positioned to provide a general edge computing framework for the Industrial Internet of Things. To this end, it adapts many protocols and provides standard API interfaces.
A project dedicated to the smart home space is [Home Edge](https://www.lfedge.org/projects/homeedge/), an open source project contributed by Samsung to accelerate edge deployment of smart home devices.


#### Summary

With more and more IoT application scenarios, IoT gateways are becoming "edge centers". It has become a very important part of the "cloud, edge, and terminal" architecture of the Internet of Things (ie, cloud platform, edge, and terminal). In the architecture design and functional development of the IoT gateway, the contents that need to be paid attention to are:

1. The IoT gateway is the bridge between the device and the cloud platform. The gateway has various southbound and northbound interfaces. Therefore, the protocol conversion function is a function that must be included. It ensures the reliable upload of IoT device data and the effective execution of cloud platform control commands.
2. Pay attention to the realization of the basic functions of the gateway. These functions include functional modules such as storage function, security management, device management, gateway configuration, and over-the-air upgrade. They not only ensure the safe and reliable transmission of data but also provide support for remote maintenance.
3. The ability of the gateway to support edge computing needs to be determined according to the business scenario. Edge computing has three impacts on the architecture of IoT gateways: 
        
    - **Container technology**. The application of container technology is helpful for developers to quickly develop and test applications, and it is also convenient for maintainers to deploy and update applications on the gateway on a large scale. At the same time, based on the container automatic operation and maintenance platform technology, functions such as application orchestration can be realized. 
    - **Autonomous ability**. It can ensure the normal execution of the data analysis and processing tasks of the gateway when the network environment is unstable. 
    - **Development framework**. The development framework is beneficial to the development work of developers and is also beneficial to the cooperation and interoperability of various components.