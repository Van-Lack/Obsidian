
**Tip:** Remember to find PII on application that shouldn't be supposed at 100% to leak user info.


from: [What I learnt from reading 126* Information Disclosure Writeups. | by BrownBearSec - Freedium](https://freedium.cfd/https://medium.com/@BrownBearSec/what-i-learnt-from-reading-126-information-disclosure-writeups-d896c5d5a2a4) (2022)

- _[Information Disclosure is the 3rd highest paying bug](https://www.hackerone.com/top-ten-vulnerabilities)_. (More than IDOR, SQLi, PrivEsc, etc)

- Credentials — 18.2%
- PII — 14.1%
- Internal Documents — 12.1%
- Emails — 10.1%
- API keys — 10.1%
- Phone numbers — 8.1%
- Source code — 5.1%
- Etc


How are Information Disclosures discovered?

- API Abuse — 45.6%
- Directory bruteforcing — 17.6%
- Accidentally Public — 17.6%
- Dorking — 10.3%
- Leaking APIs 8.8%

**Leaking APIs**

A _leaking API_ is when a **functioning-as-intended** API, returns too much information.

These are usually APIs that return too much information that wasn't necessary, usually without the need for authentication

![Picture of a leaky API](https://miro.medium.com/v2/resize:fit:700/0*PVEaqdavPVuALPbE.png)

Image credit to the amazing Veshraj Ghimire ([https://medium.com/pentesternepal/hacking-dutch-government-for-a-lousy-t-shirt-8e1fd1b56deb](https://medium.com/pentesternepal/hacking-dutch-government-for-a-lousy-t-shirt-8e1fd1b56deb))

This is a _fairly obvious_ case, however, they can be more discrete. For example, if the service is trying to anonymise users, **is there userID leaked?** usually this would be useless, however, you can compare this to their deanonymised account, and if they are the same it would let you disclose someone's identity. ([Source](https://feed.bugs.xdavidhu.me/bugs/0003))

## Dorking

- Look on third party sites or out of scope assets. **Testing on out of scope assets is illegal**, passive looking is not. Eg, dork for `company.atlassian.com` and see if there are open Jira dashboards.
- Git dork for uncommon password formats. Instead of dorking for "username" "password", try dorking for connection strings like `postgres://` in the hopes you find something like `postgress://username:password@subdomain` ([Source](https://www.infosecarticles.com/github-the-goldmine-for-p1s-and-p2s-sensitive-information-exposure-via-github-by-a-company-employee/))
- If you find something, **can you turn it into a nuclei template?** Get 10x the bounties for the same amount of work researching. For example, [th3.d1p4k](https://medium.com/u/70f4fe357787) found "/plesk-stat" whilst hunting (which leaked logs), turned it into a nuclei template, and then used it on other targets for easy

Credit: [https://dewangpanchal98.medium.com/microsoft-bug-bounty-writeup-5ee4a7264dbf](https://dewangpanchal98.medium.com/microsoft-bug-bounty-writeup-5ee4a7264dbf)

My personal advice that I've used to find critical **API keys, credentials and more:** is to subdomain enumerate Gitlab or source code hosting subdomains, and then dork there. Github has been searched by everyone now, and git dorking is getting harder and harder.

example of dork:


Dork: **site:target.com intitle:index.of**

  
![](https://miro.medium.com/v2/resize:fit:671/1*yqpBXl_t5sg6M86Q1xaVrg.png)

  
![](https://miro.medium.com/v2/resize:fit:875/1*de0SWTZ5XFFHatIsHFUtlg.png)

![](https://miro.medium.com/v2/resize:fit:875/1*oKx4Q6WcJ5JFT74TAsRtIw.png)

**Pleas-Stat:** Plesk-stat is Log analyzer which generates advance web, streaming, ftp or mail server statistics, graphically. It can analyze log files from all major server tools like Apache log files (NCSA combined/XLF/ELF log format or common/CLF log format), WebStar, IIS (W3C log format) and a lot of other web, proxy, wap, streaming servers, mail servers and some ftp servers. [There are two reference 1) [awstats](https://www.awstats.org/) and 2) [webalizer](http://www.webalizer.org/).]

## Other Dorking Examples:

## Overview of this Vulnerability

This is a vulnerability where some information which is very sensitive/critical/confidential is publicly available or disclosed where it shouldn’t be. For Example,

![Example of Info Disclosure](https://miro.medium.com/v2/resize:fit:542/1*k3zFHrUTEQ-3uOkDbE-FvA.png)

This is one example of the vulnerability where the Sensitive Information like _DB_USERNAME, DB_PASSWORD_ is disclosed Publicly.

# 1. Exposed Credentials & Authentication Data

### Usernames & Passwords

```
site:example.com intext:"username" intext:"password" -git

site:example.com filetype:txt OR filetype:log "username" OR "password"

site:example.com inurl:admin OR inurl:login OR inurl:portal

site:pastebin.com "password" OR "login" OR "credentials"
```

# Hardcoded Credentials in Configuration Files

```
site:example.com filetype:env "DB_PASSWORD=" OR "DB_USER="

site:example.com filetype:json "AWS_ACCESS_KEY_ID=" OR "AWS_SECRET_ACCESS_KEY="

site:example.com filetype:config "password=" OR "apikey="
```

# Exposed SSH Keys & Private Keys

```
site:example.com filetype:pem OR filetype:key "BEGIN RSA PRIVATE KEY"

site:example.com "BEGIN OPENSSH PRIVATE KEY"
```

![](https://miro.medium.com/v2/resize:fit:64/0*uAYtAxP9uBFnhtkV.png)

# 2. Internal & Confidential Documents

# Leaked Internal Documents

```
site:example.com filetype:pdf OR filetype:doc OR filetype:xls "confidential" OR "internal use only"

site:example.com filetype:xls OR filetype:csv "employee salary" OR "payroll"

site:example.com filetype:docx "restricted" OR "not for public release"
```

# Exposed Source Code Containing Secrets

```
site:example.com filetype:php OR filetype:js OR filetype:env "DB_PASSWORD="

site:example.com intitle:"index of" "config.php"

site:github.com "password" OR "api_key" OR "secret_key"
```

![](https://miro.medium.com/v2/resize:fit:64/0*x_BsLZBExxe37m-z.png)

# 3. Financial & Payment Information

# Banking & Credit Card Data
```

site:example.com intext:"credit card number" OR "CVV" OR "Visa" OR "MasterCard"

site:example.com intext:"bank account number" OR "routing number"

site:example.com filetype:xls OR filetype:csv "financial report"
```

![](https://miro.medium.com/v2/resize:fit:64/0*wQwo_JoXAqOIKvIb.png)

# 4. Server & Configuration Leaks

# Sensitive Logs & Backup Files

```
site:example.com filetype:log OR filetype:txt "error log" OR "debug log"

site:example.com intitle:"index of" "backup"

site:example.com filetype:sql "database dump" OR "backup"
```

# Cloud & API Keys Exposure

```
site:example.com filetype:json "google_api_key=" OR "firebase_api_key="

site:github.com "api_key" OR "access_token" OR "client_secret"
```

![](https://miro.medium.com/v2/resize:fit:64/0*n_dEykQNrrJT-qW1.png)

# 5. Misconfigured & Open Directories

# Open Directory Listings

```
site:example.com intitle:"index of" "parent directory"

site:example.com intitle:"index of" "private" OR "confidential"

site:example.com intitle:"index of" "backup" OR "database"
```

# Exposed Admin Panels & Dashboards

```
site:example.com inurl:admin OR inurl:dashboard OR inurl:secure

site:example.com "admin panel" OR "restricted access"
```

![](https://miro.medium.com/v2/resize:fit:64/0*GCpqORLlSzWqci3S.png)

# 6. Sensitive Emails & Contact Information

```
site:example.com intext:"internal use only" OR "do not distribute"

site:example.com intext:"classified" OR "sensitive information"

site:example.com "employee email" OR "staff contact"
```

----

## JS Quick Recon:

> **Bug Intro**

"While browsing a website, we only see the content intended for users. But wait — there's more! Hidden files on websites may contain sensitive information like usernames, passwords, API keys, personal data, organizational secrets, cookies, or tokens. Some files might even be left on the server by mistake. If these fall into a hacker's hands, they could lead to a serious data breach."

> **Methodology i Followed**

1. **Find Subdomains and Increase Attack surface**

> subfinder -d target.com -all -recursive > subdomain.txt

> **Filter out Live domains**

> ==cat subdomain.com.txt | httpx -silent -ports 80,443,3000,8080,8000,8081,8008,8888,8443,9000,9001,9090 | tee -a alivesubsport.txt==

> **Find URLS using WayBackUrls** , **If not installed then link is in reference.**

> cat alivesubsport.txt | waybackurls > allurls.txt

Now sit back, wait until this is finished. We will give keywords, file extensions to grep and it will filter out.

2. **Grep tool usage**

> cat allurls.txt | grep 'keyword / extensions /parameters'

Below i am providing list of keywords / extensions / parameters to **find SSRF, LFI, RFI , SELF XSS, SENSITIVE FILES, OPEN REDIRECTION, PII , SECRETS, SESSIONS, TOKENS** and keywords to look for in waybackurls.

1. **Sensitive .JS files**

**Most of the time .js files has sensitive data** inside it like secret keys, tokens, endpoints , hidden paths . Because JavaScript works as backend .

> cat allurls.txt | grep '\.js' > jsurls.txt

Now manually visit all URLs in browser, try to read each line find for keywords using **ctrl + f** :- api, token, sessionid ,id, userid , id=, key= ,secret, config etc.you can also automate this process by SecretFinder tool. Link to tool provided in references. Install the tool as prescribed there.

> cat {jsurls.txt} | while read url; do python3; SecretFinder/SecretFinder.py -i $url -o cli; done

**Note:- Be mindful while giving Directory / File location to avoid file/ folder not found error.**

2. But more than jsfiles there are **Other Bunch of Sensitive Files Too.** find other sensitive files it could be csv file, excel sheet, pdf, or .sh file.

> cat allurls.txt | grep -Eo 'https?://[^ ]+\.(env|yaml|yml|json|xml|log|sql|ini|bak|conf|config|db|dbf|tar|gz|backup|swp|old|key|pem|crt|pfx|pdf|xlsx|xls|ppt|pptx)'

3. **Find jwt tokens and decode in jwt.io for finding token based IDOR**

> cat allurls.txt | grep 'eyJ'

4. **Other Tokens and Sessions**

> cat allurls.txt | grep 'sessionid='

> cat allurls.txt | grep 'token='

> cat allurls.txt | grep 'token='

> cat allurls.txt | grep 'secret='

> cat allurls.txt | grep 'secret'

> cat allurls.txt | grep 'code='

> cat allurls.txt | grep 'code'

5. **Keywords below you can use in grep**

> cat allurls.txt | grep 'keyword'

**Keywords:- @gmail , @doMainName , password, passwd, pass, uname, uid, userid, payment, orderid, invoice, private, name, /api/ , admin, configuration, settings, pay, receipt, payid, phno, pwd, mobile, phone, number, mail. Guess and toggle.**

6. Keywords for finding path to exploit XSS, LFI \ RFI, Open Redirection . Payloads you can use in References.

### SSRF:-

> ?dest={target} ?redirect={target} ?uri={target} ?path={target} ?continue={target} ?url={target} ?window={target} ?next={target} ?data={target} ?reference={target} ?site={target} ?html={target} ?val={target} ?validate={target} ?domain={target} ?callback={target} ?return={target} ?page={target} ?feed={target} ?host={target} ?port={target} ?to={target} ?out={target} ?view={target} ?dir={target}

### XSS:-

> ?q={payload} ?s={payload} ?search={payload} ?id={payload} ?lang={payload} ?keyword={payload} ?query={payload} ?page={payload} ?keywords={payload} ?year={payload} ?view={payload} ?email={payload} ?type={payload} ?name={payload} ?p={payload} ?month={payload} ?image={payload} ?list_type={payload} ?url={payload} ?terms={payload} ?categoryid={payload} ?key={payload} ?login={payload} ?begindate={payload} ?enddate={payload}

### **OPEN-REDIRECTION:-**

> ?next={payload} ?url={payload} ?target={payload} ?rurl={payload} ?dest={payload} ?destination={payload} ?redir={payload} ?redirect_uri={payload} ?redirect_url={payload} ?redirect={payload} /redirect/{payload} /cgi-bin/redirect.cgi?{payload} /out/{payload} /out?{payload} ?view={payload} /login?to={payload} ?image_url={payload} ?go={payload} ?return={payload} ?returnTo={payload} ?return_to={payload} ?checkout_url={payload} ?continue={payload} ?return_path={payload}

### **LFI / RFI:-**

> ?cat={payload} ?dir={payload} ?action={payload} ?board={payload} ?date={payload} ?detail={payload} ?file={payload} ?download={payload} ?path={payload} ?folder={payload} ?prefix={payload} ?include={payload} ?page={payload} ?inc={payload} ?locate={payload} ?show={payload} ?doc={payload} ?site={payload} ?type={payload} ?view={payload} ?content={payload} ?document={payload} ?layout={payload} ?mod={payload} ?conf={payload}

### SQLi :-

> ?id= ?page= ?dir= ?search= ?category= ?file= ?class= ?url= ?news= ?item= ?menu= ?lang= ?name= ?ref= ?title= ?view= ?topic= ?thread= ?type= ?date= ?form= ?join= ?main= ?nav= ?region=

### RCE :-

> ?cmd= ?exec= ?command= ?execute= ?ping= ?query= ?jump= ?code= ?reg= ?do= ?func= ?arg= ?option= ?load= ?process= ?step= ?read= ?function= ?req= ?feature= ?exe= ?module= ?payload= ?run= ?print=

### **References:-**

[SSRF PAYLOADS](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Server-Side-Request-Forgery-Payloads.txt) , [XSS Payloads](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Cross-Site-Scripting-XSS-Payloads.txt), [OPEN-REDIRECTION PAYLOADS](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Open%20Redirect/Intruder/Open-Redirect-payloads.txt), [SQLi PAYLOADS](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/SQL-Injection-Payloads.txt) , RCE PAYLOADS:- ([Windows](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/OS-Command-Injection-Windows-Payloads.txt), [Linux](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/OS-Command-Injection-Unix-Payloads.txt)).

**For RFI** , try to provide file URL which is hosted in another server / website and is malicious files(shell). If it rendered then RFI successful.

**LFI Payload lists:**- [1](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Linux-Log-Files.txt). [2](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Linux-Sensitive-Files.txt). [3](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Windows-Log-Files.txt). [4](https://pentestbook.six2dez.com/enumeration/web/lfi-rfi). [5](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Windows-Sensitive-Files.txt). [6](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Directory-Traversal-Payloads.txt)

**File Extension:-** [Wordlist](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/File-Extensions-Wordlist.txt)

**Install** [WayBackUrls](https://github.com/tomnomnom/waybackurls) , [SecretFinder](https://github.com/m4ll0k/SecretFinder)

---

## Information Disclosure Hunting:


**Google Dorking for Exposed Files**

```bash
site:domain.com ext:log OR ext:env OR ext:conf OR ext:sql OR ext:bak OR ext:json
```

- Searches for sensitive files indexed by Google

> **Other variations**

```css
intitle:"index of" site:domain.com
site:domain.com "password"
```

2. **Searching JavaScript Files for Secrets**

- Command to extract JavaScript files

```bash
katana -u https://www.domain.com -jc -o domain-js-files.txt
```

- Command to search for secrets

```perl
grep -E "(apiKey|secret|token|password)" domain-js-files.txt
```

- Finds API keys credentials and sensitive data

> **Unminify JS files for better readability**

```typescript
js-beautify domain-js-file.js
```

3. **Finding Java Files for Sensitive Information**

- Collect JS URLs from Burp, WaybackURLs, and a crawler
- Combine all links into a single file
- Run getJS on the combined file
- Save output to JS alllinks.txt
- Run Link Finder to extract endpoints from JS files
- Analyze JavaScript files with Js-Analyse

> **Commands**

- **Extract Java files from a target website**

```bash
waybackurls domain.com | grep "\.java" | tee java-files.txt
```

- This collects all Java file links indexed by the Wayback Machine

> **Download and analyze Java files**

```bash
mkdir java-files && cd java-files
cat ../java-files.txt | while read url; do wget "$url"; done
```

- This downloads all discovered Java files for manual inspection

> **Search for sensitive information in Java files**

```perl
grep -rE "(password|apikey|secret|database)" .
```

- Identifies sensitive keywords in Java files

> **Decompile Java class files using jadx**

```cpp
jadx -d output/ target.class
```

- Decompiles Java bytecode to readable Java source

4. **Finding Exposed Config Files via Wayback Machine**

```bash
waybackurls domain.com | grep -E "\.log|\.json|\.config|\.sql|\.env" | tee old-files.txt
```

> **Check if they still exist**

```bash
cat old-files.txt | httpx -mc 200
```

- Helps retrieve old sensitive files that may still be accessible

5. **Creating a Custom Wordlist**

> **Extract URLs from Wayback Machine**

```bash
waybackurls domain.com | tee wayback-urls.txt
```

> **Now extract words from those URLs to build a custom wordlist**

```bash
cat wayback-urls.txt | awk -F'/' '{print $NF}' | sort -u > domain-wayback-wordlist.txt
```

> **Extract Hidden Directories from Robots.txt & Sitemap.xml**

- Many websites hide sensitive paths in robots.txt and sitemap.xml

```perl
curl -s https://www.domain.com/robots.txt | grep "Disallow" | awk '{print $2}' > domain-robots.txt
curl -s https://www.domain.com/sitemap.xml | grep -oP '(?<=<loc>).*?(?=</loc>)' | awk -F'/' '{print $NF}' | sort -u > domain-sitemap.txt
```

6. **Use Assetnote Wordlists**

```ruby
wget https://wordlists.assetnote.io/data/manual/3k-wordlist.txt -O domain-assetnote.txt
```

7. **Extract Words from Website**

- Use cewl to extract words from site that may reveal hidden directories

```bash
cewl -d 3 -m 5 -w domain-cewl.txt https://www.domain.com
```

8. **Merge & Optimize Wordlists**

- Now combine all these wordlists into final-wordlist.txt

```bash
cat domain-wayback-wordlist.txt domain-robots.txt domain-sitemap.txt domain-assetnote.txt domain-cewl.txt | sort -u > final-wordlist.txt
```

9. **Crawling for Hidden Endpoints with Custom Wordlists**

- Using FFUF

```cpp
ffuf -w final-wordlist.txt -u https://www.domain.com/FUZZ -mc 200
```

- Using Dirsearch

```bash
dirsearch -u https://www.domain.com -w final-wordlist.txt -x json,log,config,sql,env,bak
```

10. **Automating Enumeration with Photon**

- Photon is a powerful web crawler for OSINT purposes

```bash
python3 photon.py -u https://www.domain.com --wayback
```

11. **Proxying Requests for Better Anonymity**

- Using a proxy while scanning can help maintain anonymity and bypass restrictions

```perl
python photon.py -u https://www.domain.com --proxy http://127.0.0.1:8080
```

12. **Checking for Misconfigured CORS**

```bash
curl -I https://www.domain.com/api/example -H "Origin: evil.com"
```

- If response contains

```makefile
Access-Control-Allow-Origin: *
```

- Then website API may allow unauthorized access


## Tools

> **Photon**

https://github.com/s0md3v/Photon

> JS-Beautify

https://github.com/beautify-web/js-beautify

> JADX

https://github.com/skylot/jadx


by: https://x.com/Commanak46 on X:

![[Pasted image 20250223121923.png]]

----

Use of nuclei template for automation:

[nuclei-templates/exposures/configs/plesk-stat.yaml at master · projectdiscovery/nuclei-templates](https://github.com/projectdiscovery/nuclei-templates/blob/master/exposures/configs/plesk-stat.yaml)


![](https://miro.medium.com/v2/resize:fit:875/1*7m0Y9O-gWCVvpCC4vQMmfg.png)

## Example of PII with Nuclei:

The target’s been **around since** 2016, so surely someone stashed a backup or two somewhere, right? Old servers, old habits. I took a trip down to the Nuclei templates repo and found one for hunting **_ZIP_** backups.

Now, here’s the fun part: I ignored those boring 200 OK pages and instead focused on the 302 **_redirects_** — because those login pages? Total pain in the ass. I filtered them out like this:

> cat httpx_report.txt | grep 302 | awk ‘{print $1}’ | anew 302_status.txt

With the list in hand, I sent Nuclei to do its thing:

> cat 302_status.txt | nuclei -t ~/private/zip-backup.yaml -c 40 -project armin -rl 400 -v | anew lowkey.txt

*Two hours in* — bam! — a hit. The terminal lights up with one word: uploads.zip. And yah, that’s when I knew something juicy was waiting inside.

Now I had to confirm it, so I ran it again:

> nuclei -u [https://redacted.supersecret.com/](https://redacted.supersecret.com/) -t ~/private/zip-backup.yaml -v -debug

The **ZIP** was a _1 GB beast_

> wget [https://redacted.supersecret.com/uploads.zip](https://redacted.supersecret.com/uploads.zip)

And just like that — poof! — the file’s on my cloud machine. I unzipped it and nearly fell off my chair: 150k+ PII records. I’m talking:  
- Phone numbers  
- Locations with lat/long  
- Emails  
- Names

![](https://miro.medium.com/v2/resize:fit:875/1*bEX_xwtT0Nza6FQZLtaNzQ.png)

						Some random excel sheets and csv files.

![](https://miro.medium.com/v2/resize:fit:875/1*YsPAipSYrDbUWOao49QrSQ.png)

							Couple of PII

That wasn’t all. I found Excel files named sales_january.xlsx, sales_december.xlsx, and — get this — dealers_india.xlsx. No _MS Excel_ on my rig (yeah, I’m poor), so I threw them into Google Sheets. And in the dealers file alone? 14k cleartext records. I sat there, staring at it, thinking: “Critical AF.”

![](https://miro.medium.com/v2/resize:fit:875/1*fPYFpB48Q0Bs9LmF8MDtJw.png)

						Super secret excel sheet-Hush


---

This is a _fairly obvious_ case, however, they can be more discrete. For example, if the service is trying to anonymise users, **is there userID leaked?** usually this would be useless, however, you can compare this to their deanonymised account, and if they are the same it would let you disclose someone's identity. ([Source](https://feed.bugs.xdavidhu.me/bugs/0003))


7. To no one's surprise, **Source code** was a big problem. Looking in **static JS** files, **html** files yielded lots of **PII, API keys, credentials** and more.
8. _Is the app just badly made?_ There were multiple examples of **PII leaking** settings persisting after being disabled. ([example](https://otmastimi.medium.com/users-location-diclosure-in-the-nearby-friends-feature-fabd24be05cb)), or poor implementations of **anonymity** ([example](https://3bodymo.medium.com/the-easiest-2500-i-got-it-from-bug-bounty-program-8f47ea4aff22)).
9. Check metadata of images downloaded, images uploaded that arent stripped can reveal location and **PII.**
10. Sometimes developers **don't mess up!** _But sometimes other employees do.._. Are subcontracted employees or non-technical employees leaking secret information on their personal pages?

#### Directory Bruteforcing

_I've covered Directory Bruteforcing_ extensively _in my blog on_ _[Content Discovery](https://medium.com/@Sm9l/bug-bounty-recon-content-discovery-efficiency-pays-2ec2462532b1)__,_ so once more, I'll talk about the **advanced tips** to **reduce the unnecessary requests** which hammer a target's server, and waste your time.

- Running seclist's `raft-large-directories-lowercase.txt` might get you url paths, but it **won't get you assets**. _Sensitive information is stored in files, not random pages on the site_ (usually). So **always fuzz for extensions**. The most common extension across the writeups was **.json**
- **Try to fuzz "smarter",** use _**small**_ **realistic** wordlists with _lots of extensions_, instead of massive generic wordlists. What's more likely to leak information `domain.com/emeapartner2007/` _(taken from raft-large-directories-lowercase.txt)_ or `domain.com/dev.zip`?
- Something I've discovered is that if you come across the generic Microsoft IIS page, use the [SNS tool](https://github.com/sw33tLie/sns), to find endpoints instead of just generic bruteforcing.

11. To no one's surprise, **Source code** was a big problem. Looking in **static JS** files, **html** files yielded lots of **PII, API keys, credentials** and more.
12. _Is the app just badly made?_ There were multiple examples of **PII leaking** settings persisting after being disabled. ([example](https://otmastimi.medium.com/users-location-diclosure-in-the-nearby-friends-feature-fabd24be05cb)), or poor implementations of **anonymity** ([example](https://3bodymo.medium.com/the-easiest-2500-i-got-it-from-bug-bounty-program-8f47ea4aff22)).
13. Check metadata of images downloaded, images uploaded that arent stripped can reveal location and **PII.**
14. Sometimes developers **don't mess up!** _But sometimes other employees do.._. Are subcontracted employees or non-technical employees leaking secret information on their personal pages?

#### API Abuse

Now for the big one… 45.6% of the Information Disclosures were got from API Abuse. Here's what we learnt.

15. **58%** of the API Abuse was through **IDORs.**
16. **27.8%** of the API Abuse was via **GraphQL**
17. The majority of API Abuses that were not IDORs occurred through exploiting **poorly implemented access controls** on API endpoints.
18. API Abuse was the _most common way to leak someone's identity_.

---

## Ultimate Nuclei Templates: Private Collection for Quick Bounties

> **What Are Nuclei Templates?**

Nuclei templates define the rules and logic by which Nuclei scans and detects vulnerabilities. These YAML based templates define the request method, target endpoints response matches and expected conditions to determine if a vulnerability is present.

List: [coffinxp_private_templates](https://github.com/coffinxp/nuclei-templates)

![[Pasted image 20250202213026.png]]

## Open Redirect

Open Redirect is a vulnerability where a web application improperly forwards users to untrusted sites allowing attackers to redirect victims to phishing or malicious websites. This template helps us to detect Open Redirect vulnerabilities by injecting various query parameters with a domain URL and checking if a redirection occurs

> cat domains.txt | nuclei -t openRedirect.yaml --retries 2

## **WP-Setup Disclosure**

This template helps identify the wp-admin/setup-config.php file which may expose sensitive information like credentials. Its often classified as a P1 vulnerability in bug bounty programs

> cat domains.txt | nuclei -t wp-setup-config.yaml

![](https://miro.medium.com/v2/resize:fit:700/1*zVzDeYUkxJ2MT0OcoCcNfA.jpeg)

![](https://miro.medium.com/v2/resize:fit:700/1*Wl4usORRI3Wfyg2dHjSO9g.jpeg)

## Microsoft IIS Scanner

Exploiting this vulnerability can leak files containing sensitive information such as credentials, configuration files, maintenance scripts and other data

> cat domains.txt | nuclei -t iis.yaml -c 30

![](https://miro.medium.com/v2/resize:fit:700/1*VsBpssp8dYAG-pQn9jqvGg.jpeg)

After discovering this vulnerability you can use the ShortScan tool to identify sensitive files and directories by using this command:

> shortscan https://domain.com -F

![](https://miro.medium.com/v2/resize:fit:700/1*XmEgerbeKL2YTqNQJyBHcA.jpeg)

## Git Exposure

The .git exposure vulnerability occurs when a Git repository .git directory or configuration files are accidentally exposed to the public internet due to misconfigured web servers. This can leak sensitive information such as source code, commit history and credentials which attackers could exploit to gain unauthorized access or identify vulnerabilities in the system

```
cat domains.txt | nuclei -t gitExposed.yaml  
```

![](https://miro.medium.com/v2/resize:fit:700/1*y1VdtnCiNIVozcc_zHiFqA.jpeg)

Next you can use the Git Dumper tool to recover deleted commits and extract additional details from the Git repository

![](https://miro.medium.com/v2/resize:fit:700/1*sASoQ8LMPWUloW2b9HSXDw.jpeg)

## CORS Misconfiguration

CORS (Cross-Origin Resource Sharing) vulnerability occurs when a web application improperly allows requests from untrusted domains enabling attackers to access sensitive data or perform unauthorized actions on behalf of users. This can happen if CORS settings are too permissive allowing malicious websites to interact with the application

```
cat domains.txt | nuclei -t cors.yaml
```

![](https://miro.medium.com/v2/resize:fit:700/1*ymWkb6HJPzHaJoaynjNFwA.jpeg)

to verify cors you can use burpsuite repeater or use the following CURL command.

```Perl
curl -H 'Origin: http://example.com' -I https://domain.com/wp-json/ | grep -i -e 'access-control-allow-origin' -e 'access-control-allow-methods' -e 'access-control-allow-credentials'

curl -H 'Origin: http://example.com' -I https://domain.com/wp-json/
```

![](https://miro.medium.com/v2/resize:fit:700/1*ZYWcN0xJJVc-hHrRfjv59g.jpeg)

## Crendential Disclosure

Credential disclosure is the exposure of sensitive information like passwords or API keys often due to poor storage or access controls leading to unauthorized access.

>cat domains.txt | nuclei -t  credentials-disclosure-all.yaml -c 30

![](https://miro.medium.com/v2/resize:fit:700/1*qFks0wYAU24IW81RXI1clQ.jpeg)

## Blind SSRF

Blind SSRF is when an attacker tricks a server into making requests to internal systems without seeing the response. The attacker infers success based on indirect clues like timing or errors. Its risky as it can expose internal resources.

> cat domains.txt | nuclei -t blind-ssrf.yaml -c 30 -dast

![](https://miro.medium.com/v2/resize:fit:700/1*oFpz1NNXhDGm8EW0PhRZLw.jpeg)

After detecting Blind SSRF use the Response SSRF template to access /etc/passwd and other internal files from the server

![](https://miro.medium.com/v2/resize:fit:700/1*goegPXPDVaTxjGbN95FSQg.jpeg)

To verify use the following CURL command it will show you etc passwd file of system

![](https://miro.medium.com/v2/resize:fit:700/1*nvxnLNGH-gOmEgiiMRbzdA.jpeg)

## SQL injection

SQL Injection is a type of attack where an attacker inserts malicious SQL code into a query allowing them to manipulate or access a database in unauthorized ways. this template will help to detect error based sql injection

> cat domains.txt | nuclei -t errorsqli.yaml -dast

![](https://miro.medium.com/v2/resize:fit:700/1*OmTXtz2uHqrAX3IRUxuPxw.jpeg)

![](https://miro.medium.com/v2/resize:fit:700/1*qeRaC97hWghINaGW0do0VQ.jpeg)

## Swagger-Ui XSS

Swagger XSS occurs when an attacker injects malicious scripts into Swagger UI a tool for API documentation. This can lead to unauthorized actions, data theft or ~={blue}defacement of the API’s interface impacting users interacting with it=~

![](https://miro.medium.com/v2/resize:fit:700/1*bi77pzgYXzuF9JGzBQv4gQ.jpeg)

After this, open the results urls If an XSS popup appears you’ve found a valid swagger XSS vulnerability. If no popup appears the version is not vulnerable to Swagger XSS.

![](https://miro.medium.com/v2/resize:fit:700/1*tu3tNDZV6MHh4bukTJJaYg.jpeg)

If XSS doesn’t work you can also use an HTML fake login phishing page by appending the following URL with the ?configUrl= parameter:

```
https://gist.githubusercontent.com/zenelite123/af28f9b61759b800cb65f93ae7227fb5/raw/04003a9372ac6a5077ad76aa3d20f2e76635765b/test.json
```

![](https://miro.medium.com/v2/resize:fit:700/1*Hh0yNM6u_QZH-0hhPn_rzQ.jpeg)

You can also detect Swagger XSS using this simple httpx oneliner with the following command:

```Perl
subfinder -d domain.txt -all -slent | httpx-toolkit -path /swagger-api/ -sc -content-length -mc 200
```

![](https://miro.medium.com/v2/resize:fit:700/1*bFlJfeuet0UOWQUHU8wMDw.jpeg)

## CRLF injection

CRLF Injection is when an attacker inserts malicious newline characters into input fields potentially allowing them to manipulate headers, create HTTP response splitting or inject malicious content in web applications.

```Perl
cat domains.txt | nuclei -t cRlf.yaml -rl 50 -c 30
```

![](https://miro.medium.com/v2/resize:fit:700/1*_t2NIAOdTe0dWbWlKyz4ng.jpeg)

After finding this you can confirm the vulnerability using Burp Suite or simply by using the CURL command.

```Perl
curl -I "https://domain.com/%0aSet-Cookie:coffin=hi;"
```

![](https://miro.medium.com/v2/resize:fit:700/1*-u9GEgJKCOWDDjMi0UGQVw.jpeg)

---

# Automation XSS, SSRF, LFI:


Today will show u how you can find ssrf xss and lfi using [gf](https://github.com/tomnomnom/gf) , [httpx](https://github.com/projectdiscovery/httpx) , [waybackurls](https://github.com/tomnomnom/waybackurls) , [qsreplace](https://github.com/tomnomnom/qsreplace) , [gau](https://github.com/lc/gau) tool .

This will help you in bug bounty because its advanced bug bounty tips.

### So let's start

### XSS

> _First let's start find xss for these we will use these tools_ _[gf](https://github.com/tomnomnom/gf)_ _,_ _[httpx](https://github.com/projectdiscovery/httpx)_ _,_ _[waybackurls](https://github.com/tomnomnom/waybackurls)_ _,_ _[qsreplace](https://github.com/tomnomnom/qsreplace)_ _, and command is like this :_

_`cat file.txt | gf xss | grep 'source=' | qsreplace '"><script>confirm(1)</script>' | while read host do ; do curl –silent –path-as-is –insecure "$host" | grep -qs "<script>confirm(1)" && echo "$host 33[0;31mVulnerablen";done`_

![None](https://miro.medium.com/v2/resize:fit:700/0*JuzQeoQjapXEzub8)

This command will find **XSS** in the target domain.

![[Pasted image 20250308080223.png]]

### SSRF

Now let's see how we can find **ssrf** using these tools. Here is the command to find SSRF on Target URLs

`findomain -t example.com -q | httpx -silent -threads 1000 | gau | grep "=" | qsreplace` [`http://YOUR.burpcollaborator.net`](http://your.burpcollaborator.net/)

![None](https://miro.medium.com/v2/resize:fit:700/0*7LSLH9537j7HcP59)

Here it will Filter the possible parameter of **ssrf** and also will send the request to your collaborator.

### LFI

> _Follow this command to find LFI :_

`findomain -t example.com -q | waybackurls |gf lfi | qsreplace FUZZ | while read url ; do ffuf -u $url -mr "root:x" -w ~/wordlist/LFI.txt ; done`

![None](https://miro.medium.com/v2/resize:fit:700/0*6aOM9lF5n5zjLB4Z)



----


![[Pasted image 20241130183555.png]]

---

## For a specific domain

```
https://web.archive.org/cdx/search/cdx?url=bugbountyhunter.xyz/*&output=text&fl=original&collapse=urlkey
```

## For subdomains (wildcard search)

```
https://web.archive.org/cdx/search/cdx?url=*.bugbountyhunter.xyz/*&output=text&fl=original&collapse=urlkey&filter=statuscode:200
```

While this process is effective and can help uncover useful information, my approach is slightly different and aims to increase the likelihood of discovering sensitive data. Here’s how I do it:

I always start my hunting with simple subdomain enumeration and directory fuzzing.

## Fuzzing Directories

**Why is this important?**  
Directory fuzzing allows you to uncover directories that might store sensitive information. It’s crucial because it helps you narrow down the URLs to focus on when searching the Wayback Machine.

## **The Technique:**

19. Use directory search tools to identify directories containing files like PDFs, documents, presentations, videos, or other directly accessible files.  
    I usually used wfuzz, dirsearch and ffuf

20. Even if the directory doesn’t initially seem to contain sensitive information, note its structure.
21. Once identified, check for older snapshots of that specific directory on the Wayback Machine ([http://web.archive.org/](http://web.archive.org/)).

This method increases the chances of finding sensitive information hidden in older versions of directories, making it a valuable addition to your bug bounty workflow.

**Note:** I created my own private custom python tool that notifies me when the tool found a potential sensitive info online including info from wayback machine. (I cannot share the tool for obvious reason) as the tool contains some privates keys for some online services.

**The Finding**

During my initial recon of the target organization `<REDACTED>`, I discovered multiple subdomains. I began fuzzing these subdomains, and one domain, `https://redacted.redacted.redacted.com/xxx/x/`, revealed a directory containing a `.PDF` file. Since this domain appeared to store files like PDFs, I focused my efforts on this domain/endpoint and ran my reconnaissance tools.

A few minutes into running the tool, it flagged potential sensitive information. Upon reviewing the logs, I noticed the information originated from `http://web.archive.org/`. At this point, I decided to manually dig deeper.

I examined the specific endpoint and checked random archive URL snapshots, example searching: `web.archive.org/web/<redacted-dates>*/https://redacted.redacted.redacted.com/xxx/x/`.


I found a URL that requested a password to view the document, suggesting that this endpoint likely contained sensitive information.

So get all the URL in that specific endpoint using this curl command and manually checking the results, you can play with the output.txt file by filtering strings using grep:

```
curl -L 'https://web.archive.org/cdx/search/cdx?url=redacted.redacted.redacted.com/xxx/x/*&output=text&fl=original&collapse=urlkey&filter=statuscode:200' > output.txt
```


Since many URLs followed a similar structure, I randomly inspected others and was shocked to find over 10 URLs accessible without authentication. Upon reviewing their content, I found numerous PDF documents labeled **“Confidential and Proprietary”** on their footers. These PDFs contained a significant amount of sensitive information.

![](https://miro.medium.com/v2/resize:fit:700/1*NU1LnUTXUFijkgRTm3VYLA.png)

			PDF footer labelled as confidential docs


I’ve watch and analyze each information and the video recording contained highly critical information about the organization. As a result, they prioritized fixing this issue and awarded me **$10,000** for this submission alone.

![](https://miro.medium.com/v2/resize:fit:700/1*B3nGkfUiHVRDsGmb6GOfPg.png)

			Bounty awarded via HackerOne API

## Bounty and Resolution

The bounty was awarded via HackerOne API. While I initially intended to submit more than 10 sensitive endpoints, after my fourth submission, they marked the remaining reports as **Informative** for the following reason:**_)_**

> three similar reports (redacted, redacted and redacted) to the one you sent us, an internal investigation was launched to identify and restrict access to multiple publicly accessible documents that could potentially contain confidential information.

>Temporarily, this report will be marked as Informative. Once the investigation is concluded, we will begin the triage of this report and the other similar ones.

# Disclosure Timeline

- 2024–12–15 — Report submitted to <*REDACTED> Program team.
- 2024–12–15 — Report status changed to Triaged.
- 2024–12–16 — <*REDACTED*> team ask how I managed to get the endpoints.
- 2024–12–16 — I responded explaining my automated recon tool and my directory fuzzing approach that I’ve explained above. Including leveraging the wayback machine to extract multiple endpoints/URLs
- 2024–12–19 — Report status changed from Triage to Closed (Resolved)
- 2024–12–19 — I’ve verified that the issue is not reproducible anymore
- 2024–12–24 — $10,000 Bounty rewarded.

----

Next to do list:

![[Pasted image 20250202205640.png]]