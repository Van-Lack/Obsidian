
## Email Verification Bypass

_you don't really verify the email when you're supposed to:_

### 🔓 Access Control Gaps Post-Registration

**After signing up but before verifying your email, try accessing restricted endpoints like /dashboard, /admin, or /user/settings. Many systems forget to block access at this stage.**


---



When I start hunting on a target for the first time with zero prior knowledge of it, the first thing I do is create tasks. For example…

1. understanding the application (use like a normal user and **catching NOS**)…

## What Are “Nos”?

In the world of logic bugs, there’s someone named [**Arch**](https://www.youtube.com/watch?v=G1RHa7l1Ys4) who talked about the concept of “nos.” I love this idea, so I created some keywords for my work based on it.

- **Nos** means “no” — it’s when the application tells you something is not allowed.
- This is often related to **access control vulnerabilities**.

## Examples of “Nos”:

1. **In a free plan:** You can create only one project._#nos_
2. **For specific roles:** A “member” role can edit the project but cannot delete users._#nos_
3. **Paid features:** Function Z is available only in the paid plan._#nos_

This mindset helps you explore and find **access control vulnerabilities**.

And ever time when i see nos i write in my note app ( obsidan)

**EX :In a free plan:** You can create only one project. (than I make some scenarios to try it later. )

# another things I write in my nots :

## **1. Write Tips for Your Favorite Bug**

- Whenever you read a write-up or see tips on how to bypass something, **write it down**.
- If your target has a structure similar to what you read, even with different API calls, make notes about it. ( read writeups is very good resources)

## **2. Track Your Progress and Tasks**

**Understand the Requests and Logic:**

- Look at how the system works. For example:
- If you find user levels like admin = level 1, moderator = level 2, etc., think:
- “Can I, as a normal user, become admin?”
- Is there an API call for this? Or is it only the frontend showing these roles?
- Imagine small scenarios:

“If I change this request, can I bypass something?”

“Can I unlock admin features as a normal user?”

## **3. Write What You Do Daily**

- Keep a daily record of your progress.

**EX :Day 1: 2024–12–13**

- I started testing the comments section.
- Found an interesting parameter called `?reply_to`.

**You can also record a short video of what you do if you don’t want to write everything down.**

## **4. Save What You Learn**

- When you read a book, blog, or write-up, **save the important info** in one place.
- Use tools like GPT to summarize it into something easy to read later.

## **Make Blogs From Your Notes:**

If you want to share your knowledge and grow in the bug bounty community, start blogging. It will help others and improve your skills too.


---



---

Study it first to understand this bug:


<iframe width="886" height="522" src="https://www.youtube.com/embed/4WCbXGSP2Jg" title="$10,000 Bounty for IMPROPER ACCESS CONTROL-GENERIC Vulnerability | BUG BOUNTY" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>