BSides Vancouver: 2018 (Workshop)

Boot2Root challenge.

VM image available here.

Boot the image from VM Player or VirtualBox and keep the network settings in bridge-mode for easier IP assignment and then get the attacker (often, Kali) ready.  Would strongly recommend to keep this network out of Internet reach 

Enumeration:  

First, find the IP address of the target.

netdiscover –r 10.11.1.0/24

