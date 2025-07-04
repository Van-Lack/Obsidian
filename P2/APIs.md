**Tip:** Add Custom Header:

https://www.youtube.com/shorts/9yluWEG3-ik?feature=share

Custom headers in HTTP requests and responses are used to pass additional information between the client and the server that is not covered by standard headers. Here are some common uses for custom headers:

1. **Authentication**: Custom headers can be used to pass authentication tokens or API keys.
    
    - Example: `X-API-Key: your-api-key`
        
2. **Tracking and Analytics**: Custom headers can be used to track user activity or gather analytics data.
    
    - Example: `X-Tracking-ID: unique-tracking-id`
        
3. **Debugging and Troubleshooting**: Custom headers can provide additional information for debugging purposes.
    
    - Example: `X-Debug-Info: debug-data`
        
4. **Custom Logic**: Custom headers can be used to implement specific logic on the server side, such as rate limiting or feature toggles.
    
    - Example: `X-Feature-Flag: new-feature`
        
5. **Localization**: Custom headers can pass geographic or language information to serve localized content.
    
    - Example: `X-User-Language: en-US`
        
6. **Caching**: Custom headers can provide information about caching mechanisms.
    
    - Example: `X-Cache-Status: HIT`
        

Custom headers are flexible and can be tailored to the specific needs of an application.

Adding custom headers in Burp Suite can indeed increase your chances of discovering vulnerabilities in API requests. Custom headers can help you test for various security issues, such as:

1. **Authentication Bypass**: By adding or modifying authentication headers, you can test if the API properly validates and enforces authentication.
    
    - Example: `X-API-Key: your-api-key`
        
2. **Access Control**: Custom headers can help you test if the API enforces proper access control mechanisms.
    
    - Example: `X-User-Role: admin`
        
3. **Input Validation**: By adding headers that contain special characters or payloads, you can test if the API properly sanitizes and validates input.
    
    - Example: `X-Custom-Header: <script>alert('XSS')</script>`
        
4. **Rate Limiting**: Custom headers can be used to test if the API enforces rate limiting and throttling.
    
    - Example: `X-Rate-Limit-Test: 1`
        
5. **Debugging and Information Disclosure**: Some APIs might reveal additional information when certain headers are present.
    
    - Example: `X-Debug-Info: true`
        

Using Burp Suite to add and manipulate custom headers allows you to thoroughly test the API for potential vulnerabilities and misconfigurations.

---
## API Fuzzing:


**Tip:** _When you want to find different api version other than like v1.1 v1.2 v2.1 etc.._

![[Pasted image 20241211001756.png]]

The key to trying to find different versions of the API is to dump all of them via internet archive, waybackurl and looking at that specific endpoint of the domain and see if other versions had been captured or cached.

