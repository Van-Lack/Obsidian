**Automate with FOFA's API**

```python
import requests  
url = "https://fofa.info/api/v1/search?email=YOUR_EMAIL&key=API_KEY&qbase64=Ym9keT0iL2FwaS92MSI="  
response = requests.get(url)  
print(response.json())  
```

_Spare LOT of time._

Use Example:

_A misconfigured **Prometheus endpoint** was leaking **internal Kubernetes metrics**, including **server IPs and credentials**.

```bash
body="Prometheus" && title="Metrics"
```

-- Ask DeepSeek For Deeper Understanding of the script and more Efficency

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

This **Nuclei template** is designed to detect **outdated software versions** by checking HTTP headers, JavaScript, and CSS files for version strings. Here's how you can use it effectively:

```yaml
id: outdated-software-detectioninfo:

  name: Outdated Software Detection
  author: pentester_x
  severity: low
  description: |
    Detects outdated software versions by extracting version information from headers, scripts, and stylesheets.reference:
    - https://nvd.nist.gov/vuln/search
    - https://www.cvedetails.com
    - https://www.exploit-db.com
    - https://cve.mitre.orgtags: outdated,software,vulnerable,version,cverequests:
  - method: GET
    path:
      - "{{BaseURL}}"
      - "{{BaseURL}}/version"
      - "{{BaseURL}}/status"
      - "{{BaseURL}}/server-info"
      - "{{BaseURL}}/api/version"
      - "{{BaseURL}}/v1/info"
      - "{{BaseURL}}/robots.txt"matchers:
      - type: regex
        part: header
        regex:
          - '(?i)(Server|X-Powered-By|Version):.*?(Apache|nginx|PHP|WordPress|Tomcat|MySQL)/(\d+\.\d+\.\d+)'
          - '(?i)(\b\d+\.\d+\.\d+\b)'extractors:
      - type: regex
        name: software_version
        group: 3
        part: header
        regex:
          - '(?i)(Server|X-Powered-By|Version):.*?(Apache|nginx|PHP|WordPress|Tomcat|MySQL)/(\d+\.\d+\.\d+)'
      - type: regex
        name: software_version
        regex:
          - '(?i)v?(?:ersion)?[\s:]*(\d+\.\d+\.\d+)'- method: GET
    path:
      - "{{BaseURL}}/static/main.js"
      - "{{BaseURL}}/css/styles.css"
      - "{{BaseURL}}/app/build.js"
      - "{{BaseURL}}/assets/scripts.js"matchers:
      - type: regex
        regex:
          - '(?i)v?\d+\.\d+\.\d+'
          - '@version\s+\d+\.\d+\.\d+'extractors:
      - type: regex
        name: software_version
        regex:
          - '(?i)v?(\d+\.\d+\.\d+)'
          - '@version\s+(\d+\.\d+\.\d+)'

```

### **Step-by-Step Guide to Using the Nuclei Template**

#### **1. Save the Template**

- Copy the YAML code and save it as `outdated-software-detection.yaml` in your Nuclei templates directory (e.g., `~/nuclei-templates/`).

Use the following command to scan a target:

```bash
nuclei -u https://target.com -t outdated-software-detection.yaml
```

If you have a list of URLs (e.g., `urls.txt`), use:

```bash
nuclei -l urls.txt -t outdated-software-detection.yaml
```

The template will:  
✅ Check HTTP headers (`Server`, `X-Powered-By`, etc.) for version info.  
✅ Scan `/robots.txt`, `/version`, `/server-info`, etc.  
✅ Look for version strings in JavaScript/CSS files (`main.js`, `styles.css`).  
✅ Report any detected versions (e.g., `Apache/2.4.29`, `WordPress 5.7.2`).

#### **4. Verify & Exploit (If Vulnerable)**

