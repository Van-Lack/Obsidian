**CSRF — Cross Site Request Forgery**

I will do a brief explanation of every vulnerability just to remind you in case you don’t have a clear vision or you just forgot.

**Cross-Site Request Forgery (CSRF)** is a type of security vulnerability that occurs when an attacker tricks a user’s browser into making an unintended and unauthorized request. This usually happens when a user is authenticated on a website, and the attacker can exploit this trust to perform actions on behalf of the user without their consent.

So, you have found your target, in my case, my target will be some random website.

Firstly, when you are on your target website, you should open up **Chrome developer tools** by pressing F12 or **Right-click** and click **Inspect** or **CTRL+Shift+i.**

After you have opened the developer tools, navigate over to **Network tab.** This tab is responsible for any HTTP requests that are being made on your target website.

Then just start playing around with the website. This website is a chat based website so lets try to send a message. When I try to delete a message, this request has been made to the target website’s API.

![[Pasted image 20241123003114.png]]

Then just start playing around with the website. This website is a chat based website so lets try to send a message. When I try to delete a message, this request has been made to the target website’s API

![[Pasted image 20241123003403.png]]

If you click on the request you can see what has happened.

![[Pasted image 20241123003459.png]]


Now you can test to see if you can send messages on behalf of other users via CSRF vulnerability. Right click on the request, click **Copy** and then click **Copy as fetch**

![[Pasted image 20241123003513.png]]
Now lets test to see if there is a CSRF vulnerability.

Go to another website, I recommend you go to **example.com** and open again developer tools but this time navigate over to the console tab.

![[Pasted image 20241123003544.png]]

paste what you have copied before:

![[Pasted image 20241123003730.png]]

However, you might see **mode** property here. Make sure the **mode** property is set to **no-cors**. Also make sure the **credentials** is set to **include**
![[Pasted image 20241123003740.png]]

Now just hit enter and see what happens.

![[Pasted image 20241123003749.png]]

As you can see, this request has failed, meaning this website has proper protection against requests that are sent from another domain, in my case from example.com.

So what now? I didn’t find a **CSRF** and should I stop looking for them? I mean this website probably has protected every API endpoint, right? No! Always test for more. I will now see if I can add a message reaction like a smiley face or sad face from another domain. Lets test that out!

So I will make a message reaction on this website and there is a HTTP request made.

![[Pasted image 20241123004101.png]]

I will again click **Copy** and **Copy as fetch**, go to example.com, set **mode** to **no-cors** and try sending the request.

![[Pasted image 20241123004112.png]]

Now I will hit enter to send this request from **example.com**.

![[Pasted image 20241123004311.png]]

to see if the request received a 200 Status code.

![[Pasted image 20241123004128.png]]

As you can see, this request was authorized and now let me check if I added a reaction to a target message.

![[Pasted image 20241123004139.png]]

Sure did!


This is a CSRF vulnerability where an attacker can create an exploit website which once the victim visits, it will automatically send a message reaction request to a certain message and add a reaction on victim’s behalf to that message if the victim is logged in.

The console tab executes code as if it was executed on a page where the developer tools are opened. So, this just means the example.com sent an authorized and authenticated request to add a reaction to a whole different website and it worked. You can now try different things for example updating profile information then see which request did that in the network tab, copy it, go to example.com, open console and paste the request, change the **mode** to **no-cors** and try sending it from example.com. If it works, then you found a vulnerability, if it doesn’t then look for something else, just like I did.

---

suddenly went to my burp and created another account their and captured the delete account request and created a csrf poc and saved it in my desktop on .html file(make sure to save it on all files). Then on my chrome I created another account and logged in, then. I clicked the submit button from the csrf poc and 🤯 MY ACCOUNT WAS DELETED .

*steps to reproduce:*

first intercept the delete functionality:

![[Pasted image 20250118153919.png]]

then create a CSRF POC:

![[Pasted image 20250118131657.png]]

-> copy html

![[Pasted image 20250118131725.png]]

Save it to a file:

![[Pasted image 20250118131740.png]]

_then create a new account, so log-in as a new user:_

![[Pasted image 20250118131835.png]]

put the file "delete.html" in a new tab of the browser, and click the botton:

![[Pasted image 20250118131950.png]]

![[Pasted image 20250118132013.png]]

and the victim should be automatically logged out and account deleted:

![[Pasted image 20250118154043.png]]


I submitted the bug putting the severity medium on csrf and after one day they triaged the report and moved the severity to High 7.1.

