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

First, you will need Chisel. Head to the GitHub, [Here is the GitHub for Chisel](https://github.com/jpillora/chisel).

{% highlight bash linenos %}
sshuttle -r <username>@<ip_addr> <remote_network> -e 'ssh -i id_rsa_file'
{% endhighlight %}

The above command expects two parameters, -r and -e:
+ -r : (also --remote) is the remote network you want to access
+ -e : (also --ssh-cmd) is the ssh command to run. In this case, because we have an id_rsa file of the victim user, we use the basic ssh's -i flag

Our command would thus look like:

{% highlight bash linenos %}
sshuttle -r root@10.10.110.10 172.18.1.0/24 -e 'ssh -i id_rsa.txt'
{% endhighlight %}

We would now have access to the first subnet, 172.18.1.0/24, and also where DC01's interface 172.18.1.5 resides. We could now continue with enumeration of the new network. Our network diagram woulod appear like:

<img src="/images/posts/pivoting/Pivoting_SShuttle.PNG" alt="pivoting-sshuttle" width="500"/>

And that's it for Part 1! In Part 2, I will continue on with using Chisel, and how to pivot further into environments.
