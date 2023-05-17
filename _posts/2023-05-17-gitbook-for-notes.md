---
layout: post
title:  "GitBook for Notes"
author: nanobyte
date:   2023-05-17
description: How to use a local GitBook repository to host your notes!
tags: Notetaking GitBook
---

I prefer to use GitBook for my notes. I really the the structure, compared to others like OneNote. The other benefit I find to GitBook is I absolutely enjoy the search functionality. I try to keep my notes in the best hierarchy and logical order which I can - but since I document everything, it can get rather cluttered.

I was using GitBook online, until I discovered I could also host it locally. Setup was rather pain free, as it uses Node.js to provide markdown files as content. Since I already had a web server in my homelab I could use, I will not cover how to stand one up. I initially set this up on my Kali box, but plan to migrate this to my CentOS Stream 9 web server when I am done.
Come back for the remainder of this guide soon!

First, I am creating my web root within `/opt/gitbook-wiki`, as the temporary directory. Once I am done, this will reside within `/var/www/github-wiki` on my CentOS box.

Once in your working directory, install Node.js. Node.js is a runtime environment and library for JavaScript. We are installing Node.js to use Node Package Manager, or NPM, to install additional packages for Node.js.

{% highlight bash linenos %} 
sudo apt install nodejs
{% endhighlight %}

Once Node.js is installed, install gitbook-cli:

{% highlight bash linenos %} 
npm install gitbook-cli -g
{% endhighlight %}

Once installed, if you run the command `gitbook init` to initialize, you will receive an error. This is because GitBook is no longer maintained as a project, and the newer `graceful-fs` packages have broken the ability to run as is. In `nano /usr/local/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/graceful-fs/polyfills.js`, comment out the following lines:

{% highlight bash linenos %} 
# Comment out:
//  fs.stat = statFix(fs.stat)
//  fs.fstat = statFix(fs.fstat)
//  fs.lstat = statFix(fs.lstat)
{% endhighlight %}

Now, we are able to init and install GitBook:

{% highlight bash linenos %} 
gitbook install
{% endhighlight %}

At this point, we would be able to serve GItBook. Once we run this, we should have a GitBook server listening on port 4000, by default:

{% highlight bash linenos %} 
gitbook serve

...
Starting server ...
Serving book on http://localhost:4000
{% endhighlight %}

