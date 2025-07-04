
conversation: https://copilot.microsoft.com/chats/sjDpfeyDRi22AW2G4AsQp & https://chat.deepseek.com/a/chat/s/77692b0a-279e-4a3a-ab23-b24386c9d0ee

```bash
# Create the script file
nano recon.sh

# Paste the entire script content (from my previous response)
# Press Ctrl+O to save, then Ctrl+X to exit nano

# Give executive perms
chmod +x recon.sh
```

### **First-Time Setup (Debian)**

Run these commands **before first use**:

```bash
# Install dependencies
sudo apt update && sudo apt install -y git jq whatweb python3-pip golang parallel

# Install Go tools
go install -v github.com/projectdiscovery/{subfinder,httpx,nuclei,naabu,interactsh-client}/v2/cmd/...@latest
go install -v github.com/{tomnomnom/assetfinder,LukaSikic/subzy,hahwul/dalfox/v2}@latest

# Install Python tools
pip3 install bbot waymore
git clone https://github.com/devanshbatham/ParamSpider && cd ParamSpider && pip3 install -r requirements.txt

# Add Go binaries to PATH
echo 'export PATH=$PATH:~/go/bin' >> ~/.bashrc
source ~/.bashrc

```

### **7. Troubleshooting**

#### **Permission Denied?**

```bash
# Fix permission issues
chmod +x recon.sh
sudo chown $USER:$USER recon.sh
```

#### **Tools Not Found?**
```bash
# Verify installations
subfinder -version
httpx -version
nuclei -version

```
#### **Update All Tools**

```bash
go install -v github.com/projectdiscovery/{subfinder,httpx,nuclei,naabu}/v2/cmd/...@latest
nuclei -update-templates
```
---

### **8. Pro Tips**

1. **Run in Background**:
    
```js

# comes pre-installed with the coreutils package

nohup ./recon.sh example.com > scan.log &
```
    
- **Monitor Progress**:
    
```js
tail -f ~/recon/example.com/scan-summary_*.md
```
    
- **Schedule Daily Scans** (Cron Job):
    
```js
crontab -e
# Add this line (runs at 2 AM daily):
0 2 * * * /path/to/recon.sh
```

