---
layout: post
title:  "Pivoting Basics - Part 1"
author: nanobyte
date:   2023-06-26
description: Part 1 of my pivoting basics
tags: HTB OSCP Pivoting
---

What the f*** is pivoting? In short, it is used when you have a foothold onto a compromised machine, and wish to access a network that is otherwise not publically facing, such as a second internal network interface. For an example, let's consider the following. Let's imagine you have a web server, call it WEB01, with a public IP of 10.10.110.10, which you have compromised and have access to. If you wish to pivot to an internal network, 172.18.1.0/24, pivoting on the WEB01 box would provide you access to the internal  172.18.1.0/24 to your Kali Linux machine.

For the following scenario, and for the rest of my walkthroughs, the following will remain true:

Kali Linux (Attacking Machine): 10.10.15.100
First Pivot Machine (Public IP): 10.10.110.10
First Network: 172.18.1.0/24
Second Network: 172.18.2.0/24
Second Pivot Machine: 172.18.2.5
Third Pivot Network: 172.18.3.0/24

To begin this tutorial, we have the ability to login to WEB01 with SSH credentials. It can be a user, or admin/root, it does not matter. We wish to access DC01, located at 172.18.2.5:

![pivoting-initial](/images/posts/pivoting/Pivoting_Initial.PNG)

<h3>Title</h3>

First, I am creating my web root within `/opt/gitbook-wiki`, as the temporary directory. Once I am done, this will reside within `/var/www/github-wiki` on my CentOS box.\

{% highlight bash linenos %}
Code
{% endhighlight %}
