# How to Route Traffic from WSL to Burp Suite on Windows: A Step-by-Step Guide

## Easily Capture and Analyze WSL Network Traffic Through Burp Suite

### **Introduction**

If you're using Kali Linux on WSL (Windows Subsystem for Linux) and want to capture or analyze its traffic in Burp Suite, you may run into challenges. **Routing traffic from WSL to Burp Suite on a Windows host can be tricky due to firewall restrictions.** In this guide, we'll walk you through the complete process of proxying Kali WSL traffic through Burp Suite installed on your Windows machine.

#### **Understanding WSL and Its Network Structure**

WSL allows you to run Linux distributions directly on Windows without needing a virtual machine or dual boot. While WSL shares networking resources with the Windows host, configuring its traffic to pass through external applications like BurpSuite requires some tweaks.

#### **Prerequisites for Proxying WSL Traffic**

- A Windows machine with WSL installed.
- Kali Linux set up in WSL.
- BurpSuite installed on Windows.

### **Step 1: Install and Set Up Kali Linux on WSL**

1. Open PowerShell as Administrator.
2. Install Kali-linux using:

```css
wsl --install -d kali-linux
```

3. Update and upgrade packages:

```sql
sudo apt update && sudo apt upgrade -y
```

### **Step 2: Install BurpSuite on Windows**

- Download BurpSuite from the official website.
- Follow the installation instructions.
- Launch BurpSuite and ensure it's ready for proxying

### **Step 3: Identifying system IP Addresses**

On the Windows host open PowerShell or Command Prompt and run:

```bash
> ipconfig


Ethernet adapter vEthernet (WSL (Hyper-V firewall)):

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::ad3a:894:590a:190d%43
   IPv4 Address. . . . . . . . . . . : 172.23.96.1
   Subnet Mask . . . . . . . . . . . : 255.255.240.0
   Default Gateway . . . . . . . . . :
```

Locate the IPv4 Address under Ethernet adapter vEthernet (WSL (Hyper-V firewall)) on your Windows machine. Copy this IP address and run a ping test from Kali WSL to check if the host is reachable.

```bash
╭─[coffinxp@Lostsec]─[~]                                                                  (WSL at )─[ 76%]─[21,08:28]
╰─ ping 172.23.96.1
PING 172.23.96.1 (172.23.96.1) 56(84) bytes of data.
64 bytes from 172.23.96.1: icmp_seq=1 ttl=128 time=0.409 ms
64 bytes from 172.23.96.1: icmp_seq=2 ttl=128 time=2.00 ms
64 bytes from 172.23.96.1: icmp_seq=3 ttl=128 time=1.52 ms
64 bytes from 172.23.96.1: icmp_seq=4 ttl=128 time=3.00 ms
64 bytes from 172.23.96.1: icmp_seq=5 ttl=128 time=0.402 ms
64 bytes from 172.23.96.1: icmp_seq=6 ttl=128 time=0.348 ms
64 bytes from 172.23.96.1: icmp_seq=7 ttl=128 time=2.18 ms
64 bytes from 172.23.96.1: icmp_seq=8 ttl=128 time=1.59 ms
64 bytes from 172.23.96.1: icmp_seq=9 ttl=128 time=2.43 ms
64 bytes from 172.23.96.1: icmp_seq=10 ttl=128 time=2.26 ms
```

The first time you try to ping this IP to test connectivity it won't work because the Windows **firewall blocks all incoming requests by default.**

### **Step 4: Configure Windows Network for WSL**

1. Open Windows Defender **Firewall**.
2. Go to Advanced Settings.
3. Disable firewall protection for both Domain, Private and Public networks under the WSL interface by uncheck the vEthernet (WSL (Hyper-V firewall)) box and click apply and Ok.

![None](https://miro.medium.com/v2/resize:fit:700/1*Ht2dvDWZEbow5DNC4_3eiw.png)

Now from Kali WSL try pinging the host's IP address to verify network connectivity. You should see a response indicating that the host is reachable.

### **Step 5: Testing Traffic with wget**

Now test the proxy by running the following wget command in Kali WSL

```bash
 wget https://google.com -e use_proxy=on -e http_proxy=http://172.23.96.1:8080

╭─[coffinxp@Lostsec]─[~]                                                                  (WSL at )─[ 68%]─[21,09:25]
╰─ wget https://google.com -e use_proxy=on -e http_proxy=http://172.23.96.1:8080
--2025-03-21 09:25:45--  https://google.com/
Resolving google.com (google.com)... 142.250.77.238, 2404:6800:4002:81f::200e
Connecting to google.com (google.com)|142.250.77.238|:443... connected.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://www.google.com/ [following]
--2025-03-21 09:25:47--  https://www.google.com/
Resolving www.google.com (www.google.com)... 172.217.167.196, 2404:6800:4002:81d::2004
Connecting to www.google.com (www.google.com)|172.217.167.196|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: 'index.html.1'

index.html.1                      [ <=>                                             

2025-03-21 09:25:47 (2.02 MB/s) - 'index.html.1' saved [19101]
```

If you see a response like this, **it means your proxy is successfully configured with the WSL IP**

### **Step 6: Configure BurpSuite as a Proxy** listener

1. Open BurpSuite and go to Proxy → Options.
2. add a new listener on [host_ip]:8080.
3. Click on specific address and select WSL host IP address.

![None](https://miro.medium.com/v2/resize:fit:700/1*gfz7lryZSugE4yDQpyb2qg.png)

To further confirm that Burp Suite is capturing the requests try using a tool like ffuf or sqlmap with the proxy configured For example:

```bash
sqlmap -u 'http://testphp.vulnweb.com/artists.php?artist=1' --dbs --batch --proxy=http://hostip:8080  
```

![None](https://miro.medium.com/v2/resize:fit:700/1*3vVDw5oWi5C2847z-8QTCA.png)

Now you'll see all the WSL traffic routed through Burp Suite, allowing you to inspect, intercept and analyze the requests.

#### Why Use BurpSuite for Proxying WSL Traffic?

- Analyze HTTP/HTTPS requests in real-time.
- Useful for penetration testing.
- Debug and monitor web applications.
- Inspect API requests and responses

### **Conclusion**

By following these steps you can successfully route traffic from WSL through Burp Suite, allowing for effective interception and analysis. Whether for penetration testing, debugging or learning, this setup is invaluable for security professionals

---