---
layout: post
title: "Writing a VPN server using Go in 30 minutes"
date: 2019-01-19 11:55:46 +0530
comments: true
categories: [Golang,GO,VPN,networking,tcp,osi,udp,ipv4]
---

I have been playing around with various VPN products and the underlaying tech. As a hobby project I implemeted a simple VPN server using `GO`. For the matter of demo, I have used [`ToyVPN`]() as client, with minor changes in handshake.

## Pre-requisite
- `TUN/TAP` interface
- `Netfilter` and `iptables` utility

### TUN/TAP
- We can write a `VPN` server that can work on layer 2 (Data Link Layer) or layer 3 (Network layer)
- Based on which layer you pick, you will be using TUN/TAP interface

	| Interface | Layer | Packet type |
	| --- | --- | --- |
	| TAP | 2 | Ethernet |
	| TUN | 3 | IP |

- For more information, read [this](https://www.kernel.org/doc/Documentation/networking/tuntap.txt) documentation of TunTap drivers.

### `Netfilter` and `iptables`
-  In the Linux ecosystem, `iptables` is a widely used firewall tool that interfaces with the kernel's `netfilter` packet filtering framework. [1] [2]
-  In simple words, `netfilter` provides a set of hooks at the different stages of packet routing inside the kernel. 
-  For example, a hook when Packet is about to get routed (PREROUTING), or when packet is about to be sent to a local process to handle (INPUT) etc
-  Threre are 5 such hooks

	| Hook | Flow | 
	|---|---|---|
	|PREROUTING|Before routing of the packet| 
	|INPUT|Before sending packet to a local process|
	|FORWARD|Before forwarding packet out of the machine (external destination)|
	|OUTPUT|After local process has processed the packet| 
	|POSTROUTING| After routinng of the packet is completed|

![](/public/images/nat-chains.gif)


## External links
- [1] [https://www.digitalocean.com/community/tutorials/a-deep-dive-into-iptables-and-netfilter-architecture](https://www.digitalocean.com/community/tutorials/a-deep-dive-into-iptables-and-netfilter-architecture)
- [2] [https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands)