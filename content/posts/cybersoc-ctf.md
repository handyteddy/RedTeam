---
title: "CyberSOC CTF Challenge"
date: 2020-10-25T03:09:07-03:00
draft: false
cover: "images/cbc.png"
blogs: ["CTF_Challenges"]
---


## CyberSOC CTF Challenge 
{{< image src="images/cbc.png">}}
{{< figure src="/images/cybersocctf/cbc.png" title="Steve Francia" >}}
The CyberSOC challenge is a quite interesting test of **patience, perseverance, and the stock ability to think**
The challenge claims a link is hidden in the provided image, A link to which the challenge will be started.

{{< image src="images/hidden.png">}}

> Fusce pharetra suscipit orci nec tempor. Quisque vitae sem sit amet sem mollis consequat. Sed at imperdiet lorem. Vestibulum pharetra faucibus odio, ac feugiat tellus sollicitudin at. Pellentesque varius tristique mi imperdiet dapibus. Duis orci odio, sodales lacinia venenatis sit amet, feugiat et diam.


My initial thought was **"Security by Obscurity"** which led me to believe certain information was hidden in the images either in metadata or hidden in bits of unused data of the image file. 
So therefore i tried extracting metadata using Exiftool, and Checking for hidden data using Steg-analysis like Steghide and Stegoveritas …All to no Avail 

Paying a little attention the image, we notice a `Join🛡️.ws` , The Shield emoji is similar to Cybersoc logo, so i initially started by substituting the "Shield emoji" for "Cybersoc"
Below is the list of domains i tried 
`joincybersoc.ws` `joincs.ws` `joincybersocafrica.ws` `join.cybersoc.ws`
amongst several others domain iterations,
after several minutes i paid attention to the .ws landing page and noticed the "Register Your .WS Domain 🙂.ws" as in the image below

{{< image src="images/wslanding.png">}}

I immediately did a quick research and found informations related to emoji Domains
which converts an emoji to punnycode and subtitute the converted character for a domain name

{{< image src="images/punnycode.png">}}

#### Heading to xn- join-3683c.ws
We are greeted to begin  with the CTF challenge


## Challenge 1
The question asks, **What you see ?**, which implies we aren't seeing what we ought to see
Getting curious i dragged the mouse around and figured what the flag was

{{< image src="images/whatyousee.png">}}

## Challenge 2

Zero is more than nothing : and we are provided with binary numbers
 
`01111001 01101111 01110101 01110100 01101000 01101001 01101110 01101011 01111001 01101111 01110101 01110011 01101101 01100001 01110010 01110100`

A quick Search of Binary decoder give's several options which inturn leads to an anwser

{{< image src="images/crypti.png">}}

## Challenge 3
if a **flag is more than just cloth and ink**, What else could it be ?

{{< image src="images/flags.png">}}

Asked to crack a code and all we were given are flags, So most definitely we need text characters from the flags.

Seeing flags i immediately knew we were dealing with a **alpha-2 codes**
 >which are two-letter country codes defined as part of the ISO 3166 standard published by the International Organization for Standardization (ISO), to represent countries, dependent territories, and special areas of geographical interest.

An example of alpha 2 code below

{{< image src="images/alpha2code.png">}}

So substituting our flags for the alpah2code equivalent should give us our flag,
but one solution leads to another problem 
How can we identify the nationality of the flags?, All i did was crop out each individual flag and did **google reverse image search** to figure out the Country the flag belonged to


## Challenge 4
Download and run "run.exe"

Well curiosity kills, most people would definitely click a link that says **Don't click me** which led me to be quite wary of what an executable with the name "run" does ,

Needed to be sure of the app's content and what it intends to do, so simple reverse engineering with the `strings run.exe` reveal a hidden flag.
Easy right ?


## Challenge 5
Geolocation 

Provided with a picture and the sole aim of figuring where the photograph was taken, i had little experience from working as an amnesty international decoder 

>Amnesty Decoders is a global network of digital volunteers, all using 
 their computers or phone to help Amnesty's researchers sort through pictures, documents and information and track and expose human rights violations

{{< image src="images/geolocation.png">}}

After messing around with reverse image searches with no results , Paying more attention i noticed a quite popular Landmark to the far left 
_The Ikoyi Link Bridge , Two Power line's , and what seems to be like a dock for boats_

So ignoring the regular directional map and utilizing the rich satellite imagery we come up with a obvious flag

{{< image src="images/satelite.png">}}

## Challenge 6
Satellite Point Of View was a straight forward test of sight - _lol_

The challenge was about Picking out glarings structural buildings that had similarities to characters in the English alphabet 

Each "Datum Point" has a single character of it's own and the addition of all datum point is the flag

{{< image src="images/datum.png">}}

## Challenge 7
when the "geolocation" image was captured?

Metadata summarizes basic information about data, and stores that information in unused bits in the file 

>ExifTool is a free and open-source software program for reading,  writing, and manipulating image, audio, video, and PDF metadata


Running exiftools on the picture reveals

{{< image src="images/exiftools.png">}}

Clearly we can see When the picture was taken

## Challenge 8
which device was used to capture the image?
A quick search of SM-F700F reveals the flag


{{< image src="images/device.png">}}

Congratulations on completion of the challenge and remember

>#### The difference between a noob and a hacker is that a hacker has failed more than a noob has ever tried