```bash
#!/bin/bash

# =====[ INITIAL CHECKS ]=====
if [ -z "$1" ]; then
  echo "[*] No domain passed. Reading from targets.txt..."
  while read -r domain; do
    bash "$0" "$domain"
  done < targets.txt
  exit 0
fi

# =====[ CONFIGURATION ]=====
DOMAIN=$1
DATE=$(date +"%Y-%m-%d")
TIME=$(date +"%H:%M")
STAMP="$DATE $TIME"
HASH_STORE=".last_hash_$DOMAIN"
BASE_DIR="$HOME/recon"
TARGET_DIR="$BASE_DIR/$DOMAIN"
CACHE_DIR="$TARGET_DIR/cache"
SLACK_WEBHOOK="https://hooks.slack.com/..."  # Set your webhook here

# Create directories
mkdir -p "$TARGET_DIR" "$CACHE_DIR"

# =====[ TOOL CHECKS ]=====
check_tool() {
  if ! command -v "$1" &> /dev/null; then
    echo "[!] $1 not found. Please install it first."
    exit 1
  fi
}

check_tool subfinder
check_tool assetfinder
check_tool amass
check_tool httpx
check_tool jq

# =====[ 1. PRECHECK: NEW ASSETS via crt.sh ]=====
echo "[*] Checking for new cert entries on crt.sh..."
CRT_SUBS=$(curl -s "https://crt.sh/?q=%25.$DOMAIN&output=json" | jq -r '.[].name_value' | sed 's/\*\.//g' | sort -u)
echo "$CRT_SUBS" > "$TARGET_DIR/crtsh_subs.txt"

# If no change from last cert crawl, exit early
CRT_HASH=$(echo "$CRT_SUBS" | md5sum)
if [ -f "$HASH_STORE" ] && grep -q "${CRT_HASH%% *}" "$HASH_STORE"; then
  echo "[✓] No new cert-based subs. Skipping recon."
  exit 0
fi
echo "${CRT_HASH%% *}" > "$HASH_STORE"

echo "[*] New assets in cert logs detected, starting recon..."

# =====[ 2. SUBDOMAIN ENUMERATION ]=====
echo "[*] Enumerating subdomains (parallel)..."
subfinder -d $DOMAIN -all -recursive -o "$CACHE_DIR/subfinder.txt" &
assetfinder --subs-only $DOMAIN > "$CACHE_DIR/assetfinder.txt" &
amass enum -passive -d $DOMAIN -o "$CACHE_DIR/amass_$DATE.txt" &
bbot -d $DOMAIN -o "$CACHE_DIR/bbot_$DATE.json" &
wait

# Process BBOT output
jq -r '.output.domains[]?' "$CACHE_DIR/bbot_$DATE.json" 2>/dev/null > "$CACHE_DIR/bbot.txt"

# Merge and deduplicate
cat "$CACHE_DIR/"*.txt | sort -u > "$TARGET_DIR/final-subs.txt"

# =====[ 3. DELTA DETECTION ]=====
echo "[*] Checking for new subdomains..."
if [ -f "$TARGET_DIR/previous-subs.txt" ]; then
  comm -13 "$TARGET_DIR/previous-subs.txt" "$TARGET_DIR/final-subs.txt" > "$TARGET_DIR/new-subs.txt"
  if [ -s "$TARGET_DIR/new-subs.txt" ]; then
    echo "[!] New subdomains found:"
    cat "$TARGET_DIR/new-subs.txt"
    notify-send "Recon Update for $DOMAIN" "$(wc -l < "$TARGET_DIR/new-subs.txt") new subdomains"
    # Slack notification
    curl -X POST -H 'Content-type: application/json' --data "{\"text\":\"New subs found for $DOMAIN\"}" $SLACK_WEBHOOK
  fi
fi
cp "$TARGET_DIR/final-subs.txt" "$TARGET_DIR/previous-subs.txt"

# =====[ 4. LIVE HOST DETECTION ]=====
echo "[*] Probing alive hosts..."
httpx -silent -threads 200 -ports 80,443,8080,8000,8888 -l "$TARGET_DIR/final-subs.txt" > "$TARGET_DIR/alive-$DATE.txt"

# =====[ 5. SUBDOMAIN TAKEOVER CHECK ]=====
echo "[*] Checking for subdomain takeovers..."
subzy run --targets "$TARGET_DIR/alive-$DATE.txt" --concurrency 50 --hide_fails --verify_ssl -o "$TARGET_DIR/subzy_$DATE.txt"

# =====[ 6. PORT SCANNING ]=====
echo "[*] Running port scan on alive hosts..."
naabu -list "$TARGET_DIR/alive-$DATE.txt" -top-ports 1000 -o "$TARGET_DIR/naabu_$DATE.txt"

# =====[ 7. WEB CONTENT DISCOVERY ]=====
echo "[*] Scanning for admin panels..."
whatweb -i "$TARGET_DIR/alive-$DATE.txt" --color=never | grep -i "admin\|login\|dashboard" > "$TARGET_DIR/admin_panels_$DATE.txt"

# =====[ 8. PARAMETER DISCOVERY ]=====
echo "[*] Finding URL parameters..."
python3 paramspider.py -d $DOMAIN --level high -o "$TARGET_DIR/params-$DATE.txt"

# =====[ 9. URL COLLECTION ]=====
echo "[*] Gathering URLs with waymore..."
waymore -i $DOMAIN -mode U -oU "$TARGET_DIR/waymore_$DATE.txt"
cat "$TARGET_DIR/waymore_$DATE.txt" | grep "=" | grep "&" | grep "?" > "$TARGET_DIR/urls_with_params.txt"

# =====[ 10. VULNERABILITY SCANNING ]=====
# XSS Scanning
if [ -s "$TARGET_DIR/urls_with_params.txt" ]; then
  echo "[*] Running XSS scan..."
  cat "$TARGET_DIR/urls_with_params.txt" | dalfox pipe -o "$TARGET_DIR/xss_$DATE.txt"
fi

# Nuclei Scanning
echo "[*] Running Nuclei quick scans..."
nuclei -l "$TARGET_DIR/alive-$DATE.txt" -t ~/nuclei-templates/ -severity critical,high -o "$TARGET_DIR/nuclei_$DATE.txt"

# SSRF Check
echo "[*] Starting interactsh for OOB testing..."
interactsh-client -v &> "$TARGET_DIR/interactsh_$DATE.log" &
INTERACTSH_PID=$!
sleep 10  # Give it time to start
# Add your SSRF testing logic here
kill $INTERACTSH_PID 2>/dev/null

# =====[ 11. GITHUB RECON ]=====
if [ -f ~/.gitdorker/config.yaml ]; then
  echo "[*] Running GitHub dorking..."
  python3 ~/tools/GitDorker/GitDorker.py -t $DOMAIN -q "ext:sql|inurl:admin|ext:bak" -o "$TARGET_DIR/gitdork_$DATE.txt"
fi

# =====[ 12. REPORT GENERATION ]=====
echo "[*] Generating report..."
SUMMARY="$TARGET_DIR/scan-summary_$DATE.md"
SUBS=$(wc -l < "$TARGET_DIR/final-subs.txt")
ALIVE=$(wc -l < "$TARGET_DIR/alive-$DATE.txt")
NEW=$(wc -l < "$TARGET_DIR/new-subs.txt" 2>/dev/null || echo "0")

cat <<EOF > "$SUMMARY"
# 🔎 Recon Snapshot – $STAMP

**Domain:** \`$DOMAIN\`  
**Subdomains:** $SUBS  
**Alive:** $ALIVE  
**New Since Last:** $NEW

## 🧰 Tools Used
- [x] Subfinder
- [x] Assetfinder
- [x] Amass
- [x] BBOT
- [x] Httpx
- [x] ParamSpider
- [x] Waymore
- [x] Nuclei
- [x] Naabu
- [x] Subzy
- [ ] Remaining Tools...

## 🚨 Findings Summary
- Potential admin portals: $(grep -i 'admin' "$TARGET_DIR/alive-"*.txt | wc -l)
- Long URLs w/ params: $(wc -l < "$TARGET_DIR/urls_with_params.txt" 2>/dev/null)
- Critical findings: $(grep -c "critical" "$TARGET_DIR/nuclei_$DATE.txt" 2>/dev/null || echo 0)

## ⏱ Runtime: $STAMP
EOF

# =====[ 13. CLEANUP ]=====
echo "[*] Cleaning up old scans..."
ls -td "$TARGET_DIR"/* | tail -n +4 | xargs rm -rf

echo "[✔] Recon complete! Report saved to:"
echo "$SUMMARY"
```

