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

<h3>Installing GitBook</h3>

First, I am creating my web root within `/opt/gitbook-wiki`, as the temporary directory. Once I am done, this will reside within `/var/www/github-wiki` on my CentOS box.

Once in your working directory, install Node.js. Node.js is a runtime environment and library for JavaScript. We are installing Node.js to use Node Package Manager, or NPM, to install additional packages for Node.js.

{% highlight bash linenos %}
sudo apt install nodejs
{% endhighlight %}

Once Node.js is installed, install gitbook-cli:

{% highlight bash linenos %}
npm install gitbook-cli -g
{% endhighlight %}

Once installed, if you run the command `gitbook init` to initialize, you will receive an error. This is because GitBook is no longer maintained as a project, and the newer `graceful-fs` packages have broken the ability to run as is. In `/usr/local/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/graceful-fs/polyfills.js`, comment out the following lines:

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

At this point, we would be able to serve GitBook. Once we run this, we should have a GitBook server listening on port 4000, by default:

{% highlight bash linenos %}
gitbook serve

...
Starting server ...
Serving book on http://localhost:4000
{% endhighlight %}

<h3>Content Structure</h3>

GitBook creates a structure for content, by default, based off directory structure it finds in the root directory. You can also create your own, by making a `SUMMARY.md` file in your root directory, in my case, `/opt/gitbook-wiki`. The structure of this file is very easy. Each "Chapter" will not be indented, and each set of "Pages" under that chapter will be indented. I made a `wiki` directory in my root directory, to manage my content.

The file should have the title of the pge in square brackets, and the relative path to the markdown file in parenthesis, indented with a tab:

{% highlight bash linenos %}
# Summary
* [Welcome](README.md)
* [Web Hacking](wiki/web-hacking/README.md)
        * [checklist](wiki/web-hacking/checklist.md)
* [RevShell](wiki/revshell/README.md)
        * [Windows](wiki/revshell/windows.md)
        * [Linux](wiki/revshell/linux.md)
* [Cheat Sheets](wiki/cheat-sheets/README.md)
{% endhighlight %}

This will create your structure that are linked in the left sidebar for your wiki.

<h3>Customization</h3>

I did not heavily customize my GitBook, but there is no default way to collapse chapters by default. To fix this, I installed a plugin, "collapsible-chapters". I also wanted to use tags in my GitBook wiki. This is completed by creating a `book.json` file in the root of your GitBook directory, in my case, `/opt/gitbook-wiki`. To use the `tags` plugin, you will need to make an empty `tags.md` file in your root directory. My `book.json` looks like the following:

{% highlight bash linenos %}
{
    "title": "Nan0Byt3's Notes",
    "plugins": ["collapsible-chapters",
     "tags"]
}
{% endhighlight %}
