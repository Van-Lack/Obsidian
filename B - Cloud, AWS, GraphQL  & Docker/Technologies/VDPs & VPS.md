

> CONTABO

https://contabo.com/en-us/vps/

_Finally, I took the decision and bought it just recently for 1 month. If the experience is good I will continue this same VPS provider for upcoming months or even years as well based on my personal requirements._

![None](https://miro.medium.com/v2/resize:fit:700/1*qspg-IE5lVTtKnm0-mtpGA.png)

4.50 Euro per month (INR 410 approx)

Ram = 4GB , **vCPU Cores = 4** 🤑

![None](https://miro.medium.com/v2/resize:fit:700/1*_WSKGAeUMRYIVD79dR3DBA.png)

			Only EU region is free.

![None](https://miro.medium.com/v2/resize:fit:700/1*ML8gNMJE9lsCMWvkhBESQA.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*wRGu7UUCk98VFxz8ZDMnfw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*UuAmTutd_pjsPHRcFmdP1Q.png)

Password Requirements

![None](https://miro.medium.com/v2/resize:fit:700/1*0TTpE6wyZD-q7P4LGLxk0w.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*M05NUVuVuyCDzjKom8INiw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*aLfC2158MEjO_XUzMHvAmA.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*Z6ctZdhQd4rddzg7RhthrQ.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*G43gin46FQ7CdABf65cJfw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*MFs5-SCsfVK3RWxTjrpU4Q.png)

Here, I selected `I protect Data by myself` because as of now, I am still at the beginner learning stages and bought it for only 1 month to explore stuff.

![None](https://miro.medium.com/v2/resize:fit:700/1*GHXfscgMQbzbmLtn3bt1-w.png)

**Final price: 5.31 EUR (including 18% VAT) ~ 488 INR (approx)**

![None](https://miro.medium.com/v2/resize:fit:700/1*unQqXH_Pq-3RdgqnTdRmRA.png)

During the order processing, it is asked from the customer for what purpose you are using this VPS.

👉I mentioned clearly for `Bug Bounty & Pentesting`

![None](https://miro.medium.com/v2/resize:fit:700/1*6-Y1BOs3YWvrL1LbaEtnGw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*1_FXCAJiHWNitxzCSZ87_A.png)

All necessary login credentials and how to proceed are provided via email, using it you can login to your Customer Control Panel, and take note and backup for all necessary things.

![None](https://miro.medium.com/v2/resize:fit:700/1*VVILHbAFyJYQ51X7YEDNBA.png)

> SSH (From Linux)

> ssh username@x.x.x.x

![None](https://miro.medium.com/v2/resize:fit:700/1*fBd4dMFVvhBRACUX7gTG_Q.png)

> SSH Login (From Windows)

Download: https://www.putty.org/

![None](https://miro.medium.com/v2/resize:fit:700/1*rVSI6h8D_9B1r7SigReCLw.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*vORIrh9hMX8NBte9Mj-GsQ.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*MX9MTDk3eUy1yD_XXE1EXA.png)

> 🚨**SSH Login via password is not a good secure practice. In upcoming articles, I will show how to generate SSH key pairs and login using SSH private keys. 

```bash
uname -ar
```

![None](https://miro.medium.com/v2/resize:fit:700/1*GWOrMh2_Soh4TO8CcV0nZQ.png)

_Free space_

```bash
df -h
```

![None](https://miro.medium.com/v2/resize:fit:700/1*ol7ZSXjpKX8WR7ny9vHnHA.png)

> **Snapshots**

1. Go to [https://my.contabo.com](https://my.contabo.com/)
2. Navigate to **VPS Control** tab section

![None](https://miro.medium.com/v2/resize:fit:700/1*h5hk3kk-2egZGlFP7TQz9w.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*45F4AlUvV9wY5auUnBLaEg.png)

Click on the `Snapshots` icon image. Then click on `Create Snapshot` Enter any name and click on `OK`

![None](https://miro.medium.com/v2/resize:fit:700/1*zfIYz3STeXfoY9Qh61THPQ.png)

_But for the 1 month that I have with 4GB Ram+4 vCPU cores, only 1 free snapshot is provided. I took a snapshot at the beginning itself so that if anything goes wrong during setup and installation of tools, I can roll back or revert back safely to the initial state._

> **SSH Login from Windows via Bitvise SSH Client**

![None](https://miro.medium.com/v2/resize:fit:700/1*QNGvlVc02iYCW_4rjbAVMA.png)

Instead of Putty SSH client, I am using Bitvise SSH client to change the font size easily and if you just select the text in terminal via mouse, it will be copied to clipboard and for pasting from host system to terminal simply right-click (Just mentioned for absolute beginners )🙂

Enter IP and port > click `Login` > enter password.

Then click on `New terminal console`

![None](https://miro.medium.com/v2/resize:fit:700/1*eXv-LYsxIrFnTMgTPiX19A.png)


> apt update 
> apt install pipx

**install waymore example:**

```
pipx install waymore
mv /root/.local/bin/waymore /usr/bin/
```

_Here, instead of_ _`pip`_ _or_ _`pip3`__, I prefer to use_ _`pipx`_ _to avoid dependency conflicts._

Ask to ChatGPT: `Tell 5 reasons why pipx better than pip or pip3`

> pipx ensurepath


![None](https://miro.medium.com/v2/resize:fit:700/1*J4I4REGBr5J5q4tHBGsiqg.png)

> `apt install unzip`

Double-check manually for each command that involves critical operations like `rm`

**2️⃣ Subfinder**

```bash
cd /tmp wget https://github.com/projectdiscovery/subfinder/releases/download/v2.7.0/subfinder_2.7.0_linux_amd64.zip unzip subfinder_2.7.0_linux_amd64.zip rm subfinder_2.7.0_linux_amd64.zip mv /tmp/subfinder /usr/bin/ subfinder -h
```

**3️⃣ URO**

- declutters url lists for crawling/pentesting

```
pipx install uro 
pipx ensurepath
```

> apt install plocate

![None](https://miro.medium.com/v2/resize:fit:700/1*AyX9auj8t81xDpSZAylUwg.png)

I don't want to open a new terminal or re-login, so I am moving it directly to `/usr/bin/`

> `mv /root/.local/bin/uro /usr/bin/ uro -h`

**4️⃣ Nuclei**

```
cd /tmp wget https://github.com/projectdiscovery/nuclei/releases/download/v3.3.9/nuclei_3.3.9_linux_amd64.zip unzip nuclei_3.3.9_linux_amd64.zip
```

![None](https://miro.medium.com/v2/resize:fit:700/1*bJU55Q-vJoZfN9XFvrTOgg.png)


```
`rm *.md
rm nuclei_3.3.9_linux_amd64.zip
mv nuclei /usr/bin/`
```

  
![None](https://miro.medium.com/v2/resize:fit:700/1*J3Xd1PwtjnL6XSs5ySJeJQ.png)

>`cd ~ 
> `mkdir bughunt` 
>`cd bughunt`

![None](https://miro.medium.com/v2/resize:fit:700/1*TW_K-i-gxHW1Qj7LIG9uqg.png)

_Installing the templates_

>`nuclei -ut`

![None](https://miro.medium.com/v2/resize:fit:700/1*FfNzab441bDK8WT-dTzU4w.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*ltqqdbbEcB-YIdjFh5BAyA.png)

**5️⃣ KATANA**


```
cd /tmp wget https://github.com/projectdiscovery/katana/releases/download/v1.1.2/katana_1.1.2_linux_amd64.zip

unzip katana_1.1.2_linux_amd64.zip
rm *.md 
mv katana /usr/bin/ 
rm katana_1.1.2_linux_amd64.zip 
katana -h
```



Do the same with: **6️⃣HTTPX** **7️⃣ Arjun (Hunting hidden parameters)** **8️⃣ NOTIFY**

```
cd /tmp wget https://github.com/projectdiscovery/notify/releases/download/v1.0.7/notify_1.0.7_linux_amd64.zip
unzip notify_1.0.7_linux_amd64.zip 
rm *.md 
rm notify_1.0.7_linux_amd64.zip
mv notify /usr/bin/
```

**9️⃣ XSS VIBES**

> git clone https://github.com/faiyazahmad07/xss_vibes.git

```bash
cd xss_vibes/
ls -la
```

![None](https://miro.medium.com/v2/resize:fit:700/1*3ylu_ehrsTJCm1It8qzppQ.png)

> `pipx install wafw00f`

![None](https://miro.medium.com/v2/resize:fit:700/1*gUgwnhRsrVvdVVW4KzKvzg.png)

```
 mv /root/.local/bin/wafw00f /usr/bin/
  apt install wafw00f -y
   cd xss_vibes
    mkdir urls
```

![None](https://miro.medium.com/v2/resize:fit:700/1*H_q70sQptZ_VBjxKp9sEbQ.png)

Fixing the error

`nano main.py`

Insert `r`

![None](https://miro.medium.com/v2/resize:fit:700/1*AYfm3rnoY9o1Wt4jkr3SrA.png)

`python3 main.py -f urls/1.txt -t 5`

![None](https://miro.medium.com/v2/resize:fit:700/1*PiBLe9Pbrpjn6pmKovSedA.png)


----

![[Pasted image 20250418134612.png]]

Awesome [#Cybersecurity](tg://search_hashtag?hashtag=Cybersecurity) Datasets - URLs & Domain Names - Network traffic - Malware - WebApps - Email - Passwords - Honeypots and more. [https://github.com/shramos/Awesome-Cybersecurity-Datasets](https://github.com/shramos/Awesome-Cybersecurity-Datasets)

---

# Free VPS Setup?

**So What Is A VPS ?**

Imagine you have a powerful computer that you can split into smaller parts. Each part acts like its own mini-computer with its own space and resources. This is similar to how a Virtual Private Server (VPS) works.

#### How Can it Help With Bug Bounties?

A VPS is like having a special remote computer that you can use just for these bug bounty hunts. It's not a physical computer, but it works like one. With a VPS, you can set up tools and programs to automatically search for security issues in websites and apps. This lets you hunt for bugs even when you're not using your personal computer, and it can run these tasks 24/7.

#### What are some of the platforms that provide VPS service ?

1. AWS EC2
2. DigitalOcean
3. Linode
4. Vultr
5. Google Cloud Platform (GCP)
6. Microsoft Azure
7. Oracle Cloud Infrastructure (OCI)
8. UpCloud
9. Hetzner Cloud

#### Are They Free?

These platforms provide a starting credit that is valid for 3–4 months after which you have to pay.

Today we will be using Digital Ocean because they provide a $200 credit for students which is valid for 1 whole year.

So all you need is a student email to get a free VPS and even if you can't get one don't worry you can use [This Referral Link](https://m.do.co/c/b5c80ea3da15) To get free $200 credit

### Registration

So first you need to create an account of course, if you are a student you can link your account with GitHub to claim the $200

If you are not a student you can use [https://m.do.co/c/b5c80ea3da15](https://m.do.co/c/b5c80ea3da15) this referral link to create an account and get $200 credit and get started with the automation!

Once you are registered click on the Create button on the top right corner and select Droplets

![None](https://miro.medium.com/v2/resize:fit:700/1*83PNJRBW9psU6jQDTpVcJw.png)

**Then It asks you to choose a region, choose the one you live closest to.**

![None](https://miro.medium.com/v2/resize:fit:700/1*JqdDjAvE7Z4cwle8VkOt0g.png)

**Now Select an image, I am going to use ubuntu**

![None](https://miro.medium.com/v2/resize:fit:700/1*4SbpDL3SmaiuHwdwlZWVcw.png)

Now while selecting the plan you need to choose wisely, I will advice to start with the basic plan to get to know your needs and how much you use it.

![None](https://miro.medium.com/v2/resize:fit:700/1*kkHOtTPyXswmcTWEUTjsWg.png)

**Now in the authentication method I prefer password but again its your own choice.**

![None](https://miro.medium.com/v2/resize:fit:700/1*yUxegbRAEKuYmAAfuESVHA.png)

Now you can create a password and make sure to remember it!

After finishing you can click the create droplet button to create your own box!

Once it's Done, you would see your box's IP on the dashboard.

### Connect to your VPS

To connect to your VPS, you can access via SSH. If you are using Mac or Unix-like system, just simply put in below command

```bash
ssh root@<your-vps-ip>
```

If you are using Windows, you might need to download putty to access via SSH.

After logging in, I advice to create a new user so that you don't have to run everything as root.

### How to utilize it in Bug Bounties?

You can use the screen command to keep the processes running even when your system is not turned on.

`screen` is a terminal multiplexer utility in Linux that allows you to create and manage multiple terminal sessions within a single terminal window or SSH session. **It's particularly useful when working remotely, managing long-running processes, or handling multiple tasks simultaneously.**

```bash
sudo apt-get install screen
```

To create a new screen:

```bash
screen -S new-screen-name
```

Then You can run the tool you want and exit the screen, the tool will keep running .

To join the screen:

```css
screen -r name
```

To list all the screens:

```bash
screen -ls
```

To exit a screen use ctrl + A + D

---
## Private VDPs for Testing:

```
aasaan.app
agentnoon.com
agentiq.com
altera.com
amnic.com
archlabs.com
arthrex.com
ashbyhq.com
aspire.io
attio.com
athenian.com
boardgamegeek.com
branchapp.in
browan.com
bugsnag.com
bvnk.com
cadence.care
camber.health
cartfulsolutions.com
certara.com
choco.com
chronosphere.io
cirrus-runners.app
clerk.com
classcreator.io
coderabbit.ai
colabra.ai
copiawealthstudios.com
csx.com
dyson.co.uk
drugs.com
edusynch.com
entri.com
eway.io
findigs.com
formance.com
futurefund.com
geckoboard.com
getalembic.com
getcodeshealth.com
getpicnic.com
globalgiving.org
gov2biz.com
herondata.io
http.net
hypernode.com
idelic.com
intellihinc.com
intellicredit.com
isolta.com
jobylon.com
jobreg.no
justappraised.com
kentico.com
kustomer.com
lemonadelxp.com
lisainsurtech.com
lilt.com
loxo.co
m.io
messagegears.com
muckrack.com
mytrove.co.nz
nadineintl.com
narmi.com
nextdlp.com
nxtport.com
ouster.com
parliament.nz
privy.com
ps16.co.uk
ptm.services
qatalog.com
rebuyengine.com
reconcept.nl
renofi.com
rentalsunited.com
reverscore.com
route.com
segmed.ai
signup.com
sigmacomputing.com
simplerisk.com
simbase.com
sleuth.io
sourceful.com
spyfu.com
sweethawk.com
tenjin.com
trackabout.com
transip.eu
trelica.com
vanta.com
vendr.com
wealthy.in
weglot.com
wisetack.com
workera.ai
ymeadows.com
yokahu.co

```