### **Expected Output Structure**

All results will be saved in:

```bash
~/recon/
└── example.com/
    ├── alive-2024-03-15.txt
    ├── admin_panels_2024-03-15.txt
    ├── nuclei_2024-03-15.txt
    ├── scan-summary_2024-03-15.md
    └── cache/
```


### 🚀 What This Script Does (In Plain Terms)

It automates **passive recon** for a given domain (like `target.com`) and includes:

#### 🧠 Smart Triggering

- It checks **Certificate Transparency logs** (via `crt.sh`) to see if there are _new subdomains_.
    
- If nothing new is found, it skips the scan — saving time and resources.
    

#### 🌐 Subdomain Enumeration (Parallel)

- Uses tools like `subfinder`, `assetfinder`, `amass`, and `bbot` to find as many subdomains as possible.
    
- Runs them in **parallel** to speed things up.
    

#### 📁 Caching & Deduplication

- Saves individual tool results in a `cache/` folder.
    
- Combines all subdomains, removes duplicates, and stores them in `final-subs.txt`.
    

#### 🔍 Change Detection

- Compares current results with the last scan to identify **new subdomains**.
    

#### 🌍 Live Host Discovery

- Uses `httpx` to probe each subdomain and find those that are **actually live**.
    

#### 🔎 Parameter & URL Harvesting

- Tools like `paramspider` and `waymore` extract:
    
    - Query parameters (e.g., `?id=`)
        
    - Long, juicy URLs (great for XSS/SSRF)
        
    - These are helpful for deeper vulnerability scans.
        

#### 🧾 Obsidian Integration

- Automatically creates a **Markdown summary** (`.md`) for each scan — great for keeping track inside your Obsidian vault.

### **Example Workflow**

```bash
# 1. Create targets file
echo "example.com" > targets.txt

# 2. First run (full scan)
./recon.sh targets.com

# 3. Check results
ls -la ~/recon/example.com/

# 4. View report
cat ~/recon/example.com/scan-summary_*.md
```


## Run Script check **only once every 12 hours**, you have two main paths:

### 🕰️ Option 1: Use `cron` (Best Practice)

The most reliable way is to **schedule the script with** `cron` to run automatically every 12 hours. Here’s how:

#### 💡 Add to crontab:

**Open your crontab editor:**
```bash
crontab -e
```

#### 🕛 Add this line (runs at midnight and noon):

```js
0 */12 * * * /full/path/to/recon.sh
```

