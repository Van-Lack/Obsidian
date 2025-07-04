

[Insecure deserialization | Web Security Academy](https://portswigger.net/web-security/deserialization)

**Here’s where the creativity came into play:** The Instagram Authentication flow will automatically send the user to the previously defined `_next_` parameter—our crafted URL, which contains the malicious message reflecting back on the legitimate Instagram domain.

![](https://miro.medium.com/v2/resize:fit:875/1*2jXUcOpstR6KWV5vao__TA.png)

Since the error message was on OAuth endpoint, we crafted a new URL with a more convincing and dangerous message related to authentication error as malicious message.

By leveraging a message that seems like a typical login error, I combined the **social engineering tactic** with **technical exploitation** to increase the likelihood that users would follow the malicious instructions.

```
https://www.instagram.com/oauth/authorize/third_party/error/?message=We%20are%20unable%20to%20log%20in%20your%20account.%20Please%20contact%20support@attacker.com
```

This new message was reflected on the error page as:

![](https://miro.medium.com/v2/resize:fit:848/1*H7aQy75gG6xyn7Lw4seIhQ.png)

_“We are unable to log in your account. Please contact support@attacker.com”_

By sending the crafted link via Facebook Messenger, When an **unauthenticated Instagram user** clicks this link in Facebook Messenger, they are redirected to the Instagram login page, as **the platform requires the user to authenticate**.

![](https://miro.medium.com/v2/resize:fit:518/1*nEkYE1jYDCthkrMOYL5yDA.png)

> Once they successfully logged in, they were shown the malicious message saying _“We are unable to log in your account. Please contact support@attacker.com”._, effectively tricking them into believing there was an issue with their account.

## Enhancing the Impact: Targeting Mobile Users

One of the primary challenges with phishing attacks is convincing users that the spoofed content is legitimate.

We realized that **mobile users** were especially vulnerable in this situation, When accessing the malicious link on their mobile devices, the full URL might not be visible, and they would only see the domain **“instagram.com”**.

![](https://miro.medium.com/v2/resize:fit:500/1*MrgNTF-0QFAf0Gqm_NtT0g.png)

Since mobile users don’t see the full URL and have just logged in through a legitimate Instagram interface, they are likely to trust the message and follow the attacker’s instructions.

This helps conceal the fact that the user is being redirected to a malicious endpoint and creates a **convincing phishing scenario, tricking users into thinking their login attempt failed and that they need to contact the malicious support email.**

P.S:  The security Team of Meta reported it as an **informative**

----