![](https://miro.medium.com/v2/resize:fit:656/1*lLnbj2pw1RypKdCM35dYDw@2x.jpeg)


----

### CSRF: A complete guide to exploiting advanced CSRF vulnerabilities

_what's Cross Site Request Forgery vulnerability?_

CSRF—vulnerabilities are one of the most exploited web security vulnerabilities that result in performing unwanted actions.

Impact:   From basic action modification (such as liking a post, following a new member or updating a metric's value) to a full account takeover by changing the victim's email address or password, disabling 2-FA or resetting a password.

source: https://www.intigriti.com/researchers/blog/hacking-tools/csrf-a-complete-guide-to-exploiting-advanced-csrf-vulnerabilities

### When is it vulnerable to CSRF?

 3 main conditions have to be met:

1. The targeted functionality or feature must be privileged
    
2. Your session ID must be a cookie with the SameSite cookie policy set to "None" or "Lax"
    
3. And lastly, the HTTP request shouldn't carry any unpredictable values
    

Let's elaborate more on these 3 conditions.

#### 1) The targeted functionality or feature must be privileged

The endpoint or app route must perform a privileged action. In general, it should bring change to data on the victim's behalf. This can be adding, updating or deleting data for example.  
  
You will often notice that no measures have been taken to protect public contact forms. However, exploiting CSRF in this context will usually result in no impact.

#### 2) Your session ID must be a cookie with the SameSite cookie policy set to "None" or "Lax"

The session ID cookie must have the SameSite policy set to "None" or "Lax". When it's set to anything else other than "None" or "Lax", the browser will refuse to forward the cookies during exploitation. 

That will make the CSRF attempt futile as the server **won't be receiving any authentication credentials, a crucial step in CSRF exploitation.**

This also means that endpoints that require a non-standard HTTP header or Authorization request header for authentication are not susceptible to CSRF attacks as credentials won't be automatically forwarded.

**There is, however, an exception to this rule. If your target makes use of HTTP Basic or a certificate, your browser will forward your credentials in each request.**

## 1) Exploiting basic CSRFs

Consider the following example HTTP request:

![Basic CSRF example](https://www.datocms-assets.com/85623/1723746689-image_1.png?auto=format "Basic CSRF example")

							Basic CSRF example

CSRF in the most basic form can be exploited by crafting the following proof of concept:

```html
<!DOCTYPE html>
<html>
  <body>
    <h1>CSRF Proof of Concept</h1>
    <form method="POST" action="https://app.example.com/api/profile/update">
      <input type="hidden" name="new_email" value="user@example.com">
      <input type="submit" value="Submit Request">
    </form>
  </body>
</html>
```

The proof of concept can be hosted and the link to that resource is then sent to the victim. When opened, the following steps describe a CSRF attack:

1. The victim will open the malicious link sent by the attacker
    
2. The victim's browser will perform the HTTP request
    
3. The server will acknowledge the HTTP request and make the state-changing action on behalf of the victim
    

Sometimes it can be that easy, but usually it's not. Let's take a look at some other more advanced cases.

## 2) Content-Type based CSRF

In some contexts, developers develop web apps to only send data over to the API in JSON. Some content types, like JSON, <mark style="background: #FF5582A6;">trigger another browser security measure (CORS) and will make our CSRF attempt futile.</mark>

**To bypass this case, we can check if the API server is capable of handling other content types such as form data**. 

![CSRF Example 2](https://www.datocms-assets.com/85623/1723747144-image_2.png?auto=format "CSRF Example 2")

						CSRF Example 2

You may also come across an API that only accepts data as JSON. Even in that case, it is possible to send data in JSON without triggering CORS by enforcing the **"text/plain" content type!**

![CSRF Example 3](https://www.datocms-assets.com/85623/1723747134-image_3.png?auto=format "CSRF Example 3")

						CSRF Example 3

```html
<!DOCTYPE html>
<html>
  <body>
    <form action="https://app.example.com/api/profile/update" method="POST" enctype="text/plain">
      <input type="hidden" name='{"test":"x' value='y","new_email":"attacker@example.com"}'/>
      <input type="submit" value="Submit request"/>
    </form>
    <script>history.pushState('','','/');document.forms[0].submit();</script>
  </body>
</html>
```

- The attacker **hosts a malicious page** containing the hidden form.
    
- The victim **visits the page**, and the form **auto-submits** using JavaScript:

```html
<script>document.forms[0].submit();</script>
```

-  Since the session cookie is **automatically sent**, the request is **authenticated**, allowing the attacker to alter the victim’s account.
    
### **How to Prevent This**

- Set **SameSite=Strict** to prevent cookies from being sent with cross-site requests.
    
- Use **CSRF tokens** to verify that the request originates from a legitimate source.

_what are referer header checks?_

### **How Referer Headers Work**

The **Referer** header in an HTTP request contains the URL of the previous page. A server can check this header to confirm the request originated from an expected source.

### **Example Scenario**

Imagine you have a website `https://securebank.com` that processes fund transfers.

#### **1. Secure Request (Valid Referer)**

```http
POST /transfer HTTP/1.1
Host: securebank.com
Referer: https://securebank.com/account
Cookie: sessionID=abcd1234
Content-Type: application/x-www-form-urlencoded

amount=500&to=recipient123
```

✅ Since the **Referer** matches `securebank.com`, the server allows the request.

#### **2. CSRF Attack Attempt (Invalid Referer)**

An attacker hosts a malicious page at `https://evil-site.com` that tries to submit the form **on behalf of a logged-in user**:

```http
POST /transfer HTTP/1.1
Host: securebank.com
Referer: https://evil-site.com
Cookie: sessionID=abcd1234
Content-Type: application/x-www-form-urlencoded

amount=500&to=attacker123

```

❌ The server detects an **unexpected Referer** (`evil-site.com`) and **rejects the request**, preventing the attack.

### **How to Implement Referer Checks**

In **PHP**, you can validate the Referer like this:

```php

if (strpos($_SERVER['HTTP_REFERER'], "securebank.com") === false) {
    die("Unauthorized request.");
}

```

In **JavaScript (Express.js)**:

```js
app.post('/transfer', (req, res) => {
    const referer = req.get('Referer');
    if (!referer || !referer.includes("securebank.com")) {
        return res.status(403).send("Invalid request source.");
    }
    // Process transfer
});

```

`if (!referer.includes("securebank.com")) {` → If the request **did NOT** come from securebank.com, block or reject it.

### **Limitations**

- The **Referer header can be manipulated**, so it's not foolproof.
    
- **Some browsers strip Referer headers for privacy** (e.g., SameSite=None settings).
    
- **Use CSRF tokens alongside Referer validation** for stronger security.

## 3) Method-based CSRF

Another CSRF case is method-based CSRF. **This case can be exploited by simply changing the HTTP method. HTTP methods like PUT or PATCH trigger CORS as well.**

To bypass this, we can try to change the HTTP method (and content type if necessary) to POST or PUT.

![CSRF Example 4](https://www.datocms-assets.com/85623/1723747264-image_4.png?auto=format "CSRF Example 4")

								CSRF Example 4

## 4) Referrer-based CSRF

The Referer request header is often processed by analytics tools but is sometimes also used to prevent CSRF. The approach developers commonly take when using the referer header as an anti-CSRF mitigation is by validating it (or part of it). If the validated value is matched against a whitelist, the request is accepted.

![CSRF Example 5](https://www.datocms-assets.com/85623/1723747441-image_5.png?auto=format "CSRF Example 5")

							CSRF Example 5

We can bypass this whitelist if no strict patterns have been defined:

![CSRF Example 6](https://www.datocms-assets.com/85623/1723747544-image_6.png?auto=format "CSRF Example 6")

							CSRF Example 6

Another case you should test for is when no referer header is sent with the request and when there is no fallback set. 

**Developers think that it is not possible to send a request through a browser without a referer header.**

We can however craft a proof of concept that'd do just that, send a request without a referer header and bypass this anti-CSRF case:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <!-- Prevent referer header from being sent -->
    <meta name="referrer" content="no-referrer">
  </head>
  <body>
    <form action="https://app.example.com/api/profile/update" method="POST">
      <input type="hidden" name="new_email" value="attacker@example.com"/>
      <input type="submit" value="Submit request"/>
    </form>
    <script>history.pushState('','','/');document.forms[0].submit();</script>
  </body>
</html>
```

## Automated tooling

Testing all API endpoints and app routes for CSRF attacks can be a tedious task, especially in a complex web application. Automated tooling can help ease out this task, and often **with quite high accuracy as CSRF attacks are simple.**

Below are a few open-source tools listed that can help with automating CSRF attacks.

#### Bolt

Bolt is an automated CSRF exploitation tool developed in Python3. Bolt is equipped with a crawling engine and is capable of crawling your target.

[https://github.com/s0md3v/Bolt](https://github.com/s0md3v/Bolt)

#### XSRFProbe

XSRFProbe is an advanced CSRF exploitation toolkit. It is equipped with a crawling engine and is capable of performing extensive CSRF tests.

[https://github.com/0xInfection/XSRFProbe](https://github.com/0xInfection/XSRFProbe)

#### CSRF PoC Creator

CSRF PoC Creator is a Burpsuite extension that can help you generate proof of concepts from raw HTTP requests in Burpsuite. It is available for both the Community as well as the Professional edition of Burpsuite.

[https://github.com/rammarj/csrf-poc-creator](https://github.com/rammarj/csrf-poc-creator)

---

Report:  https://hackerone.com/reports/834366

---

https://anonysm.medium.com/bypassing-methods-that-i-used-to-find-csrf-vulnerabilities-b7dbf88cdb0a 


https://medium.com/@iPsalmy/exploiting-csrf-and-otp-reuse-how-weak-token-management-enables-password-reset-attacks-leading-to-c2f6b914f398

----
