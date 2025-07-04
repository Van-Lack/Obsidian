source: [20 Open Redirect Bugs in Few Minutes | by AbhirupKonwar | Nov, 2024 | OSINT Team](https://osintteam.blog/20-open-redirect-bugs-in-few-minutes-c9fdabf75642)

## Legitimate Example🟢

https://domain.com/abc.php?redirect=https://sub.domain.com

Clicking on above url , should redirect users to sub.domain.com, but if “redirect” parameter is vulnerable to Open Redirect, we replace it with our own malicious domain and send it to victim.

## Attacker Modified🔴

https://domain.com/abc.php?redirect=http://evil.com


On evil.com , we host our undetected malware with ransomware capabilities , if anyone makes a GET request to [http://evil.com,](http://evil.com,/) initiate the steps that the malware developer has designed for.

## Custom Google Dorks only for OpenRedirect🦀

site:domain.com inurl:keyword1here inurl:keyword2here .......  
  
Make your own custom dorks using combinations of keywords below  
  
```
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


Just grab thousands of urls using waymore, check and observe carefully what are the list of keywords that present in those urls where it is potential target for open redirect vulnerability testing, based on it you can understand why I included set2,set3,set4 keywords

## Open Redirect Rust Scanner Basic Logic💡

The main logic is just the basic thing to find all parameter values that starts with “=http” and replace with “=http://evil.com” and match specific strings within the response body that implies that it is definitely evil.com only.


## Finding the URLs🤔

You can find the urls using 4 methods

1. Archived URLs using waymore

```shell
waymore -i domain.com  -mode U -oU waymore_domain.com  
cat waymore_domain.com | grep "=http" | sort -u | uro > testurls.txt
```

### Command Breakdown

1. `waymore -i domain.com -mode U -oU waymore_domain.com`:
    
    - `waymore`: This is a tool used to find URLs from various sources like the Wayback Machine, Common Crawl, Alien Vault OTX, URLScan, and VirusTotal.
        
    - `-i domain.com`: Specifies the input domain for which you want to find URLs.
        
    - `-mode U`: Sets the mode to retrieve URLs only. Other modes include `R` for downloading responses only and `B` for both URLs and responses.
        
    - `-oU waymore_domain.com`: Specifies the output file to save the retrieved URLs.
        
2. `cat waymore_domain.com | grep "=http" | sort -u | uro > testurls.txt`:
    
    - `cat waymore_domain.com`: Displays the contents of the file `waymore_domain.com`.
        
    - `grep "=http"`: Filters the lines that contain `=http`, which is likely used to find URLs with specific parameters.
        
    - `sort -u`: Sorts the filtered lines and removes duplicates.
        
    - `uro`: This is a tool used to normalize URLs, ensuring they are in a consistent format.
        
    - `> testurls.txt`: Redirects the final output to a file named `testurls.txt`.


2. Google/Bing/Yahoo/Duckduckgo dorking

3. Manually use the main web app as a normal user, click and use all the features and functionality that it provides, as well as the obscure endpoints which is visible only when you do certain things like registration, valid purchasing, when you are on a specific country (use VPN , who knows you might unlock more hidden places of the app) ,it’s difficult to do mapping of all the requests and making test cases for it. Alright , then we check the burp history after few days of crawling the web app, and then peforming burpsuite global search for commonly known OpenRedirect parameters.

Active crawling and spidering using Katana

Method1 and 2 is what I covered in this article, and method3 will be quite vague to showcase , clicking on everything which you can understand yourself guys, 

just start clicking and using the main web app so that it generates enough traffic in Burp / ZAP proxy history , then making global param searches and testing it one by one.

# RUST SCANNER🦀

![](https://miro.medium.com/v2/resize:fit:875/1*zBVpozuT-L_zWW6uM8VyyQ.png)

![](https://miro.medium.com/v2/resize:fit:875/1*kcaarKkyfl0C2Ecbwj8cAA.png)

## see code in the article

```C
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
..................=------=====------:....... :+-  
:+********+++++=---====+*****#*+=++++=++**++++=+*- --:  
..-######**********===--=++***++++***++***+**++++++***+++++++***=  
.----==+++****+++*****############**##**********#**+++++++======.......:..........:  
=#***************#****++#####*++---*###****-.....::.  
##*******#####*+=-:. +****=:. +*+###*#+  
+######**+=-:. +**+: . .####*#=  
.#*+=:. =***- :####*#+  
.*#*= :####*#*-  
.-=: .*###**#*-  
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
continue; }  
  
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

```
.  
.  
.  
.  
  
[dependencies]  
reqwest = { version = "0.11", features = ["blocking"] }  
colored = "2.0"  
url = "2.2"
```

or

```
[package]
name = "projectname1234"
version = "0.1.0"
edition = "2018"

[dependencies]
reqwest = { version = "0.11", features = ["blocking"] }
colored = "2.0"
url = "2.2"

```

## How to Run?

1. Make a new project in Rust using cargo

cargo new projectname1234

2. Check the Cargo.toml file and add the dependencies shared above.

3. Edit the main.rs and paste the rust code.

4. Now build the project and run

>cargo build  
  cargo run


> OPENBUGBOUNTY

This is publicly available for others to view, so it’s not blurred.

![](https://miro.medium.com/v2/resize:fit:875/1*X17lKbkqJrS3S2q8JpwPig.png)

![](https://miro.medium.com/v2/resize:fit:875/1*d242-PX98HTtT-LSs8WerA.png)

---

example: we go into the log-in form and ask for the reset password functionality but we notice that after that, there is a redirection like:

![[Pasted image 20241125061618.png]]


An attacker can inject an end-URL right after the upserve.com/  

![[Pasted image 20241125062217.png]]

In some cases the server might check if there is the "domain" word case in the direct or not:

![[Pasted image 20241125062415.png]]

in that case you can put a subdomain of yours e.g:
![[Pasted image 20241125062454.png]]

Sometimes the server might have a white list too, you could try to bypass that by putting "whitelist" before the domain name:

![[Pasted image 20241125062606.png]]

Another example:

## Chaining XSS:

And we can see that there is actually a "gap" in the script, which could possibly be a filter bypass to inject javascript directly.

![[Pasted image 20241125063210.png]]

![[Pasted image 20241125063140.png]]

And it seems it has encoded some special characters too.

So what if attacker sends this particular URL to a user?

Well it will redirect into a log-in page, and if the user, log-in into the webpage, the users cookies will be leaked to the attacker.

![[Pasted image 20241125063317.png]]

Cheatsheet for Open Redirect: [https://github.com/swisskyrepo/Payloa...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbHRneFRrS2VkOVlBc1NBUHNrbU95bmhMbDQ0UXxBQ3Jtc0tsRkotRGxkcXJaa2NxSkZXZmdEbW1feGJEVk40eWZOZjFSWHJhQjVkaTRvRVNtXzN1cFdQUGVkdGxRRjBjZG41WkNoNWhzTFozVDZhd2FmM3c1cHEyUUVackplaE9kM2ZoOGtnSzJ4S1N6dlNxNjliTQ&q=https%3A%2F%2Fgithub.com%2Fswisskyrepo%2FPayloadsAllTheThings%2Ftree%2Fmaster%2FOpen%2520Redirect&v=aT9NxM8KlLA) 

Reports: [https://hackerone.com/reports/469803](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbWE3djVfYjFVeG5uUloxdEJHc0hXQ092SFBvUXxBQ3Jtc0tsOVB4bmdxRzNMcjZ1Y2h2VzdpTWtlNGw1bDcxcGN1eGx5YmN0MjVBU1dpM0FqMkF5MWt1WXJUVUQyazFKekpEX2tuRjNQS0JiVkFSajhvS3p6YnZHX1l4WXFTaWVHaGlTRDI4bkhhcGFndG9qWWlGYw&q=https%3A%2F%2Fhackerone.com%2Freports%2F469803&v=aT9NxM8KlLA) [https://hackerone.com/reports/311330](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbEcyOHExUkxuUzBhblk0elo4ZHBWX0x1UXVRZ3xBQ3Jtc0ttM2NhWmxiTGFtakozTEdYOXFtM3hJNDVnRGh5ZVRpcmUzWFF6Sm85c3lWTmlQbFhHT3UtVG9ONEx1WDVjRjJKMnJZeWU4eHpQTjJrTDdGeTFpbTFOV3NZcUlVYlNkRXg4U0QwcGp1cEJodFBZZkt2VQ&q=https%3A%2F%2Fhackerone.com%2Freports%2F311330&v=aT9NxM8KlLA) 

Blog: [https://www.bugbountyhunter.com/hacke...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa290SnpETUZNUGhLS2RsckNUaXZsZVdBNnBRQXxBQ3Jtc0traXJJU2ItaGpINktsMFlNMHRDQ3VpclVXdnBwdDJPMWU1cXJoRDRVeHI1U3hNVVpvSklfdnlzZEl6UzFXVXAwTTVEZEZLczBabXRtOEJoeUZRWEZJT2ZXQWxkbXpDTnAtaUs5NWd5R21FZEtIeVFhMA&q=https%3A%2F%2Fwww.bugbountyhunter.com%2Fhackevents%2Freport%3Fid%3D432&v=aT9NxM8KlLA)

----
## Example:
url was: `example.com/login?redirect=/platform`

try some common open redirect payloads.

`google.com` `//http:google.com` `//google.com` `google.com//google.com` The list is quite long. You can check out them [here](https://pentester.land/cheatsheets/2018/11/02/open-redirect-cheatsheet.html) and [here](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Open%20Redirect). After some fuzzing one of the payload `http://;@google.com` redirected me when i logged in.

Next to see if it was was vulnerable to XSS, i inserted `javascript:alert(1)` and i got a pop-up.

---

**Tip:** When hunting for Open Redirect, after finding the vulnerable url  try to escalate to xss and maybe find a ATO:

open redirect + xss: [From Open Redirect to Reflected XSS manually | by Rodric | Medium](https://medium.com/@rodricbr/from-open-redirect-to-reflected-xss-manually-64e633a3d23f)


[Account takeover via stored xss. Hi everyone! This is Vikram Naidu, Bug… | by vikram naidu | Medium](https://medium.com/@vikramroot/account-takeover-via-stored-xss-b774f7a2a3ab)

---

#source: [Epic Games Open Redirect Bypass: How I Earned My First $500 Bounty | by Professor Software Solutions | Medium](https://bughuntar.medium.com/my-first-bug-open-redirect-at-epic-games-500-bounty-d0c03de60fa7)


- I was learning about open redirects and experimenting with different payloads.
- During my practice, I came across a URL from Epic Games, which appeared to be vulnerable to redirection manipulation.
- After several tests, I found that the redirect would only work if I used four backslashes (`////`) before the host in the `redirectUrl` parameter. The payload that successfully bypassed the filter was:


> &redirectUrl=////evil.com


- This was an unexpected behavior, but it demonstrated how an attacker could redirect a user to a potentially malicious site.

# Example URLs

**Valid Redirect Link:**

```
https://www.redacted.com/id/login?lang=en-US&noHostRedirect=true&redirectUrl=https%3A%2F%2Fstore.redacted.com%2F

```
**Exploited Open Redirect Link:**

```
https://www.redacted.com/id/login?lang=en-US&noHostRedirect=true&redirectUrl=////evil.com
```

- Website: [https://bughuntar.com](https://bughuntar.com/)

---
#source: https://youtu.be/GAT_eRQWvAM

![[Pasted image 20241203203003.png]]

We can see that without the support browser endpoint we won't get a redirect:

![[Pasted image 20241203203458.png]]

![[Pasted image 20241203203047.png]]



open redirect with that endpoint:


![[Pasted image 20241203203158.png]]

![[Pasted image 20241203203525.png]]

---

Open redirect on Shopify:

![[Pasted image 20241203203647.png]]

Find open redirect vulnerabilities with gf
By @ofjaaah

Here’s a cool one-liner to help find open redirect vulnerabilities. All you need is to provide the target domain name:

echo "tesla.com" | waybackurls | httpx -silent -timeout 2 -threads 100 | gf redirect | anew

This is what the command does in detail:
1. Collect all URLs of the target domain from the Wayback Machine
2. Attempt to download all the URLs quickly in 100 parallel threads in order to identify working URLs
3. For all working URLs, match any potentially vulnerable parameters to open redirect
4. Print out only unique, potentially vulnerable URLs

---