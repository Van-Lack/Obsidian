
##  Drupal

_what is Drupal technology?_

Drupal is **a free and open-source content-management framework that can be customized and is suitable for developing simple websites or complex web applications**.

# Drupal 7.x Exploitation (#Update)   (Aug 7, 2023)

### Road Map for Drupal Pentesting:

> [https://www.xmind.app/m/MTBCay/](https://www.xmind.app/m/MTBCay/)

![[Pasted image 20250309020152.png]]
# Content Management System (CMS)

**Description**

- A CMS is a powerful tool that helps build a website without the need to code everything from scratch.

**Consists of two key components:**

1. Content Management Application (CMA): the interface used to add and manage content.
2. Content Delivery Application (CDA): the backend that takes the input entered into the CMA and assembles the code into a working, visually appealing website.

![[Pasted image 20250309020604.png]]

		Drupal Wappalyzer example.

![[Pasted image 20250309020650.png]]

# Drupal Description

**An open-source CMS that is popular among companies and developers.**

- written in PHP and supports using MySQL or PostgreSQL.
- allows users to enhance their websites through the use of themes and modules.
- indexes its content using nodes & page URIs are usually of the form **/node/<_nodeid>**
- newer installs of Drupal by default block access to the **CHANGELOG.txt** and **README.txt** files.

## Drupal supports three types of users by default:

1. **Administrator:** has complete control over the Drupal website.
2. **Authenticated User:** can log in to the website and perform operations such as adding and editing articles based on permissions.
3. **Anonymous:** only allowed to read posts.

## Drupal website can be identified in several ways:

1. Header or footer message Powered by Drupal
2. Standard Drupal logo
3. Presence of a CHANGELOG.txt file or README.txt file
4. Page source
5. Clues in the robots.txt file  
    ⇒ references to /node

![](https://miro.medium.com/v2/resize:fit:700/1*L0n95Hum8NvHrIxnDYXrrQ.png)

								Drupal Discovery

> Lab: gain RCE via the various Drupal instances on the target host. When you are done, submit the contents of the flag*.txt file in the /var/www/drupal.ex.com directory.

Let’s try to identify Drupal Version of a random target uses **Drupal using CHANGELOG.txt file with cURL:**

```bash
curl -s http://drupal-qa.ex.com/CHANGELOG.txt | grep -m2 ""
```

![](https://miro.medium.com/v2/resize:fit:470/1*1gxm2V78K1H3sQU8FLSMvQ.png)

The current running version is Drupal **7.30**

After searching for Known Vulnerabilities, we found [CVE-2014–3704](https://www.drupal.org/SA-CORE-2014-005), known as Drupalgeddon, affects versions 7.0 up to 7.31 and was fixed in version 7.32.

## Drupalgeddon

this Vulnerability was a pre-authenticated SQL injection flaw that could be used to upload a malicious form or create a new admin user.

![](https://miro.medium.com/v2/resize:fit:700/1*N0rIZhaIcPfImJ5F9Q0-XQ.png)

Let’s try adding a new admin user with this [PoC](https://www.exploit-db.com/exploits/34992) script.

```bash
# python2.7  drupalgeddon.py -t <*url*> -u <*username*>-p <*password*>  
python2.7  drupalgeddon.py -t http://drupal-qa.ex.com -u admin -p admin
```

![](https://miro.medium.com/v2/resize:fit:700/1*3AjG-X37vHKhswnxVKLgWQ.png)

**We successfully exploited the vulnerability and created a new admin user** to gain access to Drupal admin panel and enable the `PHP Filter` module to achieve remote code execution.

## Enabling PHP Filter Module

![](https://miro.medium.com/v2/resize:fit:700/1*Itzjyu9Ce6jgZ2wFPLORcg.png)

Login with the new admin account credentials that we have created:

- admin:admin

Enable the PHP filter and Save configuration.

![](https://miro.medium.com/v2/resize:fit:700/1*7EgUZ2nzjULgHRru7quAtg.png)

Go to Content → Add content → create a Basic page → add malicious PHP snippet → set Text format drop-down to PHP code:

![](https://miro.medium.com/v2/resize:fit:700/1*NSxUkRDx_WRwslPkuZ0Kpg.png)

Successfully created a basic page with malicious PHP code to gain RCE later.

![](https://miro.medium.com/v2/resize:fit:540/1*SInOqCSV8QJ4yg1vsj7_fQ.png)

Let’s use cURL or browser to run the commands and gain RCE:

```bash
curl -s http://drupal.ex.com/node/4?yas=ls
```

We Successfully listed the /var/www/drupal.ex.com directory and found flag*.txt file.

![](https://miro.medium.com/v2/resize:fit:700/1*-iI8XUCNAgJWALfT7a4kzQ.png)

**Finally,** once an admin user is added from exploiting the Drupalgeddon vulnerability that affects our current Drupal running version **7.30**, we could log in and enable the `PHP Filter` module to achieve remote code execution.


---- old

---

# PHP


#PHP_Vulns

---

## 0-Click Account Takeover Exploit Using Punycode IDN Attacks 

_LostSec Methodology_

#Specific_CVE_Exploit

--- by Nahamsec: https://www.youtube.com/watch?v=4CCghc7eUgI