

> https://wordlists.assetnote.io/

_You can download all of the wordlists at once, using the following command:_

```bash
wget -r --no-parent -R "index.html*" https://wordlists-cdn.assetnote.io/data/ -nH -e robots=off
```


wordlists: [danielmiessler/SecLists: SecLists is the security tester's companion. It's a collection of multiple types of lists used during security assessments, collected in one place. List types include usernames, passwords, URLs, sensitive data patterns, fuzzing payloads, web shells, and many more.](https://github.com/danielmiessler/SecLists) & https://github.com/gmelodie/awesome-wordlists?tab=readme-ov-file
[password wordlists]([SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt at master · danielmiessler/SecLists](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/10-million-password-list-top-100.txt))
[Username wordlists]([SecLists/Usernames/Names/names.txt at master · danielmiessler/SecLists](https://github.com/danielmiessler/SecLists/blob/master/Usernames/Names/names.txt))
[Web Content]([SecLists/Discovery/Web-Content/big.txt at master · danielmiessler/SecLists](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/big.txt))

[KaliLists/dirbuster/directory-list-2.3-medium.txt at master · 3ndG4me/KaliLists](https://github.com/3ndG4me/KaliLists/blob/master/dirbuster/directory-list-2.3-medium.txt)

[https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS](https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS)
[https://github.com/netsecurity-as/subfuz/blob/master/subdomain_megalist.txt](https://github.com/netsecurity-as/subfuz/blob/master/subdomain_megalist.txt)


>[A list of good wordlists for BB](https://medium.com/@loyalonlytoday/a-list-of-good-wordlists-for-bug-bounty-hunters-7a6562df2aba)
### 1. Yassine Aboukir’s Wordlist Collection

🔗 [View on GitHub](https://gist.github.com/yassineaboukir/8e12adefbd505ef704674ad6ad48743d)

This curated list by **Yassine Aboukir** is an excellent starting point for bug hunters. It contains multiple high-quality wordlists categorized for different use cases, including:

- **Subdomains**
- **URLs & Endpoints**
- **Common directories**
- **API paths**
- **Custom wordlists from real-world engagements**

### 2. Combined Wordlists by 0xspade

🔗 [View on GitHub](https://github.com/0xspade/Combined-Wordlists)

This repository provides a **massive collection of wordlists** specifically optimized for **bug bounty reconnaissance** and **penetration testing**. It includes:

- **DNS Wordlists:** Subdomain brute-forcing lists
- **Fuzzing Lists:** For directory and endpoint discovery
- **Common Passwords:** To test weak authentication systems
- **Custom Wordlists:** Merged and refined from various sources

### 3. OneListForAll by six2dez

🔗 [View on GitHub](https://github.com/six2dez/OneListForAll/blob/main/onelistforallmicro.txt)

OneListForAll is an **intelligent wordlist generator** that adapts based on your target. It is particularly effective for:

- **Web Fuzzing** (directories, files, and endpoints)
- **API Discovery** (hidden API endpoints and parameters)
- **Optimized Payloads** (crafted for real-world security testing scenarios)

### 4. Orwagodfather’s Wordlist Collection

🔗 [View on GitHub](https://github.com/orwagodfather/WordList)

This repository contains numerous **wordlists for reconnaissance and fuzzing**, including:

- **Custom subdomain enumeration wordlists**
- **Web fuzzing lists** for tools like `ffuf`, `dirsearch`, and `gobuster`
- **Dynamic wordlist generation scripts**

### How to Use These Wordlists?

> install with: 'sudo wget'

Once you’ve downloaded the wordlists, you can integrate them into your favorite tools:

### 1. Subdomain Enumeration

Use `subfinder`, `amass`, or `assetfinder` with a wordlist to discover subdomains:

```
subfinder -d target.com -w wordlist.txt -o results.txt
```

### 2. Directory & File Fuzzing

Use `ffuf` or `gobuster` to find hidden directories and files:

```bash
ffuf -u https://target.com/FUZZ -w wordlist.txt
```

 **Search for publicly exposed** `**.bakup**` **file**,

```bash
dirsearch -u https://target.com/ -f -F -x 403,404
```

### 3. API Endpoint Discovery

Find hidden API endpoints with `feroxbuster` or `dirsearch`:

```bash
dirsearch -u https://api.target.com -w api_wordlist.txt
```

### 4. Brute Force Password Cracking

Use `hydra` or `john` for password attacks:

```
hydra -L users.txt -P passwords.txt ssh://target.com
```


**! Remember to use "locate" to find the right wordlists path.**

see wordlists: cd /usr/share/wordlists
![[Pasted image 20241130204852.png]]

Take a 100 lines of a specific wordlist:

![[Pasted image 20241130200509.png]]


Additional resource:

[SecLists — The Ultimate Security Wordlist Collection](https://github.com/danielmiessler/SecLists)

---


- Use DirBuster, Gobuster, DirSearch or FFuF

#resources: https://github.com/secfigo/Awesome-Fuzzing & https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content

[Directory Brute-Forcing: All Methods You Need to Know — File Extension, Status Filter, and Recursive Mode | by Shaikh Minhaz | Oct, 2024 | Medium](https://medium.com/@shaikhminhaz1975/well-now-that-youve-put-your-mind-in-the-right-direction-and-started-testing-a-website-the-first-c900776d6f89)

![[Pasted image 20241130195115.png]]



```bash
python3 dirsearch.py -u https://old.silvioceccato.edu.it -e html,php,txt -t 150 -x 403,404,500,429 -i 200,301,302 --random-agent --delay=2


```

try with a file:

```bash
dirsearch -l urls.txt -t 150 -x 403,404,500,429 -i 200,301,302 --random-agent -o dirs.txt

```

```bash
dirsearch -u https://sub2.sub1.domain.com -x 403,404,500,400,502,503,429 -e xml,json,sql,db,log,yml,yaml,bak,txt,tar.gz,zip -w /usr/share/seclists/Discovery/Web-Content/ApacheTomcat.fuzz.txt

dirsearch -u https://sub2.sub1.domain.com -x 403,404,500,400,502,503,429 -e xml,json,sql,db,log,yml,yaml,bak,txt,tar.gz,zip -w /path/to/wordlists/database.txt
```

![[Pasted image 20241223214406.png]]

## Wordlists type

Next, I ran a oneliner to fuzz all subdomains in background for many hours without any downtime (default wordlist by dirsearch)

```bash
while read -r subs; do dirsearch -u "https://$subs" -x 404,403,429 -i 200 --exclude-sizes 0; done < subs.txt
```

- Previous fuzzing didn’t gave any results, now I went to each subdomain manually via browser, check the technologies via wappalyzer, so this time I perform **dedicated tailored fuzzing**.

> PHP

https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/PHP.fuzz.txt

> JAVA

https://github.com/Steiner-254/Java-Fuzzing-Wordlist/blob/main/Java-Fuzzing-Wordlist

> ISS

https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/IIS.fuzz.txt

> Adobe Experience Manager

https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/AdobeCQ-AEM.txt

https://github.com/linuxadi/bug-bounty-fuzz-wordlist/blob/main/aem.txt

> NGINX

https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/nginx.txt

> CVE Related Paths

https://github.com/maverickNerd/wordlists/blob/master/cvePaths.txt

	If any path found, run nuclei with CVE detection.

> APACHE

https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/apache.txt

> CMS

Example: Wordpress, joomla, umbraco, drupal, etc…

https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content/CMS


> DUTCH wordlists (any domain with .nl top-level domain)

https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content/dutch

---

## Rate Limiting

source: [Earn $5000 After Learning How to Bypass the Rate Limiting for API Bug Hunting . | by Rishav anand | Nov, 2024 | Medium](https://medium.com/@anandrishav2228/earn-5000-after-learning-how-to-bypass-the-rate-limiting-for-api-bug-hunting-89dc40289120)

test rate limiting:

nano test_rate_limiting.py

```Python
import requests
import time

try:
    for i in range(50):
        response = requests.get('https://example.com/api/endpoint')
        print(f"Request {i+1}: {response.status_code}")
        time.sleep(0.1)  # Adjust timing to test rate-limiting behavior
except KeyboardInterrupt:
    print("\nScript interrupted by user. Exiting...")

```

---
## GoBuster

The GoBuster tool has a sleek Command Line Interface(CLI) built-in with the Go language. You need to have Go Programming language installed.


I. from GO

> go install github.com/OJ/gobuster/v3@latest

II from Kali:

> sudo apt install gobuster

**Usage**

								gobuster help

![](https://miro.medium.com/v2/resize:fit:875/1*S2t7qTCGGTD4JjrMCUgudA.png)

GoBuster has 4 modes of operation

- dir mode — directory brute-forcing
- dns mode — DNS subdomain brute-forcing
- s3 — Enumerate open s3 buckets
- vhost — virtual host brute-forcing for names

You can use the above help command with specific modes as well

```
gobuster help <mode>
```

# **dir mode**

Default options

```
gobuster dir -u <URL> -w ~_/usr/share/wordlists/dirb/common.txt_
```


![](https://miro.medium.com/v2/resize:fit:875/1*jxCgKYLAajSQf9PZAAD4mQ.png)

- -u is for the URL
- -w is for the wordlist

Default options and Verbose mode enabled:

```
gobuster dir -u <URL> -w ~_/usr/share/wordlists/dirb/big.txt_ -v
```

Specific status codes:

```
gobuster dir -u <URL> -w ~_/usr/share/wordlists/dirb/big.txt_ -s 200,301,302
```

Default options

```
gobsuter -m dns -w ~_/usr/share/wordlists/dirb/common.txt -u google.com_
```

![](https://miro.medium.com/v2/resize:fit:875/1*r2TaanFKuZ6WZzyIGG6gSQ.png)

Default options with IP option (-i)

```
gobsuter dns -d <URL> -w ~/wordlists/subdomain.txt -i
```

# **vhost mode**


```
gobuster vhost -u <URL> -w commom-vhosts.txt
```

# **s3 mode**


```
gobuster s3 -w buckets.txt
```


###  Checking for Backup and Old Files

Developers often leave backup or old versions of files on the server, which can reveal sensitive information.

**Command:**

```bash
gobuster dir -u <http://target.com> -w /usr/share/wordlists/dirb/common.txt -x .bak,.old,.zip,.tar.gz
```

----

## FFUF

#ffuf

_example usage:_

I noticed something while browsing the site, which is the link, especially endpoints

it was look like :

> [_https://example.com/webpages/EntaWebmain/anything.aspx_](https://example.com/webpages/EntaWebmain/anything.aspx)

In another place it changes, for example :

> [_https://example.com/webpages/EntaWebpayment/anything.aspx_](https://example.com/webpages/EntaWebmain/anything.aspx)

I immediately concluded that it was an internal path to call files and paths.

Then I said to myself why there are not more hidden paths? I used **webarchive** and all related tools in same purpose like **gau**,**gauplus** and even used crawling like **katana** but nothing !

I said then it’s time for FUZZING, the Command :

```bash
ffuf -u https://example.com/webpages/EntaWebFUZZ -w /usr/share/Seclists/Discovery/Web-Content/Common.txt -c -mc all -fc 404 -t 200

- mc all -> for match all status code
- -fc 404 -> hide 404
- -t -> threads (i was working on VPS so i set it little bit high)
```

And my feeling was right! I found admin but it was 403 Forbidden

Then I knew there was also a file to FUZZ on it, Previously I knew that the site was built on .NET and the extension was aspx, so I repeated almost the same process!

```bash
ffuf -u https://example.com/webpages/EntaWebadmin/FUZZ.aspx -w /usr/share/Seclists/Discovery/Web-Content/Common.txt -c -mc all -fc 404 -t 200
```

Then boom! I found a test file on the site!

![](https://miro.medium.com/v2/resize:fit:1250/1*stWarsNLrm2CpzJwy09hhA.png)My conclusion is that it is a file in the administrator’s environment to test sending emails.

Immediately I entered my account in the **reception** field and after trying in the **conformation** field I found that it consists of a number to confirm the **reservation** and the **reservation** field also has the same function, so I entered a random number of 6 digits and found that a message was sent to my account with sensitive information! The personal email of the registered person, name, address and payment transfers!

![](https://miro.medium.com/v2/resize:fit:1250/1*J3NTGGepAUgMa55x1NfkKw.png)

![](https://miro.medium.com/v2/resize:fit:593/1*cidA52wiugbZvRbCF2ou6Q.png)

	I immediately reported it and was accepted as a P2

---

#PII

# Advanced Wordlist Tactics Bug Bounty Automation

## A. Hybrid Attacks

Combine directory and file wordlists for maximum coverage:

# Merge wordlists  

> cat directory-list.txt file-list.txt > combined.txt  

# FFUF with parallel scanning    

> ffuf -w combined.txt -u [https://target.com/FUZZ](https://target.com/FUZZ) -t 100

## B. Mutation Rules

Use **rules to modify words** (e.g., `admin → Admin, ADMIN, admin123`):

# FFUF with rule-based mutations    

```bash
ffuf -w wordlist.txt -u https://target.com/FUZZ -e .php -maf rules/dynamic-script.rule
```

**Rules Example**:

```
append:123    
prepend:test    
uppercase
```

## C. Target-Specific Customization

- **For PHP Sites**: Prioritize `.php`, `.inc`, `.phtml`.
- **For Java Apps**: Fuzz `.jsp`, `.do`, `.action`.
- **For APIs**: Test `/v1`, `/v2`, `/graphql`, `/swagger-ui.`

# Directory Enumeration

## FFUF & Gobuster (General Web Apps)

**Wordlist**: `directory-list-2.3-medium.txt` (SecLists)

**Why**: Balanced coverage of common directories (e.g., `/admin`, `/backup`) without excessive bloat.

**Download**:

> git clone https://github.com/danielmiessler/SecLists.git  

- Path: `SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt`
- **Advanced Tip**: Combine with **dynamic rules** for depth:

# FFUF: Append extensions while fuzzing    

>ffuf -w directory-list-2.3-medium.txt -u https://target.com/FUZZ -e .php,.bak,.old    
  
# Gobuster: Specify extensions    

> gobuster dir -u https://target.com -w directory-list-2.3-medium.txt -x php,bak,old  

## API/Modern Web Apps

**Wordlist**: `api-wordlist.txt` (Custom)

**Why**: Targets routes like `/v1/users`, `/graphql`, or `/swagger.json`.

**Build Your Own**:

1. Extract endpoints from JS files using **LinkFinder**.
2. Scrape GitHub for the target’s API docs (search `site:github.com "target.com/api"`).
3. Merge with `raft-medium-words.txt` (SecLists).

- **Sample Command**:

```bash
ffuf -w api-wordlist.txt -u https://target.com/FUZZ -mc 200 -H "Content-Type: application/json"  
```

# File Enumeration

## Common Files (Backups, Configs, Logs)

**Wordlist**: `raft-large-files.txt` (SecLists)

- **Why**: Covers `backup.zip`, `.env`, `config.php`, and more.
- **Path**: `SecLists/Discovery/Web-Content/raft-large-files.tx`

**Pro Tip**: For **FUZZING FILENAMES WITH TIMESTAMPS** (e.g., `backup_20231001.tar`):

# Generate date-based wordlists    


```bash
for i in {2020..2023}; do echo "backup_${i}"; done > dates.txt    
ffuf -w dates.txt -u https://target.com/FUZZ.tar  

```
## Sensitive File Extensions

**Wordlist**: Custom list for high-value extensions:

```
.env    
.git    
.svn    
.htaccess    
.bak    
.swp    
.tar.gz  
```

**Usage**:

# FFUF    
> ffuf -w extensions.txt -u https://target.com/indexFUZZ    
  
# Gobuster    

```bash
gobuster dir -u https://target.com -w extensions.txt -s 200,301 -x env,git,bak  
```

# Cloud-Specific Enumeration

## AWS S3/GCP Buckets

**Wordlist**: `bucket-names.txt` (Custom)

- **Why**: Cloud buckets often use names like `prod-target-assets` or `target-backup`.
- **Build Strategy**:
- Use the target’s name/abbreviations (e.g., `target-prod`, `tgts3`).
- Merge with `cloud-buckets.txt` from **SecLists**.

**Command**:

```bash
ffuf -w bucket-names.txt -u https://FUZZ.s3.amazonaws.com -mc 200 -fs 0  

```
## Kubernetes/Internal Paths

**Wordlist**: `kubernetes.txt` (Custom)

- **Include Paths**:

```
/k8s/    
/kubernetes/    
/api/v1/namespaces    
/metrics    
/healthz  
```

- **Download**: Use `Discovery/Web-Content/raft-large-directories.txt` (SecLists) as a base.

# Server Headers & Misconfigurations

## Tool 1: Nmap

**Why Use It?**  
Nmap identifies server versions, open ports, and HTTP misconfigurations.

**Basic Command:**

```bash
nmap -sV --script=http-headers,http-title -p 80,443 target.com  
```
- `-sV`: Service version detection
- `--script`: Run specific NSE scripts

**Advanced Tactics:**

1. **Check Dangerous HTTP Methods:**

```bash
nmap -p 80,443 --script http-methods --script-args http-methods.url-path='/admin' target.com  
```

- Look for `PUT`, `DELETE`, or `TRACE` methods enabled.

**2. Find Directory Listings:**

```bash
nmap --script http-enum -p 80 target.com  
```

**Secret Trick:** Use `http-shellshock` script to test for Shellshock vulns in CGI endpoints.

## Tool 2: Nikto

**Why Use It?**  
Nikto automates checks for **6,000+ vulnerabilities**, including outdated servers and insecure headers.

**Basic Command:**

> nikto -h https://target.com  

**Advanced Flags:**

1. **Evade Detection:**

> nikto -h target.com -Tuning 1 -evasion 8  

- `-Tuning 1`: Scan only “interesting” files
- `-evasion 8`: URL-encode requests

1. **Check Specific Issues:**

> nikto -h target.com -Plugins "apache_expect_xss"  

**Pro Tip:** Parse Nikto’s output for:

- `X-Powered-By` headers (e.g., PHP 5.2.4 → exploit!)
- `Server: Apache/2.4.7 (Ubuntu)` → Check for CVEs.

# Secret Tips from Real-World Hunts

1. **The .git/.svn Heist**:  
    Found a `.git` directory? Use `git-dumper` to download the entire repo and check for API keys/hardcoded secrets.
2. **Backup File Extensions**:  
    Always test `www.zip`, `backup.tar`, or `index.php~` (common editor backups).
3. **Header Hacking**:

- Use `curl -I https://target.com` to quickly check headers.
- Hunt for `X-Debug-Token` (exposes debug pages) or `X-AspNet-Version`.

**4. Proxy Everything**:  
Route traffic through Burp Suite to manually inspect interesting responses.

# Automation & Reporting

1. **Bash Scripting**:  
    Automate scans and save outputs for reports:

```bash
#!/bin/bash   
ffuf -w wordlist.txt -u https://target.com/FUZZ -o ffuf.json   
nmap -sV -p 80,443 target.com -oN nmap.txt   
nikto -h target.com -output nikto.html
```

**2. Critical Findings for Reports**:

- Exposed admin panels (e.g., `/admin` with default creds)
- Server versions linked to CVEs
- Directory listings leaking credentials


---

# Clean output files:


You can use a simple command-line tool like awk to extract just the directory names from your directories.txt file. Here's how you can do it:

Using awk:

```bash
awk '{print $1}' directories.txt > clean_directories.txt
```
This command will take the first column (which contains the directory names) from directories.txt and save it to clean_directories.txt.

Using cut: Alternatively, you can use the cut command:

```bash
cut -d ' ' -f 1 directories.txt > clean_directories.txt
```
This command uses a space as the delimiter and extracts the first field, which is the directory name.

Using sed: You can also use sed to remove everything after the first space:


```bash
sed 's/ .*//' directories.txt > clean_directories.txt
```
This command replaces everything after the first space with nothing, leaving only the directory names.

After running any of these commands, you will have a clean_directories.txt file containing only the fuzzed directory names

---


**_dirsearch-u_** [**_https://target.com_**](https://target.com/) **_-i 200,300–399,401,403,500 -r_**

- _-r to do recursive scan_
- _-i to specify the status codes we want in output_
- _-u to specify the target url_

_I got a 200 status code on .htaccess & web.config files. Also I manually verified it was publicly exposed. I reported it under sensitive files exposure, should be a vuln._

## Login
Test for default log-in creds:

> Default Credentials: The Path of Least Resistance

**Tools and Techniques**

> **Tools:**

- `Hydra`: For brute-forcing credentials if account lockout isn't enabled.
- `Shodan`: To find publicly exposed admin panels or IoT devices.
- `Nmap + NSE Scripts`: For identifying services running with default configurations.

> **Methodology:**

- Enumerate services with tools like `Nmap`.
- Identify default admin panels and try common credentials like `admin:admin` or `root:toor`.
- Use Shodan to locate services exposed with weak or default credentials.

```
- **admin/admin**
    
- **admin/password**
    
- **admin/1234**
    
- **admin/12345**
    
- **admin/123456**
    
- **admin/admin123**
    
- **admin/root**
    
- **root/root**
    
- **root/admin**
    
- **root/password**
    
- **user/user**
    
- **user/password**
    
- **guest/guest**
    
- **guest/password**
    
- **test/test**
    
- **test/password**
```


brute force basic log-in page:

```
ffuf -w /path/to/usernames.txt:/path/to/passwords.txt -u http://target.com -H "Authorization: Basic FUZZ"

or

hydra -L /path/to/usernames.txt -P /path/to/passwords.txt http-get://target.com

```

**Mitigation Steps**

- **Policy Enforcement**: Mandate changing default credentials during deployment.
- **Regular Audits**: Use tools like `Lynis` or `Qualys` to scan for weak configurations.
- **Password Management**: Implement a centralized password vault like HashiCorp Vault.

---

## Approach

Generally my way of hunting is semi-auto and after doing enumuration I came up with the interesting end-points that has `*.lenovo.com/.git` which is forbidden (`403`). As a hacker you are thinking it right, when I further enumurate with `*.lenovo.com/.git/config` it gave me `200` status. Now I have a clue what I need to do next.

![](https://miro.medium.com/v2/resize:fit:394/1*AFGsRG8L_bbChh4LmgnKPA.png)

![](https://miro.medium.com/v2/resize:fit:424/1*BrimfbH8x6c6DSDs9ZwDcg.png)

## Bypassing 403 with git Dumper

#_404 

[Git-Dumper](https://github.com/arthaud/git-dumper)

Now you know that you can directly access any file content after `.git/{loot}` , if you are familier with CTF we have popular tool called [git-dumper](https://github.com/arthaud/git-dumper) which generally crawl and do fuzzing to dump all the possible `.git/files`. If you don’t want use git-dumper you can make own tool or even use `wget` to get the same result.

![](https://miro.medium.com/v2/resize:fit:656/1*sEJ3Sfkt1EPJVZJSPHb4rA.png)

> python3 git_dumper.py https://url/.git /path 

Now after dumping data I ended up with so many data and next thing I wanted to do was git analysis.

![](https://miro.medium.com/v2/resize:fit:656/1*YHAFzRgGyXr0KYCub-TOoQ.png)

I started with git analysis and ended up finding so much information about the org, information contained PII information, usernames, private ips and so on. Here is some sample info that i got from git-analysis.

![](https://miro.medium.com/v2/resize:fit:656/1*Bv_79770V_Moe0qEZg5xJQ.png)

**some command for git analysis;**

```
git reflog   
git show $refloghash  
git log   
git show $loghash  
gif diff $commithash1 $commithash2
```

----

## Exploit exposed .env files:

## Probing Endpoints

To uncover potential misconfigurations, I used a extension that was suggested by [coffinxp](https://medium.com/u/b5e6f179c752) called “endpoint extractor”. This extracted all the links, apis, and endpoints that might be present in the .js file.

Whiles going through the links, i found an endpoint:

/v2/env

So, i used this endpoint on all its subdomains using **httpx** flag of

“ — path /v2/env” and opened all the valid urls

```bash
ex: httpx --path /v2/env <subdomain>
```

To my surprise, the server returned the `.env` file in plain text.

## Step 3: Analyzing the Contents

The file contained sensitive information, including:

- internal diagnostic information
- endpoints with logging data
- Keys for third-party services like payment gateways and email providers.

![](https://miro.medium.com/v2/resize:fit:656/1*oLzAoMCds4b8KRJwy8mfCQ.png)


Script example usage:

```python
import httpx
import asyncio

async def test_endpoint(subdomain):
    url = f"http://{subdomain}/v2/env"
    async with httpx.AsyncClient() as client:
        response = await client.get(url)
        if response.status_code == 200:
            print(f"Valid URL: {url}")
        else:
            print(f"Invalid URL: {url} (Status Code: {response.status_code})")

subdomains = ["subdomain1.example.com", "subdomain2.example.com", "subdomain3.example.com"]  # Add your subdomains here

async def main():
    tasks = [test_endpoint(subdomain) for subdomain in subdomains]
    await asyncio.gather(*tasks)

asyncio.run(main())

```

The exposure of `.env` files could lead to:

1. **Database Compromise**: Attackers could gain unauthorized access to the database, leading to data theft or manipulation.
2. **API Abuse**: Exposed API keys could allow attackers to abuse third-party services, incurring financial or reputational losses.

----
### Crack hashes:


> hashcat -m 11600 -a 0 -o cracked.txt hash.txt /home/kali/OxygenWordlist.txt -O

resume session after checkpoint:

> hashcat --restore


---

# Mass Hunting for Leaked Sensitive Documents

#PII - TO DO

Detailed article from twitter.com/ott3ly:

- Project discovery’s public bug bounty programs
- BBSCOPE tool from sw33tLie
- Preparing VPS for Mass Hunting PDF Files
- Scanning the Targets For Big Bucks

source: https://ott3rly.com/mass-hunting-for-leaked-sensitive-documents/

![[Pasted image 20250613210447.png]]


