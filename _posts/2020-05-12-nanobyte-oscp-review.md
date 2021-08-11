---
layout: post
title:  "Nan0byt3s OSCP Review"
author: nanobyte
date:   2020-05-12
description: Post Exam Writeup for my Offensive Security Certified Professional (OSCP)
tags: OffSec OSCP cert-writeup
---

1. Pre-Exam Talk
2. Preparation
3. The Labs
4. Buffer Overflow Preparation
5. The Exam
6. Top-Five Takeaways
7. End Rant

<H2>1. Pre-Exam Talk</H2>

Hello, and thanks for taking the time to read my Offensive Security Certified Professional (OSCP) certificate review! I first got eager to earn my OSCP when a close friend earned his and told me about what it meant to hold the certification. I recall thinking what a "badass" he was, being able to hack into computers and gain administrative access to computers. I was computer savvy, but not fluent with Kali or the terminal. But nonetheless, I wanted to earn this certificate, more than anyone knew. I say this to give those with little command-line-fu the courage to enroll.

<H2>2. Preparation</H2>

I began my studies when I was introduced to <a href="https://www.hackthebox.eu/" target="_blank">Hack the Box</a>. For those that do not know what Hack the Box (HTB) is, it is a network of machines that are vulnerable. You receive a VPN connection pack that you use to connect to the network for these vulnerable machines. I began the overwhelming process of learning Linux, all while learning how to perform the attacks by hand I learned from my Certified Ethical Hacker certificate. Sure, I KNEW what a SQL Injection attack was, but was I able to perform one on my own? That's what SQLMap was for! But these automated tools are not allowed on the OSCP exam. I worked my way through machines, starting with the easy rated boxes and working my way up from Linux to Windows. In those early days of HTB, forum checks were numerous, and calls for aide were constant.

One thing I started early was keeping rudimentary notes of my accomplishments, mostly walkthroughs, so I had my own reference of the tools I used. This helped me, I knew what the syntax and output of the tools looked like. These writeups are still in my notes, and I even used them on my exam. During this time, I found that I was creeping away from SQLMap to perform SQLi attacks manully, taking the time to research what I was doling. I knew MetaSploit was limited on the OSCP exam, so I actively attempted to exploit anything I could on my own. I want to say that this was my biggest success. I learned how to use the tools I needed to on the exam from the start. Not saying I have NEVER used MetaSploit, but just avoided it where I could. 

The nights I was not on HTB, I was reading books such as <i>Penetration Testing</i> by Georgia Weidman, <i>The Web Application Hacker's Handbook</i> by Dafydd Stuttard and Marcus Pinto and <i>Hacking: The Art of Exploitation</i> by Jon Erickson just to name my top three favorites. These got me into a methodology, and not just wildly and frantically looking for vulnerabilities. Reading became just as important as HTB. The list goes on of other books I used for reference, or started and not finished... I will get back to those, one day. You are not forgotten, <i>Black Hat Python<i>!

