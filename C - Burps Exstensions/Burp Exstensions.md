resource: https://cylent.net/blog/talaat/burp-suite-ai-smarter-scanning-powered-by-ai/
### What is Burp Suite AI?

_Burp Suite AI is a new feature set that enhances Burp's active scanner with artificial intelligence capabilities — not to replace human testers, but to **augment** them. It's designed to understand applications like a skilled penetration tester would: interpreting user flows, **understanding forms, handling JavaScript-heavy interfaces, and even making intelligent attack decisions**._

🔗 [Official Reference — Burp AI Capabilities](https://portswigger.net/burp/ai/capabilities)

**Key Capabilities:**

- Context-aware scanning with **ML-powered insight**
- Better handling of **complex authentication flows**
- Detection of **logic-based vulnerabilities** that traditional scanners often miss
- Reduces false positives by understanding business logic
- Build a full complex attack scenario to discovered vulnerabilities showing steps to reproduce for each technique. 
- AI powered extension to make even usage of Burp Suite more seamless. 

### New Features with Burp AI

### **Explorer**: 

Explore Issue autonomously investigates vulnerabilities identified by Burp Scanner, saving you time and effort.

It follows up on issues like a human pentester would — attempting exploits, identifying additional attack vectors, and summarizing findings so you can validate and demonstrate impact more efficiently.

We can find this feature in the scan results of any new scan that we have performed:

![](https://cdn-images-1.medium.com/max/800/1*zaygcVI0jIXYlanA1QQVxA.png)

Once we press on (Explore Issue) now we will go to new exploring window from Burp Suite, at this window it show exact steps taken to perform different security test cases on discovered vulnerability:

![](https://cdn-images-1.medium.com/max/800/1*N0tc8YLgwkQAfny52Kx9YQ.png)

At each step, explorer is giving clear description of what was done exactly along with Request & Response to verify as well:  

![](https://cdn-images-1.medium.com/max/800/1*MRCKMKaeXPeC-gXk7kjb0w.png)

All steps are chained as they are completing one attack scenario tries to get maximum impact of each discovered vulnerability: 

![](https://cdn-images-1.medium.com/max/800/1*yu8HjvcSEFQCUQWYChg4tw.png)

We can expand this window as well to view full information of request or response, or send it to repeater for further manual actions:

![](https://cdn-images-1.medium.com/max/800/1*tcQG7jNJgNDlAGG7KAgWmg.png)

At some steps and if explore fails to perform any of test cases it tells you exactly where it failed with informative explanation. 

### Broken access control false positive reduction

False positives in automated security testing can waste valuable time. Burp enhances Broken Access Control scan checks by **intelligently filtering out false positives before they’re reported, helping to free up your time to focus on real threats.**

We can configure this option in the new scan window: 

![](https://cdn-images-1.medium.com/max/800/1*bSRC8jMDrFhMAPm1SwUlDw.png)

### **Explainer**:

Explainer enables you to quickly understand **unfamiliar technologies without leaving Burp Suite**. 

**Highlight any part of a Repeater message and click a button to get an AI-generated explanation**. Explainer provides instant insights into headers, cookies, JavaScript functions, and more, to help you quickly identify potential security implications without disrupting your workflow.

To test this feature, we will use a request resides in the repeater tab and will try to ask it about any component of the request

![](https://cdn-images-1.medium.com/max/800/1*qG7yajJF1kvW6pI6k0S4Gw.png)

At the previous screen we asked explainer about 2 things, first what is **Accept-encoding header**. second we asked about the **XSS payload that we have used before.**

### AI-powered extensions

The Montoya API enables you to add advanced AI features into your Burp Suite extensions.

**Your extensions can now send prompts to an AI model, allowing for real-time input analysis and intelligent responses.** There’s no need for complex setup, such as managing API keys, as all AI interactions are handled within Burp Suite’s secure AI infrastructure.

### AI-powered recorded logins

Configuring authentication for web apps can be time-consuming and error-prone. **Burp can use AI to generate recorded login sequences automatically, saving time and eliminating the possibility of human error.**

We can use this feature in (Application Login) tab in new scan configuration:

![](https://cdn-images-1.medium.com/max/800/1*awW3AQ-1ep89UB2GBg7zZQ.png)

### **My Verdict**

Burp Suite AI doesn’t aim to replace a skilled human, but it’s **like having an intelligent junior pentester sitting beside you**, taking notes and pointing out things you might miss while you’re busy elsewhere.

For pentesters, bug bounty hunters, and appsec engineers — it’s definitely worth exploring. And with the **AI arms race in security** just getting started, PortSwigger is making a bold and exciting first move.


----


## Intercept Traffic with Burpsuite on Phone



<iframe width="1521" height="561" src="https://www.youtube.com/embed/0CIpMDJmPpc" title="Burpsuite proxy browser and App Interception" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

----

### GetAllParams (GAP)

_GetAllParams (GAP) is an alternative Burp Suite extension capable of actively and passively detecting new parameters. The extension can also be used to generate custom wordlists, making this an indispensable tool. GAP is available on GitHub:

[https://github.com/xnl-h4ck3r/GAP-Burp-Extension](https://github.com/xnl-h4ck3r/GAP-Burp-Extension)

### Arjun

Arjun is a popular open-source, Python-based parameter discovery tool. It provides support for multiple content types and HTTP request methods, ideal for discovering parameters in several contexts. Arjun is available on GitHub:

[https://github.com/s0md3v/Arjun](https://github.com/s0md3v/Arjun)

### x8

x8 is a blazing-fast, Rust-powered hidden parameter discovery suite. With its extensive configuration support and the ability to use custom wordlists, it makes it easy to perform any type of parameter discovery scan. x8 is available on Github:

[https://github.com/sh1yo/x8](https://github.com/sh1yo/x8)

### ParamMiner

ParamMiner is a simple-to-use plugin to actively find unreferenced parameters within Burp Suite. 

It also comes with several extensive wordlists to help you quickly **start new bruteforcing scans.** ParamMiner is available on the BApp Store and on GitHub:

[https://github.com/portswigger/param-miner](https://github.com/portswigger/param-miner)