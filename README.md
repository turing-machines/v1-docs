---
description: Hello and welcome to the Turing Pi V1 documentation.
---

# Turing Pi V1 documentation

Turing Pi V1 is a 7 nodes cluster on a mini ITX board. It’s a scale model of bare metal clusters like you see in data centers. The Turing Pi board can be used as a local mini server to host apps, containers, Kubernetes. The Turing Pi also is a great tool for learning cloud-native technologies, Serverless, Microservices on a bare-metal cluster. The Turing Pi top/main node can act as a NAT/Router for the rest of the nodes. If you move the cluster from one location to another, all the node's IPs stay the same. 

### Some of the Turing Pi V1 features:

* Flash mode - flash RPi compute modules using the Turing Pi board
* Boot mode - boot OS through eMMC or SD card or netboot 

{% hint style="info" %}
Please note, you can use either eMMC or SD card storage. You can't use both options. 
{% endhint %}

* Power management for each node \(on/off/reboot\)
* Real-time clock \(RTC\)
* I2C cluster management bus
* Compute modules hot-swap

Please visit the sections below for more detailed information about the Turing Pi V 1 board. 

{% page-ref page="specs.md" %}

{% page-ref page="get-started/" %}

{% page-ref page="install-kubernetes-k3s/" %}

{% page-ref page="faq.md" %}



