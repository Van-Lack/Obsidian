source: https://freedium.cfd/https://medium.com/@kumawatabhijeet2002/subdomain-enumeration-bbot-subfinder-sublist3r-assetfinder-amass-e4880cf4ab5b

[BBOT Official Site](https://www.blacklanternsecurity.com/bbot/Stable/)

install BBOT:

> sudo apt install pipx

```bash
pipx install bbot
```

- **Ensure Path**:

```bash
pipx ensurepath
```

- This ensures that the pipx bin directory is correctly added to your PATH environment variable.
- **Run BBOT**: After installation, restart your terminal, and type:

```bash
bbot
```

### ⚙️ Configuring BBOT for Maximum Results

To get the most out of BBOT, it's best to configure it with API keys from different services. Once BBOT is started, it automatically creates a configuration file at:

```bash
/home/<username>/.config/bbot/bbot.yml
```

![None](https://miro.medium.com/v2/resize:fit:700/0*leYrKalMPU-yI2wt.png)

Here's a quick look at how you can configure it with your API keys:

API keys to work:

[BeVigil](https://bevigil.com/osint-api), [BinaryEdge](https://binaryedge.io), [BufferOver](https://tls.bufferover.run), [C99](https://api.c99.nl/), [Censys](https://censys.io), [CertSpotter](https://sslmate.com/certspotter/api/), [Chaos](https://chaos.projectdiscovery.io), [Chinaz](http://my.chinaz.com/ChinazAPI/DataCenter/MyDataApi), [DNSDB](https://api.dnsdb.info), [Fofa](https://fofa.info/static_pages/api_help), [FullHunt](https://fullhunt.io), [GitHub](https://github.com), [Intelx](https://intelx.io), [PassiveTotal](http://passivetotal.org), [quake](https://quake.360.cn), [Robtex](https://www.robtex.com/api/), [SecurityTrails](http://securitytrails.com), [Shodan](https://shodan.io), [ThreatBook](https://x.threatbook.cn/en), [VirusTotal](https://www.virustotal.com), [WhoisXML API](https://whoisxmlapi.com/), ZoomEye API [china](https://api.zoomeye.org) - [worldwide](https://api.zoomeye.hk), [dnsrepo](https://dnsrepo.noc.org), [Hunter](https://hunter.qianxin.com/), [Facebook](https://developers.facebook.com), [BuiltWith](https://api.builtwith.com/domain-api)

Open your BBOT configuration file and add your API keys:

```bash
mousepad /home/<username>/.config/bbot/bbot.yml
```

### Starting Subdomain Enumeration

Now for the fun part! To begin subdomain enumeration, simply run:

```bash
bbot -t target.com -p subdomain-enum
```

## JS files recon

#js

**---firstly do some  recon with other commands, like:**

_This command will help you find subdomains and then filter for JavaScript files._


```
echo "target.com" | hakrawler -js -depth 2
```

```
python3 linkfinder.py -i "https://target.com/static/js/main.js" -o cli
```

```
python3 JSParser.py -u https://target.com/static/js/main.js
```


> nano subs1 && nano subs2

>cat Subs*.txt | anew | tee AllSubs.txt

>cat AllSubs.txt | httpx -o AliveSubs.txt

These steps help refine your target list and ensure you’re focusing on live, responsive subdomains, and if [**httpx**](https://github.com/projectdiscovery/httpx) works slowly u can use something like [**SegFault**](https://blog.amrelsagaei.com/segfault-your-ultimate-cybersecurity-workspace/) for a better internet connection.

>cat AliveSubs.txt | waybackurls | tee urls.txt

Filters URLs with parameters and saves them for parameter-based testing.

> cat urls.txt | grep -iE '.js'| grep -ivE '.json'|sort -u | tee js.txt

# subjs fetches javascript files from a list of URLS or subdomains.

> cat urls.txt | subjs | tee subjs.txt

Analyzing javascript files can help you find undocumented endpoints, secrets, and more.

## Download All links in js.txt and do search about these


code:

```bash
file="js.txt"  
# Loop through each line in the file  
while IFS= read -r link  
do  
    # Download the JavaScript file using wget  
    wget "$link"  
done < "$file"
```


## 2° Method Download all javascript files

### Step 1: Download JavaScript Files

You can use a combination of tools like `wget` and `subfinder` to download the JavaScript files.

1. **Find JavaScript Files**:

```
subfinder -d target.com | httpx -silent -mc 200 -content-type "application/javascript" -o jsfiles.txt

```

**Download JavaScript Files**:

```bash
wget -i jsfiles.txt -P js_files/
```

### Step 2: Extract Endpoints from JavaScript Files

Use tools like `linkfinder` to extract endpoints from the downloaded JavaScript files.

```Bash
git clone https://github.com/GerbenJavado/LinkFinder.git
cd LinkFinder
pip install -r requirements.txt
```

```Bash
for file in js_files/*.js; do
    python linkfinder.py -i "$file" -o cli >> endpoints.txt
done

```

### Step 3: Compare Endpoints

To compare the endpoints across different JavaScript files, you can use a simple `diff` command or more advanced tools like `meld` for visual comparison.

Use `diff`
```
diff -u endpoints_old.txt endpoints_new.txt > diff_output.txt
```

**Use** `meld` **for visual comparison**:

```
meld endpoints_old.txt endpoints_new.txt
```


---


Extracts JavaScript files from URLs for analysis.

```bash
grep -r -E "aws_access_key|aws_secret_key|api key|passwd|pwd|heroku|slack|firebase|swagger|aws_secret_key|aws key|password|ftp password|jdbc|db|sql|secret jet|config|admin|pwd|json|gcp|htaccess|.env|ssh key|.git|access key|secret token|oauth_token|oauth_token_secret" /path/to/directory/*.js
```

Make sure to replace `/path/to/directory` with the actual path to the directory where your .js files are located. The command will recursively search for the specified keywords in all .js files within that directory.

# JavaScript Analysis using nuclei

Once you get the JS URLs you can use nuclei exposures tag on it get more sensitive information .

To run a Nuclei command on the `js.txt` file with the `exposures` tag, you can use the following command:

> nuclei -l js.txt -t ~/nuclei-templates/exposures/

or

![[Pasted image 20241223213102.png]]


```
nuclei -u {single subdomain or list of subdomains} -tags medium,high,critical
```

**This command will check the medium,high, critical templates stored in the tool and scan our target and if any vulnerabilities found it will reflect as output**

403 bypass: https://github.com/Dheerajmadhukar/4-ZERO-3

```
installation: 

git clone https://github.com/Dheerajmadhukar/4-ZERO-3.git

cd 4-ZERO-3

->  bash 403-bypass.sh -h

->  bash 403-bypass.sh -u https://target.com/secret --exploit
```

![[Pasted image 20241223213524.png]]

![[Pasted image 20241223232740.png]]

If some of the results pop-up to be a "200 response" test it on burpsuite by adding the payload cuz it could be a **false positive**:

![[Pasted image 20241224000153.png]]


![[Pasted image 20241224000113.png]]
						Access

![[Pasted image 20250321154825.png]]

🔖The ultimate 403 Bypass wordlists and tester notes by JHaddix

📱 Github: 🔗 Link (https://github.com/Arcanum-Sec/hack_tips/blob/main/403bypass.md)



----
## Automating reading .js files

Another approach is to use a tool called GetJS by 003random which can be found here: [https://github.com/003random/getJS](https://github.com/003random/getJS). GetJS will take a list of domains and extract any .js files found on each domain. This is useful if you want to sort through lots of .js files for new urls/endpoints (which we explain below) on a mass scale. If the website you are testing **does** contain a specific .js file for each feature/endpoint, then you could begin scanning for endpoints to discover new JS files.

---

## WaybackFinder

A tool that efficiently s~={red}earches for backup files within Wayback Machine or archive.org URLs.=~ (like WayMore) Here’s a quick overview of how I was able to locate backups containing `.sql` files in a matter of minutes. This write-up aims to provide insights into how you can make the most of this tool.

> GitHub: [https://github.com/anmolksachan/WayBackupFinder](https://github.com/anmolksachan/WayBackupFinder)

To start, I selected a target and ran the tool using a custom option instead of scanning through an extensive list of extensions. For this specific case, I focused on `.zip` files. Within seconds, the tool generated a list of `.zip` files that were not directly accessible on the website but were still available on archive.org.

![](https://miro.medium.com/v2/resize:fit:700/1*9wVniML-0sPwkZnf2xf31g.png)

I came across a suspicious .zip file and attempted to open it directly. However, I was redirected to an authentication page, suggesting that the file is either no longer available or requires authentication for access.

![](https://miro.medium.com/v2/resize:fit:700/1*q_ugrbizCRdRerHx2opiRA.png)

Using the link located through [WayBackupFinder](https://github.com/anmolksachan/WayBackupFinder) and accessed via web.archive.org, I successfully downloaded the .zip file.

![](https://miro.medium.com/v2/resize:fit:700/1*xkiXign40Ck0U6cwQZyn4g.png)

![](https://miro.medium.com/v2/resize:fit:700/1*e4oKz_sIwEq6t5e84bl_yw.png)

Upon extracting the file, several `.sql` files were discovered.

![](https://miro.medium.com/v2/resize:fit:700/1*dARCT1mMUJ4hdazV5VdPDw.png)

This is an example of how the tool can be used to identify backups, uncover information disclosure, and detect other critical vulnerabilities.

Read more about this tool: [https://anmolksachan.medium.com/unlock-hidden-backups-with-waybackupfinder-py-7b98041a82d9](https://anmolksachan.medium.com/unlock-hidden-backups-with-waybackupfinder-py-7b98041a82d9)


----

source: https://shubhamrooter.medium.com/deep-subdomains-enumeration-methodology-da606be0c4c3
# Active Enum

1. DNS Brute Forcing [ using puredns]

DNS brute-forcing is performed using the `puredns` tool. This involves setting up prerequisites by installing `massdns` and `puredns`, downloading resolvers and DNS wordlists, and then using `puredns` to brute-force subdomains.

```bash
#Prerequisites  
git clone https://github.com/blechschmidt/massdns.git  
cd massdns  
make  
sudo make install  
  
#Installing the tool  
go install github.com/d3mondev/puredns/v2@latest  
  
# Download Resolvers List  
wget https://raw.githubusercontent.com/trickest/resolvers/main/resolvers-trusted.txt  
  
# You even can make yours  
git clone https://github.com/vortexau/dnsvalidator.git  
cd dnsvalidator/  
pip3 install -r requirements.txt  
pip3  install setuptools==58.2.0  
python3 setup.py install  
dnsvalidator -tL https://public-dns.info/nameservers.txt -threads 100 -o resolvers.txt  
  
# Download dns wordlist    
wget https://wordlists-cdn.assetnote.io/data/manual/best-dns-wordlist.txt   
  
# Brute Forcing  
puredns bruteforce best-dns-wordlist.txt example.com -r resolvers.txt -w dns_bf.txt
```


2. Permutations

Permutation techniques are used to generate variations of subdomains. Wordlists are used with the `gotator` tool to create permutations, which are then resolved using `puredns`.

```bash
# Permutation words Wordlist  
wget https://gist.githubusercontent.com/six2dez/ffc2b14d283e8f8eff6ac83e20a3c4b4/raw  
# Run   
gotator -sub subdomains.txt -perm dns_permutations_list.txt -depth 1 -numbers 10 -mindup -adv -md | sort -u > perms.txt  
# DNS resolve them and check for valid ones.  
puredns resolve permutations.txt -r resolvers.txt > resolved_perms  
# Hint: Collect subdomains that is not valid and make compinations then resolve them u may git valid unique subdomains that is hard to find   
gotator -sub not_vali_subs.txt -perm dns_permutations_list.txt -depth 1 -numbers 10 -mindup -adv -md | sort -u > perms.txt
```

3. Google Analytics

The `AnalyticsRelationships` tool is used to find subdomains associated with a target domain based on Google Analytics tracking codes.

```bash
git clone https://github.com/Josue87/AnalyticsRelationships.git  
 cd AnalyticsRelationships/Python  
 sudo pip3 install -r requirements.txt  
 python3 analyticsrelationships.py -u https://www.example.com

```


4. **TLS, CSP, CNAME Probing**

The `cero` tool is used for TLS, CSP, and CNAME probing to gather additional subdomain information.
```bash
go install github.com/glebarez/cero@latest  
  #tls  
  cero in.search.yahoo.com | sed 's/^*.//' | grep -e "\." | sort -u  
  #cls  
  cat subdomains.txt | httpx -csp-probe -status-code -retries 2 -no-color | anew csp_probed.txt | cut -d ' ' -f1 | unfurl -u domains | anew -q csp_subdomains.txt  
  # cname  
  dnsx -retry 3 -cname -l subdomains.txt

```
5. **Scraping(JS/Source code)**

: Subdomains are probed using the `httpx` tool, and the obtained URLs are then fed into `gospider` for web crawling. The output is cleaned and filtered to obtain the scraped subdomains.

```bash
# Web probing subdomains  
cat subdomains.txt | httpx -random-agent -retries 2 -no-color -o probed_tmp_scrap.txt  
  
# Now, that we have web probed URLs, we can send them for crawling to gospider   
gospider -S probed_tmp_scrap.txt --js -t 50 -d 3 --sitemap --robots -w -r > gospider.txt  
  
#Cleaning the output   
sed -i '/^.\{2048\}./d' gospider.txt  
cat gospider.txt | grep -Eo 'https?://[^ ]+' | sed 's/]$//' | unfurl -u domains | grep ".example.com$" | sort -u scrap_subs.txt  
  
# Resolving our target subdomains  
 puredns resolve scrap_subs.txt -w scrap_subs_resolved.txt -r resolvers.txt

# Recursive Enumeration
```

This step involves performing recursive enumeration by iterating over the subdomains and using tools like `subfinder`, `assetfinder`, `amass`, and `findomain` to discover additional subdomains.


# Recursive Enumeration

This step involves performing recursive enumeration by iterating over the subdomains and using tools like `subfinder`, `assetfinder`, `amass`, and `findomain` to discover additional subdomains.

```bash
#!/bin/bash  
  
go install -v github.com/tomnomnom/anew@latest  
subdomain_list="subdomains.txt"  
  
for sub in $( ( cat $subdomain_list | rev | cut -d '.' -f 3,2,1 | rev | sort | uniq -c | sort -nr | grep -v '1 ' | head -n 10 && cat subdomains.txt | rev | cut -d '.' -f 4,3,2,1 | rev | sort | uniq -c | sort -nr | grep -v '1 ' | head -n 10 ) | sed -e 's/^[[:space:]]*//' | cut -d ' ' -f 2);do   
    subfinder -d $sub -silent -max-time 2 | anew -q passive_recursive.txt  
    assetfinder --subs-only $sub | anew -q passive_recursive.txt  
    amass enum -timeout 2 -passive -d $sub | anew -q passive_recursive.txt  
    findomain --quiet -t $sub | anew -q passive_recursive.txt  
done
```

# Finish Work

Finally, the obtained subdomains from different steps (horizontal and vertical enumeration) are consolidated and filtered using the `httpx` tool.

```bash
cd subs/  
cat horizontal/ptr_records.txt | sort -u > horizontal.txt  
cat Vertical/Active/* | sort -u > active.txt  
cat Vertical/Pssive/* | sort -u > passive.txt  
cat * | sort -u > all_subs.txt  
cat all_subs.txt | httpx -random-agent -retries 2 -no-color -o filtered_subs.txt
```

