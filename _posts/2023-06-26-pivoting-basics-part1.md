---
layout: post
title:  "Pivoting Basics - Part 1"
author: nanobyte
date:   2023-06-26
description: Part 1 of my pivoting basics
tags: HTB OSCP Pivoting
---

<h3>Pivoting!?</h3>

What the h311 is pivoting? In short, it is used when you have a foothold onto a compromised machine, and wish to access a network that is otherwise not publically facing, such as a second internal network interface. For an example, let's consider the following. Let's imagine you have a web server, call it WEB01, with a public IP of 10.10.110.10, which you have compromised and have access to. If you wish to pivot to an internal network, 172.18.1.0/24, pivoting on the WEB01 box would provide you access to the internal  172.18.1.0/24 to your Kali Linux machine.

For the following scenario, and for the rest of my walkthroughs, the following will remain true:

+ Kali Linux (Attacking Machine): 10.10.15.100
+ First Pivot Machine (Public IP): 10.10.110.10
+ First Network: 172.18.1.0/24
+ Second Network: 172.18.2.0/24
+ Second Pivot Machine: 172.18.2.15
+ Third Network: 172.18.3.0/24
+ Third Pivot Machine: 172.18.3.20

To begin this tutorial, we have the ability to login to WEB01 with SSH credentials. It can be a user, or admin/root, it does not matter. We wish to access DC01, located at 172.18.2.5:

<img src="/images/posts/pivoting/Pivoting_Initial.PNG" alt="pivoting-initial" width="500"/>

<h3>SSHuttle</h3>

To start, imagine you want to pivot from WEB01, from 172.18.1.5 interface, to enumerate DC01 on 172.18.1.5. As most HTB machines use SSH on their first foothold, I will assume SSH id_rsa key is available. The easiest tool, and my goto, is SSHuttle. It is baked into Kali Linux, but if you are not using Kali, it can be installed easily. [Here is the GitHub for SShuttle](https://github.com/sshuttle/sshuttle).

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

If all is correct, we will receive 'c : Connected to Server':

<img src="/images/posts/pivoting/SSHuttle-Connected.PNG" alt="pivoting-sshuttle" width="500"/>

We would now have access to the first subnet, 172.18.1.0/24, and also where DC01's interface 172.18.1.5 resides. We could now continue with enumeration of the new network. Our network diagram woulod appear like:

<img src="/images/posts/pivoting/Pivoting_SsHuttle.PNG" alt="pivoting-sshuttle" width="500"/>

And that's it for Part 1! In Part 2, I will continue on with using Chisel, and how to pivot further into environments.