- - Replace `/full/path/to/recon.sh` with the actual path to your script file.
        
    - If your script requires a domain or uses `targets.txt`, make sure it's set up inside the script (which you already did).
        
- **Save and close the editor**
    
    - If using nano: press `Ctrl + O` to save, then `Ctrl + X` to exit.

This way, your script checks every 12 hours **systematically**, and will gracefully exit if nothing has changed (thanks to your `crt.sh` hash check).

 _**If you want to log the script’s output somewhere (very handy for debugging), you can redirect it like this:**

```js
0 */12 * * * /path/to/recon.sh >> ~/recon/recon-cron.log 2>&1
```

Now you’ve got a silent and self-documenting recon drone humming in the background. Want to trigger a Discord or Telegram ping on completion? That’s doable too.

## New things To Add:

```bash
# After subdomain enumeration tools (subfinder/amass/etc), before httpx:

# Merge all discovered subdomains
cat "$CACHE_DIR/"*.txt | sort -u > "$TARGET_DIR/final-subs.txt"

# Add Chaos results
if [ -n "$CHAOS_KEY" ]; then
  echo "[*] Querying ChaosDB..."
  chaos-client -d $DOMAIN -key $CHAOS_KEY -silent >> "$TARGET_DIR/chaos_$DATE.txt"
  cat "$TARGET_DIR/chaos_$DATE.txt" >> "$TARGET_DIR/final-subs.txt"
  sort -u "$TARGET_DIR/final-subs.txt" -o "$TARGET_DIR/final-subs.txt"
fi

```

### **Expected Output**

Chaos results will:

1. Save to `chaos_$DATE.txt`
    
2. Merge with other subdomains
    
3. Get probed by httpx in the live host check phase

**Rate Limiting**: Chaos has API limits (~50 reqs/min)

```bash
chaos-client -d $DOMAIN -key $CHAOS_KEY -delay 2 # 2s delay between requests
```

### **Verification**

Test Chaos standalone first:

```bash
chaos-client -d example.com -key $CHAOS_KEY | httpx -silent
```

_Should return live subdomains from ChaosDB.

### **Troubleshooting**

If you get `403 Forbidden`:

1. Check your API key is valid
    
2. Verify email confirmation
    
3. Check usage limits:
    

```bash
chaos-client -key $CHAOS_KEY -stats
```


**.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo..oOo.oOo.oOo.oOo.**
## Requirements File:

### 📦 Requirements (Tools & Libraries)

Before you run this script, make sure the following tools are installed:

|Tool|Purpose|
|---|---|
|`subfinder`|Subdomain enumeration|
|`assetfinder`|Subdomain discovery|
|`amass`|Passive recon on domains|
|`bbot`|Holistic recon automation|
|`httpx`|Subdomain liveness checking|
|`paramspider`|Parameter extraction|
|`waymore`|Deep URL enumeration via Wayback, etc.|
|`jq`|JSON parsing (for bbot, crt.sh)|
|`curl`|For fetching from crt.sh|
|`notify-send`|Desktop notifications (Linux GUI environments)|

> Optional additions if you want to expand further: > - `dalfox`, `xsstrike`, `nuclei`, `ffuf`, `gospider`, `trufflehog`, etc.



## Simple Automated SDTO Daily Recon:

```bash
#!/bin/bash  
domain=$1  
subs=$(subfinder -d $domain -silent)  
echo "$subs" | httpx -silent | tee live.txt  
subzy run --targets live.txt --hide_fails >> subzy.txt  
nuclei -l live.txt -t takeovers/ >> nuclei-takeovers.txt
```

🧠 Cron it. Monitor it.

Another script, recon.sh:

