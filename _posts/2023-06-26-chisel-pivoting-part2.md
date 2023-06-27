---
layout: post
title:  "Pivoting with Chisel - Part 2"
author: nanobyte
date:   2023-06-26
description: Part 2 of my pivoting basics, and how to use Chisel
tags: HTB OSCP Pivoting
---

This is a continuation of my Pivoting Basics, and Part 2, and how to use Chisel to pivot. Part 1 dove into what pivoting was, and how to pivot with SSHuttle. In this post, I will continue from where we left off. As a reminder, here is our scenario:

For the following scenario, and for the rest of my walkthroughs, the following will remain true:

Kali Linux (Attacking Machine): 10.10.15.100
First Pivot Machine (Public IP): 10.10.110.10
First Network: 172.18.1.0/24
Second Network: 172.18.2.0/24
Second Pivot Machine: 172.18.2.15
Third Network: 172.18.3.0/24
Third Pivot Machine: 172.18.3.20

This is where we are picking up, with an established SSHuttle session to DC01. At this point, I will assume you have comrpomised the domain, and have discovered DC02, available at 172.18.2.15, and require access to 172.18.2.0/24:

<img src="/images/posts/pivoting/Pivoting_Part2_Initial.PNG" alt="pivoting-part2-initial" width="500"/>

<h3>Chisel</h3>

First, you will need Chisel. Head to the GitHub, and download Chisel from Releases for the architecrture of the victim, and for your Kali. I will assume that the victim is Windows. [Here is the GitHub for Chisel](https://github.com/jpillora/chisel).

Before we run anything, we first need to modify /etc/proxychains4.conf file on our Kali Linux. Add the following to the bottom of the file:

{% highlight bash linenos %}
socks5 127.0.0.1 1080
{% endhighlight %}

The above line adds a SOCKS5 proxy, on localhost (127.0.0.1) at port 1080. Once complete, we can stasrt our chisel listener on our Kali machine. In a terminal, run the following command:

{% highlight bash linenos %}
./chisel server -p 80 --reverse
{% endhighlight %}

The above starts chisel, running it as a server, with the following parameters:

+ -p : listening on port 80. Why port 80? Because often unrecognized or unusual ports may be blocked by firewall rules.
+ --reverse 


