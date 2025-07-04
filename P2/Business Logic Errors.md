repo: [TOP_BUSSINESS_LOGIC](https://github.com/reddelexc/hackerone-reports/blob/master/tops_by_bug_type/TOPBUSINESSLOGIC.md)
## Re-utilize a coupon by sending it multiple times at the same time:

_in the WebApp we can't create more than three dashboards so we may try to intercept the request when creating one and then add more than 3 of them.. like (5) requests_

![[Pasted image 20250104142321.png]]


**Tip:** in each one of the requests you may want to add a json with the name of the dashboard.

We can create a group and add all of the requests in it:

![[Pasted image 20250104142110.png]]

![[Pasted image 20250104142444.png]]

![[Pasted image 20250104142630.png]]

After sending the attack, verify if it gave a "successfull attempt":

![[Pasted image 20250104142651.png]]

![[Pasted image 20250104142718.png]]


----

## Examples:

> **Rediscovering the ‘Contact Us’ Form**

After not being able to find any bugs for like few hours, I went back to my notes to review what I had tested and what I might have missed.  
That’s when I noticed the ‘Contact Us’ form, which I had completely forgotten about.

So I decided to give it a shot and tested it. Just as I captured the request after filling in all the details, I noticed something out-of-the-box.  
There were keys named “to”, “from”, “subject”, “html”!!!!

![](https://miro.medium.com/v2/resize:fit:875/1*lvqhZf8ukgG8aeBJzA_4yQ.png)

email request (left) & actual email (right)

And yes you guessed it right, I could change their values, and a check was only applied on the “from” param.

And the most interesting thing was that in the “html” all the html tags were working, so I was able to customize the email body however I want, can add links, icons, buttons, anything at all.  
So I copy pasted the whole structure of what the real email from that company looks like and added malicious links to it.

Now, just imagine a scenario where you receive an email like this, but it’s from a hacker.

![](https://miro.medium.com/v2/resize:fit:875/1*vXwIgb4JSi83jW2ApreAMw.png)

	an actual email formed by me with phishing link embedded in download button

I created a well-structured email, taking care of every minute detail. And this was the result!

![](https://miro.medium.com/v2/resize:fit:845/1*Nc-CCqSMdXHGG0Xhwy5bLg.jpeg)

a perfectly crafted email and sent via internal email of the company

 However, I found out that we could also add attachments to the emails.

I added a few more comments to the report, showing them all the possibilities with this vulnerability. In the end, this convinced them to raise the severity to ‘Medium’

----

