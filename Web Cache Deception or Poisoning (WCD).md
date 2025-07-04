

**Caching:** is the process of storing data in the file system

- A cache decides whether to store a page based on certain factors like the URL or file extension (e.g., `.css`, `.jpg`, `.html`), and headers like `Cache-Control`.

Example Url: https://[target-redacted]/au/accountaccount%0Atest23.css


- If the server treats `/profile.php/foo.jpg` the same as `/profile.php`, it might return **user-specific content** (like a private profile page).
    
- The cache stores this response, thinking it's a public image.
    
- The attacker then accesses the same URL and gets the cached sensitive content.

**Cache keys:** Caches identify request from requests components known as cache key.

![[Pasted image 20241125181917.png]]

First you identify if the server is storing the cache, when u send a request to the server and u captured the response with a cache header like :

if you see "X - Cache status "

![[Pasted image 20241125182400.png]]


You know that caching is happening here, once you have that in mind, you can test for **web cache poisoning**

the web cache poisoning is a vuln where the attack manipulates the web cache by injecting malicious payloads and harmfull HTTP response is served to other users.

For example l'ets say that there is a website that uses query parameters to generate  content:

URL: ![[Pasted image 20241125182814.png]]

![[Pasted image 20241125182852.png]]

So an attacker could send a request with an additional malicious parameter that the server ignores but the **caching mechanism doesn't**

![[Pasted image 20241125183007.png]]

this additional parameter as an excessive payload, the server response normally with the product page for iD 123, but the cache stores the response, including the malicious payload:

![[Pasted image 20241125183738.png]]

Now any malicious user who visits the URL gets the cached page, which **now includes the malicious javascript payload that leads to XSS attack**


But why is happening? why the server ignores the response, but still servers it?

For that you need to understand what is **Cache Buster**

A cache buster is a technique where u put a small a small addition to a URL:
![[Pasted image 20241125184208.png]]

![[Pasted image 20241125184323.png]]

Usually a query string or a version number and u send it to the CDN (Content Delivery Network)

You send it multiple times to the server and the server stores the new response instead of the old one,

So you use a **Cache Buster to load the most recent version of a resource** and once you confirm that the new cache is being stored, you can inject your malicious payload, like XSS payload.

## Unkeyed Input and Keyed input

keyed input refer to the elements like headers or query parameters in the request that the caching mechanism uses as part of the cache key to **determinate if a request is unique or not**

For example, in the following Urls if the cache is configured to use the Id parameter as **keyed input** like in the image below:

![[Pasted image 20241125185023.png]]

notice that the id is different, in this case the cache would store separates responses from id, because the cache parameter is part of the cache key, so it depends on the server configuration, **what parameters are stored into caches** and what parameters or headers are used to identify a particular request.

### Unkeyed Input: 
	refers to elements of the request, like query parameters or headers,
	
that the cache ignores when deciding whether to serve **a cache response**

For example, if the user agent header is an unkeyed input in the following request,  in the first request the id is "123"

![[Pasted image 20241125190502.png]]

And the second request is also from id 123: 
![[Pasted image 20241125190606.png]]

but these both request are generated from different user agent, the first **User Agent** is chrome and the second one is firefox, which means these are different users, the first one is using chrome, the second one is using firefox,

Now they both have different values, but it is not part of the cache key so the server is going to ignore that **but still going to serve the same cached response**

! As an hacker it is very important to find the **unkeyed Input** because the server ==is going to serve the same response to all users==
even the value of the unkeyed Input is different which is going to be beneficial for us, because we want our **XSS payload to be delivered to all the users** so we have to *find something that the server is not using as cache key to determinate a request*

So that the same response will be served to all users and all those users will have an XSS payload in their response.

## Methodology POC:

Click on "view details" and just capture the request on your burpsuite and in the response see if there are any headers like "x-cache" "cache" "cache-control" etc...

![[Pasted image 20241125193651.png]]

![[Pasted image 20241125193949.png]]


The next thing you have to do is to **see if there is a cache Buster** and u can do that by adding **a small string with any random value**

add something like "&x=acc.js"

![[Pasted image 20241125194241.png]]



And if in the response cache says "X-Cache: miss" then means that this **Cache Buster is working**

![[Pasted image 20241125194638.png]]

Send it again and it will say **that is cached now:**

![[Pasted image 20241125194609.png]]


Now we have to find an unkeyed input so we can deliever this same response to every users, which means a malicious one.


You can do that by using Param Miner to get headers:

![[Pasted image 20241125194837.png]]

So let's say you find a header "X-Forwarded Host" to construct  URLs redirects or cache keys without proper validation 

an attacker can manipulate the header to control the content  that gets cached, this means the attacker  can send a crafted request with malicious **X forwarded host value** (in this case)

