#Header_Injection
# Bypass Host Header Injection Protection

source: https://youtu.be/AAhZp1A4I0A

## Summary:

🧠 Host Header Injection → Password Reset Poisoning

A simple yet powerful chain that leads to account takeover! 🔓

🚨 Attack Flow:
1️⃣ Web app builds the password reset link based on the Host header
2️⃣ Attacker crafts request:

```HTTP
POST /reset HTTP/1.1  
Host: attacker[.]com 
```
... 
3️⃣ App includes attacker domain in reset link
4️⃣ Victim clicks reset → Token goes to attacker[.]com
5️⃣ 🎯 Attacker captures token → Resets password → Account takeover 💀


from here we have three functionalities:
![[Pasted image 20250125120932.png]]

	 The Login functionality, The register functionality and the forgot password function.

Obviously, we want to check the forgot password functionality because this is where **the email is going to get sent**.

Where we might to brute force the OTP, crack the reset link or do a host header injection over here, where is going to be, the best scenario possible.


Send link for password reset functionality and intercept:

![[Pasted image 20250125121611.png]]

-> send to repeater:

![[Pasted image 20250125121641.png]]

![[Pasted image 20250125122016.png]]

		the host header, is exactly the host present in the link.

Because if you go to see your mail box and look for the reset password link, you can see it contains the host inside:

![[Pasted image 20250125123034.png]]

Now to test the vuln, simply add a subdomain in the host and test if the domain gets reflected in the reset password or not:

![[Pasted image 20250125133722.png]]

! It might tell you "404" but still try to look into the reset password:

![[Pasted image 20250125133824.png]]


But if the domain dosen't validate your request, then try to add the domain name of the app aswell:



![[Pasted image 20250125133951.png]]


put 'attacker.com' as subdomain by the ':' quote

**Check if the Application check THE last value of the input:**


![[Pasted image 20250125134216.png]]

A few little more techniques: 

https://github.com/daffainfo/AllAboutBugBounty/blob/master/Host%20Header%20Injection.md


![[Pasted image 20250125135432.png]]


## How to Test "Forgot Password" for Bugs — A Guide for BB Hunters & Pentesters by Medusa

#### Typical Workflow of Forget Password

In most cases, the "Forgot Password" or password reset flow follows a standard pattern:

1. You visit the login page and click on "Forgot Password."
2. The application prompts you to enter your registered email address.
3. After submitting the email, a password reset link is sent to your inbox.
4. You click the one-time-use link, which opens a page to set a new password.
5. Once you enter and submit the new password, it replaces the old one, and you will be able to log in with it.

#### 1. Rate Limiting

Rate limiting restricts the number of times a specific action (like requesting a password reset) can be performed in a given timeframe. It's usually based on IP address, email, or account ID.

**How to test using Burp:**

1. Send a reset request in the browser and capture it in Burp Repeater.
2. Send the same request multiple times quickly.
3. If you don't see any signs of blocking, send the request to **Burp Intruder** with the same email as the payload. Set it to make 50–100 requests.
4. Check responses in **t**he Intruder results tab. Do you get a `429 Too Many Requests` Or something different after a point?
5. Try adding or spoofing headers like:

```http
X-Forwarded-For: 127.0.0.1
```

**Hackerone Reports for reference:**


> https://hackerone.com/reports/751604

> https://hackerone.com/reports/2052795

#### 2. Response Manipulation

**Many applications use an OTP (One-Time Password) step during password resets to verify ownership of the account. OTPs are typically sent via email or SMS.** 

However, insecure implementations may allow attackers to bypass or manipulate the OTP verification step by abusing how the server processes responses or verifies the code.

Where does this fit in the flow?

```sql
User enters email → Receives OTP → Enters OTP → Proceeds to set new password
```

_If the OTP step is weak, you might be able to bypass it entirely or guess a valid code due to poor validation or improper rate limiting.

**How to test using Burp:**

1. Submit the email and capture the request that initiates OTP delivery.
2. Intercept the request in **Burp Proxy** or send to **Repeater**.

```HTTP
POST /verify-otp HTTP/1.1
Host: target.com
Content-Type: application/json

{
  "email": "user@example.com",
  "otp": "123456"
}
```

3. It might return something like:


```json
{
  "success": false,
  "message": "Invalid OTP"
}
```

In Burp Proxy, before the response reaches the browser, change it to:

```json
{
  "success": true,
  "message": "OTP verified"
}
```

Then **forward** the modified response to the browser.

4. If the frontend is trusting the manipulated `success: true` response, no real OTP validation is being enforced server-side.

**Try submitting a new password even though no valid OTP was verified. If it works, you've bypassed the OTP check completely!**

----

by: https://youtu.be/lGYCqWKaon0

#Email/Login_Bypass
## Introduction: When we're about to Login, we intercept the response:

![[Pasted image 20250624222005.png]]


-> click "login"   on the Target App -> Do intercept -> Response to this req.

-> forward req.

![[Pasted image 20250624222120.png]]

And as we can see on the **Response section** We have a "302 Found"  & "Location   "/dashboard"

![[Pasted image 20250624222244.png]]

And.. very important to view, you can view the HTML code:


![[Pasted image 20250624222343.png]]

**In all the modern Browser, if a 302 response code is found,<mark style="background: #BBFABBA6;"> YOU SHOULD NOT SEE THE HTML CODE RENDERING IN THE Response Part**</mark>


![[Pasted image 20250624221606.png]]


Now to see the  Dashboard or the following page after the Login, simply forward again and turn of "intercept"


![[Pasted image 20250624222620.png]]

## Bypassing The Login Page:

First thing first... we will try to access directly into the DashBoard/the page after Login, **without logging in as a user**.

So we turn **intercept on** and try directly accessing the dashboard.

![[Pasted image 20250624223004.png]]


> [!NOTE] Warning
> Make sure the <mark style="background: #D2B3FFA6;">GET response is referring to the dashboard page</mark>, IF NOT, try forwarding again until you find it**, and just then, capture it.**

-> Do intercept -> response to the req.

-> forward req.

![[Pasted image 20250624223121.png]]

As we can see the WebServer confirms we're not authenticated, since it redirects back to the login page:  `Location: /`

![[Pasted image 20250624223749.png]]

But if you take a closer look, you can see that the HTML  Datais still **rendered**

![[Pasted image 20250624224025.png]]

The thing is... if the **response code is 302 Found** The browser isn't going to show you this data to redirect you to whatever redirection **Header is given to you.**


_**Now What if... instead of 302 redirect we insert**  "200 OK"?

![[Pasted image 20250624221509.png]]

And once u inserted "200" in the HTTP response, it might **give you the dashboard view** _Bypassing the Login page.

-> forward the request and stop capturing.


![[Pasted image 20250624221658.png]]


If this is **Patched** the response body might be this one:

![[Pasted image 20250624224922.png]]

							fixed version


.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:..:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:..:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._