- Take the detected versions and check for known CVEs:
    
    - [NVD (nvd.nist.gov)](https://nvd.nist.gov/vuln/search)
        
    - [Exploit-DB (exploit-db.com)](https://www.exploit-db.com)

### **Bonus Tips**

🔹 **Combine with Other Templates**: Run multiple checks at once:

```bash
nuclei -u https://target.com -t cves/ -t exposures/
```

🔹 **Rate Limiting**: Use `-rate-limit 50` to avoid overwhelming the target.  
🔹 **Proxy Support**: Use `-proxy http://127.0.0.1:8080` to inspect traffic in Burp Suite.

### **What This Template Does Well**

✔ Detects **version leaks** in headers & static files.  
✔ Covers common paths (`/version`, `/server-info`).  
✔ Uses **regex** to extract version numbers (e.g., `1.2.3`).

### **Possible Improvements**

- Add more **version-specific paths** (e.g., `/CHANGELOG.txt`).
    
- Include **more software** (Drupal, Joomla, etc.).
    
- Add **CVE matching** directly in the template.


----

![[Pasted image 20241130184058.png]]

![[Pasted image 20241130184129.png]]

#find_env_backups_with_nuclei
# Advanced Tactics: Beyond Default Templates

## 1. Crafting Laser-Focused Custom Templates

Most P4/P5 bugs are **context-specific** and not covered by generic templates. Let’s build a custom template to find **exposed** `**.env**` **files** (a common P4 issue):

```bash
id: exposed-env-file  
info:  
  name: Exposed Environment File  
  severity: low  
  author: your-name  
http:  
  - method: GET  
    path:  
      - "{{BaseURL}}/.env"  
      - "{{BaseURL}}/env.production"  
      
    matchers:  
      - type: word  
        words:  
          - "DB_PASSWORD"  
          - "API_KEY"  
        condition: and
```

**Why This Works**: Many apps accidentally expose environment files. This template checks for critical keywords (e.g., `DB_PASSWORD`) to reduce false positives.

**_Example of an .env file:_**

```bash
ENV="DEVELOPMENT"  
LOG_LEVEL="DEBUG"  
SMTP_HOST="smtp.targetdomain.com"  
SMTP_PORT=587  
SMTP_USER="contact@targetdomain.com"  
SMTP_PASS="TargetSecurePassword2024"  
SMTP_TLS=0  
SMTP_CONNECTION_TIMEOUT_SECONDS=5  
DB_HOST="targetdbserver.targetdomain.com"  
DB_DATABASE_NAME="target_database"  
DB_USER="target-db-user"  
DB_PASSWORD="2024TargetSecurePassword"  
PAYMENT_GATEWAY="payment.targetdomain.com"  
PAYMENT_SECRET="target-secure-payment-api-key"
```

## 2. Flags That Unlock Hidden Power

**Precision Targeting**:

```bash
nuclei -l targets.txt -severity low,info -tags exposure,misconfig
```

_Scans only low/info-severity bugs tagged as exposures or misconfigurations._

**Stealth Scanning**:

```bash
nuclei -rate-limit 100 -retries 2 -timeout 5 -project
```

Slows down scans to avoid WAF detection and saves progress with `-project`.

**Context-Aware Headers**:  
Add custom headers to bypass security:


```bash
# In your template:   
headers: X-Forwarded-For: 127.0.0.1 User-Agent: Mozilla/5.0 (compatible; Googlebot/2.1)

```
## 3. Secret Tip: Weaponize Workflows

Combine Nuclei with **subdomain enum** and **HTTPx** for a killer pipeline:

```bash
subfinder -d target.com | httpx -silent | nuclei -t custom-templates/ -severity low,info -o results.txt
```

**Pro Move**: Use `interactsh` to find blind vulnerabilities (e.g., SSRF):

```bash
# In template:  
payloads:  
  ssrf-payload:  
    - "http://{{interactsh-url}}"
```

# Real-World Scenarios

## Case 1: Mass Default Credentials

**Target**: IoT device admin panels (common in IP cameras, routers).

**Template**: Check `/login` for default credentials:

id: default-creds-router http: - method: POST path: "/login" body: 'username=admin&password=admin' matchers: - type: word words: - "Welcome, Admin"

- **Why It Works**: Many devices ship with hardcoded credentials. Use `-rate-limit` to avoid lockouts.

## Case 2: Version Disclosure Hunting

```bash
id: gitlab-version-disclosure  
http:  
  - method: GET  
    path: "/help"  
    matchers:  
      - type: regex  
        regex: 'GitLab Community Edition v(\d+\.\d+\.\d+)'  
  extractors:  
    - type: regex  
      regex: 'GitLab Community Edition v(\d+\.\d+\.\d+)'  
      part: body  
      group: 1
```

Cross-reference versions with **CVEs** using `nuclei -as` (automatic scan).

# Secret Tricks Most Hunters Miss

1. **Bypass 403s**: Add `Referer: https://target.com` or `X-Original-URL: /admin` headers.
2. **Target Shadow IT**: Use `chaos-client` to fetch recent subdomains, then scan with Nuclei.
3. **Continuous Scanning**: Run Nuclei nightly via GitHub Actions with `schedule` (target new assets).
4. **False Positive Filters**: Add `negative: true` in templates to exclude common noise patterns.

# Conclusion

Automating P4/P5 bugs isn’t glamorous, but it’s a **force multiplier**. By combining Nuclei’s speed with custom templates and stealthy flags, you can dominate low-severity findings while reserving manual effort for critical bugs. Remember:

- **Update templates daily** (new checks = new opportunities).
- **Always validate results** (false positives burn rapport).
- **Chain findings** (5 P5s might = 1 P3).


###  4. How to Prevent Exposing Sensitive Data

• Use .gitignore: Ensure the .env file is added to .gitignore to prevent accidental commits.  
  
• Environment Variable Management: Use tools like Docker, AWS Secrets Manager, or Azure Key Vault to securely manage sensitive information.

• Server Configuration: ~={orange}Configure servers to deny access to hidden files like .env using .htaccess for Apache or equivalent for other servers.=~

• Cloud Security Best Practices:  
— Use Secret Management Tools: Utilize cloud-native secret management services to store and manage environment variables securely.  
— Proper IAM Configuration: Ensure that only authorized personnel and services can access sensitive files in cloud environments.  
— Secure Storage Configurations: ~={green}Regularly audit cloud storage permissions and configurations to avoid public access to sensitive data.=~

## How to Use FOFA, Shodan.io, and Hunter.io for Advanced Cyber Reconnaissance

> https://freedium.cfd/https://medium.verylazytech.com/how-to-use-fofa-shodan-io-and-hunter-io-for-advanced-cyber-reconnaissance-602c23093fce

#### What Is FOFA?

**FOFA (Fingerprint of All)** is a powerful Chinese cybersecurity search engine that indexes global internet-facing assets by parsing banner data from ports, protocols, and services. It is widely used by red teamers and APT analysts.

#### Creating a FOFA Account

- Visit [https://fofa.info/](https://fofa.info/)
- Register a free or paid account for access to advanced search functionality.
- Paid accounts get more queries and deeper search capabilities.

### Basic FOFA Search Syntax

FOFA uses its own logic syntax:

`app="Apache" && country="US"`

**Operators:**

- `&&` (AND)
- `||` (OR)
- `!` (NOT)

**Common Filters:**

- `title=`
- `header=`
- `body=`
- `ip=`
- `port=`
- `domain=`

### Powerful FOFA Search Examples

**Find exposed Jenkins servers:**

`app="Jenkins"`

**Search for industrial control systems in Japan:**

`protocol="modbus" && country="JP"`

**Look for VPN devices with admin portals:**

`title="VPN" && body="Login"`

#### Exporting FOFA Results

- Use the **export** feature to get results in **CSV/JSON**.
- FOFA API allows **automated reconnaissance** via scripts.

#### FOFA Tips for Bug Bounty Hunters

- Search for subdomains of targets.
- Track misconfigured cloud storage.
- Monitor exposed IoT devices in scope.

#### Best Fofa Cheatsheets:

- [https://github.com/FofaInfo/Awesome-FOFA/tree/main](https://github.com/FofaInfo/Awesome-FOFA/tree/main)

![None](https://miro.medium.com/v2/resize:fit:700/1*VrrZLzAherDJqXuh8-LyFQ.png)


## Other FOFA Dorks:

**1️⃣ Subdomain Takeover POC Pages**

```bash
body="subdomain takeover poc by"
body="subdomain takeover by"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*mnds_oqcLeK8GvIByjymoA.png)


**2️⃣ Python or Django Debug Pages**

```bash
title="OperationalError at /" && body="Traceback"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*3o1-Xwz5ON0UsReMzGAUVg.png)

**3️⃣ Web Apps Exposing SQL Errors**

```ini
#MySQL error
body="You have an error in your SQL syntax"

#Oracle SQL
body="ORA-00933"

#PostgreSQL error
body="PG::SyntaxError:"
```

**4️⃣ API Gateways or Swagger UI**

```ini
title="Swagger UI" && body="basePath"
title="API Documentation" && body="jwt"
```

**5️⃣ XDebug Interface**

```ini
body="Xdebug" && header="Xdebug"
```

Test ports: 9003,9000

**6️⃣ Fingerprint Potential WAF Bypasses or Soft Fail Pages**

```ini
body="403 Forbidden" && header!="Cloudflare" && header!="Akamai"
```

**7️⃣ Exposed Gogs or Gitea Git Servers**

```bash
title="Gitea: Git with a cup of tea"
body="Powered by Gogs"


#others
body="config/database.yml"
body="aws_access_key_id"
```

**8️⃣ Exposed RabbitMQ or Message Queues**

```bash
title="RabbitMQ Management" && port="15672"
body="RabbitMQ Management" && header="Server: Cowboy"
```

**9️⃣ Discover Package Registries or Artifact Repositories**

Nexus Repo:

```bash
title="Nexus Repository Manager" && body="Welcome to Nexus"
```

JFrog Artifactory:

```bash
title="Artifactory" && body="Artifact Repository Browser"
```

May lead to possible information disclosure of

- Internal libraries (privately pushed jars, debs)
- Hardcoded credentials or secrets

**🔟 Misconfigured Reverse Proxies**

```bash
title="Envoy Admin" && body="stats"
title="Traefik" && body="Routers"
body="HAProxy"
title="Statistics Report for HAProxy"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*j59pUNMdcYywe6lH_NvvQQ.png)

**1️⃣1️⃣Exposed Redis Without Auth**

```bash
banner="redis" && port="6379" && header!="Authorization"
```

**1️⃣2️⃣ GitHub Actions or CI/CD**

```bash
body="/.github/workflows/" && title="index of"
body="actions/setup-node"
body="GITHUB_TOKEN="
```

![None](https://miro.medium.com/v2/resize:fit:700/1*EV59pBPkBMmXqiY-KZWk5g.png)


**1️⃣3️⃣No route to host**

```bash
body="No route to host" 
```

![None](https://miro.medium.com/v2/resize:fit:700/1*xMy6idxwjvzAzngobmOH0w.png)

FOFA Search Engine

![None](https://miro.medium.com/v2/resize:fit:700/1*J_XPOoFcb-tp0_dyC6zDBQ.png)

**1️⃣4️⃣Forgotten** **`.DS_Store`** **Indexes**

Use `.DS_Store` parser to enumerate hidden files/directories

```bash
title="index of" && body=".DS_Store"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*uJsXWDHNIWvYz96EyLXguw.png)

Tools for parsing `.DS_Store` files:

> https://github.com/lijiejie/ds_store_exp

> https://github.com/gehaxelt/Python-dsstore

**1️⃣5️⃣Internal Dev Tool Panels via Title/Tech Stack Confusion**

- FastAPI docs interface

```ini
icon_hash="439373620"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*v03DikgMaLg_k-tIJjO8mw.png)

- Rundeck login

```ini
icon_hash="919189549"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*XM8nt2ApaVIDwo3tq7mEMg.png)

- Weird port exposure = high probability of misconfig

```bash
app="phpMyAdmin" && port!=3306 && title="phpMyAdmin"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*aE2QgzdlVheCJJL3sg1U9w.png)

***-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*-*-****-*-*-*-*-**-*-*-*-*-**-*-*-*-*-**-*-*-*

### 📧 Hunter.io: The Ultimate Email & Domain Reconnaissance Tool

#### Using Hunter.io for Domain Email Enumeration

- Go to [https://hunter.io/](https://hunter.io/)
- Enter the target domain (e.g., `example.com`)
- Instantly get email formats like:
- `john.doe@example.com`
- `j.doe@example.com`

### Key Hunter.io Features

- **Email Finder:** Identify emails tied to a company.
- **Email Verifier:** Check if emails are deliverable.
- **Domain Search:** Get a full list of public email addresses for a domain.
- **Author Finder:** Find the author of a blog or document.

### Hunter.io API Integration

> curl "https://api.hunter.io/v2/domain-search?domain=example.com&api_key=YOUR_API_KEY"

Returns:

```json
{
	"data": { 
		"emails": [ 
		{"value": "jane.doe@example.com", "type": "personal"}, 
		{"value": "support@example.com", "type": "generic"} 
		]
	} 
}
```

### Why Hunter.io Is Powerful for Social Engineering

- **Identify employees for phishing simulations.**
- **Validate credentials dumps with verified email formats.**
- **Perform targeted recon for red teaming engagements.**

### Combining FOFA, Shodan, and Hunter.io for Maximum Reconnaissance

#### Step-by-Step Recon Workflow

1. **Start with Hunter.io** to get the organization's email patterns and discover key personnel.
2. **Use FOFA** to find internet-facing infrastructure, apps, exposed databases, and subdomains.
3. **Use Shodan** to find live services, firmware leaks, camera feeds, and misconfigured endpoints.
4. **Correlate results** to identify high-value assets.
5. **Pivot between tools** to map a full threat landscape.

### Example Scenario: Recon on TargetCorp.com

- **Hunter.io:** Reveals email format `first.last@targetcorp.com`
- **FOFA:** Finds `GitLab`, `Kibana`, and `ElasticSearch` panels exposed under subdomains.
- **Shodan:** Identifies outdated `OpenSSH` and accessible `Redis` servers.

This intelligence enables **phishing simulations**, **initial foothold exploitation**, and **lateral movement planning** in red team exercises.


————————————————————————————————————

# Default Directory Listing

Used to locate web servers where directory listing is enabled unintentionally by the developer or sysadmin, and serving the delicious spicy foods directly to hackers with no efforts.

```bash
http.html:"index of /"
```

# Backup Files

When changes are made by the web developer, first they make backup of existing important files and then start working on the project, but this is the point where we get in easily :) . In other cases, they completely forget about it and remains there as it is. While some are honeypots to trap threat actors and just sniff what nefarious things the hacker is trying to play with the system and then catch them hopefully if beginner or just wait for the experienced threat actors to make a small mistake and it’s game over 💀

```bash
http.html:"index of /" http.html:"backup"
```

# Compressed Achives

Similar to backup files, here the developer might be working on a linux system mostly as we can assume by looking at the extension itself.

```bash
http.html:"index of /" http.html:"tar.gz"
```

# Database Files

One of the easy wins in bug hunting is information disclosure, mostly need not go into highly technical manual details like in manual WAF bypass for injection attacks, and directly enjoy the sensitive information it is providing for free🤑, admin credentials, user credentials, PII , internal structure of the database, tables, rows,columns, hashing used by the DB admin and unlimited opportunities of a persistent attacker.Not every company has regular pentest , audits, or not even a free vulnerability disclosure program, due to which these novice companies are the best fit for international attackers , attack and sell the data in dark web :)

```bash
http.html:"index of /" http.html:"database"  
http.html:"index of /" http.html:"sql.gz"  
http.html:"index of /" http.html:".sql"  
http.html:"index of /" http.html:".db"  
http.html:"index of /" http.html:"db_backup"  
http.html:"index of /" http.html:"mysql.dump"  
http.html:"index of /" http.html:".mdb"
```

# Configuration Files

Find web servers where configuration files are accessible, in those files we can see details that store settings, preferences for various applications , servers and the OS. High probability of data leaks like API keys, tokens, file paths and directories, application settings, network information and much more.

```bash
http.html:"index of /" http.html:".xml"  
http.html:"index of /" http.html:"config.xml"  
  
#tomcat: db connection strings  
http.html:"index of /" http.html:"server.xml"
```

# Wordpress Configuration Files

```bash
http.html:"index of /" http.html:"wp-config.php.txt"  
http.html:"index of /" http.html:"wp-config.txt"  
http.html:"index of /" http.html:"wp-config.php.bak"  
http.html:"index of /" http.html:"wp-config.php.old"  
http.html:"index of /" http.html:"wp-config.php.backup"  
http.html:"index of /" http.html:"wp-config.php.zip"  
http.html:"index of /" http.html:"wp-config.php.tar.gz"
```

**Tip:** fuzz for endpoints
```bash curl
cat subdomains.txt | xargs -P10 -I{} curl -s -o /dev/null -w "{}: %{http_code}\n" {}/wp-json/lp/
```

# Passwords

Passwords related to database, FTP, SSH, admin panel, email accounts, CMS login, network devices, RDP, VNC,etc…

```bash
http.html:"index of /" http.html:"pwd"  
http.html:"index of /" http.html:"pass.txt"  
http.html:"index of /" http.html:"password"  
http.html:"index of /" http.html:"password.txt"  
http.html:"index of /" http.html:"passwords.txt"  
http.html:"index of /" http.html:"passwords.zip"
```

# Windows Server Config Files

Configuration file of Microsoft IIS Windows Server.

```bash
http.html:"index of /" http.html:"web.config"
```

# Exposed Logs

Records of who accessed the server, their IP addresses, timestamps and the resource that was served to the client, login attempts information, application logs that capture application events and status.Database logs that holds inforation of database queries, transactions and errors, further releaving SQL and data structures used.Firewall logs that includes IP,protocols and red flags of threats.

```bash
http.html:"index of /" http.html:".log"  
http.html:"index of /" http.html:"access.log"  
http.html:"index of /" http.html:"error.log"  
http.html:"index of /" http.html:"php_error.log"  
http.html:"index of /" http.html:"debug.log"
```

# Configuration and Version Control Files

`**.env**` **File**: This is a configuration file typically used in development environments to store sensitive environment variables. Commonly found in web applications, `.env` files may contain:

- **Database credentials** (username, password, host, and port)
- **API keys and tokens** for third-party services
- **Secret keys** for session handling or encryption
- **SMTP credentials** for email servers
- **Debugging and logging settings**

`**.svn**` **Directory**: This is a directory created by the Subversion (SVN) version control system. When exposed, it can leak:

- **Source code** and **historical versions** of files, revealing internal application logic
- **Commit messages** that may describe vulnerabilities or sensitive changes
- **Developer notes** that may include hardcoded credentials, server paths, or deployment details
- **File metadata** that can give attackers insight into the structure of the application and directories

```bash
http.html:"index of /" http.html:".env"  
http.html:"index of /" http.html:".svn"
```

# Git Repositories

[Google Dork Alternative](https://www.youtube.com/watch?v=7QRH-SYprgk&list=PLqOQy6wxttBju5pKnatxQRY3fTejOkgZ9&index=2)

- `**.git**` **Directory**: Exposing the `.git` folder allows attackers to access the full codebase, commit history, and branches, which can reveal sensitive information like hardcoded credentials and the app’s internal logic.
- `**gitconfig**` **File**: This file contains Git configuration settings, user information, and possibly stored credentials

```bash
http.html:"index of /" http.html:".git"  
http.html:"index of /" http.html:"gitconfig"
```

---

##  Mass Hunting with FOFA Dorking

> https://medium.com/bugbountywriteup/mass-hunting-with-fofa-dorking-ad733f90a49e

### Error Pages & Debug Endpoints

Find exposed XDebug consoles:

```bash
body="Xdebug" && header="Xdebug"
```

Locates Django/Python debug pages

```bash
title="OperationalError at /" && body="Traceback"
```

### 2. API & Swagger UIs

```bash
title="Swagger UI" && body="basePath"
title="API Documentation" && body="jwt"
```

### 3. SQL & Application Errors

MySQL:

```bash
body="You have an error in your SQL syntax"
```

Oracle:

```bash
body="ORA-00933"
```

PostgreSQL:

```bash
body="PG::SyntaxError:"
```

### 4. Subdomain Takeover Clues

```bash
body="subdomain takeover poc by"
body="subdomain takeover by"
```

### 5. Dashboards & Admin Panels

Monitors Netdata instances

```bash
title="netdata dashboard"
```

Targets setup pages

```bash
http.html:"create admin" / http.html:"admin setup"
```

#Jenkins 

## Basic Jenkins Identification

```bash
title="Jenkins" || header="X-Jenkins" || body="jenkins"
```

## Exposed Jenkins Dashboards (No Authentication)

```bash
title="Jenkins" && header="X-Jenkins" && body="dashboard" && status_code="200"
```
## Jenkins with Script Console Access (Potential RCE)

```bash
title="Jenkins" && body="scriptText" && path="/script"
```

## Unauthenticated Jenkins Instances


```bash
title="Jenkins" && body="manage" && body="log in" && status_code="200" && body!="anonymous"
```

## Specific Vulnerable Versions


```bash
title="Jenkins" && header="X-Jenkins" && body="Jenkins ver." && body="2.3"  // Example for version 2.3
```

## Jenkins with Public Configuration Access


```bash
title="Jenkins" && body="configure" && body="system_config"
```

## Jenkins with Anonymous Read Access

```bash
title="Jenkins" && body="Overall/Read" && body="anonymous"
```

## Jenkins with Public API Access


```bash
title="Jenkins" && body="api/json" && status_code="200"
```

## Jenkins with Public Job Creation

```bash
title="Jenkins" && body="New Item" && body="create a job"
```

## Jenkins with Public User Creation


```bash
title="Jenkins" && body="Create User" && status_code="200"
```

### Usage Tips:

1. Combine these dorks with other filters like:
    
    - `&& country="US"` (for specific countries)
        
    - `&& after="2023-01-01"` (recently updated instances)
        
2. Focus on instances that show:
    
    - No authentication requirements
        
    - Older versions (check version numbers)
        
    - Exposed administrative interfaces
        
3. Always verify manually before testing as FOFA results may include false positives.

- look-out for Jenkins affected **RCE version**:  **≤ 2.441**.
### **How to Check Your Jenkins Version**:

1. Visit `http://<jenkins-server>/login` → Version is displayed at the bottom.
    
2. Or use:
    

```bash
curl -s http://<jenkins-server>/api/json | grep "jenkins-version"
```

### **Mitigation Steps**:

1. **Upgrade Jenkins** to the latest LTS version.
    
2. **Disable Script Console** for non-admins.
    
3. **Restrict Anonymous Access** (Configure Global Security → "Logged-in users can do anything").
    
4. **Audit Plugins** (many RCE flaws originate from vulnerable plugins).
    

### **Exploit Resources**:

- [Jenkins Advisory Database](https://www.jenkins.io/security/advisories/)
    
- [Exploit-DB](https://www.exploit-db.com/?q=jenkins)

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

**TO SEE: https://rokkamvamshi18.medium.com/using-shodan-to-find-open-jenkins-instances-and-exploit-them-1e7491ca27e3

---
#### How to Create Custom Templates

#Nuclei_Templates


**Nuclei Forge** – A visual editor for creating Nuclei YAML templates. Build templates by filling out fields like name, author, severity, description, tags, and references. It generates the corresponding YAML code in real-time.

> https://forge.bugbountyhunting.com

---

1. Navigate to the Nuclei templates directory:

> cd ~/nuclei-templates

2. Create a new template file:

> nano my_custom_template.yaml

3. Define a simple YAML-based template:

```bash
id: custom-sql-injection  
info:  
  name: Custom SQL Injection Finder  
  severity: high  
  description: "Detects SQL Injection vulnerabilities on specific endpoints."  
requests:  
  - method: GET  
    path:  
      - "{{BaseURL}}/search.php?query=1' OR '1'='1"  
    matchers:  
      - type: word  
        words:  
          - "syntax error"
```

4. Run your custom scan:

> nuclei -u https://target.com -t my_custom_template.yaml

### 2. Automating Nuclei with Bash & Python

To increase efficiency, bug bounty hunters often automate Nuclei scans. Below are simple scripts for automation.

#### 1. Bash Automation Script

```bash
#!/bin/bash
# Nuclei Automation Script
target_list="targets.txt"
output_dir="nuclei_results"

mkdir -p $output_dir

while IFS= read -r target; do
    echo "Scanning $target..."
    nuclei -u "$target" -t ~/nuclei-templates/ -o "$output_dir/$(basename "$target").txt"
done < "$target_list"

echo "Scanning complete. Check results in $output_dir/"

```

#### 2. Python Automation Script

```python
import subprocess

targets = ["https://example.com", "https://targetsite.com"]
output_dir = "nuclei_results"

subprocess.run(["mkdir", "-p", output_dir])

for target in targets:
    output_file = f"{output_dir}/{target.replace('https://', '').replace('/', '_')}.txt"
    print(f"Scanning {target}...")
    subprocess.run(["nuclei", "-u", target, "-t", "~/nuclei-templates/", "-o", output_file])

print("Scanning complete. Check results in the nuclei_results folder.")
```

### 3. Real-World Use Cases of Nuclei in Bug Bounty

#### 1. Mass Scanning Subdomains for Vulnerabilities

- Using **subfinder** or **Amass**, enumerate subdomains:

```typescript
subfinder -d target.com -o subdomains.txt
```

- Pipe the output into Nuclei:

```bash
cat subdomains.txt | nuclei -t ~/nuclei-templates/ -o findings.txt
```

#### _**2. API Security Testing**_

- Many bug bounty targets involve APIs. Use Nuclei to scan API endpoints:

```bash
nuclei -u https://api.target.com -t ~/nuclei-templates/api/
```

#### 3. Detecting Exposed Files & Secrets

- Scan for sensitive files such as **.env**, **backup files**, and **Git repositories**:

```bash
nuclei -u https://target.com -t ~/nuclei-templates/exposures/
```

### 4. Best Practices for Using Nuclei in Bug Bounty

1. **Update Templates Regularly:**

```typescript
nuclei -ut
```

**2. Use Rate Limiting to Avoid Detection & Bans:**

```bash
nuclei -u https://target.com -rl 10
```

**3. Combine Nuclei with Other Tools:**

- **Burp Suite** for manual validation
- **FFUF** for directory fuzzing

---

# gRPC FOFA Dork Recon for Pentest

_what is gRPC?_

**gRPC (Google Remote Procedure Call)**—a high-performance, open-source framework for communication between services, especially in microservices architectures.

Because gRPC uses HTTP/2 and Protocol Buffers (Protobufs) instead of traditional REST and JSON, it introduces unique challenges and attack surfaces. Pentesters focus on:

- **Service Enumeration**: Identifying available gRPC services and methods, often using tools like `grpcurl`, `grpcui`, or custom scripts.
    
- **Input Fuzzing**: Sending malformed or unexpected data to test how the service handles edge cases or potential injection attacks.
    
- **Authentication & Authorization Testing**: Verifying whether endpoints are properly secured and if privilege escalation is possible.
    
- **Burp Suite Integration**: Using extensions like `grpc-web-burp-extension` to decode and manipulate gRPC-Web traffic for deeper inspection.
    
- **Exploiting Misconfigurations**: For example, testing for SQL injection, insecure deserialization, or exposed debug endpoints.
    

_Since gRPC is binary and strongly typed, traditional web pentesting tools often fall short—so specialized tools and techniques are essential

#### 🔍 FOFA DORKING

```bash
header="grpc-status" 
header="Grpc-Message"
header="application/grpc"
```

![[Pasted image 20250617204451.png]]

```bash
header="application/grpc-web" 
header="application/grpc-web+proto"
```

or

```bash
header="x-grpc-web" 
header="grpc-timeout"
```

![[Pasted image 20250617204525.png]]

**DNS Names with grpc prefix**

> cert="grpc"


![[Pasted image 20250617204637.png]]

![[Pasted image 20250617204654.png]]

**Envoy server proxying gRPC traffic 🚦**

```bash
header="X-Envoy-Upstream-Service-Time" 
header="X-Envoy-Upstream-Service-Time" && header="grpc"
```

🔖 Some more

```bash
port="50051" || port="50052" 
body="grpc.health.v1.Health" 
body="grpc.reflection.v1alpha.ServerReflection" body="grpc_server_handled_total"
body="grpc_code" 
body="grpc_service="
header="application/grpc" && (port="443" || port="50051" || port="8443") body="grpc-gateway" && header="application/json"
```

**🤖 Prompt Ideas**

1️⃣ _Teach me gRPC pentesting methodology with complete steps_

2️⃣ _I need a complete GRPC pentesting workflow with a simple ASCII diagram to help in visualization_

3️⃣ _Can you give me a real world example of attacker targeting gRPC, showing the complete attack lifecycle?_

4️⃣ _Which industry or sector or businesses are more likely to use GRPC_

5️⃣ _Provide some latest news about threat actor's successful attack due to vulnerable GRPC endpoints_

6️⃣ _Currently I am stuck in a red team engagement where I am trying to replicate or simulate like an external attacker that is targeting GRPC endpoint for data exfiltration, assist me in it with complete detailed steps along with commands with explanation one by one._


---
## Hacking etico dei server RDP: alla scoperta dei segreti svelati per accedere ai desktop remoti!

#RDP

🚨 Live RDP Recon in azione! In questo tutorial, svelo come hacker e cacciatori di bug possono scoprire server RDP aperti e segreti esposti per accedere a sistemi remoti.


🔍 Cosa imparerai: 
**Come trovare server RDP pubblici** 
**Strumenti e metodi per estrarre i segreti RDP**
**Tecniche di caccia ai bug nel mondo reale**
**Consigli professionali per lo sfruttamento del desktop remoto .**

🔥 Strumenti utilizzati: Subfinder, Nuclei, Shodan, Google Dorking e altri. 

Find RDP servers:

> has_screenshot:true port 3389

![[Pasted image 20250612163355.png]]


Get a IP manually:

![[Pasted image 20250612164010.png]]

**Or get it with shodan Faced analysis:**

![[Pasted image 20250612164159.png]]

And to get all the ips in a rather fast way, we're going to simply **View the page source** and copy all the HTML so we're going to have all the **ips**

![[Pasted image 20250612164430.png]]

Then save it on a file:

![[Pasted image 20250612164512.png]]

And then just filter out all the Ips out of it:

![[Pasted image 20250612171711.png]]
## Exploiting Path:

**You basically look for the most common CVE in RDP servers and then search a github only tool for it and fetch for it searching through all the ips**

So ask deepseek for the most common CVEs in RDP servers [show cves you have found in shodan]
 
An example of cve: 

>CVE-2019-0708 vulnerability -- github:  https://github.com/davidfortytwo/bluekeep

E.G: ![[Pasted image 20250612173118.png]]

![[Pasted image 20250612172554.png]]

	Look for CVE by nmap scan:

1. nmap -sV --script=rdp-vuln-ms12-020 -p 3389 -iL ip.txt


## Brute Force Server:

```bash
1. ncrack -vv --user Administrator -p /usr/share/wordlist/rockyou.txt rdp://192.168.186.130
```

```bash
2. hydra -l Administrator -P pass.txt rdp://192.168.*.* -V -f -t 4
```

## Join:
```bash
 rdesktop 192.168.10.130 -u Administrator -p Admin123
```


🔗 LINK: 📖 I MIEI LIBRI PREFERITI: Bug Bounty Bootcamp: La guida per trovare e segnalare le vulnerabilità web - [https://amzn.to/4k5RZXB](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbG1GNUhwVjZaWE9RVFdIU3hoY1B5U1RQYnZUQXxBQ3Jtc0ttVkZ5dHJXZHVsU19QNU9icS1EXzBCQng0WEFuMTZNeDIwaEstUXc1cEZydDQ5WURucWVZMVhuUWNPaEZOT1A3bEQwRzFRT0FJRDNvM21QSmV1R1BSbll5LWRfM3UzOGRMUUswMTdFaFJtZ1VIZWZpOA&q=https%3A%2F%2Famzn.to%2F4k5RZXB&v=JlavbZMj6nM) 

Hacking API: Rompere le interfacce di programmazione delle applicazioni web -[https://amzn.to/431CqtW](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGhQdkVjNEV6QVFUelNoYlVRWmJxbFdsRWFZZ3xBQ3Jtc0tsd01EeE13OFhfb0thSUMwTjZJbTFoZU9aQ25MN2lnUGMwYmozM1AzSEgtS2pleWZmRGs3cmZmcXdPakJ1MEpmQ3NzMG91bkxrSnlCckpfVFBDMURNaWNIdjFEZDlua3JYMHdONWQ2ZU11QzN4Nm50dw&q=https%3A%2F%2Famzn.to%2F431CqtW&v=JlavbZMj6nM) 

Black Hat GraphQL: Attaccare le API di nuova generazione - [https://amzn.to/416zObv](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbXpFYnBVX3VQeEVPVjFaWTE4ZVVRMVRUcUpEd3xBQ3Jtc0trSFI4V3NuUnFBSldxdDdwYlk1RDB2Mll1SVp6V2xBUzY0c05hcG85VWw2VlMxdVFwOEJzR1pLLUFHMUJyVjNmT1laZEpHWWtWOFloMDlNNlJUOWREaGpSeE5MM3lCTjBuYTBxWXpEYUlFZ0gxX3J3UQ&q=https%3A%2F%2Famzn.to%2F416zObv&v=JlavbZMj6nM)

📢 𝗦𝘁𝗮𝘆 𝗖𝗼𝗻𝗻𝗲𝗰𝘁𝗲𝗱 with the creator: Twitter: [https://x.com/Haxshadow7](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbmFJQktmUHd2T0ZDajM2Q2JULXFqMXhoUWpoUXxBQ3Jtc0trOVZwNF9ZMXhVenJLcU1yWUxIMDhfdHFTdzNQNVRQcGQtdkJSa1JzS2F0LS11V1NUMGVJM245TmFyekkyNDI3T0VFbGpiOFlzX1l6clhNdmpyaEh6QmloV1hKb2JTYlBsRUNzbVREME1LTGt1UXFkRQ&q=https%3A%2F%2Fx.com%2FHaxshadow7&v=JlavbZMj6nM) Facebook: @haxshadow

----

# Legion Hunter Hidden Endpoints
## Urlscan, fofa, google, shodan and virustotal😈

#easy_boounty 

```bash
site:urlscan.io inurl:result "domain.tld"
```

Before you proceed, navigate to options and change Scan Visibility to **Private**

![None](https://miro.medium.com/v2/resize:fit:700/1*SyqffzWE4du-XBtnwmOBEg.png)

Enter the endpoint you want to scan

![None](https://miro.medium.com/v2/resize:fit:700/1*2WVcLSXB_uNb4YueEeDFQA.png)


![None](https://miro.medium.com/v2/resize:fit:700/1*RHfT6EjieOkozdVJtPo7Xg.png)


> 66 HTTP Transactions

![None](https://miro.medium.com/v2/resize:fit:700/1*D741zc_2j2ApGVLnR7i0_w.png)


> 78 Links

![None](https://miro.medium.com/v2/resize:fit:700/1*EkFUk3iL6xlaRm1ZazXufw.png)


> Javascript Global Variables

![None](https://miro.medium.com/v2/resize:fit:700/1*x7M82LkgjrvU-Kdm5mPvwA.png)


After going through all these, you may think like I am simply entering a domain to scan through an online tool and pasting the results, may be there is nothing worth to dive into as such with creativity.

_All these are recon information, but how you will leverage it is what matters at the end. You can find all hidden assets, but **you need the knowledge to test them as well.** This article is mainly for those who knows how to test but not able to find hidden assets to get non-duplicate fresh bug._

**🔍 FOFA Recon**

Now I will use FOFA Search Engine to find all webpages of a particular target that contains a variable name/object/function in it's DOM or HTML body.

```bash
domain="target.com" && body="gform_i18n"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*3hv-dM1SDiEMD5erUCbnxg.png)


![None](https://miro.medium.com/v2/resize:fit:700/1*spVgYOkHkBQcPfGQICNK3A.png)


Click on **Website body** icon and search for that particular string we are focusing from the beginning.

![None](https://miro.medium.com/v2/resize:fit:700/1*DTAe6RsIiRJYbFiEgDMEZw.png)


Let's say I found RXSS on a particular subdomain sub.example.tld, these are the things I will do next to mass target all other assets of target example.tld

- Take note of the parameter name which is vulnerable to RXSS , then fetch all url endpoints using `waymore` and **grep for that parameter name. Then test all those now.**
- Find the favicon hash of the subdomain and find similar assets.
- Try to figure out the naming convention of the subdomain that you found previously vulnerable, then try to pick other subdomains with similar names. If you don't find urls using waymore, use katana instead.
- View the page source , and pick something that is unique , variable name,object, comment, etc… and use the above mentioned FOFA dork technique.

> IPs, Domains, Hashes associated

![None](https://miro.medium.com/v2/resize:fit:700/1*-lD4yFohQDoXP_1uJ4-zuw.png)


> Content Tab (Forms)

![None](https://miro.medium.com/v2/resize:fit:700/1*KB6VR5RfDU_F3QTWR1RS2w.png)


**🗃️ Subdomains (via Urlscan)**

```bash
#to find only subdomains (sub.domain.com) excluding www.domain.com
site:urlscan.io inurl:result intitle:"domain.tld" -intitle:"www.domain.tld"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*XECSXHQ7RwWjQmrhWcfljg.png)

**🦀 VirusTotal Relations Recon**

Now let's say we want to go more deep inside the subdomain

`urs.earthdata.nasa.gov`

Use below virustotal path to find more subdomains associated with it.

```bash
virustotal.com/gui/domain/sub2.sub1.domain.tld/relations
```

> Subdomains — 4

![None](https://miro.medium.com/v2/resize:fit:700/1*OYDNYcGBLf0b4I_Xu2TBPQ.png)


> Siblings — 722

![None](https://miro.medium.com/v2/resize:fit:700/1*9QSa8YPmPGGoYNggvhyyLg.png)

---
#### 🔍Some more URLScan Results search

1️⃣ Domain creation date and recent hostnames

```
urlscan.io/domain/sub.domain.tld
```

![None](https://miro.medium.com/v2/resize:fit:700/1*DODjfpDYA-rOxdLyV9oklg.png)


2️⃣ With submission date:

```bash
site:urlscan.io inurl:result "Submission: On April 10"
site:urlscan.io inurl:result "April 10th 2025"
```

3️⃣ Submission Type

```bash
#via api
site:urlscan.io inurl:result "nasa.gov" "via api"

#via manual
site:urlscan.io inurl:result "nasa.gov" "via manual"
```

4️⃣ Specific to a particular subdomain

```lua
site:urlscan.io inurl:result "earthdata.nasa.gov"
```

5️⃣ Potentially Malicious

```bash
site:urlscan.io inurl:result "Potentially Malicious" "domain.tld"
```

6️⃣ URL Path & Parameter names Keywords

```lua
site:urlscan.io inurl:result "param_name="
```

![None](https://miro.medium.com/v2/resize:fit:700/1*RDDpK_4_LKFIUCti7ZhnhQ.png)

7️⃣ TLS Certificate

```lua
site:urlscan.io inurl:result "Let's Encrypt"
site:urlscan.io inurl:result "TLS certificate: Issued by E6"
site:urlscan.io inurl:result "Issued by R11"
site:urlscan.io inurl:result "Issued by R10"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*bILCYvxbeTwtbcetvSXFHg.png)

8️⃣ First time scanned

```lua
site:urlscan.io inurl:result "domain.tld" "This is the only time"
```

9️⃣ Malicious Results

```lua
site:urlscan.io "Attention: Verdicts and comments can only be added"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*Yd_1vMTQHx2F8bbLxoRyhQ.png)


🔟 Scanned from a particular Country

```lua
site:urlscan.io inurl:result "Scanned From US"
```

![None](https://miro.medium.com/v2/resize:fit:700/1*nwEGfIhpSJgwGAsnIyb92A.png)

**💡Pro Tip** > Use the notify tool and get an automated alert on Discord for any new subdomains every x min/hour/day/week/month.

---