![[Pasted image 20241125195721.png]]

Which may change the response that the cache stores and serves to users.

Add "x forwaded host" in the response:
![[Pasted image 20241125195931.png]]


![[Pasted image 20241125195900.png]]

L'ets say a server relies on "x forwaded host" to construct URLs, redirects or cache keys, without proper validation! 

**An attacker could manipulate  to control the content that gets Cached**

![[Pasted image 20241125200554.png]]

hackerone report:

![[Pasted image 20241128001028.png]]

You can see that there is a random naame and value in the response, Cache-status HIT means it is been cached.

This attacker found an **unkeyed Input:** 


![[Pasted image 20241128001304.png]]

So in this case the unkeyed input is the **X-forwarded-port and X-forwarded-host**

So these two headers are not used as cache key identifiers by the server.

and these two headers has these evil.acronis.com which is the attacker controller server and is send to the webApp application

### Response

This link header has the actual URL of the attacker controlled server, means that the value is being reflected here, and is also being reflected in  **multiple href values** 

![[Pasted image 20241128002154.png]]

it means that the malicious value injected by the attacker is being included in the HTTP response headers and in the HTML content of the page. This can have several implications:

1. **Link Header**: The `Link` header is used to provide metadata about the relationship between the current document and other resources. If a malicious value is reflected in the `Link` header, it can manipulate how browsers and other clients interpret the relationships between resources. This can lead to incorrect caching behavior or even redirect users to malicious sites.
    
2. **Multiple href Values**: The `href` attribute in HTML is used to define the URL of a link. If a malicious value is reflected in multiple `href` attributes, it means that the attacker has successfully injected their payload into several links on the page. This can cause users to unknowingly click on links that lead to malicious sites or trigger unintended actions.

. When other users access the cached version of the page, they will receive the poisoned response, which can lead to the execution of malicious scripts or redirection to harmful websites.

## How it looks like in the UI:

![[Pasted image 20241128002355.png]]

---

## Cache Deception Full ATO:

source: https://nokline.github.io/bugbounty/2024/02/04/ChatGPT-ATO.html

First we're going to log-in as an attacker

![[Pasted image 20250114211507.png]]

After that we can see we're redirected to the profile (sensitive endpoint):

![[Pasted image 20250114211634.png]]

Now by just intercepting the request of the profile page, we then send it to the repeater to analyze the response:

![[Pasted image 20250114211622.png]]


![[Pasted image 20250114211743.png]]

					response

We take a closer look at the response and see that there is no caching and no storing:

![[Pasted image 20250114211855.png]]

**This means that this endpoint should not be cached by any means because THIS endpoint is storing sensitive data.**

We exploit this by adding a .css file:

![[Pasted image 20250114212513.png]]

If the application is actually vulnerable it will think **to fetch a css file** 

So its automatically going to save this page into the **cache server**

click "send" one time:

We still have the "cache-control revalidate":

![[Pasted image 20250114212703.png]]

But if we hit "send" again:

![[Pasted image 20250114212802.png]]

			the cache-control header has been removed

Which clearly means that the server has **cached our request**

Now validate the vulnerability by copying the url and look if the sensitive endpoint is present:

![[Pasted image 20250114213014.png]]

![[Pasted image 20250114213029.png]]

_now how can we exploit this?_

Supposively our victim has already logged in, into the server, **The server can simply create a link (which is what we did) and send it to the victim**

// victim side:


![[Pasted image 20250114213302.png]]

Once the victim opened the link.... the attacker can simply open the link in a new tab:

![[Pasted image 20250114213733.png]]


![[Pasted image 20250114213811.png]]

BOOOM!! Their info got cached  into the cache server  and we're successfully able to fetch it.

## SINTESI:

In a web cache deception attack, the attacker tricks the cache into storing sensitive information by making it appear as if the request is for a static resource. Here's how it generally works:

1. **Crafting the URL**: The attacker modifies the URL to include a fake static file extension, like `GET /profile?random_parm=test.css`. This makes the cache server think it's a request for a CSS file.
    
2. **Victim Interaction**: The attacker convinces the victim to visit this malicious URL. When the victim does, the server responds with sensitive information (like email, username, etc.).
    
3. **Caching the Response**: The cache server, misled by the fake file extension, stores the response as if it were a static resource.
    
4. **Accessing Cached Data**: The attacker can now access the cached response by simply visiting the same URL. Since the cache server serves the stored response, the attacker can see the victim's sensitive information.
    

The attacker doesn't need to set up their own server. They just need to send the malicious URL to the victim, and the cache server handles the rest by storing and serving the sensitive data.

