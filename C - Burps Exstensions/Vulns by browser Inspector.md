
[Unveiling Vulnerabilities: How Malicious Collections Hack Postman Users Through Code Generation | by Mohammed Dief | Medium](https://medium.com/@demonia/unveiling-vulnerabilities-how-malicious-collections-hack-postman-users-through-code-generation-f9854f8e27c7)

![[Pasted image 20241201010621.png]]

[5 Crazy Hacks Using Inspect Element | by Vignesh Rajendran | PositiveNaick Analytics | Medium](https://medium.com/positivenaick-analytics/5-crazy-hacks-using-inspect-element-6aabccec94c9)

----

** Cookie editor.**

![None](https://miro.medium.com/v2/resize:fit:700/1*d2dF8z-InKln5qCYVCpjJQ.png)

**You can edit and save your cookies. using this extenstion.**

![None](https://miro.medium.com/v2/resize:fit:700/1*aHjkZORIXFR3cVZZkVSkWw.png)


**7.Whatruns**

![None](https://miro.medium.com/v2/resize:fit:700/1*OvOtycf_KhBISh2JtKonxg.png)

**This will help to check what are the serives are running on that website.**

![None](https://miro.medium.com/v2/resize:fit:700/1*V5utRamjyvtQhY29sc6G6Q.png)


----

[A01: Broken Access Control and A05: Security Misconfiguration Leads to Unauthenticated Access to Paid Content | by enigma | Dec, 2024 | Medium](https://medium.com/@enigma_/a01-broken-access-control-and-a05-security-misconfiguration-leads-to-unauthenticated-access-to-0897e3bec491)

----

source: https://coffinxp.medium.com/how-to-identify-sensitive-data-in-javascript-files-jsrecon-306b8a2e6462

The first step in identifying sensitive data is to manually inspect the JavaScript files loaded by a web page. Here’s how to do this:

- Open the Target Webpage press Ctrl+U to open the source page of website.
- Press Ctrl+F and search for .js to see all the js files present on the website
![[Pasted image 20241224143757.png]]

- Look at the JavaScript files. Click on the URLs for some of the JavaScript files. You’ll notice that they contain a lot of data, some of which is potentially sensitive.
- Now you can search keyword like api, token, password, jwt, or secret, aws etc. if these present in the js file you can report it to there program by showing further impact.

Instead of doing it manually, you can use the  [Lazyegg.js Chrome Exstension](https://github.com/schooldropout1337/lazyegg) to fetch all the .js files that are avaible on the website.

![[Pasted image 20241224143926.png]]

- Visit the site that you want to fetch js file. refresh the site and click on extension. it will automatically extract all .js file URLs from the target website.
- Copy the collected URLs and paste them into a multiple URL opener extension. This will load all JavaScript endpoints simultaneously, allowing you to inspect them more quickly.
- Search for Sensitive Keywords Once the js files are open, use the search function to look for keywords like api, token, password, jwt, or secret. These keywords can help you identify potential sensitive data

You can use  [Endpointer](https://chromewebstore.google.com/detail/endpointer/ppliilneafplhagjhhphcjmjdmbjagcp) to fetch all endpoints that are found on the webpage:




![[Pasted image 20241224145747.png]]

-> just reload the page, to fetch the data, click onto the exstension -> panel and you should see all the endpoints there:

![[Pasted image 20241224151050.png]]

## JS Files Enumeration:

#js 

# Automating the Process with Command-Line Tools

For a more efficient and scalable solution, using command-line tools can significantly improve the process of detecting sensitive data in JavaScript files.

## Active Crawling with katana

Fetch JavaScript Files with Katana to gather all Active JavaScript files from the target website.

> katana -u example.com -d 5 -jc | grep '\.js$' |tee  alljs.txt

- -u samsung.com: Specifies the target URL or domain to scan
- -d 5: Sets the depth of the scan to 5, meaning the tool will follow links up to five levels deep.
- -jc: Stands for JavaScript crawling, instructing the tool to specifically focus on discovering JavaScript-related resources.
- grep ‘\.js$’: Filters the results to include only lines that end with .js, which is the file extension for JavaScript files.
- tee alljs.txt: Saves the output to a file named alljs.txt while also displaying it in the terminal.

## Passive Crawling with GAU

> echo www.samsung.com | gau | grep ‘\.js$’ | anew alljs.txt

- gau: to collect known URLs for the specified domain from various sources, such as the Wayback Machine and other public archives.
- grep’\.js$’: Filters the results to include only URLs ending with .js, which corresponds to JavaScript files.
- anew alljs.txt: Adds the filtered JavaScript URLs to the file alljs.txt, ensuring that only unique entries are saved, avoiding duplicates.

Now, you have a refined list of unique JavaScript endpoints ready for analysis.

## Refining Results with HTTPX

> cat alljs.txt | httpx-toolkit -mc 200 -o samsung.txt

- cat alljs.txt: Reads and outputs the content of the file alljs.txt, which contains a list of JavaScript URLs.
- httpx-toolkit: A tool used for HTTP requests to check the status of URLs.
- -mc 200: Filters the results to include only URLs that return an HTTP status code of 200, indicating successful responses
- -o samsung.txt: Saves the filtered list of URLs with a 200 status code into a file named samsung.txt.

# Extracting Sensitive Information with JSLeak tool

With a refined list of JavaScript endpoints, the next step is to extract all hidden links and sensitive information.

> cat samsung.txt | jsleaks -s -l -k

jsleaks: A tool used for analyzing JavaScript files to detect potential sensitive information or leaks.

- -s: to Enables secretFinder
- -l: to Enables linkFinder
- -k: to Check status code

The output will display hidden links, secret keys, and their corresponding status codes.Review these results carefully to identify potential sensitive data leaks.


# Advanced Scanning with Nuclei

For a more in -depth analysis, we can use the nuclei vulnerability scanner that uses customizable templates to detect security issues in web applications.

cat samsung.txt | nuclei -t prsnl/credentials-disclosure-all.yaml -c 30

- cat samsung.txt | nuclei -t /home/coffinxp/nuclei-templates/http/exposures -c 30
- -t /home/coffinxp/nuclei-templates/http/exposures: Specifies the path to the template directory that contains predefined templates for identifying exposure-related vulnerabilities. you can also use credentials-disclosure-all custom template for detecting the leaks crendential it contains all types of regex pattren.
- -c 30: Sets the concurrency level to 30, meaning 30 requests will be processed simultaneously to speed up the scanning process.

Analyze the results: Nuclei will provide a detailed list of detected API keys and other sensitive data.Review these results to understand potential vulnerabilities

# Comprehensive Analysis with LazyEgg tool

LazyEgg is a powerful tool for extracting various types of data from a target URL. It can extract links, images, cookies, forms, JavaScript URLs, localStorage, Host, IP, and leaked credentials.

```
cat samsung.txt | xargs -I{} bash -c ‘echo -e “\ntarget : {}\n” && python lazyegg.py “{}” — js_urls — domains — ips — leaked_creds — local_storage’
```

- — js_urls: Extracts JavaScript file URLs from the target.
- — domains: Identifies domains associated with the target.
- — ips: Retrieves IP addresses related to the target.
- — leaked_creds: Checks for leaked credentials associated with the target.

![[Pasted image 20241224153334.png]]

this command will identify hidden endpoints, URLs, domains,ip and leaked creds within .js files.



So all the commands are:

```bash
katana -u example.com -d 5 -jc | grep '\.js$' |tee -a alljs.txt

echo example.com | gau | grep '\.js$' | anew alljs.txt

cat alljs.txt | httpx-toolkit -mc 200 -o example.txt

cat alljs.txt |uro |sort -u |httpx-toolkit -mc 200 -o exam.txt

cat exam.txt |jsleak -s -l -k
```

---
## Summary:

source: https://freedium.cfd/https://medium.com/@hackersatty/how-to-find-hidden-api-endpoints-and-secrets-in-javascript-files-for-bug-bounties-web-app-f4ea92d16954

#js 

### 🧠 Intro (Set the tone)

> _You're not just looking for_ _`.js`_ _files. You're looking for_ _**intent**__,_ _**structure**__, and_ _**the logic behind the frontend**__. This guide walks you through exactly how to treat JavaScript like a roadmap to the backend. You'll discover endpoints, API calls, exposed tokens, error handling systems, and even internal dev notes._

> _Let's stop playing recon. Let's start reverse engineering web apps like pros._

![hackersatty javascript](https://miro.medium.com/v2/resize:fit:700/1*IeFBZy0HJru3wig3cEJo3Q.png)

### 🛠 Step 1: Katana + Post-Processing — Making Sense of the Mess

Katana gives you all URLs, not just JS. We want to **filter only JavaScript files**, and then **classify them**:

```bash
katana -u https://target.com -jc -o katana_raw.txt cat katana_raw.txt | grep '\.js' | sort -u > js_files.txt
```

Now we separate useful JS from noise:

```bash
# Let's pull the JS content
mkdir js_files
cat js_files.txt | while read url; do
    filename=$(echo $url | awk -F/ '{print $(NF)}' | cut -d'?' -f1)
    curl -s $url -o js_files/$filename
done
```

### 🎯 Classify JS Files

Here's what most real-world web apps have:

- `main.js`, `bundle.js`, `app.js` – Main logic
- `auth.js`, `session.js` – Login/Token flows
- `api.js`, `requests.js`, `services.js` – API calling layer
- `vendor.js`, `webpack.js` – Third-party (ignore unless fingerprinting)

You can use `grep` magic to classify:

```bash
grep -irl "fetch(" js_files/ > fetch_based.txt
grep -irl "axios" js_files/ > axios_based.txt
grep -irl "token" js_files/ > tokens.txt
grep -irl "apiKey" js_files/ > apikeys.txt
```

### 🧵 Step 2: Deconstruct the JS — Find Logic, Not Just Links

Here's what the **average recon guy misses**: they look for endpoints, but **not how they're used**. So, you should dig into how the **client-side logic structures requests**.

### 🧪 Examples to Extract:

- Dynamic URL builds:
- `const baseUrl = "https://api.target.com/"; const final = baseUrl + "user/" + id + "/data";`
- ✨ Use regex:
- `grep -Pozr "https?:\/\/[^\s\"']+" js_files/ | sort -u > endpoints.txt`
- Auth Headers / Tokens:
- ``headers: { Authorization: `Bearer ${userToken}` }``
- ✨ Grep it:
- `grep -irn 'Authorization' js_files/`  **(This command will output any line in your JS files that contains the word "Authorization".)**

! **Tip:**  "js_files" is a directory path, not a command.

Quick command explanation:

```bash
grep -Pozr "https?:\/\/[^\s\"']+" js_files/ | sort -u > endpoints.txt
```

- `grep -Pozr ... js_files/`:
    
    - `-P` tells grep to use Perl-compatible regex.
        
    - `-o` outputs only the part of the file that matches the regex (in this case, URLs).
        
    - `-z` treats the input as null-terminated, which can help with binary files or files that don’t have typical line breaks.
        
    - `-r` performs the search recursively inside the `js_files/` directory.
        
- The regex `"https?:\/\/[^\s\"']+"` finds strings that start with `http://` or `https://` until a space or the end of a quoted string.
    
- `| sort -u > endpoints.txt` then sorts the output uniquely and writes it to `endpoints.txt`.
### 🧬 Search for Developer Notes:

```bash
grep -iE "todo|fixme|bug|devNote|debug" js_files/*
```

### 🔐 Step 3: Hidden Secrets, Tokens, Dev Notes

Most JavaScript gets minified — but devs still leave traces. Here's what to extract:

```bash
grep -Eo 'AIza[0-9A-Za-z\\-_]{35}' js_files/* >> secrets.txt 
grep -Eo 'sk_live_[0-9a-zA-Z]{24}' js_files/* >> secrets.txt 
grep -Eo 'eyJ[a-zA-Z0-9-_=]+?\.[a-zA-Z0-9-_=]+\.?[a-zA-Z0-9-_.+/=]*' js_files/* >> jwt.txt
```

### Command breakdown:

- **egex Breakdown:**
    
    - `AIza`: All Google API keys start with this prefix.
        
    - `[0-9A-Za-z\\-_]{35}`: Matches the remaining 35-character alphanumeric sequence.
        
- **Potential Risk:** If found, **unauthorized users may exploit these keys** to interact with Google services, such as Maps, Firebase, or OAuth authentication.

`sk_live_`: All Stripe live keys start with this.

**Potential Risk:** Finding a **live Stripe key** could allow **unauthorized payment processing** or **customer data access**.

- - **JWTs typically start with** `"eyJ"` (base64 encoded).
        
    - **Captures the three-part JWT structure:** `Header.Payload.Signature`
        
- **Potential Risk:**
    
    - JWTs contain authentication info; if leaked, they can be **used to impersonate sessions**.
        
    - If signed **without expiration**, they might remain valid **even after password resets**.

### 📡 Step 4: Map Real Backend Flow with fff + Live Testing

Use [`fff`](https://github.com/tomnomnom/fff) by Tomnomnom to test all endpoints you've scraped.

```bash
cat endpoints.txt | fff -o responses/
```

Now:

- Search for status codes (`200`, `403`, `500`)
- Look for interesting outputs
- `grep -Ri "error" responses/ grep -Ri "token" responses/ grep -Ri "admin" responses/`

Bonus: Use [`httpx`](https://github.com/projectdiscovery/httpx) to check for open APIs:

```bash
cat endpoints.txt | httpx -mc 200 -o live.txt
```

### 🧠 Bonus Step: Reverse Source Map for True Gold

If you find a `.map` file (e.g. `main.js.map`), it's like opening the dev's IDE.

```bash
# Install source-map-explorer 
npm install -g source-map-explorer 
# Run it
source-map-explorer main.js main.js.map
```

You'll get the entire original code with class names, variable names, and real structure.

🔥 This gives you **function names**, **internal logic**, and even **routing structure** if React/Angular is used.

### 🧩 Real Life Use Case (Add spice)

> _"I was testing a fintech app. Main JS had nothing interesting. But one old_ _`utils.js`_ _had a hardcoded staging API key. That key was live. Guess what? It led me to a Swagger page with all the endpoints. Devs forgot to lock it. Free bug bounty."_

### 🧘 Wrap Up

This isn't just about hitting `katana` and moving on. If you learn to read JavaScript like the frontend was your code, you'll uncover more than recon ever will. You'll get flow, intent, and **attack surface the devs don't want you to see**.

### Connect with Creator:

- LinkedIn: [Hackersatty](https://www.linkedin.com/in/hackersatty)

> https://hackersatty.medium.com/

**Bonus: (by Copilot)**

### **Use of Dangerous Functions for Code Execution**

- **Risk:** Functions like `eval()` or the `Function` constructor allow dynamic code evaluation, which is dangerous if user input isn’t sanitized.

```js
// Vulnerable usage of eval()
eval(userInput);

// Dangerous dynamic code generation
const fn = new Function("return " + userInput);
```

```bash
grep -Rin "eval(" .
```

**Impact:** Can lead to arbitrary code execution if an attacker controls or influences the input.

<mark style="background: #FF5582A6;">--- Look for writeup about eval() function for more examples!</mark>

**Unsafe DOM Manipulation** (Document Object Model)

- **Unsafe Assignments:**
    
    - `element.innerHTML = userInput`
        
    - `document.write(userInput)`
        
- **Risk:** These can lead to Cross-Site Scripting (XSS) if proper sanitization isn’t applied.
    
- **Search Example:**
    
    ```bash
    grep -Rin "innerHTML" .
    grep -Rin "document.write" .
    ```

source: [DOM Based Lab](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-innerhtml-sink)

### **Error and Logging Functions Exposing Sensitive Data**

- **Risk:** Debug statements or error logs that directly print out user data and system internals (e.g., `console.error(err)`) may inadvertently expose PII or server configuration details.

```js
console.error("Error processing request:", error);

```

```bash
grep -Rin "console.error" .
```

**Impact:** Detailed error messages can provide attackers with insights into the internal workings and potential weaknesses of the system.

### **Public Configuration Files (**`public-config.json`**,** `config.js`**)**

- **Risk:** Developers sometimes expose files meant for client configuration that include API endpoints, keys, feature toggles, or even hardcoded secrets.
    
- **Search Command Example:**
    

    ```bash
    find . -type f \( -iname "public-config.json" -o -iname "config.js" \)
    ```


**Environment Files (**`.env`**,** `.env.development`**,** `.env.production`**)**

- **Risk:** If these files are accidentally left in a public directory, they may leak credentials, keys, and database URIs.
    
- **Search Command Example:**
    
    ```bash
    find . -iname ".env*"
    ```

1. **Chrome or Browser Extension Manifest Files (**`manifest.json`**)**
    
    - **Risk:** In some cases (especially if a site bundles its own extension-like functionality), the manifest may list permissions, endpoints, and even developer notes or non-obfuscated endpoints.
        
    - **Search Command Example:**
        
        ```bash
        find . -iname "manifest.json"
        ```
        
2. **Build Artifact Files (e.g.,** `bundle-analyzer.json`**)**
    
    - **Risk:** Some projects output analyzer files (e.g., from webpack bundle analyzers) that detail the dependency tree and file structure—information that can be leveraged to map out the internal architecture.
        
    - **Search Command Example:**
        
        ```bash
        find . -iname "*analyzer*.json"
        ```

---

## Automated Tool to scan for endpoints, secrets & APIs

![](https://miro.medium.com/v2/resize:fit:788/0*dtV7d7-pSJx8CGrQ.png)

**Scroll down a little bit and you will find installation commands**

**I’m going to install this tool with the go command**

![](https://miro.medium.com/v2/resize:fit:788/0*QkaSiyyZ0JXPPEZ1.png)

**So I copied it and pasted it into my Kali machine then I clicked on enter**

![](https://miro.medium.com/v2/resize:fit:788/0*SZYH5ndu20P3ba_z.png)

**Then I confirmed whether it successfully installed or not with the help command**

![](https://miro.medium.com/v2/resize:fit:788/0*sAk3c6DUAXXqoR99.png)

**Now, see how to run this tool**

> **Used this tool after finding subdomains and use httpx to find alive subdomains**

**Command to find endpoints and save results into a file**

```bash
cat alived_Subs | cariddi -c 50 -t 30 -e > cariddi_result_ext1

cat alived_Subs | cariddi -e > cariddi_result_ext2
```

**RESULTS⬇️**

![](https://miro.medium.com/v2/resize:fit:712/0*ESsLnYa0EHVPat3k.png)

![](https://miro.medium.com/v2/resize:fit:788/0*oiMFRUCBxpkPT8dX.png)

![](https://miro.medium.com/v2/resize:fit:788/0*WzZKvd9MhQT4RWj3.png)

![](https://miro.medium.com/v2/resize:fit:429/0*-sp_AOrChtW-_Gmq.png)

**Find extensions⬇️**

```bash
cat alived_Subs | cariddi -c 50 -t 30 -ext 3 > cariddi_result_ext1

cat alived_Subs | cariddi -ext 3 > cariddi_result_ext2
```

**RESULTS⬇️**

![](https://miro.medium.com/v2/resize:fit:465/0*5jyWvehCxtv2vb82.png)

![](https://miro.medium.com/v2/resize:fit:464/0*99szeCX5nR3-fftV.png)

![](https://miro.medium.com/v2/resize:fit:725/0*hjlTmAn5UpVor2rI.png)

**To find secrets**⬇️

```bash
cat alived_Subs| cariddi -s -c 50 -t 20 > cariddi_result_secrets1

at alived_Subs| cariddi -s > cariddi_result_secrets2
```

**RESULTS⬇️**

![](https://miro.medium.com/v2/resize:fit:788/0*k7c6zGZm1CPKUeHz.png)

**To perform all the important things in one command⬇️**

```bash
cat alived_Subs | cariddi -c 50 -t 30 -e -ext 1 -info -s > cariddi_result_all
```

> **_EXPLORE REMAINING COMMANDS BY YOURSELF_**

----

##  Exploring Sensitive Data in JavaScript Files


[guide 2](https://infosecwriteups.com/exploring-sensitive-data-in-javascript-files-606447e6a5cd)

# Why JavaScript Files?

JavaScript files are commonly embedded in web applications to enable interactivity and functionality. However, they may also contain:

- **API keys**
- **Configuration details**
- **Hardcoded credentials**
- **Sensitive endpoints**

By learning how to analyze these files, you can identify potential security issues and better secure applications.

# Manual Techniques for Exploring JavaScript Files

# 1. Endpoint Classification

**Objective**: Identify API endpoints or hidden routes within the JavaScript files.

- **API Endpoints**: Look for URLs interacting with external services.
- **Hidden Routes**: Find URLs that aren’t directly exposed in the application but are still accessible.

**Tools**: Use browser developer tools or command-line utilities like `curl` and `grep` to manually search the files.

# 2. Keyword Search

Search for common keywords related to secrets or credentials in JavaScript files. For example:

```
grep -E "(key|token|auth|password|secret)" *.js
```

	This search can reveal sensitive information buried within the code.

# 3. Analyzing Comments

**Objective**: Look for developer comments that may inadvertently expose secrets.

Developers often leave comments for debugging or documentation purposes. Searching for specific patterns can help uncover:

- Deprecated API keys
- Hardcoded credentials

	Use regular expressions (regex) or simple text search to find these comments.


# Scenario: Testing [https://example.com](https://example.com) for vulnerabilities

1. **Gather JavaScript URLs**:

> gau example.com | grep ".js" > js-urls.txt

**2. Extract endpoints**:

> python linkfinder.py -i js-urls.txt -o cli

**3. Search for sensitive data**:

>python SecretFinder.py -i js-urls.txt -o cli


----

### CVE-2024-4367 – Arbitrary JavaScript execution in PDF.js

video  from lostsec (if not deleted):


1° Find a webpage where you can upload files and uses PDF.js technology

![[Pasted image 20241224141804.png]]


![[Pasted image 20241224141642.png]]

Upload the file and then view it:

![[Pasted image 20241224142437.png]]

![[Pasted image 20241224143102.png]]

confirms the vulnerability once the files is opened:

![[Pasted image 20241224143129.png]]

---

#Exstension 

## Browser Extensions for Bug bounty:

## Make Your Life Easier While Viewing Logs:

🦄 _Tired of junk headers or having to scroll in Burp logs?

### Before Exstension:

![[Pasted image 20250613202709.png]]

### After Exstension:

![[Pasted image 20250613202811.png]]


> 🔥 Use this Extension: https://github.com/rikeshbaniya/bodyonly (download the file, then set it up on burpsuite)

## How To load your Python script into **Burp Suite’s Extender**, follow these steps: (By Copilot)

### **1️⃣ Enable Python/Jython in Burp Suite**

- Go to **Extender** > **Options**
    
- Under **Python Environment**, browse and select **Jython standalone JAR** _(if not already set up)_
    
    - You can download it from: https://www.jython.org/download
        

### **2️⃣ Add Your Script**

- Navigate to **Extender** > **Extensions**
    
- Click **Add** and choose:
    
    - **Extension Type:** _Python_
        
    - Select the **.py file** containing your Burp extension
        
- Click **Next**, and Burp will load the script!
    
### **3️⃣ Verify Installation**

- Your extension should now appear in the **Extensions** list.
    
- If there’s an error, check **Burp Suite’s logs** under **Alerts** for debugging.

---
# General Recon:
## UrlScanIO Chrome/Firefox Extension

![[Pasted image 20250613160054.png]]

A tool to quickly retrieve information about active tab URL:

- domain/IP/ASN
- domain creation date
- phishing/malware reputation

Another free method of finding information by nickname or last name. urlscan.io page.url advanced search operator:

```
page.url:"/targetusername/"
```

Works the same as inurl: in Google, but the results are very different.


> https://chromewebstore.google.com/detail/urlscanio/loehkbkhflmmkempgkdpkkhghdiegicp


## PDF Keyword Crawler

![[Pasted image 20250618134402.png]]

#Exposed_PDFs

_Hunting for sensitive info in public PDFs?

🪩 Use PDF Keyword Crawler Firefox add-on  
📄 Load your urls.txt (with .pdf links)  
🔑 It scans for sensitive keywords automatically!

🧠 Great for discovering leaked secrets, creds, or internal docs.

👉 Add-on: https://addons.mozilla.org/es-AR/firefox/addon/pdf-keyword-crawler/


---

Scrape Domain really fast, by BrutSecurity: https://addons.mozilla.org/it/firefox/addon/brutscope-extractor/


## Temp Mail

> https://chromewebstore.google.com/detail/temp-mail-email-temporane/inojafojbhdpnehkhhfjalgjjobnhomj

### 1. Hack-Tools

This is a powerful all-in-one extension for pentesters and bug hunters. It gives you payloads for XSS, SQLi, LFI, etc., and helps with encoding/decoding and many more quick tasks.

>https://chromewebstore.google.com/detail/hack-tools/cmbndhnoonmghfofefkcccljbkdpamhi?hl=en-US

### 2. ModHeader

This extension helps you modify HTTP request headers like cookies, user-agent, origin, etc., right from your browser.

> https://chromewebstore.google.com/detail/modheader-modify-http-hea/idgpnmonknjnojddfkpgkljpfnnfcklj

### 3. Arjun Tabby

This extension finds parameters on websites by sending automated GET/POST requests. Great for fuzzing unknown inputs.

> https://addons.mozilla.org/en-US/firefox/addon/tabby-window-tab-manager/

### 4. HTTP Toolkit Helper

This extension captures, inspect and debug HTTP(S) traffic directly inside your browser — cleaner than traditional proxies.

> https://httptoolkit.com/

### 5. Live HTTP Headers

This extension captures all HTTP headers as you browse. Great for debugging redirections, cookie behavior, and more.

> https://addons.mozilla.org/en-US/firefox/addon/live-http-headersss/

### 6. NoRedirect

What it does: It stops redirects, so you can see hidden pages like admin panels.

> https://addons.thunderbird.net/en-us/firefox/addon/noredirect/

### 7. URL Extractor Pro

What it does: Pulls all URLs from a webpage, including hidden ones, for deeper testing

> https://mainwp.com/add-on/url-extractor/


### 8. JS Inspector

What it does: Scans JavaScript files on a page for potential vulnerabilities or exposed endpoints.

#js 

> https://chromewebstore.google.com/detail/js-runtime-inspector/iilpjebedgohcmlffhnkhbjhabkdhfmn

### 9. Form Filler Pro

What it does: Auto-fills forms with custom data to test input validation flaws.

> https://chromewebstore.google.com/detail/form-filler/kjhkojecdbddhfikjodhaoembdoalooc

### 10. Cache Analyzer

What it does: Checks if a site's cache is misconfigured, which can leak sensitive data.

> https://www.nirsoft.net/utils/chrome_cache_view.html

### 11. User-Agent Switcher

What it does: Changes your browser's user-agent to test mobile or old browser


> https://addons.mozilla.org/en-US/firefox/addon/uaswitcher/

### 12. Request Debugger

What it does: Lets you pause and debug HTTP requests in real-time to find logic flaws or injection points.

> https://developer.chrome.com/docs/extensions/get-started/tutorial/debug

### 13. XSS

This extension is a chrome extension tool that can inject custom scripts into the current web page.

> https://chromewebstore.google.com/detail/xss/bebjbdbgpmgdlfehkibnmgmbkcniaeij

### 14. Check XSS

This extension helps in finding site vulnerabilities such as XSS, SQL injection and CRLF

> https://addons.mozilla.org/en-US/firefox/addon/check-xss/

### 15. URL Decode/Encode

This extension is a nifty tool for decoding URLs that have been encoded. Useful when you've encoded a URL and actually want read it!

> https://chromewebstore.google.com/detail/url-decodeencode/dgoepmkoiphgabefpbapldnjmbbiaoag

### **TruffleHog — Finding Hidden API Keys**

This extension helps in finding hidden API keys on websites. It scans web pages for exposed credentials, making it a valuable tool for ethical hackers and security researchers.

#### **Why It's Useful?**

- Automates API key discovery.
- Detects secrets in source code and public repositories

> https://chromewebstore.google.com/detail/trufflehog/bafhdnhjnlcdbjcdcnafhdcphhnfnhjc


## Owasp penetration testing kit

>https://chromewebstore.google.com/detail/owasp-penetration-testing/ojkchikaholjmcnefhjlbohackpeeknd?hl=eS

### **HackTools — Payload Generator**

HackTools provides a collection of useful payloads and testing utilities for ethical hacking.

#### **Why It's Useful?**

- Pre built payloads for SQLi, XSS and more.
- Saves time during manual testing.

> https://chromewebstore.google.com/detail/hack-tools/cmbndhnoonmghfofefkcccljbkdpamhi?hl=en

### **EditThisCookie — Advanced Cookie Editor**

This extension allows you to modify, delete and manage cookies which is essential for security testing.

#### **Why It's Useful?**

- Analyze session tokens and cookies.
- Check for HTTPOnly and Secure flags.

> https://chromewebstore.google.com/detail/editthiscookie-v3/ojfebgpkimhlhcblbalbfjblapadhbol?hl=en

another alternative:
https://cookie-editor.com/

### WebRTC Protect — Protect IP Leak

This tool disables WebRTC to prevent IP address leakage, which is essential for VPN users.

#### **Why It's Useful?**

- Prevents real IP exposure.
- Essential for anonymous browsing.

> https://chromewebstore.google.com/detail/webrtc-protect-protect-ip/bkmmlbllpjdpgcgdohbaghfaecnddhni

### **Link Gopher — Extract All Links**

This extension fetches all links from a webpage, which is useful for reconnaissance.

#### **Why It's Useful?**

- Extracts URLs in bulk.
- Helps in mapping out target applications.

> https://chromewebstore.google.com/detail/link-gopher/bpjdkodgnbfalgghnbeggfbfjpcfamkf

### **FindSomething — Hidden Parameter Finder**

Scans source code and JavaScript files for interesting patterns and hidden data.

#### **Why It's Useful?**

- Identifies sensitive information like API keys and credentials.
- Helps in finding security vulnerabilities in java script.

> https://chromewebstore.google.com/detail/findsomething/kfhniponecokdefffkpagipffdefeldb

or

> https://addons.mozilla.org/en-US/firefox/addon/findsomething/

### **.git Finder — Information Disclosure**

_This extension detects exposed .git directories on websites, which can lead to source code leaks.

#Github_Recon 
#### **Why It's Useful?**

- Helps in detecting misconfigured repositories.
- Easy way to find information disclosure vulnerabilities.

> https://chromewebstore.google.com/detail/dotgit/pampamgoihgcedonnphgehgondkhikel?hl=en

### **Open Multiple URLs — Bulk URL Opener**

This extension lets you open multiple links simultaneously, saving time.

#### **Why It's Useful?**

- Automates mass link opening.
- Speeds up bug hunting tasks.

> https://chromewebstore.google.com/detail/open-multiple-urls/oifijhaokejakekmnjmphonojcfkpbbh?hl=en

###  **UA Switcher — User-Agent Spoofer**

This tool Allows you to spoof user agents and test websites on different platforms.

#### **Why It's Useful?**

- Bypass bot detection.
- Test website behavior on different platforms.

> https://chromewebstore.google.com/detail/user-agent-switcher-and-m/bhchdcejhohfmigjafbampogmaanbfkg

### **EXIF Viewer Pro — Extract Image Metadata**

This extension allows you to retrieve metadata from images directly on a webpage without downloading them.

#### **Why It's Useful?**

- Extracts metadata such as camera details, location and timestamps.
- Useful for OSINT investigations and forensic analysis.

> https://chromewebstore.google.com/detail/exif-viewer-pro/mmbhfeiddhndihdjeganjggkmjapkffm

### **WaybackURL — Fetch Archived URLs**

This extension retrieves all URLs from Wayback Machine similar to the waybackurls tool.

#### **Why It's Useful?**

- Helps in identifying past versions of web pages.
- Useful for detecting previously exposed vulnerabilities.

>https://chromewebstore.google.com/detail/wayback-machine/fpnmgdkabkmnadcjpehmlllkndpkmiak

### **SponsorBlock — Skip YouTube Sponsors**

Skips sponsored segments, intros and outros on YouTube videos.

#### **Why It's Useful?**

- Saves time by removing unnecessary video sections.
- Enhances focus while learning from cybersecurity content.

>https://chromewebstore.google.com/detail/sponsorblock-for-youtube/mnjggcdmjocbbbhaepdhchncahnbgone

### **Shodan — Website Intelligence Tool**

Provides information on website hosting, server locations and open ports.

#### **Why It's Useful?**

- Identifies exposed services and security misconfigurations.
- Assists in reconnaissance and threat analysis.

> https://chromewebstore.google.com/detail/shodan/jjalcfnidlmpjhdfepjhjbhnhkbgleap

### **EndPointer — Find Sensitive URLs**

Extracts and analyzes URLs for potential security endpoints.

#### **Why It's Useful?**

- Helps locate sensitive web application endpoints.
- Useful for penetration testing and fuzzing.

> https://chromewebstore.google.com/detail/endpointer/ppliilneafplhagjhhphcjmjdmbjagcp

### **YesWeHack VDP Finder**

Detects vulnerability disclosure programs (VDP) of visited websites.

#### **Why It's Useful?**

- Helps security researchers report vulnerabilities responsibly.
- Finds public bug bounty programs easily.

> https://chromewebstore.google.com/detail/yeswehack-vdp-finder/jnknjejacdkpnaacfgolbmdohkhpphjb

### **S3BucketList — AWS Bucket Finder**

Searches and lists AWS S3 buckets found in network requests.

#### **Why It's Useful?**

- Identifies publicly accessible S3 buckets.
- Helps in detecting misconfigured cloud storage.

> https://chromewebstore.google.com/detail/s3bucketlist/anngjobjhcbancaaogmlcffohpmcniki?hl=en

### **D3coder — Encode/Decode Tool**

Provides encoding and decoding functions for various text formats.

#### **Why It's Useful?**

- Converts encoded payloads during penetration testing.
- Supports Base64, URL encoding, and other formats.

> https://chromewebstore.google.com/detail/d3coder/gncnbkghencmkfgeepfaonmegemakcol?hl=en

### **Mitaka — OSINT Search Tool**

Searches IPs, domains, URLs and hashes across multiple threat intelligence platforms.

#### **Why It's Useful?**

- Speeds up OSINT investigations.
- Checks for blacklisted domains and malware indicators.

> https://chromewebstore.google.com/detail/mitaka/bfjbejmeoibbdpfdbmbacmefcbannnbg

### **Vortimo OSINT Tool**

A Swiss-army knife for OSINT, allowing users to bookmark, scrape and analyze web pages.

#### **Why It's Useful?**

- Stores and organizes OSINT findings.
- Highlights text across multiple pages for correlation.

>https://chromewebstore.google.com/detail/vortimo-osint-tool/mnakbpdnkedaegeiaoakkjafhoidklnf?hl=en

XSSpect: A browser extension to automate XSS injection | [@Bugcrowd](https://t.me/Bugcrowd) [https://www.bugcrowd.com/blog/xsspect-a-browser-extension-to-automate-xss-injection/](https://www.bugcrowd.com/blog/xsspect-a-browser-extension-to-automate-xss-injection/)


## Bonus:

THis extension is used to translates all languages in websites.
https://addons.mozilla.org/en-US/firefox/addon/traduzir-paginas-web/

THis extension is best use for protecting your vpn ip from webrtc exposer.
https://addons.mozilla.org/en-US/firefox/addon/happy-bonobo-disable-webrtc/

