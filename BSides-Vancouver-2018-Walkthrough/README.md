# BSides Vancouver: 2018 (Workshop)

*Boot2Root challenge.*   

VM image available [here](https://www.vulnhub.com/entry/bsides-vancouver-2018-workshop,231/#download). 

Boot the image from VM Player or VirtualBox and keep the network settings in bridge-mode for easier IP assignment and then get the attacker (often, Kali) ready.  Would strongly recommend to keep this network out of Internet reach :)

## Enumeration
First, find the IP address of the victim.

> `netdiscover –r 10.11.1.0/24`

<kbd>![netdiscover](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/netdisc.png)</kbd>

Then fire nmap to find the open ports.

> `nmap –sV –sC 10.11.1.15`

<kbd>![nmap](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/nmap1.png)</kbd>

Now we know HTTP running, run nikto.

> `nikto –C all –h 10.11.1.15`

<kbd>![nikto](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/nikto.png)</kbd>

Two catchy things from nikto output: robots.txt and it has entry “/backup_wordpress/” directory.

Let’s check robots.txt file. 

<kbd><img src="https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/robots.png" /></kbd>

Let’s check /backup_wordpress/ directory too. And there is link to login page. 

<kbd>![bkp-wp](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/bkp-wp.png)</kbd>

Fine, now let’s try other 2 open ports, start with FTP.

> `telnet 10.11.1.15 21`

It’s already been noticed from the nmap output that the FTP allows anonymous login.

<kbd>![telnet21](https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/telnet21.png)</kbd>

Nothing interesting... let’s try accessing FTP from browser as nmap showed the presence of public directory.

<kbd><img src="https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/ftp1.png" /></kbd>

Under that folder, there is user.txt file, seems interesting.

<kbd><img src="https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/ftp2.png" /></kbd>

And there were list of users in it.

<kbd><img src="https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/ftp3.png" /></kbd>

Since website uses wordpress, tried wpscan for users enumeration.

> ``wpscan –url http://10.11.1.15/backup_wordpress/  --enumerate u``

The output listed 2 login names, note that john user also present in the list of users taken from FTP.

<kbd><img src="https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/wpenum1.png" /></kbd>

Run wpscan to bruteforce for john’s password.  Since it takes time, give a try to brute forcing SSH with other user names.

> ``wpscan --url http://10.11.1.15/backup_wordpress / --username john --wordlist /usr/share/wordlists/metasploit/password.lst``

<kbd><img src="https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/wpenum2.png" /></kbd>

After a while, we have got the password for user john, this allows login to wordpress.

<kbd><img src="https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/wplogin.png" /></kbd>

On the other side, hydra tries brute forcing the users and hydra finds password for the user anne (seems other users have no SSH keys to login).
> ``hydra –l anne –P /usr/share/wordlists/nmap.lst –t 5 ssh://10.11.1.15``

<kbd><img src="https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/hydra-ssh.png" /></kbd>

## Low Privilege Shell

Login through SSH with user anne and it works as expected.

<kbd><img src="https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/hydra-ssh.png" /></kbd>

 
## Privilege Escalation – Getting Root

Just tried if this user has sudo access and unfortunately, anne has sudo access - sudo –s.

<kbd><img src="https://github.com/malsearchs/CTP-Walkthroughs/blob/master/BSides-Vancouver-2018-Walkthrough/root.png" /></kbd>

Bingo!    Got the flag!! 

But the flag says that there are many ways to get the root.  So there should be an another way to get the root through wordpress logging (remember user John?).
