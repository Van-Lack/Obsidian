
[Account Takeover via API Misconfiguration and Logical Flaws | by Greenhats | Nov, 2024 | Medium](https://medium.com/@greenhatsmail/account-takeover-via-api-misconfiguration-and-logical-flaws-14a1caa90d44)

**Tip:**

## Brute Forcing JWT Secret

Use [jwt_tool](https://github.com/ticarpi/jwt_tool) to attempt cracking weak secrets:

```bash
jwt_tool <jwt_token> -C -d wordlist.txt
```


# Tools and Methodologies

Here are the tools and techniques I used to identify and validate the API vulnerabilities:

1. Postman: Simplifies crafting and sending HTTP requests.
2. Burp Suite: Intercepts requests to uncover hidden endpoints and test parameter tampering.
3. JWT Cracker: Identifies insecure token implementations. Run using:

> python jwt_tool.py -t eyJhbGciOiJIUzI1Ni... --brute wordlist.txt

---
# Auth Bypass Via JWK Header Injection

<iframe width="928" height="522" src="https://www.youtube.com/embed/aqG6RE76NdY" title="Authentication Bypass Via JWK Header Injection" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Theory:


![[Pasted image 20241205162933.png]]
![[Pasted image 20241205163040.png]]

kid - it works as a unique identifier that is used to sign the jwt (it tells which key to use from a set of avaible key.)

![[Pasted image 20241205163629.png]]

jku - this header is also called **Json Web Key** it's a URL where you can get the public key needed to verify the JWT signature, so Jku will contain this url as a value and is going to **fetch the keys** from that particular URL to verify the JWT signature



![[Pasted image 20241205163400.png]]


jwk - a public key is directly included in the JWT header

![[Pasted image 20241205164728.png]]

If public keys and private keys are being used for encrypting and decrypting, then that is a Assymmetric Encryption which means 2 keys are involved, and in symmetry encryption, only one key is involved for both encrypting and decrypting

## JWK Header Injection

Because the JWK contains the key to sign the token.

The servers expects a small sets of just one public key to valide the JWK signature

turn on the proxy and  start exploring the webApp:

some JWT could be found after log-in:

![[Pasted image 20241205172159.png]]

So let's create a private key and a public key and we're gonna sign-in our own token on the JWT Editor:

![[Pasted image 20241205172430.png]]
then go back again to the repeater tab and change the sub name to "administrator":

![[Pasted image 20241205172540.png]]

Then click on **Attack** in the right bottom and click "embedded JWT key:" and select your signed-in key:

![[Pasted image 20241205172652.png]]

And see if the server accepts our random value:

![[Pasted image 20241205173052.png]]

			look at the response in the browser, and you should be admin

----

https://www.youtube.com/watch?v=RmR6cmyLmiI -- JWT hack

---


![Preview image](https://miro.medium.com/v2/resize:fit:700/1*Ub2hTcVumcJihyPPQUB9zg.png)

# Finding emails for this issue. Finding a easy bug to get a easy $


so let's see how to find emails using jwt tokens

after collect all subdomains find waybackurls

using waybackurls [[https://github.com/tomnomnom/waybackurls](https://github.com/tomnomnom/waybackurls)]

![None](https://miro.medium.com/v2/resize:fit:700/1*8pFgLPOKAKgbmmanyTOrvg.png)

using gau [[https://github.com/lc/gau](https://github.com/lc/gau)]

![None](https://miro.medium.com/v2/resize:fit:700/1*zeirCtOpiSg4s36qFvmIEg.png)

using waymore [[https://github.com/xnl-h4ck3r/waymore](https://github.com/xnl-h4ck3r/waymore)]

![None](https://miro.medium.com/v2/resize:fit:700/1*2xHaglXp2TUnEOE2g3bbWQ.png)

after using this command to grep jwt tokens

cat * | grep "=eyJ"

![None](https://miro.medium.com/v2/resize:fit:700/1*TmPJ5NOgAFLcSWqdPWXhBw.png)

copy one of the jwt token and use jwt.io to check what is included in that token

you can see this token is included phonenumber,email,name

**NOTE:WHEN YOU FIND A JWT TOKENS MORETHEN 10 INCLUDING PHONENUMBER,EMAIL,ADDRESS,OTHER SENSITIVE INFORMATION YOU CAN REPORT TO YOUR PROGRAM THIS AS A SEPARATE ISSUE .**

![None](https://miro.medium.com/v2/resize:fit:700/1*Y3IbasRGwuuYSuuveO-hcg.png)

now you can see in jwt leaking a email copy that email . i think you know what to do .

you can see in below images.

![None](https://miro.medium.com/v2/resize:fit:700/1*npeBiYZaAjp5NqCdu5mfdg.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*ZtmYh7VJr5q6AQBT_maPHg.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*Ub2hTcVumcJihyPPQUB9zg.png)

if your able to see chats using these emails.

Iam leaving this to your imagination how critical is this .

Thats it .

<iframe width="928" height="522" src="https://www.youtube.com/embed/hoWwcRRKUwM" title="🚨Admin Panel Bypass | Privilege Escalation with JWT (JSON Web Token) | Bug Bounty - PoC 🚨" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
———————————————————————————————————————————

TO DO

by: https://medium.com/@monethic/jwt-post-exploitation-vectors-cbaa90ac1a65


— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
## Exploiting JWT Manipulation on a Web3 Crypto App

### 🔥 The $6,000 Bug Bounty Story

A few months ago, while hunting for vulnerabilities in Web3 applications, I stumbled upon a **critical JWT (JSON Web Token) manipulation bug** that led to a **$6,000 bounty payout!**


### 🧐 What is JWT and Why Does It Matter?

JWT (JSON Web Token) is widely used for authentication in Web3 and traditional web applications. **It's a compact, URL-safe token that stores claims between parties. A typical JWT looks like this:**

To recognize a JWT:

- **Structure**: It has three parts separated by dots.
    
- **Header**: Base64Url encoded JSON object.
    
- **Payload**: Base64Url encoded JSON object.
    
- **Signature**: Base64Url encoded string created using the header, payload, and a secret key with a specified algorithm (HS256 in this case).

>eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoxMjM0NTYsImFkbWluIjpmYWxzZX0.7QDfCe1D7sPdX9Dg3pYMLkx_1J1G


This token has **three parts**:

1. **Header** — Contains the algorithm and token type.
2. **Payload** — Contains the claims (e.g., user ID, roles).
3. **Signature** — Ensures the token's integrity.

When implemented correctly, JWT ensures **secure authentication**. However, **misconfigurations** can make it a hacker's playground. 🎯

### 🔎 Finding the Vulnerability — The JWT Weakness 🕵️‍♂️

While testing a Web3 crypto platform, I noticed they used **JWTs for authentication**. A simple **Burp Suite request interception** revealed:

```json
{
  "alg": "HS256",   // we find out the Websites uses a weak type of encryption
  "typ": "JWT"
}
```

And the payload:

```json
{
  "user_id": "123456",
  "admin": false
}
```

### 🚨 The Mistake: Weak JWT Validation

The platform used **HS256 (HMAC-SHA256)** with a ~={yellow}weak shared secret key=~, which I could **brute-force** using tools like **John the Ripper** or **jwt-cracker**.

### 💡 Exploiting JWT Manipulation for Admin Access

### Step 1: Extracting the JWT Token

Copy the entire JWT token from the intercepted request. A JWT typically looks like this: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiMTIzNDU2IiwiYWRtaW4iOmZhbHNlfQ.somelongstring`

- I copied my JWT token and decoded it using [jwt.io](https://jwt.io/).

### Step 2: Brute-Forcing the Secret Key 🔓

Since the site used a weak **HS256** key, I ran:

```lua
jwt-cracker mytoken.jwt --wordlist=rockyou.txt
```

Within **minutes**, I got the secret key: `web3rocks123` 🎯

### Step 3: Crafting a Fake Admin Token

With the secret key, I modified the payload:

```json
{
  "user_id": "123456",
  "admin": true
}
```

Then, I generated a new JWT using Python:

```python
import jwt
secret = "web3rocks123"
payload = {"user_id": "123456", "admin": True}
token = jwt.encode(payload, secret, algorithm="HS256")
print(token)
```

To execute the Python code on the targeted website, you'll need to set up your environment and tools properly. Here’s a step-by-step guide:

1. **Install Dependencies:** First, ensure you have the necessary libraries installed. You can do this using pip:

```bash
pip install pyjwt
```

**Generate the JWT Token:** Using the code snippet you provided, create the new JWT token:

- **Intercept the Request:** Use a tool like **Burp Suite** to intercept the request to the target website. This lets you modify the JWT token in the HTTP request.
    
- **Replace the JWT Token:** Once you've intercepted the request, replace the original JWT token with the newly generated one from your Python script.
    
- **Forward the Request:** After replacing the token, forward the request to the server.
    
- **Verify Admin Access:** Check if the website grants you admin access. If successful, you should see admin functionalities.

### Step 4: Gaining Admin Privileges 🚀

I replaced my old JWT with the new one in **Burp Suite** and refreshed the page…

💥 **BOOM! I had admin access!** 😱

### 🔐 How You Can Protect Your Web3 App from JWT Attacks

1️⃣ **Use Stronger Algorithms** — Prefer **RS256 (asymmetric keys) over HS256**. 2️⃣ **Keep Secret Keys Secure** — Never hardcode keys in source code. 3️⃣ **Enforce Expiry Time** — Set short-lived tokens. 4️⃣ **Validate Signatures Properly** — Always verify signatures on the server side. 5️⃣ **Implement Role-Based Access Control (RBAC)** — Don't rely on JWTs alone for privilege escalation.