[API Fuzzing Lists](https://www.sqrsec.com/view/downloads/api-fuzzing-lists)

search for Endpoints: 
```

ffuf -w /usr/share/wordlists/endpoints.txt  -u "https://api.chicksgold.com/Product?filter=FUZZ&path=sell" > apiendpointschicksgold.txt

```

[Find Key in API and Earn upto $10,000 | by Rishav anand | Nov, 2024 | Medium](https://medium.com/@anandrishav2228/find-key-in-api-and-earn-upto-10-000-be346e834a6c)

![[Pasted image 20241201131354.png]]

## BruteForce API Subdomains

![[Pasted image 20241201131548.png]]



----

After finding an interessing endpoint:

curl -X GET "https://api.chicksgold.com/Product?filter=inStockCurrency&gameId=62&path=sell" -H "Host: api.chicksgold.com"


- Open Postman.
    
- Create a new GET request.
    
- Enter the URL: `https://api.chicksgold.com/Product?filter=inStockCurrency&gameId=62&path=sell`.
    
- Add a header with the key `Host` and value `api.chicksgold.com`.
    
- Send the request and analyze the response.
```
import requests

url = "https://api.chicksgold.com/Product?filter=inStockCurrency&gameId=62&path=sell"
headers = {
    "Host": "api.chicksgold.com"
}

response = requests.get(url, headers=headers)
print(response.status_code)
print(response.text)

```

These methods will help you send the GET request to the specified endpoint and analyze the response.

---

# Improper Access Control in APIs Earns $3,900 Bounty

### The Bug: Exploiting Order Modification

The platform lets users modify their orders before the shopping process begins. However, the system didn't properly validate whether a user owned the order they were modifying. This oversight allowed attackers to exploit the feature.

Here's how the bug was discovered:

### Steps to Reproduce

1. **Place Two Orders**:

- Using two accounts (attacker and victim), I placed two future-dated orders in the same shop.
- Attacker's order ID: `1813918441`
- Victim's order ID: `181396149`

2. **Capture the HTTP Request**:

- While adding an item to the attacker's order, I intercepted the following HTTP POST request:

```
POST /aviator/v2/orders/1813918441/add.json  
Content-Type: application/json  
{"products":[{"id":4799771,"qty":1,"note":""}]}
```

**Modify the Order ID**:

Researcher replaced the attacker's order ID (`1813918441`) with the victim's order ID   (`181396149`).

```
POST /aviator/v2/orders/1813918441/add.json?anonymous_id███deac090c-2b05-4402-b33f-468060058145█████white_label_key████████shipt████████segway_version██████6668a3d631495cebf307423e23a588c5f9d929c1████zip█████████████user_id█████████████████████████metro_id█████████124███████store_id████████60██████bucket_number██████72███store_location_id██████████platform████████web HTTP/2
Host: ███████
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/███████ Firefox/110.0
Accept: application/json, text/plain, */*
Accept-Language: fr,fr-FR;q████████0.8,en-US;q█████0.5,en;q████████0.3
Accept-Encoding: gzip, deflate
Content-Type: application/json
Content-Length: 154
Referer: ██████
Origin: █████████
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-site
X-Pwnfox-Color: blue
Authorization: ██████████
Te: trailers

{"zip":"████","user_id":█████,"metro_id":124,"store_id":60,"bucket_number":72,"store_location_id":██████,"products":[{"id":4799771,"qty":1,"note":""}]}

```

**Execute the Request**:

- The server added the item to the victim's order and exposed sensitive details, including:
- Cart contents
- Victim's email
- Victim's physical address

**Additional Findings**:

- By testing other order IDs, I confirmed the ability to determine order statuses (e.g., "shopping in progress" or "order not retrievable").

### Why This Is Serious

This vulnerability poses significant risks:

1. **Customer Impact**:

- **Data Exposure**: Attackers can access victims' physical addresses, raising privacy and safety concerns.
- **Financial Loss**: Victims may be billed for items they didn't order.

**2. Business Impact**:

- **Operational Disruptions**: Altered orders can disrupt delivery logistics.
- **Financial Costs**: Companies may face refunds, compensation claims, and reputational damage.
- **Customer Trust**: Affected users may lose faith in the platform's security.

- **Vulnerability Type**: Unauthorized Order Modification and Data Exposure {Improper Access Control — Generic}
- **Severity**: Critical
- **Affected Functionality**: Order Modification API

![None](https://miro.medium.com/v2/resize:fit:700/1*5-KQqY1XI9Q2f0G1uyLcJw.png)


---

## Google Maps API Keys:

>  **Google Map Api Checker.**

![None](https://miro.medium.com/v2/resize:fit:700/1*q8kglujkynwwpRvvpMgCGw.png)

**This extension will helpful to check your google map api key is vulnerable or not.**

**Click on that extension and paste your gmap api key . click on check api key.**

![None](https://miro.medium.com/v2/resize:fit:700/1*2xw3gXJr384Dy0cSOnkPBg.png)

How to test for Google maps API keys: Use the following URL to test the key:

https://maps.googleapis.com/maps/api/staticmap?center=28.61410704445088,%2077.20883462877246&zoom=12&size=2500x2000&maptype=roadmap&key=YOUR_API_KEY

Replace YOUR_API_KEY with the key you're testing.

Analyze the Response:

If you see a map: The API key is accessible and potentially exposed.

Impact of Unauthorized API Key Usage
Unauthorized use can rack up significant costs because Google Maps APIs are billed per request. If the key is misused, the organization could face an unexpected financial burden. As bug hunters, finding this vulnerability can help prevent these potential issues and showcase your value.


Set API Key Restrictions: Use the Google Cloud Console to restrict which websites or apps can use the API key. Finding and reporting an exposed Google Maps API key is a valuable find that can prevent financial repercussions for organizations.


https://r0b0ts.medium.com/how-i-proved-impact-with-google-map-api-key-7aa801616abb

----
## Examples:

#### My debut with a Critical Bug: How I found my first bug (API misconfiguration)

_it’s an **API misconfiguration** bug where I found an API key with potentially dangerous permissions in the request._

> The command I used was  
> echo “example.com” | gau | grep .js | httpx -mc 200

This led me to discover a ton of JS files. After spending some time, I came across one suspicious file. I copied the JS file data in my VS Code to make it easily readable. I dug into it and found some intriguing data.

![](https://miro.medium.com/v2/resize:fit:875/1*jf0ClCCG1WGf5MFEiYUTKA.png)

ALGOLIA and Contentful api-keys

![](https://miro.medium.com/v2/resize:fit:875/1*hgkCW5ZMxpJL_MGG46Idzw.png)

				More sensitive data

 my efforts yielded little success as the API keys and secrets I had discovered were either not exploitable, like the ALGOLIA key for the blog site, which had permissions of recommendation and search only,

![](https://miro.medium.com/v2/resize:fit:875/1*9m0wLZekcxOgPc4iwdvdig.png)

						ALGOLIA api-key permissions

I was on the brink of abandoning this target for the second time when I decided to glance through my target list in Burp Suite, searching for something interesting — a tip I’d read about in another blog. And that’s when it happened.

> A host caught my eye:  
> https://(application_id).algolianet.com

As I inspected the requests made to this host, I noticed something intriguing. Some requests used the same API key I had found in the JS file, while others contained a different API key.

This difference caught my attention, and I went back to this blog that I had read earlier:

> [https://www.secjuice.com/api-misconfiguration-data-breach/](https://www.secjuice.com/api-misconfiguration-data-breach/)

Using this knowledge, I explored the permissions of this new API key.And that’s when I had my Eureka moment.

![](https://miro.medium.com/v2/resize:fit:875/1*dWPNkuXMI2F48E2Mi8qLuw.png)

						new api-key permissions

It had permissions to add and delete objects. I didn’t try to add or delete something, fearing I might break the site. I knew that these were some dangerous permissions. So I prepared a report and submitted it. Within a day, it was triaged as high severity, but after a few days, it was updated to critical.

A list of permissions and other endpoints can be found on [https://www.algolia.com/doc/api-reference/api-methods/get-api-key/](https://www.secjuice.com/p/a8be3762-de7f-409a-8e30-c9f542ba2a79/A%20list%20of%20permissions%20and%20other%20endpoints%20can%20be%20found%20on%20https://www.algolia.com/doc/api-reference/api-methods/get-api-key/)

The most damaging permissions are

- addObject: Allows adding/updating an object in the index. (Copying/moving indices are also allowed with this permission.)
    
- deleteObject: Allows deleting an existing object.
    
- deleteIndex: Allows deleting an index (will break search completely)
    
- editSettings: Allows changing index settings. - this also allows you to run javascript when the search is used.
    
- listIndexes: Allows listing all accessible indices.
    
- logs: this will allow you to view the search logs, which can include IP Addresses and sensitive cookies.
    

## Impact

After identifying other sites which were using Algolia I discovered many of them shared the same misconfiguration and the most dangerous permission is the "editSettings" permission which allows you to set the JavaScript which is run when the search feature on the site is used.

For a large site with hundreds of thousands of daily visitors, this would've allowed an attacker to inject their own malicious code for stealing credentials, card information etc. I've also found companies who are using the search only API key in their site but are using the admin API key in their mobile apps.

XSS can be achieved with this request - if the key has the "editSettings" permission.

```bash
curl --request PUT \
  --url https://<application-id>-1.algolianet.com/1/indexes/<example-index>/settings \
  --header 'content-type: application/json' \
  --header 'x-algolia-api-key: <example-key>' \
  --header 'x-algolia-application-id: <example-application-id>' \
  --data '{"highlightPreTag": "<script>alert(1);</script>"}'
```

				https://github.com/streaak/keyhacks#Algolia-API-key

  
I was curious as to what was causing so many developers to make this mistake as by default Algolia generates a search only API key for you to put in your sites. Then i saw this while i was browsing the documentation

![[Pasted image 20241210230742.png]]

That's right, it’ll handily include your admin API key for you, in example code snippets, so you can copy and paste directly to your site

### How to Fix?

In the Algolia dashboard make sure your search only key is a search only key by checking it only has the “search” permission, if not revoke the key and generate a new search only key.  Algolia also has a feature to generate "Secure Keys" which may be worth exploring.

![[Pasted image 20241210230844.png]]

			FOFA dork: title="Algolia" || body="Algolia"


----


https://medium.com/@alishoaib5929/bug-bounty-series-found-an-api-key-by-just-running-simple-tool-c308a3a89ad8?source=read_next_recirc-----29a0aa86f816----0---------------------54d6e4dd_985b_44c8_8177_993e33d80df2-------


https://osintteam.blog/decoding-api-vulnerabilities-my-bug-bounty-saga-on-api-vulnerabilities-affaf6a7c575

https://medium.com/@sharp488/payment-bypass-via-api-request-to-activate-premium-plan-on-private-bug-bounty-program-bbd7fc91ef99


https://medium.com/@anandrishav2228/find-key-in-api-and-earn-upto-10-000-be346e834a6c