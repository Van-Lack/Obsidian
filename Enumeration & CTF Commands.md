---
created: 2024-05-16T17:28
updated: 2025-01-08T12:50
---

![[Pasted image 20240516172851.png]]![[Pasted image 20240516172953.png]]

A fast and user friendly way to interact with smb shares:
****[smbclient-ng](https://github.com/p0dalirius/smbclient-ng)****
![[example.png]]
Detect smb port:

![[Pasted image 20240204145423.png]]

Login into smb:

![[Pasted image 20240204145332.png]]

-> smbclient //10.129.118.191/WorkShares

https://github.com/t3l3machus/PowerShell-Obfuscation-Bible


network pivot:

ligolo-ng & chisel


see for anatomy of reverse shell:

https://www.youtube.com/watch?v=Z3EOEsu7ymc


[Dirsearch Accuracy Is Now Like a Sniper (Blazingly Fast) (youtube.com)](https://www.youtube.com/watch?v=goN3uadwfZk) 


## Tcp tunneling:

https://github.com/fiddyschmitt/File-Tunnel

Tunnel TCP connections through a file.


## Download


Portable executables for Windows, Linux and Mac can be found over in the [releases](https://github.com/fiddyschmitt/File-Tunnel/releases/latest) section.

![[Pasted image 20240821101826.png]]

_what is WolfCMS Exploit?_

28 ago 2015 — Every registered users who have access of upload functionality can upload an Arbitrary File Upload To perform Command Execution.

## Mastering WolfCMS Exploit - SickOs v1 CTF Penetration Testing Walkthrough


<iframe width="707" height="480" src="https://www.youtube.com/embed/JACKkCjPbiU" title="Mastering WolfCMS Exploit - SickOs v1 CTF Penetration Testing Walkthrough" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---


#### Installing P0f

On Linux systems, installing P0f is straightforward:

1. **Using a Package Manager:**

`sudo apt-get install p0f  # For Debian-based systems`

**2. From Source:** Download the source code from the official repository or website, then compile it:

```
tar -xvzf p0f.tar.gz 
cd p0f 
make 
sudo make install
```

p0F is pre-installed in Kali, so no need to download and install it. p0F is not available from the GUI in Kali, but it is built-in and is accessed via the command line.

Since its binaries (executable files) are in the /usr/bin directory and /usr/bin is in our PATH variable, we can access it from the command line from anywhere in Kali. Let's take a look at its help file by typing (please note that the middle character is the number zero 0, not the letter o):

**kali> p0f -h**

![None](https://miro.medium.com/v2/resize:fit:700/1*oV1hd5dsuGr1Lmlw0RW9Rw.png)

**p0f help**

As you can see above, p0f has a brief, but complete help file. The first section addresses the network interface options, the second section the operating mode, and the third section the performance options.

In its simplest form, you can run p0f by simply typing the command followed by an -i (interface) and then the name of the interface you want p0f to listen on — in this case — eth0:

**kali> sudo p0f -i eth0**

When we start p0f, it begins listening on the designated interface and then decoding the information from each packet as it appears.

Let's try navigating to our Kali system with a Firefox browser.

![None](https://miro.medium.com/v2/resize:fit:700/1*QmobISwfm_zCnCo5NRPctA.png)

				**p0f on microsoft.com**

As you can see, at first p0f opens, then loads 322 signatures, listens on eth0, and then enters the main event loop. When it sees a packet at the interface, it begins to decode it. First, it tells us what IP address and port it is coming from and the TCP flag that is set (SYN). Next, it tells us what OS fits the fingerprint for this packet (Linux 2.2.x-3.x). In the next stanza, it tells us what the link is (Ethernet or modem) as well as the MTU (1500).

![None](https://miro.medium.com/v2/resize:fit:700/1*W8gTxJfMcUuf3zTYEov9lw.png)

				**Target os and ip**

If we scroll down, we can see the details above that describe the target OS (Windows XP) and its raw signature.

From the same system, if we use Microsoft's Internet Explorer 9 to send packets to our Kali, you can see that p0f fingerprints the browser as "MSIE 8 or newer."

p0F can also determine the uptime of the target system. This can be key in determining how long it has been since the system admins patched the target system (security patches usually require a reboot of the system). If we scan down the output from the Kali decoding, we can see that p0f has determined that the system has been up 6 days, 16 hours and 16 minutes. Very helpful information!

![None](https://miro.medium.com/v2/resize:fit:700/1*zuAGJIX4WoNbF1I2RpL8RQ.png)

				**uptime of system**

#### Summary

Before beginning the attack, it is crucial to learn as much as possible about the target to increase the chance of success. There are numerous tools we can use to gain information without ever contacting the target from sources that have previously collected this information. These are known as passive reconnaissance techniques or sometimes referred to as open source intelligence (OSINT). Google, Netcraft, Shodan, DNS all have valuable information that can assist in tailoring your attack. A tool like p0F is capable of determining the target operating system, browser, user agent and uptime, if we can entice the target to our website. All of this information will be critical in determining which approach will most likely be successful in our attack.👍

#### Advantages of P0f

- **Stealth:** No risk of triggering intrusion detection systems.
- **Efficiency:** Analyzes traffic without affecting network performance.
- **Versatility:** Supports a range of use cases beyond OS detection.

#### Limitations of P0f

1. **Passive Only:** Requires traffic to be present for analysis.
2. **Database Dependency:** Accuracy depends on the quality of the fingerprint database.
3. **Limited Modern OS Support:** May struggle with newer operating systems if the database isn't updated.