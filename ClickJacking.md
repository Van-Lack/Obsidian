

tutorial for foundamentals:

 - [hacksplaining](https://www.hacksplaining.com/lessons/click-jacking/start)
 - [OWASP](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking)

**Identify missing headers like HSTS, CSP, and X-Content-Types:**

>  [Security Headers](https://securityheaders.com/) — **Website Security Analysis**

![[Pasted image 20250413171253.png]]

![[Pasted image 20250413172051.png]]

# First things first, what is Clickjacking vulnerability?

_Clickjacking is a type of security vulnerability where an attacker tricks a user into clicking something different from what they think they are clicking._

Imagine a hidden or invisible button placed on top of a regular button on a website. When you think you’re clicking to like a post or play a video, you’re actually clicking something harmful like giving permissions, sharing sensitive information, or making a purchase.

Note: Always make sure you find clickjacking vulnerability on sensitive endpoints like login page, registration page, payment page, password reset page, admin login etc…

How to test and find this bug:
Go to the target website and go to the sensitive endpoints like login, register, admin login, forgot password etc… and copy the url of each sensitive endpoints of the target website.
Now go to [Clickjacking Tool | Test | UI Redressing](https://clickjacker.io/))  and paste your urls one by one.


_IF ITS VULNERABLE YOU WILL GET A RESULT LIKE THIS ON CLICKJACKER TOOL._

![](https://miro.medium.com/v2/resize:fit:875/1*M2VvRkN06sjDjz47Hu-HJA@2x.jpeg)

_IF THE TARGET IS NOT VULNERABLE THEN THE OUTPUT WILL BE LIKE THIS_

![](https://miro.medium.com/v2/resize:fit:875/1*82YVeJOsb0jJ3x9C7ZiAvg@2x.jpeg)

# Mitigation to suggest to the company:

1. _Add x-frame options header_
2. _Add Content Security Policy (CSP) Frame Ancestors_
# PROOF OF CONCEPT (POC) TO ADD WITH REPORT:

**COPY and paste the below HTML code:**
```

<!DOCTYPE html>

<html>

<head>

<title>Clickjacking PoC</title>

</head>

<body>

<input type=button value=”Click here to Win Prize” style=”z-index:-1;left:1200px;position:relative;top:800px;”/>

<iframe src=”http://target.com/” width=100% height=100% style=”opacity: 0.5;”>

</iframe>
</body>

</html>

```
_If the target is vulnerable just copy the above code and change target.com to the real target you are hunting and save the file like poc.html and add this with your report and sent it to the triage team._


**Tip:** check the security headers and to my surprise it was missing **CSP** and **X-Frame-Options** headers which is prone to **clickjacking**.

## Protecting against clickjacking using CSP

The following directive will only allow the page to be framed by other pages from the same origin:

`frame-ancestors 'self'`

The following directive will prevent framing altogether:

`frame-ancestors 'none'`

Using content security policy to prevent clickjacking is more flexible than using the `X-Frame-Options` header because you can specify multiple domains and use wildcards. 

For example:

```bash
frame-ancestors 'self' https://normal-website.com https://*.robust-website.com
```

CSP also validates each frame in the parent frame hierarchy, whereas `X-Frame-Options` only validates the top-level frame.


----

--- next thing to do?  [Bypass CSP [EXPERT]](https://portswigger.net/web-security/cross-site-scripting/content-security-policy#bypassing-csp-with-policy-injection) 

<iframe width="928" height="522" src="https://www.youtube.com/embed/HnI0w156rtw" title="MetaMask - stealing ETH by exploiting clickjacking - $120,000 bug bounty" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


https://infosecwriteups.com/how-to-make-a-clickjacking-vulnerability-scanner-with-python-a53f48e70b58


---

