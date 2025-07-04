
![[Pasted image 20250613204303.png]]

### A. Email Enumeration

**Method 1: Using tool — theHarvester**

```bash
python3 theHarvester.py -d target.com -b all
```

![None](https://miro.medium.com/v2/resize:fit:700/1*AXAa57O84zw4xyn9sgQr8Q.jpeg)

**Method 2:** **Google Dorking**

```js
site:target.com "@target.com"
"email" OR "contact" OR "admin" "@example.com"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*S07yAu1G7olV-BSrJ8b_5A.jpeg)


## G-Dorking Methodology:

by [AbhirupKonwar - Google Dorking Lists:](https://medium.com/@abhirupkonwar04/list/advanced-google-dorking-8817a1178836)

Google Dorks Cheat Sheet for Hidden Paths & Exposed Files - ASP NET configuration files - open directory listings - Apache password files - SVN version control directories - open error log file and more. [https://github.com/sudosu01/-Google-Dorks-Cheat-Sheet-for-Hidden-Paths-Exposed-Files](https://github.com/sudosu01/-Google-Dorks-Cheat-Sheet-for-Hidden-Paths-Exposed-Files)
Google Dork.py:

```python
# Impostazioni
TARGET = "bytebucket.org"  # Inserisci qui il dominio target
DORKS_FILE = "C:\\Users\\NullSEC\\Desktop\\Dork.txt"
RESULTS_FILE = "C:\\Users\\NullSEC\\Desktop\\results.txt"

headers = {
    "User-Agent": "Mozilla/5.0"
}

def search_duckduckgo(query):
    url = "https://html.duckduckgo.com/html/"
    data = {"q": query}
    response = requests.post(url, data=data, headers=headers)
    soup = BeautifulSoup(response.text, "html.parser")
    results = []

    for link in soup.select('.result__url'):
        href = link.get('href')
        if href:
            results.append(href)
    return results

def main():
    with open(DORKS_FILE, "r") as f:
        dorks = [line.strip() for line in f if line.strip()]

    all_results = []

    for dork in dorks:
        query = f"site:{TARGET} {dork}"
        print(f"[+] Ricerca: {query}")
        links = search_duckduckgo(query)
        matched_links = [link for link in links if any(k in link for k in dork.split())]

        for link in matched_links:
            all_results.append(f"{query} --> {link}")

        time.sleep(2)  # Per evitare di essere bloccati

    # Salvataggio dei risultati
    with open(RESULTS_FILE, "w", encoding="utf-8") as f:
        for item in all_results:
            f.write(item + "\n")

    print(f"\n[✓] Risultati salvati nel file: {RESULTS_FILE}")

if __name__ == "__main__":
    main()

```


```bash
FIND EXPOSED FILES
site:domain.com (ext:pdf OR ext:doc OR ext:docx OR ext:zip OR ext:bak OR ext:txt OR ext:ppt OR ext:pptx OR ext:xls OR ext:xlsx)

FIND PARAMETERS
site:*.domain.com inurl:& inurl:? inurl:=

FIND PARAMETERS (SERVLETS)
site:*.domain.com inurl:servlet
site:*.domain.com inurl:servlet inurl:&
site:*.domain.com inurl:servlet inurl:=
site:*.domain.com inurl:servlet inurl:?
site:*.domain.com inurl:servlet inurl:& inurl:= inurl:?

Previously Vulnerable Parameters
site:openbugbounty.org inurl:reports "dell.com"
site:hackerone.com inurl:reports "XSS" "domain.com"

FILE UPLOAD
site:domain.com "Choose File"
site:domain.com "No file chosen"
site:domain.com "Upload"
site:domain.com "Upload here"
site:domain.com "Upload a file"
site:domain.com "Please upload your"

SENSITIVE FILES
site:domain.com "INTERNAL USE ONLY"
site:domain.com "PRIVATE AND CONFIDENTIAL"
site:domain.com "ONLY FOR"
site:domain.com "HIGHLY CONFIDENTIAL"
site:domain.com "CONFIDENTIAL"
site:domain.com "STRICTLY CONFIDENTIAL"
site:domain.com "SENSITIVE"
site:domain.com "COMPANY SENSITIVE"
site:domain.com "PRIVATE ASSET"

site:domain.com inurl:view inurl:private ext:pdf
site:domain.com inurl:upload ext:pdf
site:domain.com inurl:uploads ext:pdf
site:domain.com inurl:internal ext:pdf
site:domain.com inurl:storage ext:pdf
site:domain.com inurl:view inurl:private ext:pdf
site:domain.com inurl:upload ext:pdf
site:domain.com inurl:uploads ext:pdf
site:domain.com inurl:internal ext:pdf
site:domain.com inurl:storage ext:pdf
site:domain.com inurl:download ext:pdf
site:domain.com inurl:webview ext:pdf
site:domain.com inurl:content ext:pdf
site:domain.com inurl:_data ext:pdf
site:domain.com inurl:<keyword> ext:pdf -docs -doc -documentation -form -draft -application -sample -template -public

#inurl keywords
inurl:internal 
inurl:private 
inurl:folder
inurl:asset
inurl:_data
inurl:upload
inurl:uploads
inurl:userdata
inurl:content

#file extensions
ext:pdf
ext:doc
ext:docx
ext:txt
ext:odt
ext:odf
ext:xls
ext:xlsx
ext:csv
ext:ppt
ext:pptx

#negative filtering removing the unwanted ones
-public -sample -doc -docs -documentation -template -draft -application -form -support -default

Endpoints utilizing PDF.js library to render PDF documents
inurl:pdfjs inurl:& site:*.gov
inurl:pdfjs inurl:& site:*.gov.*
com,ai,gov,edu,net,org,in,uk,au,jp,nl,de,fr......
#or just grep from waymore or waybackurls output
waymore -i domain.com -mode U -oU waymore_urls.txt
cat waymore_urls.txt | grep "pdfjs"

IPv4 Dorking
"keyword" (site:*.*.255.* |site:*.*.254.* |site:*.*.253.* |site:*.*.252.* |site:*.*.251.* |site:*.*.250.* |site:*.*.249.* |site:*.*.248.* |site:*.*.247.* |site:*.*.246.* |site:*.*.245.* |site:*.*.244.* |site:*.*.243.* |site:*.*.242.* |site:*.*.241.* |site:*.*.240.* |site:*.*.239.* |site:*.*.238.* |site:*.*.237.* |site:*.*.236.* |site:*.*.235.* |site:*.*.234.* |site:*.*.233.* |site:*.*.232.* |site:*.*.231.* |site:*.*.230.* |site:*.*.229.* |site:*.*.228.* |site:*.*.227.* |site:*.*.226.* |site:*.*.225.* |site:*.*.224.* |site:*.*.223.* |site:*.*.222.* |site:*.*.221.* |site:*.*.220.* |site:*.*.219.* |site:*.*.218.* |site:*.*.217.* |site:*.*.216.* |site:*.*.215.* |site:*.*.214.* |site:*.*.213.* |site:*.*.212.* |site:*.*.211.* |site:*.*.210.* |site:*.*.209.* |site:*.*.208.* |site:*.*.207.* |site:*.*.206.* |site:*.*.205.* |site:*.*.204.* |site:*.*.203.* |site:*.*.202.* |site:*.*.201.* |site:*.*.200.* |site:*.*.199.* |site:*.*.198.* |site:*.*.197.* |site:*.*.196.* |site:*.*.195.* |site:*.*.194.* |site:*.*.193.* |site:*.*.192.* |site:*.*.191.* |site:*.*.190.* |site:*.*.189.* |site:*.*.188.* |site:*.*.187.* |site:*.*.186.* |site:*.*.185.* |site:*.*.184.* |site:*.*.183.* |site:*.*.182.* |site:*.*.181.* |site:*.*.180.* |site:*.*.179.* |site:*.*.178.* |site:*.*.177.* |site:*.*.176.* |site:*.*.175.* |site:*.*.174.* |site:*.*.173.* |site:*.*.172.* |site:*.*.171.* |site:*.*.170.* |site:*.*.169.* |site:*.*.168.* |site:*.*.167.* |site:*.*.166.* |site:*.*.165.* |site:*.*.164.* |site:*.*.163.* |site:*.*.162.* |site:*.*.161.* |site:*.*.160.* |site:*.*.159.* |site:*.*.158.* |site:*.*.157.* |site:*.*.156.* |site:*.*.155.* |site:*.*.154.* |site:*.*.153.* |site:*.*.152.* |site:*.*.151.* |site:*.*.150.* |site:*.*.149.* |site:*.*.148.* |site:*.*.147.* |site:*.*.146.* |site:*.*.145.* |site:*.*.144.* |site:*.*.143.* |site:*.*.142.* |site:*.*.141.* |site:*.*.140.* |site:*.*.139.* |site:*.*.138.* |site:*.*.137.* |site:*.*.136.* |site:*.*.135.* |site:*.*.134.* |site:*.*.133.* |site:*.*.132.* |site:*.*.131.* |site:*.*.130.* |site:*.*.129.* |site:*.*.128.* |site:*.*.127.* |site:*.*.126.* |site:*.*.125.* |site:*.*.124.* |site:*.*.123.* |site:*.*.122.* |site:*.*.121.* |site:*.*.120.* |site:*.*.119.* |site:*.*.118.* |site:*.*.117.* |site:*.*.116.* |site:*.*.115.* |site:*.*.114.* |site:*.*.113.* |site:*.*.112.* |site:*.*.111.* |site:*.*.110.* |site:*.*.109.* |site:*.*.108.* |site:*.*.107.* |site:*.*.106.* |site:*.*.105.* |site:*.*.104.* |site:*.*.103.* |site:*.*.102.* |site:*.*.101.* |site:*.*.100.* |site:*.*.99.* |site:*.*.98.* |site:*.*.97.* |site:*.*.96.* |site:*.*.95.* |site:*.*.94.* |site:*.*.93.* |site:*.*.92.* |site:*.*.91.* |site:*.*.90.* |site:*.*.89.* |site:*.*.88.* |site:*.*.87.* |site:*.*.86.* |site:*.*.85.* |site:*.*.84.* |site:*.*.83.* |site:*.*.82.* |site:*.*.81.* |site:*.*.80.* |site:*.*.79.* |site:*.*.78.* |site:*.*.77.* |site:*.*.76.* |site:*.*.75.* |site:*.*.74.* |site:*.*.73.* |site:*.*.72.* |site:*.*.71.* |site:*.*.70.* |site:*.*.69.* |site:*.*.68.* |site:*.*.67.* |site:*.*.66.* |site:*.*.65.* |site:*.*.64.* |site:*.*.63.* |site:*.*.62.* |site:*.*.61.* |site:*.*.60.* |site:*.*.59.* |site:*.*.58.* |site:*.*.57.* |site:*.*.56.* |site:*.*.55.* |site:*.*.54.* |site:*.*.53.* |site:*.*.52.* |site:*.*.51.* |site:*.*.50.* |site:*.*.49.* |site:*.*.48.* |site:*.*.47.* |site:*.*.46.* |site:*.*.45.* |site:*.*.44.* |site:*.*.43.* |site:*.*.42.* |site:*.*.41.* |site:*.*.40.* |site:*.*.39.* |site:*.*.38.* |site:*.*.37.* |site:*.*.36.* |site:*.*.35.* |site:*.*.34.* |site:*.*.33.* |site:*.*.32.* |site:*.*.31.* |site:*.*.30.* |site:*.*.29.* |site:*.*.28.* |site:*.*.27.* |site:*.*.26.* |site:*.*.25.* |site:*.*.24.* |site:*.*.23.* |site:*.*.22.* |site:*.*.21.* |site:*.*.20.* |site:*.*.19.* |site:*.*.18.* |site:*.*.17.* |site:*.*.16.* |site:*.*.15.* |site:*.*.14.* |site:*.*.13.* |site:*.*.12.* |site:*.*.11.* |site:*.*.10.* |site:*.*.9.* |site:*.*.8.* |site:*.*.7.* |site:*.*.6.* |site:*.*.5.* |site:*.*.4.* |site:*.*.3.* |site:*.*.2.* |site:*.*.1.* |site:*.*.0.*)


Symfony debug mode enabled: CRITICAL INFO DISCLOSURE
site:domain.com inurl:app_dev.php
site:domain.com inurl:app_dev
site:domain.com inurl:_profiler
site:domain.com inurl:profiler

OAUTH
site:domain.com / site:*.domain.com
#set1
site:domain.com "Continue with Google"
site:domain.com "Sign in with Google"
site:domain.com "Login with Google"

#set2
site:domain.com "Sign in using"
site:domain.com "Authenticate with Google"

#set3
site:domain.com "Authenticate with"

WORDPRESS DORKS
How to pick endpoints for it?
subfinder -d domain.com > subs.txt
cat subs.txt | httpx -title -ip -sc -td > httpx_subs.txt
cat httpx_subs.txt | grep -i "wordpress" > wordpress_endpoints_domain.txt
XLS & XLSX
inurl:wp-content/uploads/ ext:xlsx site:domain.com
inurl:wp-content/uploads/ ext:xls site:domain.com
inurl:wp-content/uploads/ ext:xls "@gmail.com" site:domain.com
inurl:wp-content/uploads/ ext:xlsx "@gmail.com" site:domain.com
inurl:wp-content/uploads/ ext:xls "date of birth" site:domain.com
inurl:wp-content/uploads/ ext:xlsx "date of birth" site:domain.com
inurl:wp-content/uploads/ ext:xls "INTERNAL USE ONLY" site:domain.com
inurl:wp-content/uploads/ ext:xlsx "INTERNAL USE ONLY" site:domain.com

PDF
inurl:wp-content/uploads/ ext:pdf site:domain.com
inurl:wp-content/uploads/ ext:pdf "@gmail.com" site:domain.com
inurl:wp-content/uploads/ ext:pdf "date of birth" site:domain.com
inurl:wp-content/uploads/ ext:pdf "INTERNAL USE ONLY" site:domain.com
inurl:wp-content/uploads/ ext:pdf "INTERNAL USE ONLY" site:domain.com

BACKUP FUZZING
inurl:/wp-content/backup-
inurl:/wp-content/backup-FUZZ #hyphen
inurl:/wp-content/backup_FUZZ #underscore

DIRECTORY LISTING
intitle:index of /wp-content
intitle:index of /wp-content/uploads
intitle:index of /wp-content/bak
intitle:index of /wp-content/backup
intitle:index of /wp-content/2024

PRIVATE DIRECTORY
inurl:/wp-content/ inurl:private
inurl:/wp-content/ inurl:internal

Open Jenkins Instances
intitle:"Dashboard [Jenkins]" Credentials

Atlassian Confluence Dashboard
#related CVE: CVE-2019-3396
intitle:dashboard-confluence…

phpMyAdmin
inurl:main ext:php "Welcome to phpMyAdmin" "running on"
"Welcome to phpMyAdmin" "running on" inurl:main.php
inurl:main.php phpMyAdmin
inurl:main.php "Welcome to phpMyAdmin"
inurl:sql.php "phpmyadmin"
inurl:sql "phpmyadmin"

AWS S3 BUCKET
site:*.s3.amazonaws.com
site:*.*.s3.amazonaws.com
site:*.*.*.s3.amazonaws.com
inurl:"s3.amazonaws.com"
site:*.s3.amazonaws.com intitle:Bucket loading

#here we are matching keywords in domain,title and url
#also view the page source for hidden cloud assets in all subdomains, IPs, vhosts and much more deep stuff.

GEOSERVER
inurl:"/geoserver/ows?service=wfs" site:*.gov
#use nuclei and hunt for CVE vulnerabilities for geoserver
nuclei -l urls_endpoints.txt -tags geoserver -severity critical,high,medium

Leaked Credentials On Google
In Google Sheets/Groups
site:docs.google.com/spreadsheets "company name“
site:groups.google.com "company name"

```

# Manipulated All Files on Server of a HackerOne Target😈

# Initial Basic Recon

- What type of industry or market sector the target belongs to?

> _Finance, Healthcare, Retail & E-commerce, Technology, Telecommunications, Education, Media & Entertainment, Energy & Utilities, Government & Public Sector, Transportation & Logistics_

- Can we snoop into the developer’s github, monitor daily for changes made, one small mistake like leaving the basic encoded API key in the dev code comments , thinking who will see it ☠️? **Why not make a cronjob script for it that runs on VPS and check the changes given that the repo is public, ofcourse!**

```
subfinder -d domain.com -all -recursive > all_subs.txt  
  
#stag, staging, stage-*.domain.com, stg, etc...   
cat all_subs.txt | grep -i "stag"
```

Previously if you did `httpx`to filter live subs, you might missed that, but you will see it may come live as changes are carried out with new project requirements , and much more…

- Can we find the list of employees through social media?
- Does the target use modern tech stack or old tech stack?

😎Modern apps use recent frameworks and tools for speed, scalability, and adaptability. Examples include React or Angular on the frontend, Node.js or Django on the backend, and NOSQL databases like MongoDB, ~={yellow}mostly it will be deployed on cloud platforms like AWS, Azure or GCP with Docker and CI/CD pipelines.😏These are protected with strong WAF(Web Application Firewalls) by CloudFlare, CloudFront, Imperva, Fortinet, Akamai and much more. =~

**So simply payload mass spray on all GET based parameters of archived URLs doesn’t makes sense here**, ~={purple}which _I often see hunters busy in automation of something which doesn’t even touch the main target as it is first filtered by WAF and only legitimate requests are being served._=~

**Given that the WAF is been deployed with strong protection rules , after a few couple of requests the attacker’s IP will be red flagged and thereby further payloads are easy avoided.** Can you change/rotate your IP on each request of payload spray?

🥴Older apps rely on legacy tools that often lacks scalability and modern features. Examples like: ~={orange}jQuery or plain HTML on the frontend, PHP or ASP.NET on the backend, and databases like MySQL, often deployed on physical servers without modern CI/CD or containerization setups.=~

Confidence gets auto-boosted the moment I see running old technologies without any WAF as well🤤

# Google Dorking Like a Pro🥷

I did a couple of dorks to directly get the live urls with parameters , one of them which worked successfully is stated below

```
site:domain.com inurl:= inurl:? inurl:&
```

~={cyan}This dork helps to only get those urls that are long enough with multiple parameters due to (&).=~

I got a list of 20+ urls , I proxied all the URLs through Burpsuite and sent it to Repeater Tab, played a couple of hours tweaking the parameters but ~={red}no results ;(=~

**Next, I shifted to `waymore` to find archived urls from Wayback Machine, Common Crawl, Alien Vault OTX, URLScan & VirusTotal!**

```shell
waymore -i domain.com -mode U -oU waymore_domain.com.txt
```

I used below linux command to match similarly that we were doing using google dorking, only long urls

```shell
cat waymore_domain.com.txt | grep "=" | grep "&" | grep "?"
```

Got 40+ urls this time, I made a chunk of 10 urls and visited each urls manually using browser and the chrome extension “Open Multiple URLs”


**And it’s game over guys at this point of time ☠️☠️**

I realized I touched an unauthenticated and unauthorized endpoint directly , no credentials needed!🤯

![](https://miro.medium.com/v2/resize:fit:700/1*luQVEg50QQd2hM3v7Z7-1w.png)

We can upload files, create folders as well.

![](https://miro.medium.com/v2/resize:fit:374/1*ixvL3umOghOczrGIMy-vFw.png)

I made a bugpoc.txt file with random content and uploaded the file successfully. Can’t believe it’s real guys 🫣

![](https://miro.medium.com/v2/resize:fit:700/1*22FxEHLa44M_BjYqdgLWWA.png)

However, there is no feature to view the uploaded documents sadly. But we try to attack with maximum fatal damage.

![](https://miro.medium.com/v2/resize:fit:340/1*bvSbhJJnLVd0MhYZdjPGkA.png)

## Why not upload the file with same name ?

In order to not hurt the real sensitive documents, I tried to change the existing bugpoc.txt on my local system, I added a lot of garbage data , so that the file size increases, and that’s how we check it’s was modified after re-uploading or not.

![](https://miro.medium.com/v2/resize:fit:700/1*J6pm8EV1vIgl3w97gKAXkw.png)

Got a prompt saying already that filename already exist, but we can click OK and overwrite it.


$----------------------------------------------$

#### What is urlscan.io ❓

- **urlscan.io** is a free service to scan and analyze websites.
- Any URL submitted to urlscan.io goes through an automated process where it is browsed like a normal user and the following details are extracted and retrieved

1. Domains and IPs contacted
2. Resources (Javascript , CSS ,etc…. )
3. Additional information about the web page
4. Screenshot of the page
5. DOM content, JS global variables, cookies and etc..
6. Also detect whether it is malicious or not.

> https://urlscan.io/search

🔍Advanced search operators reference:

>https://urlscan.io/docs/search/

#### 🗃️ xlsx + xls files

```makefile
domain:redacted.com AND page.url:xlsx
domain:redacted.com AND page.url:xls
```

![None](https://miro.medium.com/v2/resize:fit:700/1*X6_WXbLaGN8ohp_YHeQHAg.png)

📃 pdf files

![None](https://miro.medium.com/v2/resize:fit:700/1*bMLkIgacL15Qk0gtvwXSxg.png)

Combine with interesting URL paths like `upload` , `uploads,` `private` , `system` , `data` , `web` , `internal`

![None](https://miro.medium.com/v2/resize:fit:700/1*nAkigXIy84MfI64EoK-Gnw.png)

#### 📥 Latest Subdomains

![None](https://miro.medium.com/v2/resize:fit:700/1*bG4yZGeT8HYABf-dmIsEmw.png)

#### 😈 Javascript Files

- That's where the real treasure hides.

```makefile
domain:redacted.com AND page.url:.js
```

![None](https://miro.medium.com/v2/resize:fit:700/1*9SzaKkLfUfgZxeh7HKpluQ.png)

#### 🔨 Basic Parameter Hunting

```makefile
domain:redacted.com AND page.url:search
domain:redacted.com AND page.url:query
domain:redacted.com AND page.url:page
domain:redacted.com AND page.url:id
domain:redacted.com AND page.url:type

domain:redacted.com AND page.url:search=
domain:redacted.com AND page.url:query=
domain:redacted.com AND page.url:page=
domain:redacted.com AND page.url:id=
domain:redacted.com AND page.url:type=


etc........
```


![None](https://miro.medium.com/v2/resize:fit:700/1*_Ce4kMEnWcSVMssy2yEhag.png)

To find more parameter names refer to this

[https://www.openbugbounty.org/blog/top-100-xss-dorks](https://www.openbugbounty.org/blog/top-100-xss-dorks/)

#### 🎯 Multi-Parameter Hunting

```go
domain:redacted.com AND page.url:& AND page.url:= AND page.url:?
```

- Sometimes it may not work for small scope domains.

![None](https://miro.medium.com/v2/resize:fit:700/1*v3R-R-4fbLGwYtHni5PTiA.png)

🌶️Spicy parameter Hunting

```makefile
domain:redacted.com AND page.url:cmd AND page.url:etc
```

Look at those attackers or legit hunters testing the `cmd` and `action` parameters with malicious payloads to read the content of `/etc/passwd`

![None](https://miro.medium.com/v2/resize:fit:700/1*oxFvaKCfr6topUMoRjzXTQ.png)

👁️ Finding the hidden stuff

```ruby
domain:redacted.com AND page.url:internal
domain:redacted.com AND page.url:private
domain:redacted.com AND page.url:hidden
domain:redacted.com AND page.url:secret
domain:redacted.com AND page.url:dashboard
domain:redacted.com AND page.url:config
domain:redacted.com AND page.url:key
domain:redacted.com AND page.url:pwd
domain:redacted.com AND page.url:token
domain:redacted.com AND page.url:eyJ
```

🛢️API Endpoints

```ruby
domain:redacted.com AND page.url:api

domain:redacted.com AND page.url:api AND page.url:v1
domain:redacted.com AND page.url:api AND page.url:v2
domain:redacted.com AND page.url:api AND page.url:v3
domain:redacted.com AND page.url:api AND page.url:v4

domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:get
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:fetch
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:details
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:list
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:payment
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:order
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:format
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:export
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:retrieve
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:system
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:dashboard
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:admin
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:internal
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:private
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:secret
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:debug
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:users
domain:redacted.com AND page.url:api AND page.url:{anyversion} AND page.url:send
```

🔐OAUTH related Endpoints + Other Auth types as well

```ruby
domain:redacted.com AND page.url:oidc
domain:redacted.com AND page.url:openid
domain:redacted.com AND page.url:oauth
domain:redacted.com AND page.url:oauth2
domain:redacted.com AND page.url:auth
domain:redacted.com AND page.url:client
domain:redacted.com AND page.url:response_type
domain:redacted.com AND page.url:code
domain:redacted.com AND page.url:state
domain:redacted.com AND page.url:token
domain:redacted.com AND page.url:identity
domain:redacted.com AND page.url:callback
domain:redacted.com AND page.url:saml
domain:redacted.com AND page.url:samlprovider
```

🪛Open Redirect Endpoints

```ruby
domain:redacted.com AND page.url:uri
domain:redacted.com AND page.url:url
domain:redacted.com AND page.url:http
domain:redacted.com AND page.url:2F
domain:redacted.com AND page.url:http%3A
domain:redacted.com AND page.url:redirect
domain:redacted.com AND page.url:redirect_uri
domain:redacted.com AND page.url:redirect_url
domain:redacted.com AND page.url:forwarded
domain:redacted.com AND page.url:to
```

🧿SSRF Endpoints

```ruby color
domain:redacted.com AND page.url:dest
domain:redacted.com AND page.url:path
domain:redacted.com AND page.url:continue
domain:redacted.com AND page.url:window
domain:redacted.com AND page.url:site
domain:redacted.com AND page.url:return
domain:redacted.com AND page.url:port
domain:redacted.com AND page.url:view
domain:redacted.com AND page.url:print
domain:redacted.com AND page.url:export
domain:redacted.com AND page.url:dir
domain:redacted.com AND page.url:out
domain:redacted.com AND page.url:callback
```

**Find names of common parameters for each vulnerability type from this reference below, but test uncommon parameters as well!**

> https://github.com/1ndianl33t/Gf-Patterns/tree/master

#### 📂 File Manager Endpoints

```makefile
page.url:filemanager.php
page.url:manage AND page.url:php
page.url:file AND page.url:php
page.url:document AND page.url:php
page.url:upload AND page.url:php


# other than php
aspx,asp,jsp,jspx,do,action,cgi
```

![None](https://miro.medium.com/v2/resize:fit:700/1*5W3ggVd8cGXpcM9zYkL2vg.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*6XKUxDD8vRdeqQCjsnb5hA.png)

Sometimes 403, remove those keywords like `login` , `signin` path and directly hit the endpoint we want 😏

🧱Finding ports

```css
domain:redacted.com AND page.url:{anyportnumberhere}
```

#### 🪣 S3 Buckets

- CSV Files

```css
page.url:s3. AND page.url:amazonaws.com AND page.url:csv 
```

![None](https://miro.medium.com/v2/resize:fit:700/1*KgyZhJIwRgotnpVEcjDSrQ.png)

Any other file extension

```css
page.url:s3. AND page.url:amazonaws.com AND page.url:{extensionhere}
```

> The next part will be based on Regex and Wildcard search which is not available for anonymous users.

![None](https://miro.medium.com/v2/resize:fit:700/1*XWFodFrZroU2nQ6VoyqmMg.png)

#### 1️⃣ Subdomains

```bash
domain:redacted.com NOT page.url:www.redacted.com
domain:redacted.com NOT domain:www.redacted.com
domain:redacted.com NOT page.url:www
domain:redacted.com AND page.url:http
domain:redacted.com NOT page.url:https
```

#### 2️⃣ Other Subdomains


```Bash 
domain:redacted.com AND page.url:pre
domain:redacted.com AND page.url:prod
domain:redacted.com AND page.url:test
domain:redacted.com AND page.url:stg
domain:redacted.com AND page.url:stage
domain:redacted.com AND page.url:staging
domain:redacted.com AND page.url:live
domain:redacted.com AND page.url:dev

#also try to add (-) hyphen example
page.url:pre-
page.url:prod-
page.url:test-
page.url:stg-
page.url:stage-
page.url:staging-
page.url:live-
page.url:dev-
```

> Mostly reports about vulnerabilities on subdomains like testing, staging, and development environments won't be triaged, because it's not in production live yet, but still exceptions are there.

#### 3️⃣ Uploaded Resources

```makefile
domain:redacted.com AND page.url:attachments
domain:redacted.com AND page.url:attachment
domain:redacted.com AND page.url:attach
domain:redacted.com AND page.url:upload
domain:redacted.com AND page.url:uploads
domain:redacted.com AND page.url:resource
domain:redacted.com AND page.url:web
domain:redacted.com AND page.url:content
domain:redacted.com AND page.url:data
domain:redacted.com AND page.url:asset
```

> Same keywords but different search engines, try with everything:

> Google + urlscan.io dorking

#### 4️⃣ Extensions

```vbnet
domain:redacted.com AND page.url:php
domain:redacted.com AND page.url:aspx
domain:redacted.com AND page.url:asp
domain:redacted.com AND page.url:jsp
domain:redacted.com AND page.url:jspx
domain:redacted.com AND page.url:.do
domain:redacted.com AND page.url:action
domain:redacted.com AND page.url:cgi
```

#### 5️⃣ Account creation pages (Authenticated Testing)

```makefile
domain:redacted.com AND page.url:login
domain:redacted.com AND page.url:register
domain:redacted.com AND page.url:admin
```

#### 6️⃣ JS.MAP Files

- Very little chance of finding.

```go
domain:redacted.com AND page.url:js.map
```

- Try to grep the same in `waymore` output of archived URLs as well.

```bash
waymore -i domain.com -mode U -oU waymore_domain.txt
cat waymore_domain.txt | grep "js.map$"
cat waymore_domain.txt | grep "js.map$" | sort -u
```

#### 7️⃣ WordPress Endpoints

```bash
domain:redacted.com AND page.url:"wp-"
```

Or just take a quick look at what plugins are there.

```makefile
domain:redacted.com AND page.url:plugins
```

Wordpress uploaded files:

```vbnet
domain:redacted.com AND page.url:"wp-content" AND page.url:uploads
```

#### 8️⃣ Information Disclosure (.git) — P1 (High/Critical)

> https://systemweakness.com/exposed-git-directory-p1-bug-5fd272a62f51

#Github_Recon

```makefile
domain:redacted.com AND page.url:.git
```

![None](https://miro.medium.com/v2/resize:fit:700/1*vTm4u9d1__aEQ4X5UN8IZQ.png)

#### 9️⃣ Information Disclosure (Configuration Files and Backups)

```lua
page.url:config.zip
page.url:config AND page.url:zip
```

![None](https://miro.medium.com/v2/resize:fit:700/1*rsVzQ3sy50ie8z8SaQE6eQ.png)

#### 🔟 Direct access to Installation and Configuration

```makefile
page.url:install AND domain:redacted.com
page.url:core AND page.url:install
page.url:core AND page.url:install.php
```

_You can also mention only TLD(Top-level domains) like:_

```makefile
domain:gov
domain:edu
domain:org
domain:com
domain:uk
domain:nl
domain:jp
domain:in
```

![None](https://miro.medium.com/v2/resize:fit:700/1*AH27iyakRIfSK93cLPIdMA.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*xH-LKqlPu0a2XKX0buemkQ.png)

# Bypass OAST Mitigations

## Detection of out-of-band testing and how to bypass

**What is OAST?**

> https://portswigger.net/burp/application-security-testing/oast

If you are into defensive cybersecurity or atleast some basics of how to prevent owasp 10 attacks, you might know for the case "**Out of band**" testing, specific rules are set to directly block if well known keywords are found anywhere in the request which gives  t**he clue that some attack or pentesting or bug hunting is going around behind it, and as a result it will be blocked immediately.**

Customized servers with obfuscated or at least unknown keywords are the secret here which others might miss.


If you don't understand, let me explain in simpler terms with an example:

- You are testing for blind SSRF in the Referrer header using Burpsuite. For a potential area to dig further, first, you are expecting DNS / HTTP callback in your Burp collaborator server history.

- Still, with the above mitigations or controls in place the moment the pattern or regex of "oastify.com" gets detected, as a result, your request will be dropped and blue team guys will be able to see in their dashboards what you were trying to do.
- The same goes for nuclei to detect the pattern `interact.sh`
#### ⚓Cat & Mouse Game

- For example, you make malware that is currently undetectable when you tested with Windows 11 locally, but if you make it open-source and make it public on Github, then it's no longer undetectable, blue teamers will upload the sample to all possible malware **analyzers, your malware will be given a name, signature, id and custom rules will be made to detect your sample.**


- After that, you need to again spend time obfuscating with another custom algorithm and also bypass the existing rule detection, **and don't upload to VirusTotal which has a sharing policy, meaning any file or exe you upload, will be distributed in their community or among other AV & EDR vendors to make their detection stronger.**

- Find something that doesn't have such policies. But still, many beginner attackers make this tiny mistake. But still never believe even if they tell you they don't share (Zero Trust Policy)

Below are the dorks for urlscan.io to check the list of domains that have been tested ethically or unethically by pen-testers/attackers.

> OAST attack! Your web app has been cooked 💣


# ! USE URLSCAN.io

#### 1️⃣ OAST ONLINE

```makefile
page.url:oast.online NOT domain:oast.online
```

#### 2️⃣ OASTIFY

```makefile
page.url:oastify.com NOT domain:oastify.com
```

#### 3️⃣ OAST PRO

```makefile
page.url:oast.pro NOT domain:oast.pro
```

#### 4️⃣ OAST LIVE

```makefile
page.url:oast.live NOT domain:oast.live
```

#### 5️⃣ BURP COLLABORATOR

```makefile
page.url:burpcollaborator.net NOT domain:burpcollaborator.net
```

#### 6️⃣ INTERACTSH

```makefile
page.url:interact.sh NOT domain:interact.sh
```

### Other detection patterns

#### 😈 OpenRedirect

- Well it is not related to OAST testing, but when pen-testers test for open redirect, there is a mitigation to block all well-known payloads containing `evil.com`

```makefile
page.url:evil.com NOT domain:evil.com
```

#### 💣Command Injection Pattern

```graphql
page.url:IFS AND page.url:ping

#combined with above OAST patterns
```

#### 🔨 Potential LFI (Local File Inclusion) Attack

```bash
page.url:cat AND page.url:etc
```

#### 💉Potential SQL Injection Attack

- One suggestion to improve is to use `sqlmap` while proxying the traffic through burpsuite, and then you will be able to see how the exploitation is going behind the scenes and grasp some fundamental understanding to be better at manual testing because detection and mitigation rules are there to block `sqlmap`traffic pattern, but if you test manually using burp one by one, you might get the desired results soon.

```lua
page.url:concat AND page.url:md5
```

#### ⚔️ Potential XSS attack

```makefile
page.url:script AND page.url:xss
```

![None](https://miro.medium.com/v2/resize:fit:700/1*hdTEe28yqoyrw7ri6CLMyA.png)

				Combine the above dorks with

```makefile
domain:redacted.com AND other_dorks_here..........
```

_Not useful always because most of the time the results are few to none, but the results keep changing from time to time, every next minute there is something new to explore._


#### How to Bypass 🤑

- _Encoding techniques (url + html + unicode + hex …etc.) Use CyberChef for crafting it. Double-triple encoding with the same or combined with many different techniques or encoding algorithms._

$----------------------------------------------$


## General P2-3 Dorking:

```Shell
site:domain.com "INTERNAL USE ONLY"
site:domain.com "PRIVATE AND CONFIDENTIAL"
site:domain.com "ONLY FOR"
site:domain.com "HIGHLY CONFIDENTIAL"
site:domain.com "CONFIDENTIAL"
site:domain.com "STRICTLY CONFIDENTIAL"
site:domain.com "SENSITIVE"
site:domain.com "COMPANY SENSITIVE"
site:domain.com "PRIVATE ASSET"

#inurl keywords
inurl:internal 
inurl:private 
inurl:folder
inurl:asset
inurl:_data
inurl:upload
inurl:uploads
inurl:userdata
inurl:content

#file extensions
ext:pdf
ext:doc
ext:docx
ext:txt
ext:odt
ext:odf
ext:xls
ext:xlsx
ext:csv
ext:ppt
ext:pptx


#negative filtering removing the unwanted ones
-public -sample -doc -docs -documentation -template -draft -application -form -support -default

```

Many documents were available, giving me hope that I will get something. Hopefully, within 1–2 weeks I got a document in a secondary domain that seem worth to report about. What exactly was the data can’t be elaborated further to respect the guidelines & policies and get rid of unnecessary program violations.

> **VRT: Sensitive Data Exposure > Disclosure of Secrets > For Internal Asset**

![](https://miro.medium.com/v2/resize:fit:571/1*jKwEK4lOTwDyKPqxU0Oz4g.png)

I keep repeating in most of the google dorking articles that I published previously, you may get new results when y**ou keep monitoring daily two times for new data** (google search tools: filter by date,time and custom range) and that’s how you **get rid of duplicates most of the time.**

# How we Craft Unique Dorks using AI

In order to craft unique dorks, we don’t need to learn about Google dorking. All we need to do is just some basic prompt engineering and make AI work for us.

To generate dorks using AI, we first need to provide some sample dorks so that AI ( ChatGPT, Gemini) can learn about the dorks used to find bug bounty programs. The corrections made include:

![](https://miro.medium.com/v2/resize:fit:627/1*rUKpU0INxGmwga7u6rCeLA.png)

In this prompt, I’ve told him to analyze the file that has sample dorks. You can easily find sample dorks on the Internet

![](https://miro.medium.com/v2/resize:fit:627/1*gNQ_rmuBfuVFMSm_fTXglg.png)

In the next step, I’ve told ChatGPT to generate dorks based on the sample dorks. These generated dorks are unique, so now we have Google dorks which can help us find bug bounty programs that likely have fewer hunters.

![](https://miro.medium.com/v2/resize:fit:627/1*mtGIx1BPNq5PPg6TwmtpkA.png)

You can clearly see that by using the Google dorks generated by ChatGPT, we can find many bug bounty programs. There is a higher chance that these programs **have fewer hunters because we are finding them using unique dorks.**


---
## 📝Problem Statement

If you have tried the general dork for markup languages like XML extension

site:domain.com ext:xml

Probably, you might have noticed that it doesn’t really give the results we want.When you hover over the link, it shows the endpoint ending with .xml , but when we click and visit, it redirects to another path like `https://domain[.]com/path1/path2` and the xml data vanishes.

## 🔍Finding the hidden stuff

That’s what this article will teach you about!

dork I used= **site:target.com -www -shop -documentation**

Then I tried to add a word to search in those subdomains, you can try to do this like this= **site:target.com -www -shop -documentation “logs”**

You can also use pre-precombined dorks which are already available in [https://www.exploit-db.com/google-hacking-database](https://www.exploit-db.com/google-hacking-database)

I found a result in the search and it was having the word log inside url. I will attach the url pattern below:

> [https://target.com/logs/?goc_wbp=287329002V4Sx3OnzOy2MEOUlfpYal3LNo8M](https://cas-hm.pbh.gov.br/logs/?goc_wbp=287329002V4Sx3OnzOy2MEOUlfpYal3LNo8M)

when I clicked the url I was taken into log directory.

![](https://miro.medium.com/v2/resize:fit:627/1*Jlj0p8jDZDc_3wCH9FfSQg.png)

And I was able to access: **access.logs, error-logs and error-ssl.logs**

## 1️⃣ XSD

XML Schema Definition — Defines the structure and data types for XML documents.

> ext:xsd site:domain.com

![](https://miro.medium.com/v2/resize:fit:627/1*SvTEO2BIysmqrkiFFjlJzw.png)

## 2️⃣ XSL

Extensible Stylesheet Language — Transformation language for XML documents.

> ext:xsl site:domain.com

![](https://miro.medium.com/v2/resize:fit:627/1*QgQFecxLn_p-_0uXKahZdg.png)

## 3️⃣ PLIST

Configurations and user settings for macOS apps.

> ext:plist site:domain.com

![](https://miro.medium.com/v2/resize:fit:627/1*SYJIMyQm3fMg7sZSQ8ljTg.png)

## 4️⃣ **WSDL**

XML based content holding information about the functionalities of the web services.

> ext:wsdl site:domain.com

![](https://miro.medium.com/v2/resize:fit:627/1*VAuonvvr4YCEO2xsgIGMjA.png)

## 5️⃣ KML

Keyhole Markup language — holding geographic data. Useful when your target is government related.Although non-sensitive in nature, but in some case we may find excessive data which might be sensitive to the organization or entity.

> ext:kml site:domain.com

![](https://miro.medium.com/v2/resize:fit:578/1*AXMF_S8jShUnRj7thcsR5g.png)

## ⚓KEY-NOTES

- Combine with other keywords that is associated with credentials, access keys, tokens,secrets , db strings, keywords found in config files, etc..
- This type of information might help to uncover structure or schema of the overall app.
- Find what type of information is getting disclosed through comments in the XML file as well.
- Email addresses
- In rare cases, SQL queries (as text)
- This approach may also result into other endpoints with legacy systems.

### Waymore

## 🔍ARCHIVE GREP

```shell
waymore -i domain.com -mode U -oU waymore_domain.txt  
grep -E "\.xsd|\.xsl|\.plist|\.wsdl|\.kml" waymore_domain.xt
```
Always try to filter out commonly known endpoints with .php, .asp, aspx, .jsp, png, css, svg, etc.. and find what other interesting stuff exists in those archived URLs.


***-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-****-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*

### GitHub Dorks & Leaks

#Github_Recon

# Using Git Commands to Find and Extract Dangling Blobs

_These commands help you find and recover deleted files or sensitive data that **might still exist in a Git repository's object database**, even after they've been removed from the active history.

## Understanding the Commands

### `git fsck --unreachable --dangling --no-reflogs --full`

This is the core command that checks the repository's integrity and finds orphaned objects:

- `fsck`: Stands for "file system check"
    
- `--unreachable`: Shows objects that aren't reachable from any named reference (branch, tag, etc.)
    
- `--dangling`: Shows objects that aren't referenced by any other object
    
- `--no-reflogs`: Excludes objects that might be referenced in reflogs (giving us only truly unreachable objects)
    
- `--full`: Performs a more thorough check
    

This will output a list of unreachable objects including commits, trees, and blobs (file contents).
## The Extraction Script

```bash
mkdir -p unreachable_blobs && git fsck --unreachable --dangling --no-reflogs --full | grep 'unreachable blob' | awk '{print $3}' | while read h; do git cat-file -p "$h" > "unreachable_blobs/$h.blob"; done
```

This script does the following:

1. Creates a directory called `unreachable_blobs` to store the recovered files
    
2. Runs `git fsck` with our parameters
    
3. Filters for only blob objects (`grep 'unreachable blob'`)
    
4. Extracts just the SHA-1 hash of each blob (`awk '{print $3}'`)
    
5. For each hash, uses `git cat-file -p` to print the blob's content and saves it to a file
    

## How to Use This Against a Target

1. **Clone the target repository**:
    

```bash
git clone <target-repo-url>
cd <target-repo>
```

- **Run the extraction script**

After running this, you'll have a directory full of recovered files. You can then use:

**A typical scan command:**

```bash
trufflehog filesystem --only-verified --print-avg-detector-time --include-detectors="all" ./ > secrets.txt
```

<mark style="background: #FF5582A6;">this demonstrates why simply removing sensitive data from Git history isn't sufficient </mark>- the data **may still exist in the repository's object database** until it's garbage collected (which happens automatically after some time or can be forced with `git gc`).

To truly remove sensitive data from a Git repository, you need to use tools like `git filter-repo` or `BFG Repo-Cleaner` and then force-push the cleaned history.

---

_developers can accidentally push private API keys, passwords, SSH credentials, or database connection strings to their repos, GitHub becomes a prime hunting ground for attackers._

### 💻 How Attackers (and Researchers) Use GitHub Dorks

###  Master the GitHub Search Operators

GitHub’s search bar is more powerful than most realize. These operators help narrow down your targets:

![](https://miro.medium.com/v2/resize:fit:875/0*LPhu0PzQTmOCniDL.png)

### Dorks

- **API Keys & Credentials**
- `filename:.env DB_PASSWORD`
- `filename:config.json AWS_ACCESS_KEY_ID`
- `filename:settings.py SECRET_KEY`
- **Database Connection Strings**
- `filename:.env MYSQL_PASSWORD`
- `filename:database.yml production`
- **SSH & Private Keys**
- `filename:id_rsa`
- `extension:pem private`
- **Cloud & Service Credentials**
- `filename:credentials aws_access_key_id`
- `extension:json google_api_key`

# Real-World Examples of GitHub Leaks

1. **AWS Keys in Public Repos**  
    Attackers who find AWS keys often gain control over the entire cloud infrastructure, leading to data breaches, downtime, and hefty bills.
2. **Database Credentials Leading to Data Dumps**  
    Exposed `.env` files or `config` entries can reveal usernames and passwords. Hackers can dump entire databases, causing chaos and reputational damage. 🤯
3. **Hardcoded SSH Keys Compromising Servers**  
    Committed private SSH keys give attackers unrestricted access to servers, risking everything from data theft to ransomware.

# 🔒 Securing Your Data on GitHub

1. **Use** `**.gitignore**` **to Exclude Sensitive Files**

```
# Example .gitignore entries  
.env  
*.pem  
config.json  
database.yml
```

# 🛠️ Essential Tools for Scanning GitHub Leaks

**Automate** and **streamline** your security checks with these open-source tools:

- **TruffleHog**
- **Gitleaks**
- **Detect-Secrets**
- **GitGraber**
- **shhgit**
- **github-dorks**
- **gitrob**
- **git-all-secrets**
- **git-secrets**
- **gittyleaks**
- **GitDorker**

# Handy Dorks to Try

Here’s a sample of useful dorks. Simply plug them into GitHub’s search bar (or advanced search) and watch the results roll in:

```bash
"api_key"  
"api_secret"  
"aws_access_key_id"  
"aws_secret"  
"db_password"  
"dbuser"  
"filename:.env DB_USERNAME NOT homestead"  
"filename:id_rsa"  
"filename:wp-config.php DB_PASSWORD"  
"password"  
"private_key"  
"secret"  
"token"  
"xoxp"  
"xoxb"
```

Combine them with `org:`, `repo:`, `filename:`, and more to zero in on specific targets or file types.

### Extra Tips & Tricks

- **Automate Dorking**: Write scripts or use CI/CD pipelines to scan for secrets **before** each commit.
- **Expand Beyond GitHub**: Don’t overlook platforms like GitLab or Bitbucket for similar vulnerabilities.

---


Test for xss in input fields:

like "img onerror alert(document.cookie)" or open redirect bugs:

![[Pasted image 20241225235650.png]]


## Stay updated with CVEs:

I have two main ways how I target:

- Shodan and Google CVE dorks + exploit poc
- Version disclosure in subdomains + exploit search + poc

# How I stay updated with CVE?

![](https://miro.medium.com/v2/resize:fit:875/1*cyGEXuiQjGpp7xsiS3Fr4Q.png)

Shodan has already done the hard work for us,we just need to fetch and extract information we need accordingly.

```
> curl -s https://cvedb.shodan.io/cves | jq '.cves[] | {cve_id,summary,published_time}'
```

![](https://miro.medium.com/v2/resize:fit:750/1*U1XifoOt3aUcZTMIx731Xw.png)

![](https://miro.medium.com/v2/resize:fit:875/1*aJDzRxOhVSb2jRPbUsxEMw.png)

- Most of the CVEs have public nuclei templates along with shodan and google dork it.

> Google CVE Dorks

![](https://miro.medium.com/v2/resize:fit:875/1*8jgHniZDbUeeL4GgAxfoTw.png)

> Shodan CVE Dorks

![](https://miro.medium.com/v2/resize:fit:875/1*wjFkrpbqJy9uCup5L7sMjA.png)

```
For version disclosure , just search for “product” exploits in github

"product name" "exploit" site:github.com
```

![](https://miro.medium.com/v2/resize:fit:875/1*V3Rn3XhNsBUPCcdYTfzBng.png)

![](https://miro.medium.com/v2/resize:fit:875/1*6PMENOZKljr1ngZ7-8Y-Wg.png)

----

## Exstensions:

**Linkgopher**

![None](https://miro.medium.com/v2/resize:fit:700/1*ySI-vrCx6CM7j9TYekpBpg.png)

**It will extracts links form a webpage.**

**example your target is google .**

**you entered a dork in google .**

> **site:*.google.com**

![None](https://miro.medium.com/v2/resize:fit:700/1*uR8Az_R5iwD9P3uimU_tjQ.png)

**After searching click on linkgopher extension.**

![None](https://miro.medium.com/v2/resize:fit:700/1*3QTfwhSBTZgYyQRkCO7lSw.png)

**You can see subdomains in that site:*.google.com results page.**

![None](https://miro.medium.com/v2/resize:fit:700/1*cvis5Nm8myoOKKS930TStw.png)

**Linkgrabber**

![None](https://miro.medium.com/v2/resize:fit:700/1*vZHCYMo2pZ4OQkFk_HApUw.png)

**When you you visited a website example. google this extension will gives all links on that website.**

![None](https://miro.medium.com/v2/resize:fit:700/1*vZHCYMo2pZ4OQkFk_HApUw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*ggB3nFv8Nz6f1IP3Y6yPQw.png)

**you can copy those urls and paste into a file.**

**Open multiple url**

![None](https://miro.medium.com/v2/resize:fit:700/1*bYppq96VoQ78yxua2K9PBw.png)

**When you have more urls . this extension will helpful to open those urls at once.**

**Paste your urls.**

**Click on openurl.**

![None](https://miro.medium.com/v2/resize:fit:700/1*gSNZAHAmoZqxD1PQzqBEpg.png)

**Trufflehog**

![None](https://miro.medium.com/v2/resize:fit:700/1*QQ659ii4Xdol1_8kiBzVJw.png)

**This extension will find secrets like api,token,secrets . in your url.**

**It will popup a message when your visited url contains secrets.**

![None](https://miro.medium.com/v2/resize:fit:700/1*oevomwKgIMa4XYWw5nP6gA.png)



##  **Dotgit.**

![None](https://miro.medium.com/v2/resize:fit:700/1*2Diz95ysuvXHDluJRFPWRA.png)

**it's will give a alert when your visited url contains a .git directory.**

## How to use for exposed git creds on a website:

# 🖊️Steps to Reproduce

1.Install and enable DotGit extension

![](https://miro.medium.com/v2/resize:fit:875/1*aIxhS3N3L5HfCug6q4RRfg.png)

2.Browser to website https://sub.domain[.]com  
3.You will get a popup window in right down corner saying exposed .git directory  
4.You can download entire .git folder recursively from the extension download button.

![](https://miro.medium.com/v2/resize:fit:331/1*r08kegrOyi5o97c8fZBF4g.png)

# ⛏️Mining the Secrets

```
git clone https://github.com/internetwache/GitTools.git  
cd GitTools/Dumper  
./gitdumper.sh https://domain.com/.git/ hack_data

cd hack_data  
git log

```
Find all commits history of the developers and any other secrets exposed through config files and much more…

----

☄️Here’s a list of tools to streamline your work with Google Dorks and other search engines:
dorki.io
taksec.github.io/google-dorks-bug-bounty/
dorksearch.com
dorkme.comdorkgenius.com



https://systemweakness.com/advanced-google-dorking-part7-a8df43d00743 

```bash
site:domain.com inurl:view inurl:private ext:pdf
site:domain.com inurl:upload ext:pdf
site:domain.com inurl:uploads ext:pdf
site:domain.com inurl:internal ext:pdf
site:domain.com inurl:storage ext:pdf
site:domain.com inurl:download ext:pdf
site:domain.com inurl:webview ext:pdf
site:domain.com inurl:content ext:pdf
site:domain.com inurl:_data ext:pdf

site:domain.com inurl:<keyword> ext:pdf -docs -doc -documentation -form -draft -application -sample -template -public
```

https://medium.com/infosecmatrix/advanced-google-dorking-part8-72a44ed4d891

---

## Update your fuzzing Wordlists

## 1️⃣ GENERAL

```
site:x.com "Add" "to your wordlist"  
site:linkedin.com "Add" "to your wordlist"

```
## 2️⃣ EXPOSED

```
site:x.com "Add" "to your wordlist" "exposed"  
site:linkedin.com "Add" "to your wordlist" "exposed"
```

## 3️⃣ MISCONFIGURED

```
site:x.com "Add" "to your wordlist" "misconfigured"  
site:linkedin.com "Add" "to your wordlist" "misconfigured"
```

## 4️⃣ UNAUTHENTICATED

```
site:x.com "Add" "to your wordlist" "unauthenticated"  
site:linkedin.com "Add" "to your wordlist" "unauthenticated"
```

## 5️⃣ BUG SEVERITY BASED

```
site:x.com "Add" "to your wordlist" "P1"  
site:linkedin.com "Add" "to your wordlist" "P1"  
  
Bugcrowd Severity: P1,P2,P3,P4  
HackerOne Severity: critical,high,medium,low
```

## 6️⃣ BUG CATEGORY BASED

```
site:x.com "Add" "to your wordlist" "broken access control"  
site:x.com "Add" "to your wordlist" "remote code execution"

```
## 7️⃣ TECH STACK

```
site:x.com "Add" "to your wordlist" "IIS Windows"  
site:x.com "Add" "to your wordlist" "nginx"  
site:x.com "Add" "to your wordlist" "PHP"
```

## 8️⃣ SPECIFIC ENDPOINT BASED

```
site:x.com "Add" "to your wordlist" "dashboard"
```

## 9️⃣ SENSITIVE KEYWORDS

```
site:x.com "Add" "to your wordlist" "sensitive"  
site:x.com "Add" "to your wordlist" "secret"  
site:x.com "Add" "to your wordlist" "JWT"  
site:x.com "Add" "to your wordlist" "token"  
site:x.com "Add" "to your wordlist" "private"  
site:x.com "Add" "to your wordlist" "internal"  
site:x.com "Add" "to your wordlist" "leak"
```

## 🔟 VERSION DISCLOSURES

```
site:x.com "Add" "to your wordlist" "version"  
site:x.com "Add" "to your wordlist" "vulnerable"  
site:x.com "Add" "to your wordlist" "outdated"
```

- Instead of X try twitter and get old results as well that got crawled and indexed by our best search engine Google :)
- Google Search Tools: Filter by date, time and custom range , and get results that others might overlook.
- Instead of fuzzing with everything, pick one new endpoint that you didn’t fuzzed till now and mass fuzz on all targets with VPS power.
- Observe in what common pattern a developer or sysadmin saves the file and folder names.

---

## G-Dorks against websites:

```
site:"*.nasa.gov" | "slack"
```

I found a PDF document on NASA’s website that contained a direct link to their internal Slack workspace.

![](https://miro.medium.com/v2/resize:fit:875/1*NOl9EbqGQ-10u55bMeyhSw.png)

I was able to use any Gmail account to have the access to their internal discussions. 🚀

![](https://miro.medium.com/v2/resize:fit:875/1*7NVE2hUMg0dYvsgldCPNMA.png)

![](https://miro.medium.com/v2/resize:fit:875/1*Ma78Pd8f3KNCzlbz8s8XeQ.png)

## Impact:

Anyone can join the slack workspace with any Gmail account. Moreover, slack channels often hold confidential information—internal conversations, sensitive documents, project plans, and much more. Anyone who gets in could access all of this.


Another dork example for this kind:

```
site:domain.com "join.slack"
site:domain.com "join.slack" ext:pdf site:domain.com  "join.slack" ext:doc site:domain.com  "join.slack" ext:docx  
  
odt,odf,csv,xls,xlsx,txt....
```

Google Search gave 4 results, the first one was a PDF document. I carefully gone through it , and observed private invitation link in a small corner of the document.

I quickly clicked on it, joined using Google OAUTH and boom guys

![](https://miro.medium.com/v2/resize:fit:875/1*5dBr3nwPaTSkk8VMOdz_-g.png)

## Steps To Reproduce

1. Visit the document at

https://domain[.]com/path1/path2/xyz.pdf

2.Observe the SLACK internal workspace joining link with invite code.  
3.Click on the joining link and authenticate using google OAUTH with any gmail  
4.Observe we can see all internal discussions and chats going around as an attacker.


---
# Microsoft ADFS Recon

#SSO 

#### What is the Microsoft ADFS endpoint?

- ADFS: Active Directory Federation Services
- A web-based endpoint that handles the authentication process to the ADFS
- A feature of Windows Server that provides users with single sign-on (SSO) capabilities to access systems and applications

#### 🤖ChatGPT Prompt

- _What is Microsoft ADFS endpoint, explain with a company real-world scenario_

![None](https://miro.medium.com/v2/resize:fit:700/1*-Os_uiACWYxSRTbaOHnL7Q.png)

						Generated by ChatGPT, OpenAI

- How government-based websites might use it?

> Endpoint Example

![None](https://miro.medium.com/v2/resize:fit:700/1*_-ooVh3PHZN3F1GKEvToMg.png)

> Wappalyzer Technologies Fingerprinting

![None](https://miro.medium.com/v2/resize:fit:700/1*4KkE-gvyx73dg7Xyfg6XIg.png)

#### 🐞 Google Dork

```vbnet
site:*.gov "©" "Microsoft" "Sign in with your organizational account"
site:*.gov.* "©" "Microsoft" "Sign in with your organizational account"
```

#### 🪲Shodan Dork

Failed to create effective dork, below dork gave only 4 results

```bash
http.html:"Sign in with your organizational account" http.html:"Microsoft"
```

Alternatively, I made this dork, but it doesn't directly narrow down to what we want.

```bash
"Microsoft HTTPAPI"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*M2mtiNk8sYAadngwnB8qYA.png)

Some more keywords/phrases/sentences whatever you refer it as

```bash
"Sign in using an X.509 certificate"
```

#### 📦Archived URLs
## waymore

```bash
waymore -i domain.com -mode U -oU waymore_domain.txt
cat waymore_domain.txt | grep "/adfs"
```

#### 🤖ChatGPT Prompt

_What types of authentication takes place in Microsoft ADFS web endpoint?_

- Integrated Windows Authentication (IWA)
- Forms-Based Authentication
- Certificate-Based Authentication (CBA)
- SAML Authentication
- OAuth 2.0 Authentication
- OpenID Connect (OIDC) Authentication
- Passive Authentication (Browser-Based)
- WS-Federation Authentication
- Anonymous Access (Metadata Retrieval)

#### 📗Template Building Process

I am not sure whether it's included in official nuclei templates or not, but I gave the prompt for "the keywords and conditions to match" to ChatGPT and it **gave me the nuclei template for it just like I am trying to do with dorks.**

- This is not a vulnerability scanning template, but an initial basic recon template, which may not be useful for everyone and that's why the severity level is "info"

- The last part about "extractors", ChatGPT included by itself which you can manually see when you visit the endpoint and observe the URL parts.

```json
id: adfs-detection-baseurl

info:
  name: ADFS Authentication Portal Detection (Base URL with Year Flexibility)
  author: legionhunter
  severity: info
  description: Identifies Microsoft ADFS authentication portals by testing common paths and accommodating a dynamic year in the © Microsoft copyright.
  tags: adfs, microsoft, sso, authentication

requests:
  - method: GET
    path:
      - "{{BaseURL}}"
      - "{{BaseURL}}/adfs/ls/"
      - "{{BaseURL}}/adfs/ls/idpinitiatedsignon.aspx"

    matchers-condition: or
    matchers:
      - type: regex
        regex:
          - "(?i)Sign in with your organizational account"
          - "©\\s*\\d{4}\\s*Microsoft"
      - type: status
        status:
          - 200

    extractors:
      - type: regex
        regex:
          - "client-request-id=([a-z0-9-]+)"
```

				file to use as a nuclei template

To check whether it's working or not, I added subdomains that contains the endpoint we want, as well as other subdomains that is not related to it.

![None](https://miro.medium.com/v2/resize:fit:700/1*Hn34rsP1T7v0irQqSyvCHQ.png)


```bash
nuclei -l subdomains.txt -t template-name.yaml
```

![None](https://miro.medium.com/v2/resize:fit:700/1*uQWiuI7Rn13EEyyzeAbQCA.png)


After this I am not sure how to proceed really, maybe if the year is old we can find some exploits I guess like in traditional CTFs and scanning with CVE-based templates.🤔

Okay as a noob, I have no clue how to proceed, let's ask ChatGPT again.

#### 🤖Prompt

- _List of vulnerabilities I should test if I come across this endpoint? don't give a general checklist, something unknown or uncommon and only related to this endpoint_

![None](https://miro.medium.com/v2/resize:fit:700/1*DvQ-WIOAlNRj27EzvOXm1w.png)

- _Now one by one in detail, give the methodology and testing procedure with detailed request and response application flow._

---

## XSS Dorks:


## 1️⃣ Multiple Parameter Endpoint

We will get urls that contains more than one parameter due to (&) symbol.

```
site:*.domain.com inurl:& inurl:? inurl:=

```

And I found this url:

```
https://xyz.target.com/resources/?dispenserid=1105&model=xyz&lang=English&resourcetype=xyz_ammers, her voice_

```
Now I just randomly add **testing** word in the **dispenserid** parameter and to my surprise it reflect as it is in the source code:

![](https://miro.medium.com/v2/resize:fit:875/1*-S7oxbEtjLpt6n_3WiWJtA.png)

								?dispenserid=1105testing

So, I think like why not try to add some html in place and I add this payload:

```
payload: “><h1>testing</h1>
```

people who don’t know why I used **“>** before h1 tag then to escape the **double quote (“”) and also anchor tag(<*a*>)** I used “> before the payload and now in source code the url became something like this:

![](https://miro.medium.com/v2/resize:fit:875/1*t6nSgZWqElmiM667H27SXg.png)

						?dispenserid=1105"><h1>testing</h1>

and you can see our payload successfully works and we successfully escape **anchor tag** and execute **h1 tag.**


```
Payload I used: “><script>alert(1)</script>
```

As I used this payload and what I see 403 forbidden 😭. And my heart broke 💔. But after testing I understand that **script tag** is whitelisted so I tried with another well know payload:

```
payload: “><img src=x onerror=alert(1)>testing</img>
```
_ammers, her voice_
```
payload: "></a></tr></td></div><img src=0 onerror=prompt();>
```


And this time yes my payload works. as I cannot show you popup because it reveals company but you can see our payload works.

![](https://miro.medium.com/v2/resize:fit:875/1*yJ9v1ffaY4VytorHoXotkw.png)

You can see there as I copy the first payload which is this one:

> https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot

```
payload:  
jaVasCript:/*-/*`/*\`/*’/*”/**/(/* */oNcliCk=alert() )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/ — !>\x3csVg/<sVg/oNloAd=alert()//>\x3e
```

and to my surprise this time payload got executed and I got another **RXSS 🤑**

![](https://miro.medium.com/v2/resize:fit:875/1*e4ys1QSzkeKLBfbJzzCRlw.png)

							put it at the end of the url

### Now back at the google dorking:

## 2️⃣ SERVLET
_ammers, her voice_
Based on the tech stack, dedicated parameter dorking changes as well, this is just one such example.

```
site:*.domain.com inurl:servlet  
site:*.domain.com inurl:servlet inurl:&  
site:*.domain.com inurl:servlet inurl:=  
site:*.domain.com inurl:servlet inurl:?  
site:*.domain.com inurl:servlet inurl:& inurl:= inurl:?
```

## 3️⃣ .XD & .AXD

I have gone through some SSRF articles and reports, and I observed a pattern, the URL contains “.xd” and “.axd”.So I pick these type of endpoints for testing other injection category as well.

```
site:*.domain.com inurl:.xd inurl:&  
site:*.domain.com inurl:.axd inurl:&
```

## 4️⃣ .NSF

NSF — Notes Storage Facility associated with IBM Lotus Notes.

```
site:*.domain.com inurl:.nsf inurl:&
```

## 5️⃣ .XHTM

Webpages written in XHTML (Extensible Hypertext Markup Language). If you don’t find using dorking, use the archived URLs , it will contain these types of old pages that are build with XHTML.

```
site:*.domain.com inurl:.xhtm inurl:&
```

## 6️⃣ .ACTION

Struts based web application — Apache Struts java framework for building web apps.

```
site:*.domain.com inurl:.action inurl:&

examples:

/login.action  
/admin.action  
/search.action  
/download.action  
/upload.action
```

## 7️⃣ .SVC

These are service endpoints for Windows Communication Foundation (WCF) framework — .NET tech stack based web apps.They also handle formats like SOAP or REST.

```
site:*.domain.com inurl:.svc inurl:&

https://api.example.com/CustomerService.svc/GetCustomer?id=123  
https://api.example.com/UserService.svc/GetUser?userId=456  
https://api.example.com/OrderService.svc/GetOrder?orderId=789
```
## 8️⃣ .DO

Mostly for older Apache Struts

```
site:*.domain.com inurl:.do inurl:&
```

## 9️⃣ YEAR

```
site:*.domain.com inurl:2024 inurl:&  
site:*.domain.com inurl:2023 inurl:&
```

## 🔟 TRANSLATED PARAMETERS

Why not change the parameters to the native language of the application.

![](https://miro.medium.com/v2/resize:fit:875/1*rdut1ffrKvbc_09st4pR2Q.png)

```
site:domain.com inurl:pagina inurl:&  
site:domain.com inurl:soort inurl:&  
site:domain.com inurl:pagina inurl:?  
site:domain.com inurl:soort inurl:?  
site:domain.com inurl:pagina inurl:=  
site:domain.com inurl:soort inurl:=
```

---

FInd some spreadsheets:

```
site:*.target.com intext:"spreadsheet"
```

I found many spreadsheets, but one stood out!

![](https://miro.medium.com/v2/resize:fit:875/1*_gJaiIu2PxP7Y3jl_WYyjQ.png)

Edit Permissions

You have probably used spreadsheets, imagine for once if you had created a sheet and you were the owner, however if someone else also has the same owner permission as you, they could edit, modify and simply delete the entire sheet. Sounds dangerous right?

I was able to do the same. I had the edit permissions, I could edit, modify and delete as well, however we are ethical hackers, so I simply made a POC added my name and took screenshots and reported responsibly with my findings.

![](https://miro.medium.com/v2/resize:fit:875/1*0Z-3eWA6ay78fgHeArQiAQ.png)

							POC


## Find DashBoards:

#Jenkins

## 1️⃣ Open Jenkins Instances

==intitle:===="Dashboard [Jenkins]"== ==Credentials==

![](https://miro.medium.com/v2/resize:fit:627/1*id1m_iqPlo19a_oxGgyFvg.png)

![](https://miro.medium.com/v2/resize:fit:627/1*PPN5z4Y25_EzInqO7HyVRg.png)

## Exploit Jenkings

![](https://miro.medium.com/v2/resize:fit:700/1*vO-H5xwl82J3RNqBN5No_Q.png)

					Jenkins Instance publicly accessible.

![](https://miro.medium.com/v2/resize:fit:700/1*3F0Z3_TuzsDwasTQr4SF1g.png)

					Signup enabled on Jenkins instance.

I quickly created an account and login but unfortunately the newly created user was not having administrator permissions.

![](https://miro.medium.com/v2/resize:fit:700/1*hXjrAjzaw7BIsXOwLtORkw.png)

							User with low privileges.

Since my account was kind of view only user account. I could not perform any administrative actions such as creating pipeline, running build, managing plugins, managing users, **executing OS level commands**, etc. Still I managed to find some interesting things, one of them was “people” menu.

![](https://miro.medium.com/v2/resize:fit:700/1*B4KckuI-E-5zk4KNj6-Ttg.png)

					Found all existing users in Jenkins instance.


**At last he suggested me to bruteforce the passwords since I have all existing usernames.**

## #2: Broken Authentication Due to Weak Password.

**I dumped all the usernames. There were total 1228 users in the system. Bruteforcing even 10000 common passwords for 1228 users can eat up huge amount of time.** So I thought to bruteforce same password as username. (~={yellow}For example, If the username is xyz, try xyz as a password)=~. I used burp intruder functionality to perform bruteforce and to my surprise I found 277 out of 1228 users have same password as their usernames.

![](https://miro.medium.com/v2/resize:fit:700/1*zkTlYBxZpwGon1WsSdA7jw.png)

					Users with same username and password.

I again went to login form and entered the very first user seen in above image, in both username and password and it worked. **I was lucky because the account I’ve logged in had administrative permission. After login I had access to plenty of stuff.** 

I had access to all pipelines, plugins, script console, in short all admin functionalities which Jenkins provide.

![](https://miro.medium.com/v2/resize:fit:700/1*zbdAl67BUFlUgYeo5F3EEQ.png)

![](https://miro.medium.com/v2/resize:fit:700/1*-IeALm86vwDK3aHNvv_Oew.png)

![](https://miro.medium.com/v2/resize:fit:700/1*hX2W6sn0tXSLdFpgSn6Lzg.png)

				User with admin permissions.

## #3: Its RCE time !!

I installed terminal plugin using plugin manager. this plugin allows user to execute OS commands right from the Jenkins instance but when I try to run any command such as pwd or ls or any simple command, **It was showing only 403 Forbidden. May be this was due to cloudflare WAF.**

Nope….This was not my last hope. We have alternate way — ~={cyan}“Groovy Script Console”.=~

I went to https://subdomain.example.in/computer/ to see all slave nodes connected with the master node.

![](https://miro.medium.com/v2/resize:fit:700/1*lG-73OVz5h4kZjtTKEPXVQ.png)

I clicked on one of the slave node and navigated to script console from left side menu and entered below script and clicked on RUN button.

> println “id”.execute().text

and yeah, id command was executed on the server. Wee weee RCE.!!

![](https://miro.medium.com/v2/resize:fit:700/1*UQmAYcgSddA28a6znra5PA.png)

				RCE through Jenkins groovy script console.

![](https://miro.medium.com/v2/resize:fit:700/1*Xb9nUivDm6-sootE4bLmrQ.png)

				RCE through Jenkins groovy script console

To show impact I have prepared scripts for all nodes to retrieve sensitive information. Such as AWS secrets and SSH private keys.

![](https://miro.medium.com/v2/resize:fit:700/1*3PLIkM8FSJqE_71MEjUSSA.png)

![](https://miro.medium.com/v2/resize:fit:700/1*Gm8xNxmaylVCAQJYTDWR0Q.png)

								AWS Secrets.

![](https://miro.medium.com/v2/resize:fit:700/1*Fueibd5FpcJPboNOaDhPnw.png)

								SSH Private Key.

They fixed the vulnerability within few days and restricted access to this subdomain and rewarded me with my highest bounty till date.

![](https://miro.medium.com/v2/resize:fit:700/1*K22Fc38HGkVjYjG95BipKA.png)


## TL; DR

> 1. Signup enabled on Jenkins instance.
> 
> 2. Created account and found valid usernames.
> 
> 3. Bruteforced to find if any user has same username and password. Found 277 out of 1228 user has same username and password.
> 
> 4. Found user with admin permissions.
> 
> 5. Used script console to execute OS commands.

---

2️⃣ **Atlassian Confluence Dashboard**

#related CVE: CVE-2019-3396  
intitle:dashboard-confluence…

![](https://miro.medium.com/v2/resize:fit:627/1*vs7Gyiz4Yp_IhxMbroducA.png)

## 3️⃣ phpMyAdmin

```bash
inurl:main ext:php "Welcome to phpMyAdmin" "running on"  
"Welcome to phpMyAdmin" "running on" inurl:main.php  
inurl:main.php phpMyAdmin  
inurl:main.php "Welcome to phpMyAdmin"  
inurl:sql.php "phpmyadmin"  
inurl:sql "phpmyadmin"
```

![](https://miro.medium.com/v2/resize:fit:627/1*4VWK8zNHHv_cEKAs2-1JYA.png)

![](https://miro.medium.com/v2/resize:fit:517/1*qRLJrztXajigyRa8Pql2HQ.png)

## 4️⃣ AWS S3 BUCKET

site:*.s3.amazonaws.com  
site:*.*.s3.amazonaws.com  
site:*.*.*.s3.amazonaws.com  
inurl:"s3.amazonaws.com"  
  
site:*.s3.amazonaws.com intitle:Bucket loading  
  
  
#here we are matching keywords in domain,title and url  
#also view the page source for hidden cloud assets in all subdomains, IPs, vhosts and much more deep stuff.

![](https://miro.medium.com/v2/resize:fit:627/1*QHrz-eTYUX64Avluz8uduA.png)

![](https://miro.medium.com/v2/resize:fit:627/1*zHj18k7w14gBBx437nvZrw.png)

## 5️⃣ KIBANA DASHBOARD

![](https://miro.medium.com/v2/resize:fit:627/1*Nub-T3EniP5edrbGu5I3aA.png)

inurl:kibana inurl:app  
inurl:kibana inurl:app inurl:5601  
  
#default port 5601, similarly make shodan dork for it as well

![](https://miro.medium.com/v2/resize:fit:627/1*Re0NBstGjY0f3WsxA_5ijQ.png)

![](https://miro.medium.com/v2/resize:fit:627/1*OM9_w0Wi1ZxbCHtCf3bO1Q.png)

![](https://miro.medium.com/v2/resize:fit:627/1*lj08Nqn6Uv7kpXQrw6hblw.png)

----

# AJAX Dorking

## Recon to find AJAX Endpoints


```
inurl:ajax inurl:all "[{"id":"" "command" "data" -questions -community -documentation
```

```
inurl:ajax inurl:refresh "pluralDelimiter"
inurl:ajax inurl:offset
inurl:ajax inurl:limit inurl:offset
```

```vbnet
inurl:ajax "settings" "basePath" -docs -github -questions -community -support
```

![None](https://miro.medium.com/v2/resize:fit:700/1*nzc30mU8QMCMDSzncQBRWg.png)


```
inurl:ajax_page_state
inurl:ajax_page_state "insert"
```

---

## Recon for SPARQL Injection

> Note: This is my own hit-and-trial random experiment, it may not be effective as of now for everyone! Many more to research about it in-depth.

#### What is SPARQL?

![None](https://miro.medium.com/v2/resize:fit:700/1*bSYsgK4InGtYoA3HUxVfuQ.png)

					Google AI Search Results Screenshot

#### How did I come across this?

- I was testing for XSS and manually checking for reflected inputs and any errors in my private target.

![None](https://miro.medium.com/v2/resize:fit:700/1*53Kxhg2ySgaPx0P6L2h2-g.png)


> To break this query, first I need to learn how to write these types of queries and their development!

🐞Google Dork

```bash
site:*.redacted.com "SPARQL QUERY"
site:*.redacted.com "SPARQL" "RDF"
site:*.redacted.com "SPARQL" "Submit Query"
```

🪲Shodan Recon

```bash
http.title:"sparql"
http.html:"sparql"
http.html:"sparql" http.html:"rdf"
http.html:"sparql" http.html:"query"
http.html:"sparql" http.html:"editor"
http.html:"sparql" http.html:"run"
http.html:"sparql" http.html:"execute"
```

⛏️Fuzzing Wordlist

```bash
/sparql
/query
/query.html
/query.htm
/sparql-form
/sparqleditor
/sparqleditor.jsp
/sparqleditor.jspx
/sparqleditor.do
/rdf
/rdfgraph
```

🪼Httpx Grep

- Grep the httpx output with title keywords for `sparql`, `editor`, `query`, `engine`, `rdf`, `graph`, etc…

```bash
subfinder -d domain.com -all -recursive > subs_domain.com.txt
cat subs_domain.com.txt | httpx -td -title -sc -ip > httpx_domain.com.txt
cat httpx_domain.com.txt | grep "keywordhere"

#also manually visit   each subdomain for hidden clues.
```

I am researching about it, meanwhile here are some resources about SPARQL Injection if you are interested, it's not well-structured or organized, but it feels like a potential hidden area for testing, **or maybe I am going in rabbit holes like in traditional CTFs.**

I can't paste some of the links here as there is a possibility of getting red flags in the future, but you can follow my post on X. The link to that post is added in the below Image Caption or just do a quick Google search about "SPARQL Injection"

![None](https://miro.medium.com/v2/resize:fit:700/1*355kQ4uCOdvXT_DYWlJzsw.png)

---

#easy_boounty

_5 dorks for mass hunters!_

#### 1️⃣ Go HTTP File Server

```bash
"Back" "Hidden" "Archive Zip" "gohttpserver" "written by"
```



![None](https://miro.medium.com/v2/resize:fit:700/1*VjYbnspJNlfxBSNqRlCmRQ.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*Jw28QDYLSDwZDd5vemygMQ.png)

#### 2️⃣ Shoutcast Server

```bash
"Stream Status" "Shoutcast Server" "History" intitle:"Shoutcast Server"
```

  
![None](https://miro.medium.com/v2/resize:fit:700/1*VEez_RUMSFI9yrrdcxmjUA.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*a0lDzKrFt5psp7-DpBZXSw.png)

#### 3️⃣ New Server

```bash
"Welcome to Your" "We are committed to bringing you the best service and finest Internet hosting solutions available."
```

![None](https://miro.medium.com/v2/resize:fit:700/1*4nfPTdPKYt_QVN7KSFFeJA.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*ehRn_tJDE3lmQT4XKJh-hw.png)

#### 4️⃣ Apollo Server

```BASH
intitle:"Apollo Server" "query this graph"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*ToQrtxb_8wtuVBlLuP5fhw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*9cBmP8x_F18p_Y20xzeuFw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*DQgKWTtL3xK7Xe-HDDs4XQ.png)

#### 5️⃣ SmartFoxServer

```bash
"SmartFoxServer 2X was installed successfully and is now up and running!" "default username"
```

No results via google, so tried bing dorking!

![None](https://miro.medium.com/v2/resize:fit:700/1*qPOkCWpi3eEmdMHnNCNBpA.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*nZV3GM5Ebg5G19gTUqLE-A.png)

Very smart server and user!🙂

> Products should be designed and programmed in such a way that after installation, if you don't change the default credentials, it shouldn't start working!

### 📗What's next?

- Check if any sensitive information disclosed.
- Check version disclosure and then find if public impactful exploits available.
- View page-source (more endpoints, JS file analysis,etc…)
- If login auth, try default credentials and weak credentials
- Perform fuzzing.
- Click on every feature it provides, proxy through burp and test everything, who knows you may get a CVE in your name hopefully if that product is used by many.
- Visit each endpoint and take a note of those paths in your private fuzzing wordlists.
- Read the endpoint documentation if exists and check for sensitive actions or paths that we can leverage to extract information or anything due to mistakes or improper implementation or misconfiguration.

> **Tip:** Always manually read the page source. You will find many interesting stuff that the public automation tools might miss.Just basic to intermediate javascript knowledge is fine, need not be upto developer level, but need to know atleast what is written by the developer. The mistakes‼️

![None](https://miro.medium.com/v2/resize:fit:700/1*sWtyeGd1JqTY_gef2vcpqQ.png)

- debugMode is set to true, so we fuzz with something large or expected, the server might spit out everything with **detailed error information including further details to internal API or sensitive strings exposure.**
- isAdminGroup is set to false from client side, check in your burp history for any request where we can find this `isAdminGroup` and then set to true, and see if there is server-side validation missing or implemented correctly.
- **Directly visit the endpoint** `/cgi-bin/` and check what it gives.You might get that same result with fuzzing, but with this we can directly check in few minutes.
- Mostly importantly **do code assessment for DOM XSS , this is one of the untouched area that isn't tested by many. Majority are rushing towards reflected type XSS with automation(that also default mode).**

# Hacking Exposed CAdvisor Panels

> https://osintteam.blog/hacking-exposed-cadvisor-panels-4c80b3ae55d7
## Let's monitor along with the DevOps team 😁

### What is CAdvisor?

- Product by Google
- Open-source container monitoring tool
- It collects information about the running containers as well as the subcontainers
- Information disclosure of CPU usage, memory consumption, network throughput errors & filesystem.

In short,it knows more than the containers know about themselves.😏

_Official Repo by Google📂_

> https://github.com/google/cadvisor

_More Details below 👇_

> [how to monitor kubernets container efficently](https://cast.ai/blog/cadvisor/)

### Got stuck?

TryHackMe rooms if you are banging your head as a beginner thinking "now what are containers😨R?&qut;

## Intro to Docker:

```
https://tryhackme.com/r/room/introtocontainerisation

https://tryhackme.com/module/container-security

https://tryhackme.com/r/room/k8sruntimesecurity

https://tryhackme.com/r/room/introtodockerk8pdqk

```

### What types of organizations are more likely to use this product?

- Companies moving towards microservices and containers.
- Companies moving from legacy systems to modern cloud architecture.
- Companies that provide IT services to others.
- Startups adopting modern tech stack.

### Dorking

Now it's time to perform the dork based recon.

_🐞Shodan Dork_

```bash
http.title:"CAdvisor" 
```

![None](https://miro.medium.com/v2/resize:fit:700/1*TGo0sSqbP56ctIvPSFXJhA.png)

> Perform facet analysis and observe if your target domain exists!

![None](https://miro.medium.com/v2/resize:fit:700/1*gum7UMcueDml1k3w3NxvSw.png)

							🪲Google Dork

```bash
"Allowed Cores" "Shares" "Reservation" "Limit" -site:docker.com -site:github.com
```

![None](https://miro.medium.com/v2/resize:fit:700/1*3vY9sxxhrL13QshEOsQqrw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*udqt5E-L0Z7bBl6UygYHWg.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*rs1SIoIalcekWM_Tr0fKFw.png)


---

## FOFA Dorking for Bug Hunters

## How to use FOFA search engine for OSINT, Recon, Bug Hunting & Pentesting

![None](https://miro.medium.com/v2/resize:fit:700/1*JSFRPeWOLoYGP3s6SxL6fg.png) 

🌐FOFA Search Engine: [https://en.fofa.info/](https://en.fofa.info/)

```ini
domain="example.com"
```

2711 Unique IPs found
![None](https://miro.medium.com/v2/resize:fit:700/1*gZ_ICIfODBwm84_Kd4CmmQ.png)


100 Favicons Found

![None](https://miro.medium.com/v2/resize:fit:700/1*mYG0IlekNOZiRLx6QqoVAw.png)


Click on any favicon and automatically, the hash value will be added to the existing dork



```ini
domain="example.com" && icon_hash="xxxxxxxxxx"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*5Gl-_xwPIN2zduYpNy0S4w.png)

I try to test the Non-WAF endpoints first

![None](https://miro.medium.com/v2/resize:fit:700/1*J0Dpnjw92NfYswFouk48AQ.png)

**1️⃣ HTTPS ports apart from 443**

```ini
domain="example.com" && protocol="https" && port!="443"
domain="example.com" && protocol="https" && port!="443" && port!="80"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*TtXtrTH_P4H2CPkFuqiI_A.png)

**2️⃣ HTTP ports apart from 80**

```ini
domain="example.com" && protocol="http" && port!="80"

domain="example.com" && protocol="http" && port!="80" && port!="443"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*rN5pi1ohf46vdIaXmM4_nw.png)

If it is no longer a live endpoint, no problem because now we got idea about what ports they are using. Now we can mass scan only these particular ports on all subdomains and IPs.

**3️⃣ Cloud Buckets**

```ini
domain="example.com" && body="ListBucketResult"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*NypicBbBENjztYwAi_nVcg.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*Z_lfB1D3Vs5bDe4Ek3newg.png)

If the name is assigned the keyword "public"

```xml
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/....">
<Name>....public.....</Name>
```

then it's not useful. In other cases, we can report and ask the company whether it's meant to be public or was public unintentionally.

![None](https://miro.medium.com/v2/resize:fit:700/1*R4Gg2omCmudRJE9oAFXe6A.png)

P4 is the max severity for this unless you can find highly sensitive information or any additional hidden clues to increase the attack surface at the bug report submission time.

![None](https://miro.medium.com/v2/resize:fit:700/0*FsY8DiMDm9X4eobD.png)

**Complete writeup ⬇️**

https://osintteam.blog/p4-bug-in-healthcare-company-979db17df4dd

**4️⃣ Metrics Endpoints or Similar to it**

```bash
body="http_request_duration_seconds_sum"
body="http_requests_in_flight"
body="http_responses_total"
body="http_request_duration_seconds_bucket"
body="http_request_duration_seconds_count"
body="flask_http_request_duration_seconds_sum"
body="python_gc_objects_uncollectable_total"
body="process_virtual_memory_bytes"
body="process_resident_memory_bytes"
body="http_request_duration_highr_seconds_bucket"
body="kasiopea_assignment_total"
body="by_path_counter_total"


#combine with below using && operator
body="GET"
body="POST"
body="PUT"
body="/api"
body="/auth"
body="password"
body="security"
body="roles"
body="groups"
body="/v1"
body="/v2"
domain="example.com"


#extras
body="ghc_gcdetails_elapsed_seconds"
body="ghc_gcdetails_par_max_copied_bytes"
body="ghc_max_mem_in_use_bytes"
body="ghc_gcs_total"
```

Depending on the program, it may be accepted as an information disclosure vulnerability, if not accepted you need to test the endpoints and API paths that are disclosed, find vulnerabilities in them, and then it counts as a valid report.

> Critical Paths

![None](https://miro.medium.com/v2/resize:fit:700/1*br_ON81TFtaPiBDpycEdSw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*lG68QssU-p6QXw-PIqLITg.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*AdFyvuo8FQkg6vyizjXmlQ.png)

> Endpoints

![None](https://miro.medium.com/v2/resize:fit:700/1*putg9-SIUNmMz6I-KaQjKQ.png)

**5️⃣ Register**

```ini
body="register" && body="login"
body="register" && body="login" && domain="example.com"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*kDGIz2BuUlBSCZJqLDHV6A.png)

**6️⃣ API Endpoints**

```bash
body="/api/v1" && domain="example.com"
body="/api/v2" && domain="example.com"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*CJzWZVhl7b34iYfUB0m3RQ.png)

**7️⃣ Admin Endpoints**

```bash
body="/admin" && domain="example.com"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*wm0aJ9fZWVN7slqCmvSLzw.png)

**8️⃣ Information Disclosure**

```ini
body="any file name that leads to info disc" && domain="example.com"
```

Example:

```ini
body="config.txt" && domain="example.com"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*xn3eErLpmw6WBKgdC2NAXA.png)

**9️⃣ API Keys in JS Files**

```bash
body="any_api_key_name_you_know" && domain="example.com"

#example for algolia api key
body="algolia_api_key" && domain="example.com"
body="algolia_application_id" && domain="example.com"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*nHq5SiLoSl_URFydONzIog.png)

**🔟 Match any string with previously known vulnerable endpoints**

Example: Previously, you know sub2.sub1.example.tld to be vulnerable to RXSS via "xyz" parameter. Now you view the page source of this endpoint, match for some unique keywords, phrases, strings , function, object or variable names.

```ini
body="keyword1" && body="keyword2" && domain="example.com"
```

May be the reported endpoint got patched, but there might be more similar endpoints available that are vulnerable to same vulnerability and also same endpoint behavior.

You can find previously vulnerable endpoints that got patched via HackerOne disclosed reports or via OpenBugBounty.

```bash
#hackerone
site:hackerone.com inurl:reports "domain.tld"


#openbugbounty
https://www.openbugbounty.org/reports/domain/example.com
```

This is publicly available information and open to all. Find the similarity between endpoints, connect the dots, and find bugs.

![None](https://miro.medium.com/v2/resize:fit:700/1*EXCgb3bciGWKEuRQOl4blw.png)

> https://medium.com/@abhirupkonwar04/list/8817a1178836 --- advanced google dorking

----


![None](https://miro.medium.com/v2/resize:fit:700/1*KIlCPYUP4CZmyVnpsXhA7A.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*fyuQT1_4asR8TBCGBr_hrQ.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*JHdpsXRMTFBP32v0qef8qw.png)



#### 📌Unique vulnerability writeups


-> https://medium.com/meetcyber/easy-p3-bug-ldap-null-bind-leads-to-extract-sensitive-credentials-0d06b8d58d99



