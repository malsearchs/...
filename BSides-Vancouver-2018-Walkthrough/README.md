# BSides Vancouver: 2018 (Workshop)

Boot2Root challenge

VM image available [here](https://www.vulnhub.com/entry/bsides-vancouver-2018-workshop,231/#download).

Boot the image from VM Player or VirtualBox and keep the network settings in bridge-mode for easier IP assignment and then get the attacker (typically, Kali) ready.  Would strongly recommend to keep this network out of Internet reach.

## Enumeration:
First, find the IP address of the victim.

`netdiscover –r 10.11.1.0/24`

![netdiscover](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/netdisc.png)

Then fire nmap to find the open ports.

`nmap –sV –sC 10.11.1.15`

![nmap](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/nmap.png)

Now we know HTTP running, run nikto.

`nikto –C all –h 10.11.1.15`

![nikto](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/nikto.png)


Two catchy things from nikto output: robots.txt and it has entry “/backup_wordpress/” directory.

Let’s check robots.txt file. 

![robots](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/robots.png)