**So whatever I injected in the header `X-Forwarded-Host:` was being reflected in the site response.**

---

## Lab: Web cache poisoning with an unkeyed header

By the following highlighted Headers, is telling us that the endpoint/page is getting **cached**:

_And we can use that to try **Web Cache Poisoning_**

![[Pasted image 20250115105246.png]]

![[Pasted image 20250115105607.png]]

We want to get a SL request from the homepage:

![[Pasted image 20250115105827.png]]

Then switch to the repeater **so we can try to determinate wether this endpoint has a Cache Oracle**, so by sending the request... (in this case) we can see that the cache is missing:

![[Pasted image 20250115110346.png]]
_In this case the cache-control header informs us that the maximum age for a **cache response is 30 seconds_**

_Now the current age is 0 because we missed the cache_, but if we re-send the request we can see that the cache return to 22 seconds or so.. and you should be able to see "X-cache: hit"

Now that we identified a **suitable cache oracle**, we want to make sure that we add a cache buster and test wether or  not we can add one.

![[Pasted image 20250115112041.png]]

		You can see that in the second request we added a cache buster

After changing the response we want to see "X-cache: miss" 

![[Pasted image 20250115112443.png]]

		"index.html" will be served to any requests that comes from the users

in the second request, since we **added a cache buster** and the cache server sees that there is a new string so the **cache key changes** so the cache server is going to store a new version of the "index.html" **which is going to serve to requests coming in that match**

![[Pasted image 20250115112951.png]]

The reason is simply because when we  start injecting a request  header **just to try and trigger a harmfull response from the cache server store**

In a real pentesting situation we want to make sure that the poison index.html is only served to us because we are using that cash buster which has a unique cash key

Now the next thing (after adding a cash buster) we want to do is look for any key unkeyed inputs so we can use **param miner for that.**

![[Pasted image 20250115114342.png]]

Go to "sexstensions" select "Param Miner" and then choose the output:

![[Pasted image 20250115122119.png]]

		You can see that it has already found the "x-forward-host"

Now before we start injecting the forward host header into our request i want to take a look about what an **unkeyed input is:**

![[Pasted image 20250115122424.png]]


## Inject X-Forwarded-Host

So the request "lab.exploit-server.net" is ==our malicious request== and if we're able to use the "X-Forwarded-Host" as a delivery mechanism to distribute that malicious payload (in this case.. is a malicious server link) to all end-users:

So now that we have found the right unkeyed parameter we can **add it to  the request with the cache buster**:

![[Pasted image 20250115123223.png]]

To verify the exploit, look if is reflected into the response:

![[Pasted image 20250115123305.png]]

Now to make it visualizable to any users by visiting the endpoing:

Finally we remove  the cash buster,  and send two request, in the first one is onna be a "cache miss" but then we're going to send the request again, just to confirm is gonna be a "cache: hit"

![[Pasted image 20250115131513.png]]

![[Pasted image 20250115130853.png]]

Then go to the specific Front-page and look for a pop-up:

![[Pasted image 20250115131636.png]]

---

## Lab: Targeted web cache poisoning using an unknown header



----

## Lab: Web cache poisoning with an unkeyed cookie

https://www.youtube.com/watch?v=QBXyfM9ALSo


---


## Lab: Web cache poisoning with multiple headers

----

## Cache Deception on a Private Program example:

![](https://miro.medium.com/v2/resize:fit:700/1*1girZVp94qjkoW2x4Qq-ug.jpeg)

1. the first step was to gather all the javascript file names
2. I entred the files names that were collected into chatgpt and asked him to create 100 new javascript files in the same format that the website follows
3. I performed fuzzing on all the javascript input points on the website, and one file ~={green}worked for me /js/test-test.js → this is an alias=~
4. start analyzing javascript file i found an endpoint leak information on user -> https://target.com/test/test
5. I tried many vulnerabilities on this endpoint, but it appears to be well-secured however after testing “web cache decepection”

it turned out to be vulnerable, as it stores user information if added “/hack.css” → [https://target.com/test/test](https://target.com/test/test)/hack.css , the victim information will be stored on **“/hacker.css”** so that if the attacker open [https://target.com/test/test](https://target.com/test/test)/hack.css he will be able to steal the victim data

----



https://www.youtube.com/watch?v=ZOBwQmfrIOM 10k + 5k web cache poisoning.


https://www.youtube.com/watch?v=hRCQILCvnr0


https://medium.com/@abhijithknamboothiri96/exploiting-cache-poisoning-via-unkeyed-parameters-and-headers-in-a-drupal-application-db7a49a67ed4?source=read_next_recirc-----287fa27c8339----3---------------------83d62da5_69fa_4381_835d_7712e76ccb66-------