---
created: 2024-04-14T16:01
updated: 2024-12-08T03:09
---
This GitHub repository provides a range of search queries, known as "dorks," for Shodan, a powerful tool used to search for Internet-connected devices. The dorks are designed to help security researchers discover potential vulnerabilities and configuration issues in various types of devices such as webcams, routers, and servers. This resource is helpful for those interested in exploring network security and conducting vulnerability scanning, including both beginners and experienced information security professionals. By leveraging this repository, users can improve the security of their own networks and protect against potential attacks.

https://github.com/nullfuzz-pentest/shodan-dorks

scan with shodan on terminal:

![[Pasted image 20240903200306.png]]

Alternatives of shodan:

https://slashdot.org/software/p/Shodan/alternatives

shodan queryes lists: [GitHub - jakejarvis/awesome-shodan-queries: 🔍 A collection of interesting, funny, and depressing search queries to plug into shodan.io 👩‍💻](https://github.com/jakejarvis/awesome-shodan-queries?tab=readme-ov-file)

==! USA SEMPRE VPN  con shodan perchè in caso cadi in un honey point almeno non vieni rintracciato !==

explore things on shodan:

![[Pasted image 20240222170641.png]]

search for a model for a specific server:

![[Pasted image 20240222163618.png]]

use filters: 

![[Pasted image 20240222164051.png]]
output: camera with screenshots

![[Pasted image 20240222170035.png]]
output: everything with a screenshot

![[Pasted image 20240222170105.png]]

output: everything that has a screenshot with "john"

see for filters: 

![[Pasted image 20240222164750.png]]
! THERE AREN'T ALL

![[Pasted image 20240222165045.png]]


by: https://ott3rly.com/mastering-shodan-search-engine/

```
ssl:"Coca-Cola Company"
```

![](https://ott3rly.com/wp-content/uploads/2024/04/shodan-2-1024x521.png)

If you want to exclude results with **403** or **400**, you could use **200** to get like less results:

```
ssl:"Coca-Cola Company" 200
```

![](https://ott3rly.com/wp-content/uploads/2024/04/shodan-3-1-1024x521.png)

Alternatively, for SSL check you could try using `ssl.cert.subject.CN:"target.com"` dork, where **target.com** is your target’s root domain. For this specific case, the Shodan query for the main subdomain will look like this:

```
ssl.cert.subject.CN:"coca-cola.com"
```

## Filtering Results

If you are getting thousands of results, your next goal is just to filter them out to only leave those that are interesting. I usually click on **More…** near **TOP COUNTRIES** or **TOP PORTS**:

![](https://ott3rly.com/wp-content/uploads/2024/04/shodan-4-1024x751.png)

_You will be redirected to the page, where you can basically filter out the results. For example, by the port:

![](https://ott3rly.com/wp-content/uploads/2024/04/shodan-5-1024x477.png)

Another interesting filter is **http.title**:

![](https://ott3rly.com/wp-content/uploads/2024/04/shodan-6-1024x477.png)

## Favicon Searches

Another interesting thing that you can also do – is search by favicon. If you have noticed Coca-Cola has its own favicon. When you have a lot of results, you can click on the icon itself and it will appear at the end of the query with **http.favicon.hash:<*hash*>**:

![](https://ott3rly.com/wp-content/uploads/2024/04/shodan-7-1024x571.png)

Later you could remove other filters, and check if you can enumerate more results just by using this hash. Another way to find this favicon hash is just from the main website by using extensions like [fav-up](https://github.com/pielco11/fav-up).
## Resources For Shodan Dorking

When it comes to getting ideas, I have multiple favorite places to look for. The first one is the [Awesome-Dorks](https://github.com/0xPugal/Awesome-Dorks) repository on GitHub:

![](https://ott3rly.com/wp-content/uploads/2024/04/shodan-8-1024x642.png)

It’s pretty useful for Bug Bounty. There are some extra searches like – by the header. For example, checking for Jenkins – **html:”Dashboard Jenkins” http.component:”jenkins”**. This keyword checks in the HTML. Not all of Shodan dork repos on GitHub are useful since not all of those are made for bug bounties, but this repository is extremely helpful.

The next thing that I also use is checking for [Shodan favicon hashes](https://github.com/sansatart/scrapts/blob/master/shodan-favicon-hashes.csv):

![](https://ott3rly.com/wp-content/uploads/2024/04/shodan-9-1024x628.png)

Certain products, like Jenkins, has their like the same hash everywhere. Atlassian has its own hash and sometimes even multiple different ones.

Lastly, my favorite way to get ideas for Shodan Dorking is from Twitter. I use Google for this – `site:twitter shodan dorks`:

![](https://ott3rly.com/wp-content/uploads/2024/04/shodan-10-1024x628.png)


---

### 🌐 Shodan.io: The Search Engine for Internet-Connected Devices

#### Overview of Shodan.io

**Shodan** is a search engine that scans the entire internet for **open ports**, **services**, and **devices**. It is often referred to as the **Google of IoT**.

#### Getting Started with Shodan

- Register at [https://www.shodan.io/](https://www.shodan.io/)
- Obtain your **API key** for integration with CLI and scripts.
- Use the **Shodan CLI** for faster results:

`shodan init <API_KEY>`

### Shodan Dorking (Advanced Queries)

Examples of powerful queries:

**Detect open MongoDB instances:**

`port:27017 product:MongoDB`

**Expose webcams with no auth:**

`title:"webcamXP" country:"US"`

**Find SCADA systems:**

`port:502 modbus`

**Look for vulnerable routers:**

`product:"D-Link" country:"IN"`

### Shodan API Automation Example

```python
import shodan
api = shodan.Shodan("YOUR_API_KEY")
results = api.search("apache", limit=10)
for result in results['matches']: 
	print(result['ip_str'], result['port'])
```

![[Pasted image 20250615214011.png]]

_This GitHub repository provides a range of Shodan dorks to find vulnerabilities and configuration issues in various types of devices such as webcams, routers, and servers. _

![[Pasted image 20250611164723.png]]

### Shodan Dorks for OSINT, Recon and Bug Bounty

**Exposed Webcams** Finds IP cams running webcamXP software

`http.title:"webcamXP"`

**Open FTP Servers** Finds FTP servers that allow anonymous login

`port:21 anonymous`

**Outdated Operation Systems** Like Finding devices that running Windows 7

`os:"Windows 7"`

**Misconfigured MongoDB Databases** Finds exposed MongoDB instances without authentication

`product:"MongoDB" port:27017`

**Exposed Login Panels** Identifies admin login portals

`http.title:"Admin Login"`

**Specific Geolocation Targets** Finds services exposed in a specific country

`port:22 country:"IN"`

**Apache Servers with Expired SSL in the US** Finds Apache web servers with expired SSL certs in the US

`product:"Apache httpd" ssl:"expired" country:"US"`

**Devices Vulnerable to CVEs (e.g., Confluence CVE-2021–26084)** Finds potentially vulnerable Confluence servers

`http.html:"Atlassian Confluence" port:8090`

**ICS/SCADA Devices** Detects Modbus protocol on industrial systems

`port:502 name:"modbus"`

### Best Shodan Cheatsheets:

- [https://github.com/nullfuzz-pentest/shodan-dorks](https://github.com/nullfuzz-pentest/shodan-dorks)
- [https://github.com/lothos612/shodan](https://github.com/lothos612/shodan)
- [https://github.com/HernanRodriguez1/Dorks-Shodan-2023](https://github.com/HernanRodriguez1/Dorks-Shodan-2023)

---

Other cheat sheet:

https://haxez.org/wp-content/uploads/2022/06/HaXeZ_Shodan_Cheat_Sheet.pdf

https://cheatography.com/sir-slammington/cheat-sheets/shodan/

https://github.com/coreb1t/awesome-pentest-cheat-sheets/blob/master/docs/shodan.md


you can even search on the browser simple things like: **"interesting searches in shodan"**, some example:

**Simple Query:** Enter a simple search term in the search bar. For example, searching for “default password” will return devices using default credentials.

**- Boolean Operators:** Use AND, OR, and NOT to combine or exclude terms.  
**— Example:** `apache AND nginx` will show results containing both terms.

## 5. Using Filters Effectively

**- Geolocation:** Narrow down searches by geolocation. For example, `country:DE city:Berlin` will show devices in Berlin, Germany.  
**- Operating System:** Filter by operating system (e.g., `os:Windows`).  
**- Service:** Search for specific services (e.g., `http.title:”Welcome”` for web pages with “Welcome” in the title).

## 6. Alerts and Monitoring

**- Set Up Alerts:** Create alerts to monitor specific search queries and get notified when new results match your criteria.  
**- Network Monitoring:** Monitor your network’s IP range to identify new devices or changes.

## 7. Practical Examples

**- Finding Vulnerable Devices:** Search for devices with known vulnerabilities by using the `vuln` filter (e.g., `vuln:CVE-2021–44228`).   or <mark style="background: #FFB86CA6;">CVE-2024-30078</mark>
**- Industrial Control Systems:** Identify SCADA/ICS devices using specific search terms or filters.  
**- IoT Devices:** Discover IoT devices like webcams or smart home devices by searching for terms like `webcam` or `”smart home”`.

---

https://www.osintme.com/index.php/2021/01/16/ultimate-osint-with-shodan-100-great-shodan-queries/

https://jakejarvis.medium.com/fascinating-frightening-shodan-search-queries-aka-the-internet-of-sh-t-90d5fa0ffa79

CVE exploit:

https://starlox.medium.com/cve-2024-24919-arbitrary-file-read-in-check-point-vpn-gateways-9568991b4304

search for windows xp:

product:windows xp country:"US"

 **Shodan API:** Use the Shodan API for integrating search functionalities into your applications. This requires an API key, which you can find in your account settings.  
**- Python Libraries:** Utilize libraries like `shodan` in Python to automate searches and data collection.


How to hack IP Cameras easy and fast
https://medium.com/@Threat_Intelligence/how-to-hack-ip-cameras-easy-and-fast-72344c969f80

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
### How Searching by IP Addresses Can Reveal Hidden Gems

#PII

resource:  [How Searching by IP Addresses Can Reveal Hidden Gems | by Cuncis | Medium](https://medium.com/@cuncis/how-searching-by-ip-addresses-can-reveal-hidden-gems-e58b46e6fdc8)

Searching by IP addresses can provide a range of benefits, including access to hidden or restricted content, identification of potential security vulnerabilities, and finding information that may not be easily accessible through traditional searches. By using IP address searches, you can uncover a wealth of valuable information that might not be readily available through standard search methods.

Here are some tips for finding relevant information when searching by IP address in Google:

1. **Use specific search terms:** Include specific keywords related to the type of information you are looking for in your search query. For example, if you are looking for server information, include terms like “server,” “host,” or “network.”
2. **Use advanced search techniques:** Use advanced search techniques, such as using quotation marks to search for exact phrases or using the “site:” search operator to limit your search to specific websites or domains.
3. **Check multiple sources:** Don’t rely solely on Google search results; check other sources, such as online databases or specialized search engines, for more comprehensive results.
4. **Consider the context:** Consider the context of the IP address you are searching for, such as the type of device or server it is associated with, to help narrow down your search results.
5. **Use online tools:** Consider using online tools, such as IP address lookup tools or WHOIS databases, to gather additional information about the IP address.
6. **Verify information:** Always verify the information you find through multiple sources to ensure its accuracy and relevance to your search.

## Searching by IP address can provide a range of benefits, including:

1. **Access to hidden or restricted content:** Some websites or servers may be hidden or restricted from traditional search methods, but can be accessed through an IP address search. This can be useful for finding information that is not readily available through conventional search methods.
2. **Identification of potential security vulnerabilities:** By searching for the IP addresses of devices or servers, you can identify potential security vulnerabilities and take appropriate steps to address them. This can be particularly useful for cybersecurity professionals or those concerned about the security of their own devices.
3. **Finding information that may not be easily accessible through traditional searches:** IP address searches can reveal information about specific devices, servers, and networks that may not be easily accessible through traditional search methods. This can be useful for researchers or those seeking specific information about a particular device or server.
4. **Understanding network topology:** IP address searches can provide insight into the structure and topology of a network, including the number of devices connected to it, the types of devices, and how they are interconnected. This can be useful for network administrators or those interested in understanding how networks are structured.
5. **Troubleshooting network issues:** IP address searches can be used to troubleshoot network issues, such as identifying the source of network congestion or determining whether a particular device is causing network problems.

**_Here is the tool you can use to limit the Google search to only IP addresses that bring interesting results, just visit_** [**_https://0iq.me/gip/_**](https://t.co/Vv51WeMkBx) **_and search it. DONE!_**

![](https://miro.medium.com/v2/resize:fit:700/1*Q6iwIAQU-O6L2buHWPiZvA.png)

![](https://miro.medium.com/v2/resize:fit:700/1*S1XDGNanSCrYHsB2BakZ4w.png)

[@0x21SAFE](https://twitter.com/0x21SAFE) made an amazing web-based tool just for that, you can try it at [https://0iq.me/gip/](https://t.co/Vv51WeMkBx) or [https://github.com/SeifElsallamy/gip](https://github.com/SeifElsallamy/gip)

# Conclusion

Searching by IP address can provide a range of benefits, including access to hidden or restricted content, identification of potential security vulnerabilities, finding information that may not be easily accessible through traditional searches, understanding network topology, and troubleshooting network issues. By using IP address searches, you can uncover a wealth of valuable information that might not be readily available through standard search methods. However, it’s important to use IP address searches responsibly and ethically to mitigate potential risks, such as security, privacy, legal, and ethical risks.

-

_Source based on this tweet:_ [_https://twitter.com/0x0SojalSec/status/1667218161123610625_](https://twitter.com/0x0SojalSec/status/1667218161123610625)


alternative for viewing cams: [Insecam - World biggest online cameras directory](http://insecam.org/)


##  How Shodan Helps me to Find SMTP misconfiguration

resource: [finding Smtp open port using shodan dork | Medium](https://medium.com/@eulex/how-shodan-helps-me-to-find-smtp-misconfiguration-56f63f1116a5)

> ssl:company.com

There wasn’t intersting data, but one thing caused my attention. there was an IP with port 25 open . the subdomain of this IP wasn’t resolved to it.It was old data i think.As i researched about port 25 i founded that is a SMTP port. Just connected to It and i runed some SMTP command.The things was you could only send email From & TO emails that are under the company domain (Employees emails).How i Found this ? just by trying


As is Knowed program functions.There was a Function to create a Channel to Forward email from your email Provider to the APP. The flow was like below

1. Created an Email under company.com
2. It will gave me a Email like somthing@in.company.com
3. i had to set this email in my email provider to forward my incoming Email to this Email.

Now when i send email to milad@mycompany.com My email provider will forward it to somthing@in.company.com and then i will see what i recive in APP.

I used somthing@in.company.com email to send me from admin using SMTP misconfiguration.The Final Command was this


```http
EHLO X  
MAIL FROM:<admin_bugbounty@company.com>  
RCPT TO:<somthing@in.company.com>  
DATA  
Subject: Test Email  
From: admin_bugbounty@company.com  
To: somthing@in.company.com  
This is a test email for PROOF OF CONCEPT .  
.  
QUIT
```



[How to exploit CVE-2024–24919 path traversal | by JEETPAL | Jun, 2024 | Medium](https://medium.com/@jeetpal2007/how-to-exploit-cve-2024-24919-path-traversal-5493c50d2581)

---

**👽 Shodan Endpoint Recon**

#shodan_facet_Analysis 

_**Favicon hash generator —Get the flavicon hash of a website for shodan hunting**_

https://favicon-hash.kmsec.uk/  

Now I am picking the subdomain `sit.urs.earthdata.*.*,` and now we will find more hosts associated with this using shodan.

Go to [shodan.io](https://www.shodan.io/) and execute the following dork


```makefile
hostname:sub3.sub2.sub1.domain.tld
```

![None](https://miro.medium.com/v2/resize:fit:700/1*KF1mqvq1XH1atC5VdOWUEw.png)


Out of the many results, pay more attention to juicy endpoints like `cmr-dashboard.*.*.*`

**🗃️ Shodan Facet Analysis Demo**


```bash
https://www.shodan.io/search/facet?query=hostname%3Asub.domain.tld&facet=ssl.cert.subject.cn
```

![None](https://miro.medium.com/v2/resize:fit:700/1*xdxabCBjSR6LtZxLq_amLQ.png)

> **Hidden Attack Surface Manual Mapping**

Try to combine all these techniques with favicon hash recon as well to find more and more hidden assets that are not tested by the majority.

> https://cybersecuritywriteups.com/finding-hidden-assets-deep-recon-6a40940ea050

After finding potential unique results, do not forget to perform _**vhost fuzzing**_ as well.
<iframe width="1521" height="561" src="https://www.youtube.com/embed/lUUL2dNQI5M" title="How to Look For Virtual Hosts // How To Bug Bounty" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Bonus Tool:

![[Pasted image 20250702122113.png]]

> https://github.com/incogbyte/shosubgo

_Grab Subdomains using Shodan API!_

————————————————————————————————————


-! Updated Directory since 21/04/2'25

## Automated Shodan Recon

source_from: https://medium.com/@loyalonlytoday/776489cf8b6c

#shodan_facet_Analysis

**Scroll down a bit. You will find the installation command. Copy that command.**

![](https://miro.medium.com/v2/resize:fit:788/1*gLyqNbSMue_w1sEqTmSYGA.png)

**Paste it into your Linux machine, then click Enter.**

![](https://miro.medium.com/v2/resize:fit:788/1*bs45gBCj1u0IwaKiT9V5qw.png)

**After progress, confirm whether the tool is successfully installed or not by entering the help command**

> shodanx -h

![](https://miro.medium.com/v2/resize:fit:788/1*9nKgs928RLRNOcK69u0HbA.png)

**Now run this tool against your target organization with the below command**⬇️

shodanx org --organization yorutarget -o sodanx_result_org

**This tool uses the default facet as IP**

![](https://miro.medium.com/v2/resize:fit:788/1*XIgh_4UxJ46mwmRTPGHcLQ.png)

**You find Shodan facets in the link below**

> https://www.shodan.io/search/facet

![](https://miro.medium.com/v2/resize:fit:788/1*Xdr_soq67oISwsIZkIQxSg.png)

**Now I’m entering the ASN Facet**

```bash
shodanx org --organization yourtargetorganizaton --facet asn -ra  -o sodanx_result_yourtargetorganization
```

**RESULT** ⬇️

![](https://miro.medium.com/v2/resize:fit:788/1*37VOEwJD0Ks9P64OPEKD8A.png)

> **_Now run this tool on your target domain_**

```bash
shodanx domain -d yourtarget.com --facet asn -ra -o shodanx_results_yourtarget.com
```

```bash
shodanx domain -d yourtarget.com --facet asn -ra -o shodanx_results_yourtarget.com
```

```bash
shodanx cidr -c 1.1.1.1/24 -o shdoanx_result_yourtarget_cidr
```

```bash
shodanx ssl -sq ssl.cert.issuer.cn:yourtarget.com -fct asn -ra -o shodanx_result_ssl_yourtaget
```

**_Shodan quiries list_**⬇️

[Awesome Shodan Querries](https://github.com/jakejarvis/awesome-shodan-queries)

```bash
shodanx custom -cq hostname:"yorutarget.com" -fct port -ra -o shodanx_result_custom_query
```

**To update this tool** ⬇️

>  shodanx update

---

### I Found More Than 30 Vulnerabilities in HackerOne Public Programs.

Write-up: https://medium.com/@x0_h0nda/i-found-more-than-30-vulnerabilities-in-hackerone-public-programs-e9067988b3a0

#easy_boounty

First thing bug **information disclosure**:

I got more than one subdomain from [_SecurityTrails_](https://securitytrails.com/), this is a great site to get a lot of subdomains, then go to this site [_httpstatus_](https://httpstatus.io/) and this site in short you enter the subdomain and then it shows you whether the site is 200 or 404 or 503 This is the status of the page and here I search manually. In this case, I use manual and not automation.

First while I am hunting I go to each page separately and find out which technology the page is running on and then I fuzz this was your way to find the information disclosure vulnerability first know which ==CMS== the site is running on, you will know that about the **wappalyzer** extension downloaded to the browser then go to [https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content/CMS](https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content/CMS) and choose the CMS you are running on and it will find paths you did not knew before.

You can also search in **shodan** by this dork

```vb
Ssl.cert.subject.CN:”*.taget.com” www-authenticate: NTLM`
```

You will get “**_401 Unauthorized_**” pages. Intercept the request in Burp Suite  
And using the extension in Burp Suite which is **NTLM Challenge Decoder**  
I was able to detect:

```
Target computer name  
Domain name  
Windows version  
DNS information  
Timestamps
```

But unfortunately it was closed _Duplicate_

![](https://miro.medium.com/v2/resize:fit:311/1*vJNJPtS8A_cubApLs5wzoA.png)

![](https://miro.medium.com/v2/resize:fit:305/1*NNmqxTJt-7QPsbb6NrAM6Q.png)

![](https://miro.medium.com/v2/resize:fit:314/1*sF3aRGUzB8pCLAD5OP6kaw.png)

![](https://miro.medium.com/v2/resize:fit:315/1*AuS3oGIqgHFejiyE5Wep4Q.png)

![](https://miro.medium.com/v2/resize:fit:316/1*fbf4ECl2-nnDoxg_Jp2hKQ.png)

![](https://miro.medium.com/v2/resize:fit:311/1*OvIuUbuHylziYren0Va3WQ.png)

![](https://miro.medium.com/v2/resize:fit:788/1*GaurVAMQKIH5xyhWJco1tg.png)

2. The second thing is **XSS and injection vulnerabilities**:

In these vulnerabilities, I always use automation to get an endpoint, and these tools such as **waybackurl**, **katana**, and **arjun**, this will show you the parameter, and through it you can inject the payload.

After deep searching, I found out that the endpoint is _Index.jsp_,

and a piece of advice from me, if you find any endpoint ending with _.jsp_, copy the link and go to the **arjun tool**, and use this wordlist [https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/burp-parameter-names.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/burp-parameter-names.txt) 

I used this, then try the injection payload such as **XSS and HTML injection**. Wow, through this method, I got _3 injection vulnerabilities_ in different sites

![](https://miro.medium.com/v2/resize:fit:788/1*nOV9HfzcyptjmbqQWtAXwA.png)

![](https://miro.medium.com/v2/resize:fit:308/1*vheXa7i7xemyUe2UC7Covw.png)

![](https://miro.medium.com/v2/resize:fit:299/1*kRzUirKnjhtMdISWSKN1Eg.png)

![](https://miro.medium.com/v2/resize:fit:306/1*Z0630N0vpyxY7tPFAN113Q.png)

![](https://miro.medium.com/v2/resize:fit:312/1*ctyHn38ZbUHKKBHmpmjeTQ.png)

3. The third thing is the **RCE (Command Injection) vulnerability**:

> _CVE-2023–36845_

_there are a lot of youtube POCs:_

Mass hunting for this CVE:

>https://www.youtube.com/watch?v=OJN29QPqjG4

https://www.youtube.com/watch?v=UDjj-4oRjqg

**CVE-2023-36845** is a **Remote Code Execution (RCE) vulnerability** affecting **Juniper Networks Junos OS** on **EX Series and SRX Series** devices. The flaw allows **unauthenticated attackers** to modify the PHP execution environment by manipulating the `PHPRC` variable, enabling them to **inject and execute arbitrary code**

### **How It Works**

- The vulnerability exists in **J-Web**, the web-based management interface of Junos OS.
    
- Attackers can send **crafted HTTP requests** that modify PHP settings.
    
- By setting `PHPRC` to an arbitrary file, they can **execute malicious code remotely**.
### **Impact**

- **Critical severity** (CVSS score: **9.8**).
    
- Can lead to **full system compromise**.
    
- Exploitable **without authentication**, making it highly dangerous.
### **Affected Versions**

- **EX Series & SRX Series** devices running Junos OS **prior to specific patched versions**.
    
- Versions **before 20.4R3-S9, 21.2R3-S7, 22.4R3**, and others.


The first thing you have to do is search using shodan, and this is the dork

```vb
Ssl.cert.subject.CN:”*.target.com” http.favicon.hash:2141724739
```

When you get the **IP**, try this:

```bash
curl -kv “https://target.com/about.php?PHPRC=/dev/fd/0" — data-binary ‘auto_prepend_file=”/etc/passwd”’
```

![[Pasted image 20250422123600.png]]
But unfortunately I also received three **Duplicates** and it was closed

![](https://miro.medium.com/v2/resize:fit:309/1*biecaxmaoVfUJiiWtkcbzg.png)

4. The last thing is the vulnerabilities in **Program U.S. Dept Of Defense**:

> At first I was afraid of this program because it has 28802 Reports resolved. The subject was difficult because I am competing with this number of hunters.

But what happened for it to accept **26 valid** vulnerabilities? Let me explain to you my method for finding all these vulnerabilities. 

First, I knew the scope in order to get all the subdomains. I had to know that any domain ends with **_.mil_** . I searched on **_Google dork_**, **_shodan_**, [https://subdomainfinder.c99.nl/scans/2024-07-23/fallguys.com](https://subdomainfinder.c99.nl/scans/2024-07-23/fallguys.com) , [https://securitytrails.com/](https://securitytrails.com/), **_subfinder_**, **_assetfinder_** and **_amass_**. All of these tools enabled me to get more than 2.5 million subdomains, which is very amazing. 

>However, all of this took about three days. 

![](https://miro.medium.com/v2/resize:fit:788/1*E9z6MkVvb5vVhk1e-Oe9Vg.png)

After that, through tools such as **waybackurl** and **katana**, I was able to find an endpoint that exposes dangerous information. I was also able to do injections inside pages. It gave me more than 25 vulnerabilities. Through this, I topped the Hall of Fame in 2024 and 2025 as the most vulnerabilities discoverer in these two years.

![](https://miro.medium.com/v2/resize:fit:788/1*W3EZ49S9BWr3GpNccxcj5w.png)

![](https://miro.medium.com/v2/resize:fit:788/1*0fLR3ng3JOS4TJg5NgtMtg.png)

Follow Creator:

[https://x.com/x0_h0nda](https://x.com/x0_h0nda)

[https://www.linkedin.com/in/mohaned-ahmed-176294308/](https://www.linkedin.com/in/mohaned-ahmed-176294308/)

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
# Shodan Dorks to Find PII Data & Leaks

#easy_boounty ?

from: https://infosecwriteups.com/i-found-50-exploitable-devices-in-1-hour-using-shodan-dorking-49e825ca0f3e

Most guides tell you to use basic queries like

```vbnet
org:"Target Company"
```

**But that's like using a spoon to dig for gold.**

Here's what works:

**Filter #1: http.title**

This finds web pages with specific titles. For example:

```vb
http.title:"Apache Tomcat"
```

_This instantly reveals **thousands of Tomcat servers**, many with default credentials or outdated versions.

**Filter #2: port:**

**Narrow down by open ports.**

**Example**:

```vb
port:8080 http.title:"Jenkins"
```

Boom — **exposed Jenkins dashboards**, ripe for exploitation.

**Filter #3: vuln:**

Shodan can even flag known vulnerabilities:

```vb
vuln:CVE-2021-44228 (Log4j)
```

#### Apache Tomcat Case Study

**Discovery: How a Default Page Screamed 'Hack Me'**

I searched:

```vb
http.title:"Apache Tomcat" port:8080
```

**First result:** A server with **/manager/html** accessible — no password.

**CVEs Found:**

- **CVE-2017–12615** (Remote Code Execution)
- **CVE-2020–1938** (Ghostcat — File Read/Inclusion)
- **Default Credentials (admin:admin)**

**Impact:** Full server takeover in **3 steps**:

1. Access `/manager/html`.
2. Upload a malicious WAR file.
3. Execute commands as root.


4. Identifies open directories that may expose sensitive files.

```vb
hostname:"example.com" http.title:"index of /"
```

![[Pasted image 20250610144910.png]]

2. Searches for pages containing the term "password"

```vb
hostname:"example.com" http.html:"password"
```

![[Pasted image 20250610145026.png]]

3. Finds administrative interfaces that could be vulnerable.

```vb
hostname:"example.com" http.html:"admin"
```

![[Pasted image 20250610145101.png]]

4. Locates login pages that might be vulnerable to brute-force attack

```vb
hostname:"example.com" http.html:"login"
```

![[Pasted image 20250610145248.png]]

5. Identifies pages that reference user information

```vbnet
hostname:"example.com" http.html:"user"
```

![[Pasted image 20250610145315.png]]


```vbnet
hostname:"example.com" http.html:"email"
```

Looks for pages containing Social Security Numbers

```vbnet
hostname:"example.com" http.html:"ssn"
```

Identifies pages displaying physical addresses, potentially revealing PII.

```
hostname:"example.com" http.html:"address"
```

Searches for pages listing phone numbers

```vbnet
hostname:"example.com" http.html:"phone"
```

Finds pages containing invoices

```vbnet
hostname:"example.com" http.html:"invoice"
```

---
## Hack Dashboards with Shodan:

[resource from](https://osintteam.blog/attacker-secrets-hacking-dashboards-easily-part3-14b85ba0d38e)

## 1️⃣ OpenVPN

intitle:"Test OpenVPN Status Monitor"

![](https://miro.medium.com/v2/resize:fit:858/1*PSVBcnGtpoWDhi1P--JzIg.png)

![](https://miro.medium.com/v2/resize:fit:2359/1*6ZRGt2NPUJpznlKs74cvGg.png)
## 2️⃣ Open Hardware Monitor

intitle:"Open Hardware…

![](https://miro.medium.com/v2/resize:fit:671/1*y39KHwmZx7VSXCWvLwfEMw.png)

![](https://miro.medium.com/v2/resize:fit:516/1*oiM1E0tL66MaMJ-BOFDPAQ.png)

## 3️⃣ PowerMTA Web Monitor

intitle:"PowerMTA Web Monitor"

![](https://miro.medium.com/v2/resize:fit:946/1*n8eqeZkbJih_iw54MNWZxg.png)

![](https://miro.medium.com/v2/resize:fit:2374/1*B988SV-hgx-GWBq5x2ghCA.png)

## 4️⃣ Web Image Monitor

intitle:"Web Image Monitor" ext:cgi

![](https://miro.medium.com/v2/resize:fit:963/1*WLBYEiN0kaHzIfIPbZR8vQ.png)

![](https://miro.medium.com/v2/resize:fit:2131/1*hvDqAkD_Qg6N-Da-Zw2vbw.png)


![](https://miro.medium.com/v2/resize:fit:1668/1*0dIQ6LIy-OXJt_vMLz0omA.png)

![](https://miro.medium.com/v2/resize:fit:805/1*AtAk_mX6UH73h0nyly28CQ.png)

## 5️⃣ PowerDNS

"PowerDNS Authoritative Server" "Uptime" "Backend query load"

![](https://miro.medium.com/v2/resize:fit:1525/1*a2Mn1mtq7fnk-iCi18usUg.png)

![](https://miro.medium.com/v2/resize:fit:1514/1*5dqaoinux4Rv4aul4Z188Q.png)

## 6️⃣ DMR Server Monitor

"Home" "Masters" "OpenBridge" "The Regents of"

![](https://miro.medium.com/v2/resize:fit:963/1*gbogw8lagV1IKeH6VhmgUg.png)

![](https://miro.medium.com/v2/resize:fit:1421/1*eu61LEBN3F7648RE_nUemQ.png)

## 7️⃣ Youless Energy Monitor

intitle:"Youless energy monitor" "Elektriciteitsmeter"

![](https://miro.medium.com/v2/resize:fit:875/1*Pmwu66xRIRFeUyP799-KOw.png)

## 8️⃣ HBLink Monitor

intitle:"HBLink monitor" "The Regents of the"

![](https://miro.medium.com/v2/resize:fit:875/1*O5vhXkOkKejbMUbJNkS3dA.png)

## 9️⃣ FDMR Monitor

intitle:"FDMR monitor" "The Regents of the" -login

![](https://miro.medium.com/v2/resize:fit:875/1*oBCDGbePTE0piRN2gY9IvQ.png)

## 🔟 Room Alert

"© Copyright 2024 AVTECH Software, Inc." "Please wait while the page is loaded"

![](https://miro.medium.com/v2/resize:fit:875/1*bshvagulprF_-b07Tjrfig.png)

![](https://miro.medium.com/v2/resize:fit:1983/1*zJeos4PMLgkKEsQIfTlApg.png)

![](https://miro.medium.com/v2/resize:fit:1748/1*oAkWBlD9VPU7umU5mpvUbg.png)


---


https://claroty.com/team82/research/pivoting-from-wan-to-lan-synology-bc500-ip-camera 


[Advanced Shodan Use for Tracking Down Vulnerable Components | by Ofri Ouzan | Medium](https://medium.com/@ofriouzan/advanced-shodan-use-for-tracking-down-vulnerable-components-7b6927a87c45)

search for this vulnerabilities:

source:  https://youtu.be/r-fqJaHRoHs

<iframe width="1266" height="480" src="https://www.youtube.com/embed/r-fqJaHRoHs" title="Hacking Any Windows Machine With IPv6 Vulnerability (CVE 2024-38063)(Ethical Hacking)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

_Description. Windows Line Printer Daemon (LPD) Service Remote Code Execution Vulnerability. References.
							cve-2024-38199

**the vulnerability is not in IPv6 Protocol itself, but rather is in microsoft implementation of it within the windows kernel**

![[Pasted image 20240817070925.png]]

![[Pasted image 20240817070812.png]]


---

## Nuclei

for "i" in ls  

do this command:

![[{F95A6375-454A-475B-A629-D375B112D8C1}.png]]

- `all.txt`
- `xaa`, `xab`, `xac`, `xad`, `xae`, `xaf`, `xag`, `xah`, `xai`, `xaj`, `xak`, `xal`, `xam`, `xan`, `xao`, `xap`, `xaq`, `xar`, `xas`, `xat`, `xau`, `xav`, `xaw`

# Creating a Nuclei Template:

#source: [Video POC make Nuclei Templates](https://www.youtube.com/watch?v=VjVv1EkpMys)

<iframe width="1513" height="569" src="https://www.youtube.com/embed/Sb0EXagr5Nk" title="Nuclei Template Editor" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
Sign up here: [https://templates.nuclei.sh/](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbS1yRUR3OG9KUi1SZlZkcHJ2LUEtNHVqQkllQXxBQ3Jtc0trTmJKU1loSEhVT1pDWTVmdDJfVWdPSGN5dVJySEt2RDlQNjYyejVpRmgya3RnWDdHVExWcHZpZTUycUdpeFdJSnRUX2d1SnZIVTIwa2Y2T2xOcktQSVdoQnFMakZhTEdJclFKTnF4ZllnRFg5ZHVNSQ&q=https%3A%2F%2Ftemplates.nuclei.sh%2F&v=Sb0EXagr5Nk)
Learn more: [https://docs.nuclei.sh/editor/introdu...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa21WMGoxRHc0MUhhUkxSS2FBYWNJYU92SlRGUXxBQ3Jtc0ttVENjaEcwMnFGM2pnN2VZM2VIaVJ5Vk52dTRTc1g3WVBtMDA0Q3VtNlgxZGI5cW43QS1sRWdTNHpKUkpJSVBvem5zTlZtNDdxNHJvQzhCWGd1eDkwOFJoQ096TnkwaTVwekxTOFZ2dy14VlZzRnlvNA&q=https%3A%2F%2Fdocs.nuclei.sh%2Feditor%2Fintroduction&v=Sb0EXagr5Nk)



----

CCTV camera: https://www.youtube.com/watch?v=P7e0DJh-OO4

---

![[{5E76E0FA-1F48-4987-99AF-BCF074C114F1}.png]]

if you want to find quickly nuclei templates from its default list you can try from here according to your need just click on what u want and it give u command for direct run:

https://nuclei-templates.netlify.app/#q=pdteam

## Templates:

> https://github.com/coffinxp/nuclei-templates 

```bash
#how to use in bug bounty programs:
subfinder -d xyz.com -all  | nuclei -t crlf.yaml -rl 50
subfinder -d xyz.com -all  | nuclei -t openRedirect.yaml -rl 100
subfinder -d xyz.com -all  | nuclei -t iis.yaml
subfinder -d xyz.com -all  | nuclei -t cors.yaml -rl 100
subfinder -d xyz.com -all  | waybackurls | gf sqli | uro | nuclei -t errorsqli.yaml -rl 50
```

```
cat domains.txt | katana | grep js  | tee js.txt
```

1. `cat domains.txt | katana`: This command uses the `cat` utility to display the contents of the file `domains.txt`. It assumes that `domains.txt` contains a list of domain names or URLs and paths by | to katana to crawl urls from domains
2. `grep js`: The `grep` command is used for pattern matching in text files. In this case, it is searching for lines that contain the ".js" pattern, which indicates JavaScript files. This filters the output only to include lines that mention JavaScript files.
3. `httpx -mc 200`: This command utilizes the `httpx` tool to send HTTP requests and retrieve responses from the filtered URLs. The `-mc 200` option only shows URLs that return a successful HTTP status code of 200 (OK). This filters out URLs that do not exist or return errors.
4. `tee js.txt`: The `tee` command is used to display the output of a command and save it to a file simultaneously. In this case, it saves the filtered URLs that match the previous criteria into a file called `js.txt`.

and after that, you do: httpx -mc 200




```
nuclei -l js.txt -t ~/nuclei-templates/exposures/ -o js_bugs.txt
```

![[{FC4E698A-0735-4A16-861F-038317F84934}.png]]


code :

```
file="js.txt"  
  
# Loop through each line in the file  
while IFS= read -r link  
do  
    # Download the JavaScript file using wget  
    wget "$link"  
done < "$file"
```

```bash
grep -r -E "aws_access_key|aws_secret_key|api key|passwd|pwd|heroku|slack|firebase|swagger|aws_secret_key|aws key|password|ftp password|jdbc|db|sql|secret jet|config|admin|pwd|json|gcp|htaccess|.env|ssh key|.git|access key|secret token|oauth_token|oauth_token_secret|smtp" *.js
```

---


## Search for CVEs:


[CVE-2024-6409 Race Condition RCE in OpenSSH 8.7p1 8.8p1 ,Custom nuclei template (youtube.com)](https://www.youtube.com/watch?v=d2GXxbzKXnc)

&

<iframe width="916" height="515" src="https://www.youtube.com/embed/RA-WN7dEZVI" title="🚨 LIVE DEMO: Exploiting CVE-2024-41713 (Mitel MiCollab Vulnerability) | FOFA, Nuclei &amp; PoC" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>