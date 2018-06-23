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

![nmap](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/nmap1.png)

Now we know HTTP running, run nikto.

`nikto –C all –h 10.11.1.15`

![nikto](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/nikto.png)

Two catchy things from nikto output: robots.txt and it has entry “/backup_wordpress/” directory.

Let’s check robots.txt file. 

![robots](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/robots.png)

Let’s check /backup_wordpress/ directory too.

![bkp-wp](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/bkp-wp.png)


There is link to login page. 

![bkp-wp](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/bkp-wp.png)

Fine, now let’s try other 2 open ports, start with FTP.

`telnet 10.11.1.15 21`

It’s already been noticed from the nmap output that the FTP allows anonymous login.

![telnet21](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/telnet21.png)

Nothing interesting... let’s try accessing FTP from browser as nmap showed the presence of public directory.

![ftp1](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/ftp1.png)

Under that folder, there is user.txt file, seems interesting.

![ftp2](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/ftp2.png)

And there were list of users in it.

![ftp3](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/ftp3.png)



