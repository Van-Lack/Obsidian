---
created: 2024-06-25T02:21
updated: 2024-10-27T00:10
---

search for CVEs: [CVE - Search CVE List (mitre.org)](https://cve.mitre.org/cve/search_cve_list.html)

![[Pasted image 20240626191334.png]]

----
## Exploit Development:
<iframe width="711" height="400" src="https://www.youtube.com/embed/rX8acbhzLRg" title="My Journey to Exploit Development (CVE-2024-23897)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


### From KBs to CVEs: Understanding the Relationships Between Windows Security Updates and Vulnerabilities

Resource: [From KBs to CVEs: Understanding the Relationships Between Windows Security Updates and Vulnerabilities | Claroty](https://claroty.com/team82/blog/from-kbs-to-cves-understanding-the-relationships-between-windows-security-updates-and-vulnerabilities)


---

CVE-2024-30088 Windows Kernel Elevation of Privilege
https://github.com/tykawaii98/CVE-2024-30088

https://freedium.cfd/https://medium.com/@hunterid/cve-2024-30078-the-log4j-level-vulnerability-in-windows-wifi-driver-632de0d00c7c

---

Polyfill supply chain attack hits 100K+ sites
https://sansec.io/research/polyfill-supply-chain-attack



https://youtu.be/MkGDtCxPZSk?si=BTXqyurAwehHgP9L


CVE-2024-0762 --- research?


https://systemweakness.com/windows-defender-smartscreen-vulnerability-cve-2024-21412-exposes-financial-traders-to-a03ff476a293



https://medium.com/@ofriouzan/dissecting-and-exploiting-cve-2021-41773-and-cve-2021-42013-7c116f489ee2



Exploiting the EvilVideo vulnerability on Telegram
Discovered a 0-day Telegram for Android exploit that allows sending malicious apps disguised as videos
https://www.welivesecurity.com/en/eset-research/cursed-tapes-exploiting-evilvideo-vulnerability-telegram-android/


 https://github.com/eset/malware-ioc/tree/master/evilvideo

![[Pasted image 20240804044025.png]]

for this vulnerability to work, python needs to be installed on the victim computer

![[Pasted image 20240804044449.png]]

![[Pasted image 20240804044501.png]]


![[Pasted image 20240804044526.png]]

![[Pasted image 20240804044730.png]]


![[Pasted image 20240804044805.png]]


![[Pasted image 20240804045309.png]]

this happens because the developer blacklist the "pywz" file type, but not the real one, which is "pyzw"


---

Heap overflow in JPEG loading in Samsung's Little Kernel in bootloader allows a privileged attacker to execute persistent arbitrary code (it survives reboots and factory reset) CVE-2024-20832  
Paper: https://www.sstic.org/media/SSTIC2024/SSTIC-actes/when_vendor1_meets_vendor2_the_story_of_a_small_bu/SSTIC2024-Article-when_vendor1_meets_vendor2_the_story_of_a_small_bug_chain-rossi-bellom_neveu.pdf
Slides: https://www.sstic.org/media/SSTIC2024/SSTIC-actes/when_vendor1_meets_vendor2_the_story_of_a_small_bu/SSTIC2024-Slides-when_vendor1_meets_vendor2_the_story_of_a_small_bug_chain-rossi-bellom_neveu.pdf




https://blog.0xzon.dev/2024-07-27-CVE-2024-41637/

![[Pasted image 20240806024536.png]]


CVE-2024-7646: Ingress-NGINX Annotation Validation Bypass

https://www.armosec.io/blog/cve-2024-7646-ingress-nginx-annotation-validation-bypass/

----

A trick to hide malicious files in images :
Create an exploit, make it executable in 1 click, then use RTLO methods to make it legitimate, and you will have a basis for an RCE image.

![[video_2024-10-07_17-47-46.mp4]]

----


Vulnerabilities of Realtek SD card reader driver, part 1

https://zwclose.github.io/2024/10/14/rtsper1.html



---

https://www.linkedin.com/posts/saurabh-b294b21aa_affectedabrversions-telegramabrchannel-reference-ugcPost-7312872249072500736-La-e?

CVE-2025-30208 (POC)

https://github.com/xuemian168/CVE-2025-30208



---

# WPDirectory Mass Hunting Plugin Vulnerabilities

### Mass find any line of code in all WordPress Plugins & Themes using Golang RE2 Regular Expression Syntax

#WP_MassHunt 

by: [WP_massHunting](https://freedium.cfd/https://medium.com/system-weakness/wpdirectory-mass-hunting-plugin-vulnerabilities-e65b6ac30ab9?source=home_for_you---------2-2--------------------55b70401_ec3d_4ef7_a2f0_1db4b9a9a140-------15-------)

site: https://wpdirectory.net

![None](https://miro.medium.com/v2/resize:fit:700/1*eNv7EOqdXUm09Nxn3q0RyA.png)

First make sure to enable "Private Search" Mode

Or else your searches will be publicly visible in this list

![None](https://miro.medium.com/v2/resize:fit:700/1*pGLnZpaiM_awWXnbLH8c1w.png)

**1️⃣ Nonce-Related Functions**

```bash
wp_(create|verify|check)_nonce\(
```

![None](https://miro.medium.com/v2/resize:fit:700/1*3gN3iXu-rZyCG1j0KiFFKA.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*EBFvp0zxInp-Wbdn1kq_Fw.png)

Click on `<>` to see the associated code block with it.

**2️⃣ SQL Queries**

```bash
\$wpdb->(query|get_results|get_row|get_var|get_col)\(
```

Might be vulnerable to SQLi attacks if there is no sanitization implemented, leveraging functions like `prepare()`

If there is controllable input via any request, and there is no sanitization. Test for SQL Injection here.

**3️⃣Deprecated Function Usage**

```typescript
_?deprecated_function\(
```

![None](https://miro.medium.com/v2/resize:fit:700/1*iFAzI0AcXw1mLYvAwTMe0w.png)

**4️⃣ Match hooks related to REST API**

```
add_(action|filter)\([\s|'|"]+rest_(api|prepare|pre_dispatch)\w*['|"]
```

![None](https://miro.medium.com/v2/resize:fit:700/1*DJr4f51pKODb6YpcxyLlew.png)

**5️⃣ Find calls to** **`get_option()`** **for option handling**

```
get_option\(['|"]\w+['|"]
```

![None](https://miro.medium.com/v2/resize:fit:700/1*R7hjX8rDf_Tnn6gd7BoLPw.png)

**6️⃣ Detect use of file inclusion functions**

```
(require|include)(_once)?\(['"]?.+['"]?\)
```

![None](https://miro.medium.com/v2/resize:fit:700/1*jQ1eyWYhKpfzE9q4VOHmDg.png)

**7️⃣ ShortCodes**

```
add_shortcode\(['|"]\w+['|"]
```

![None](https://miro.medium.com/v2/resize:fit:700/1*ag1LQ93BRiNYY2FprOovdQ.png)

**8️⃣ Critical Functions**

![None](https://miro.medium.com/v2/resize:fit:700/1*yoYl69gWj8PMhlJ2PWPJQw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*j1tSeW5Fl5rjx17VTASA7A.png)

**9️⃣ URL Redirections**

```
\b(wp_redirect|header)\s*\(
```

![None](https://miro.medium.com/v2/resize:fit:700/1*NAab9g-K5VdYlB7dhmGXrA.png)

**🔟 Function Name that contains some specific keywords**

```
public\s+function\s+\w*(export|delete|archive|format)\w*\s*\(
private\s+function\s+\w*(export|delete|archive|format)\w*\s*\(
```

![None](https://miro.medium.com/v2/resize:fit:700/1*b7ULTW1X1cnZwUyGN8S9BQ.png)

**🤖 ChatGPT Prompts Ideas**

![None](https://miro.medium.com/v2/resize:fit:700/1*2J1eZzh_CoDLFsEhHfrrWA.png)

ChatGPT User Prompt Screenshot

However, it's not always 100% accurate and needs more fine-tuning in some cases!

#### **Use duck(dot)ai for more power 🤑**

- No Login Required
- Free & Private
- No AI Training
- 5 LLMs