```bash
#!/bin/bash  
  
# Check if domain is passed  
if [ -z "$1" ]; then  
echo "Usage: $0 target.com"  
exit 1  
fi  
  
DOMAIN=$1  
DATE=$(date +%Y-%m-%d)  
OUTPUT="$DOMAIN-recon-$DATE"  
mkdir -p $OUTPUT  
  
echo "[*] Starting Recon on $DOMAIN..."  
cd $OUTPUT  
  
# 1. Subdomain Enumeration  
echo "[*] Running Subfinder & Amass..."  
subfinder -d $DOMAIN -silent -o subfinder.txt  
amass enum -passive -d $DOMAIN -o amass.txt  
  
cat subfinder.txt amass.txt | sort -u > all_subs.txt  
  
# 2. DNS Brute (Gobuster)  
echo "[*] Running DNS Brute Forcing..."  
gobuster dns -d $DOMAIN -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -o gobuster.txt  
  
cat gobuster.txt | grep Found | awk '{print $2}' >> all_subs.txt  
sort -u all_subs.txt > deduped_subs.txt  
  
# 3. Probing Live Hosts  
echo "[*] Probing Live Subdomains..."  
httpx -l deduped_subs.txt -status-code -title -tech-detect -silent -o live_hosts.txt  
  
# 4. Historical URLs (Wayback + GAU)  
echo "[*] Gathering URLs from Wayback and GAU..."  
cat deduped_subs.txt | gau > gau_urls.txt  
cat deduped_subs.txt | waybackurls >> gau_urls.txt  
sort -u gau_urls.txt > urls.txt  
  
# 5. Vulnerability Scanning (Nuclei)  
echo "[*] Scanning with Nuclei templates..."  
nuclei -l live_hosts.txt -t /usr/share/nuclei-templates/ -o nuclei_findings.txt  
  
# 6. Grep for Sensitive Endpoints  
echo "[*] Searching URLs for interesting patterns..."  
cat urls.txt | grep -E "api|admin|debug|login|upload|secret|key" > juicy_endpoints.txt  
  
# 7. Output Summary  
echo "[+] Recon Summary:"  
echo " - Subdomains Found: $(wc -l < deduped_subs.txt)"  
echo " - Live Hosts: $(wc -l < live_hosts.txt)"  
echo " - Archived URLs: $(wc -l < urls.txt)"  
echo " - Juicy Endpoints: $(wc -l < juicy_endpoints.txt)"  
echo " - Nuclei Findings: $(wc -l < nuclei_findings.txt)"  
  
echo "[*] All data saved in: $OUTPUT"
```

$$_______________________________________________________________________________________$$
## A List Of UseFull OneLiners From TG  (NOT TESTED)

#OneLiners

Finding Hidden Endpoints in js via Regex :  

```bash
grep -Eo '("|\')(/[^"']+)("|\')' *.js | sort -u
```

This will list all directory references found in the JavaScript files



⚡️ Hunt Sensitive Credentials in Burpsuite requests

A good Regex for finding API Keys, Access tokens and sensitive data in requests aggregated in Burpsuite

```vb
#is all in one line:

(?i)((access_key|access_token|admin_pass|admin_user|algolia_admin_key|algolia_api_key|alias_pass|alicloud_access_key|amazon_secret_access_key|amazonaws|ansible_vault_password|aos_key|api_key|api_key_secret|api_key_sid|api_secret|api.googlemaps AIza|apidocs|apikey|apiSecret|app_debug|app_id|app_key|app_log_level|app_secret|appkey|appkeysecret|application_key|appsecret|appspot|auth_token|authorizationToken|authsecret|aws_access|aws_access_key_id|aws_bucket|aws_key|aws_secret|aws_secret_key|aws_token|AWSSecretKey|b2_app_key|bashrc password|bintray_apikey|bintray_gpg_password|bintray_key|bintraykey|bluemix_api_key|bluemix_pass|browserstack_access_key|bucket_password|bucketeer_aws_access_key_id|bucketeer_aws_secret_access_key|built_branch_deploy_key|bx_password|cache_driver|cache_s3_secret_key|cattle_access_key|cattle_secret_key|certificate_password|ci_deploy_password|client_secret|client_zpk_secret_key|clojars_password|cloud_api_key|cloud_watch_aws_access_key|cloudant_password|cloudflare_api_key|cloudflare_auth_key|cloudinary_api_secret|cloudinary_name|codecov_token|config|conn.login|connectionstring|consumer_key|consumer_secret|credentials|cypress_record_key|database_password|database_schema_test|datadog_api_key|datadog_app_key|db_password|db_server|db_username|dbpasswd|dbpassword|dbuser|deploy_password|digitalocean_ssh_key_body|digitalocean_ssh_key_ids|docker_hub_password|docker_key|docker_pass|docker_passwd|docker_password|dockerhub_password|dockerhubpassword|dot-files|dotfiles|droplet_travis_password|dynamoaccesskeyid|dynamosecretaccesskey|elastica_host|elastica_port|elasticsearch_password|encryption_key|encryption_password|env.heroku_api_key|env.sonatype_password|eureka.awssecretkey)[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([0-9a-zA-Z\-_=]{8,64})['\"]

```