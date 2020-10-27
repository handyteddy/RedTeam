---
title: "GamingServer THM Walkthrough"
date: 2020-10-22T10:56:59-03:00
draft: false
blogs: ["TryHackMe"]
---

The hack starts with simple port scan 
`Nmap- -sC -sV <machine IP> -oN Default_Scan.txt`

{{< figure src="/images/penetration-test-banner.jpg">}}

The only quick vector for information is port 80 (HTTP) and port 22 (SSH) for attack
A quick look at the web page hosted at 80 `http://<machine_ip>` give us a gaming portal with no much information on the front end

{{< figure src="/images/penetration-test-banner.jpg">}}



but a usually important `view-source:<machine_ip> `reveals what to seems to be a username which we store in a text file for later use

{{< figure src="/images/penetration-test-banner.jpg">}}


**Web enumeration** continued with the “holy Dirbuster” or “ heavenly gobuster” as you please, reveals multiple directories along with

- Dic.lst [seems to be a simple wordlist , should to be downloaded]
- Secretkey [which seems to be private key, should be downloaded]
- Manifesto.txt [very useless manifesto that’s only there to teach you poetry ]


We have a USERNAME & what seems to be an SSH key , What stops us from logging into the server using SSH, since the SSH protocol allows using a SSHKEY as an authentication
so far we set the permissions of the key file to 600 , the was SSH loves it
First we set the permission and secondly we log in

{{< figure src="/images/penetration-test-banner.jpg">}}

`“ssh -i secret_key john@<machine_ip>“ should kick us in`

{{< figure src="/images/penetration-test-banner.jpg">}}



BUT OOPS , the “secretkey” need a password
The only feasible way forward is to crack the password using a password cracker like “JohnTheRipper” ,
but innocent john has no idea of that the KEY is and needs you to convert the file to what it can understand
so therefore we firstly need to convert the SSHKEY to what JohntheRipper understands using an inbuilt John plugin called SSH2JOHN
then we use John to crack the the file using the wordlist we recovered from the webserver

{{< figure src="/images/penetration-test-banner.jpg">}}


Now we have a Username, sshkey, and the password to the sshkey

{{< figure src="/images/penetration-test-banner.jpg">}}

{{< figure src="/images/penetration-test-banner.jpg">}}

### PRIVILEGE ESCALATION
We use a method called **Lxd Privilege Escalation**
Privilege escalation through lxd requires the access of local account, 

Good thing for us since we have SSH access already

Note: the most important condition is that the user should be a member of lxd group.

If you have completed the introductory researching room on TryHackMe
Do your research on **LXD PRIVILEGE ESCALATION** and root the box


and remember 

>### The difference between a noob and a hacker is that a hacker has failed more than a noob has ever tried