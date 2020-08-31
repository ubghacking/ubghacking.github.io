---
layout: post
title:  "Powershell For Penetration Testers Downloading And Exfiltrating"
author: nanobyte
date:   2020-08-31
description: PowerShell for Penetration Testing, and how to download files to a victim machine, and exfiltrate data from the victim machine
tags: PowerShell python
---

While completing Hack the Box's Sniper machine, I came across the need to have to switch to a different user. Credentials were discovered in a db.php file, and there was no way to login as that user using external tools such as Evil-WinRM. However, thanks to PowerShell, there is a simple way to quickly write a script to store these credentials in variables, and enter a new PowerShell session to that user. To begin, the credentials discovered were Chris:36mEAhz/B8xQ~2VM. To start out, the first variables I needed to set were the username and password:

{% highlight bash linenos %}

{% endhighlight %}
