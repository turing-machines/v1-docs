# FAQ

## What can I do with the Turing Pi?

* Home server \(homelab\) and cloud apps hosting
* Learn Kubernetes, Docker Swarm, Serverless, Microservices on bare metal
* Cloud-native apps testing environment
* CI/CD environment
* Learn concepts of distributed Machine Learning apps
* Prototype and learn cluster applications, parallel computing, and distributed computing concepts
* Host K8S, K3S, Minecraft, Plex, Owncloud, Nextcloud, Seafile, Minio, Tensorflow

## Which Raspberry Pi models are compatible?

The Turing Pi V1 board supports the following models with eMMC \(all configurations\) and without eMMC:

* Raspberry Pi Compute Module 1
* Raspberry Pi Compute Module 3
* Raspberry Pi Compute Module 3+

## From where Turing Pi boots OS?

You can boot the OS either from eMMC, SD card, or netboot.

## Does each node get its own IP address?

Yes.

## How the compute modules communicate with each other?

The nodes interconnected with the onboard 1 Gbps switch. However, each node is limited to 100 Mbps USB speed. Also, there is an I2C bus to exchange some technical information between nodes, including Real-Time Clock \(RTC\).

## Do all the slots need to be filled in?

Turing Pi works with any amount of nodes. You can start with a couple of nodes and scale when needed.

## Can I flash compute modules through the board?

Yes, you can flash a compute module using a top/master node.

## How do the NIC, Ethernet, USB, HDMI, and audio ports work?

There are 8 USB on the board. Each pair of USB connected to a particular node. 2x USB routed to the top/master node, 2x to the second node, 2x to the fourth node, 2x to the 6th node. HDMI and audio connected with a top/master node.

NIC - There is an 8 port switch on the board. Each port goes to each node plus one uplink.

## Can Turing Pi function from either an ATX power supply or 12V?

Yes.