I also tackled the OSCP like HTB machines (<a href="https://www.reddit.com/r/oscp/comments/alf4nf/oscp_like_boxes_on_hack_the_box_credit_tj_null_on/" target="_blank">https://www.reddit.com/r/oscp/comments/alf4nf/oscp_like_boxes_on_hack_the_box_credit_tj_null_on/</a>). For those that are new to HTB, or new to the whole hacking world, this is where I recommend you start. If I would have tackled this list first, and not gone for HTB points, I would have been better off. These OSCP boxes all have write-ups easily searchable, and because of this when you get stuck there are multiple write-ups to get you through. And multiple ways other peoiple have hacked these machines. Doing this would have gotten me used to the tools quicker. These machines really were what got me prepared for the labs the most. I completed the list of Linux machines, then moved onto Windows. All the while, I used several different platforms for my notes, testing which one fit me the best.

Notes. Notes were essential. I constantly referred to my notes, which I have gathered over these last nine months. I tried several different platforms. I tried Cherry Tree on Linux first, and honestly really liked it. Due to not having a single VM (no homelab available yet, and I used two different PC's) I moved to Google Docs, huge mistake. Ultimately what I used in the end was GitBook. I highly recommend GitBook for its functionality and searching within GitBook is insanely fast and efficient. When I needed a reference for a tool, let's say hydra, typing in 'hydra' into the search bar not only displayed my notes in 'Web Attacks' for what the tool was, but I found practical examples from machines in HTB where I had used it.

After my lab time was done, I needed something to work on in the gap before my exam. I used <a href="https://tryhackme.com/" target="_blank">TryHackMe</a>, a similar tool to HTB. I completed TryHackMe's OSCP learning path. At this point, I was eager and ready to sign up. I had read a few books, I was an Elite Hacker on HTB, and did the OSCP like boxes. I signed up and enrolled to begin 30 days of labs to start.

<H2>3. The Labs</H2>

The OSCP labs and material were absolutely amazing. When the labs were unlocked, to keep myself on pace, I booked my exam the week after my labs ended so I would not put this off! I was enrolled into the 2020 updated course, and the new material was, honestly, great. I took the first 7 days, and watched all the videos, referencing the PDF along the way. To be honest, I found that the PDF goes over the videos verbatim, so I did not read the PDF cover to cover, but used as a reference. Anything and everything the videos covered, I put into my GitBook notes. This led to a rapid increase in size but I now had everything I needed in there as reference, and it was quick and easy to find it all when required.

Because I purchased 30 days of lab time, I did not complete the exercises and submit a lab report. For those preparing for the OSCP, in looking back, I would recommend looking at this option. I wish I would have done the lab report, even though it is extensive. The lab report requires you have a write-up of ten machines you "pwned", as well as all the lab exercises in the PDF. The lab exercises are what scared me away since I wanted to focus on playing in the labs. I was able to root 20 machines in the lab, all MOSTLY on my own. That's all I "pwned" in my lab time. For the times I was stuck, I turned to the forums, or fellow hackers I know that hold OSCP. The Offensive Security forums were very casual and useful. They were a great place to turn to for suggestions, post questions and even request nudges when stuck, similar to the HTB forums. I always used this a last-ditch effort, always trying and covering everything I could think of before reaching out for help.

While in the labs, I started building my own automation tools to help me quicjly enumerate. These crude scripts took a list of IP's from a text file, and fed them into a script one liner that output it's findings into a directory of the IP for safe keeping and reporting. Here is an example of one of my scripts, gobuster_enum.sh:

{% highlight python linenos %}
#!/bin/bash
for i in $(cat < "$1"); do
        mkdir -p $i
        sh -c "gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -u http://$i -o $i/gobuster.$i"
done
{% endhighlight %}

I could run these scripts with:

{% highlight python linenos %}
./gobuster_enum.sh IP.txt
{% endhighlight %}

I used this format, and made scripts for dirb, gobuster, nikto, and nmap scans.

Finally, before I knew it, the exam was the next day. Although I was frantic, I called it an early night and turned my computer off before dinner. I understood that anything I would do the night before, would not prep me any more. I spent the night with my family, ate a good dinner, and was in bed 9 hours before my exam start time. Get a good nights rest before your exam if ytou can!

<H2>4. Buffer Overflow Preparation</H2>

The buffer overflow scared me when I started this learning path. The idea of learning how memory worked, to create a malicious exploit to crash a program, that sent a reverse connection back for system access!? It is not bad. I looked at Justin Steven's dostackbufferoverflowgood (<a href="https://github.com/justinsteven/dostackbufferoverflowgood" target="_blank">https://github.com/justinsteven/dostackbufferoverflowgood</a>) which is an awesome tutorial. I started by spinning up a Windows 7 "exploit" machine. I installed Immunity, IDA, DNSpy, all the tools I could think of for exploit creation. I did the dostackbufferoverflowgood, and moved on to other executables. I found a vulnerable version of minishare, and exploited that, along with vulnserver. Do all you can, practice!

<H2>5. The Exam</H2>

I kicked off my exam by enumerating with the script examples above. Once scanning kicked off, I got to work on the Buffer Overflow. I quickly got a working exploit in the first hour and a half. Slow to what others get it, but for this n00b I was proud of that time frame. I then went for the next box, an easier one, where I quickly rooted it. This was two hours in, and I had already received half the exam points needed. Halfway there ...

I then got to work on the remaining three boxes. All my scans were complete at this point, and so I began working on two machines. After some time and frustration I stepped away and ate lunch. I spent 30 minutes with my family and cooled off my jets. Breaks are essential, or were for me!

When I returned, I took a peek at the harder machine. After reviewing the scan results, I found the way in through some light tinkering. With that, I performed some privilege escalation scans, and looked at another box. And, I found something I missed on my initial enumeration, that led me to finally getting in. I got onto the machine as a low-level user. I found something else interesting, and quickly leveled up.

At this point, it was dinner time, where I took another break. I walked away for another 30 minutes and took some time with my family. I must honestly say, the small breaks and stepping away, not getting caught in the rabbit holes, checked my sanity! Do not get lost on the rabbit holes, take a step back, and look at the problem. When I returned, I popped a shell on another box, and got the final find of my exam.

That is where it stayed, I was never able to privilege escalate on the final two machines. I was able to get onto all five, all without MetaSploit. I worked until the final five minutes of my exam, living off energy drinks and an hour and a half of sleep. But, at the end of my time, I took a small nap and got to work on my exam report. I finished by the evening, and 36 hours after my exam start time, my report was officially submitted...

<H2>6. Top Five Takeaways</H2>

Here are my top five takeways:

1. There is ALWAYS a way in, you just need to find it. It may not be apparent, it may require two or three vulnerabilities to get in, but EVERYTHING is hackable. Stay determined.
2. Versions. Always find them. They may be hidden from banner grabbing. You may need to do some digging, such as using WireShark and grabbing those packets to find the version. But finding exactly what you are working with is important.
3. Enumeration, enumeration, and then more enumeration. Enumeration is solid. Enumerate more.
4. When you think you are lost, take a break. Walk away, go and do something else. Towards the end of my 30 days of labs, I was burned out. It is good to step away, take a breath, go tell the family you love them, and get back to work when you can.
5. You can do anything you put your mind to. This is cliche, I KNOW. But honestly, I always wanted to try for OSCP, and a year ago it seemed daunting. But this exam attempt has made me realize that I can do things I never thought I could. It seemed out of grasp, maybe it still is. But getting here has been a journey! Pass or fail, I am proud at what I have accomplished.

<H2>7. End Rant</H2>

And this brings me to my closing comments. I learned so much about myself and my determination, living off OSCP's motto of "try harder". I did something over the last few days that challenged me entirely. To those of you in my multiple Slack and Discrod servers and Reddit threads, thank you! Exabyt3, SurgicalMittens, Xaliom, nineninenine, Gridith, The XSS Rat, CyberTuna, The Mad Human, Cyptik, Axua, and anyone I forgot, you kept me sane and helped me through when times were tough with this.

Thanks for reading!
