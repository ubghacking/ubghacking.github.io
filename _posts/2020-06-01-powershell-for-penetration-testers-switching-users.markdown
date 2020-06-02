---
layout: post
title:  "Powershell For Penetration Testers Switching Users"
author: nanobyte
date:   2020-06-01
description: PowerShell for Penetration Testing, and how to use credentials to switch users
tags: PowerShell
---

While completing Hack the Box's Sniper machine, I came across the need to have to switch to a different user. Credentials were discovered in a db.php file, and there was no way to login as that user. However, thanks to PowerShell, there are several quick and easy ways to 

{% highlight bash linenos %}
powershell -ExecutionPolicy Bypass -Command "whoami"
{% endhighlight %}
