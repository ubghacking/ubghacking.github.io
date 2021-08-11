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
3. Buffer Overflow Preparation
4. The Exam
5. Top-Five Takeaways
6. End Rant

<H2>Update</H2>

I passed my exam, and earned my OSCP!

<H2>1. Pre-Exam Talk</H2>

Hello, and thanks for taking the time to read my post exam writeup for my first attempt at the Offensive Security Certified Professional (OSCP) certificate! I say first attempt, because I just submitted my exam report yesterday, and I honestly do not know if I passed or not. Details on being unsure are to follow below. However, I am eager to write this to share with my online fellow hackers that pushed me, and stuck with me, over the last several months. You guys rock, and even though you may not think you helped me, you kept me sane and helped me more then you may know. So, thank you!

I first got eager to earn my OSCP when a close friend, exabyt3, earned his and told me about what it meant to hold the certification, years ago in the early 2010's. I recall thinking what a "badass" he was, being able to hack into computers and gain administrative access to computer systems. At this time in my life I was "computer savvy". I could build them, play computer games and build websites, but not well versed in the Linux terminal or Windows command line, at all. To be honest, it boggled my mind back then how someone could actually USE these as a means to interact with a computer. I was just to heavily reliable on what a GUI provided. But nonetheless, I wanted to earn this certificate, more than anyone knew.

<H2>2. Preparation</H2>

I began my studies over nine months ago, when I was introduced to <a href="https://www.hackthebox.eu/" target="_blank">Hack the Box</a>. For those that do not know what Hack the Box (HTB) is, it is a network of machines that are vulnerable. You receive a VPN connection pack when you make an account (you need to "hack" the login page to make an account, but don't worry there are guides to walk you through it) and once you have your connection pack, you can VPN into the network and attack these vulnerable machines. I began the overwhelming process of learning Linux, all while learning how to perform these attacks by hand from my Certified Ethical Hacker certificate. Sure, I KNEW what a SQL Injection attack was. But was I able to perform one on my own? That's what SQLMap was for! This goes into the same for any attack; local file inclusions, cross site scripting, the list goes on. I knew what the attacks were from memorization. But PERFORMING them by hand on my own was completely different. I worked my way through machines, starting with the easy rated boxes and working my way up from Linux to Windows. In those early days of HTB, forum checks were numerous, and calls for aide were constant. One thing I did do, which helped me even through OSCP, was I began to keep rudimentary notes of my accomplishments, and walkthroughs so I had my own reference of the tools I used. This helped me, I knew what the syntax and output looked like. These writeups are still in my notes, and I even used them on my exam!

However, this HTB quickly became an addiction. I referred to it as "retire anxiety". For those that know HTB, when a machine retires (to introduce a new machine, which happens weekly), points only remain for those active machines. No way did I want my points to drop! So, not only was I doing HTB constantly, but to get higher ranks, I HAD to keep going. I would say with my newfound sobriety, HTB replaced my addiction to alcohol. Not to get off subject, but this caused a lot of issues with my family, and so I had to limit my time to two or three nights a week of "studying" (as I referred to it). I had my end goal in sight and knew what I wanted. I used HTB as my main platform to begin and was on there for six months. During this time, I found that I was creeping away from SQLMap to perform SQLi attacks manully, taking the time to research what I was doling. I knew MetaSploit was limited on the OSCP exam, so I actively attempted to exploit anything I could on my own (but man, MetaSploit is so much easier). I want to say that this was my biggest success. I learned how to use the tools I needed to on the exam. Not saying I have NEVER used MetaSploit, but just avoided it where I could. 

That lasted until February of this year, three months ago. At this point, I had achieved Elite Hacker status on HTB. My team was rated in the top 100, in the low 70's. Although Xaliom carried our team with the highest points, I was always trying to be second. Unfortunately, cause I wasn't first though, I was last ... Have to live up to the Talladega Nights: The Ballad of Ricky Bobby standard. But I was starting to feel comfortable. The nights I was not on HTB, I was reading books such as Penetration Testing by Georgia Weidman, The Web Application Hacker's Handbook by Dafydd Stuttard and Marcus Pinto and Hacking: The Art of Exploitation by Jon Erickson just to name my top three favorites. These got me into a methodology, and not just wildly and frantically looking for vulnerabilities. Reading became just as important as HTB. The list goes on of other books I used for reference, or started and not finished... I will get back to those, one day. You are not forgotten, Black Hat Python!

In February, I started to tackle the OSCP like HTB machines (<a href="https://www.reddit.com/r/oscp/comments/alf4nf/oscp_like_boxes_on_hack_the_box_credit_tj_null_on/" target="_blank">https://www.reddit.com/r/oscp/comments/alf4nf/oscp_like_boxes_on_hack_the_box_credit_tj_null_on/</a>). For those that are new to HTB, or new to the whole hacking world, this is where you should start (from my point of view). If I would have tackled this list first, and not gone for points, I would have been so much better to start off. These OSCP boxes all have write-ups easily searchable, and because of this when you get stuck there are multiple write-ups to get you through. Doing this would have gotten me used to the tools, rather than fumbling my way through. These machines really were what got me prepared for the labs the most. I completed the list of Linux machines, then moved onto Windows. All the while, I used several different platforms for my notes, testing which one fit me the best.

Notes. Notes were essential. I constantly referred to my notes, which I have gathered over these last nine months. I tried several different platforms. I tried Cherry Tree on Linux first, and honestly really liked it. However, because I have multiple computers that I use, depending on how I feel that day, it was hard to back up to a repository and download every time I moved computers. I moved to Google Docs, huge mistake. Ultimately what I used in the end was GitBook. I highly recommend GitBook for its functionality and searching within GitBook is insanely fast and efficient. When I needed a reference for a tool, let's say hydra, typing in 'hydra' into the search bar not only displayed my notes in 'Web Attacks' for what the tool was, but I found practical examples from machines in HTB where I had used it (yes, all my HTB machine writeups are transferred into my GitBook.)

Now, at this point, I was eager and ready to sign up. I had read a few books, I was an Elite Hacker on HTB, and did the OSCP like boxes. It was early March now, two months ago, and I felt ready. I signed up and enrolled to begin 30 days of labs to start early April. In the meantime, I kept up on my HTB points, stuffing down my "retire anxiety"!

The OSCP labs and material were absolutely amazing. When the labs were unlocked, to keep myself on pace, I booked my exam for early May so I would not put this off! I was enrolled into the 2020 updated course, and the new material was, honestly, great. I took the first 7 days, and watched all the videos, referencing the PDF along the way. To be honest, I found that the PDF goes over the videos verbatim, so I did not read the PDF cover to cover, but used as a reference. Anything and everything the videos covered, I put into my GitBook notes. This led to a rapid increase in size but I now had everything I needed in there as reference, and it was quick and easy to find it all when required.

Because I purchased 30 days of lab time, I did not complete the exercises and submit a lab report. For those preparing for the OSCP, in looking back, I would recommend looking more at this option. I wish I would have done the lab report, even though it is extensive. The lab report requires you have a write-up of ten machines you "pwned", as well as all the lab exercises in the PDF. The lab exercises are what scared me away since I wanted to focus on playing in the labs. I was able to root 20 machines in the lab, all MOSTLY on my own. For those few boxes that were difficult, I turned to the forums, or fellow hackers I know that hold OSCP. The Offensive Security forums were very casual, and it was easy to talk to anyone in them. They were a great place to turn to for suggestions, post questions and even request nudges when stuck, similar to the HTB forums. I always used this a last-ditch effort, always trying and covering everything I could think of before reaching out for help.

At first, 30 days seemed so long. However, by the last few days, I could not believe how quickly they had flown by. I had done the lab time and was getting more and more nervous as my exam date crept nearer. After the labs were done, I needed something to work on before my exam. I had booked it one week after my lab time ended. During the last days of my lab time, someone told me about <a href="https://tryhackme.com/" target="_blank">TryHackMe</a>, a similar tool to HTB. Because I had already done the HTB OSCP-like machines, I turned to TryHackMe's OSCP learning path. I am happy I did and will keep my TryHackMe membership to continue learning more with this as well! This is another great site, "easier" than HTB but great with vulnerable machines to hack.

Finally, before I knew it, it was May 10th, and the exam was the next day. Although I was frantic, I called it an early night and turned my computer off before dinner. I understood that nothing I would do the night before, would prep me any more. I spent the night with my family, ate a good dinner, and was in bed 9 hours before my exam start time. I was to eager and nervous! I could hardly sleep.

<H2>3. Buffer Overflow Preparation</H2>

The buffer overflow scared me when I started this learning path. The idea of learning how memory worked, to create a malicious exploit to crash a program, that sent a reverse connection back for system access!? It is not bad. I looked at Justin Steven's dostackbufferoverflowgood (<a href="https://github.com/justinsteven/dostackbufferoverflowgood" target="_blank">https://github.com/justinsteven/dostackbufferoverflowgood</a>) which is an awesome tutorial. I started by spinning up a Windows 7 "exploit" machine (again, I use several computers, and this was portable). I installed Immunity, IDA, DNSpy, all the tools I could think of for exploit creation. I did the dostackbufferoverflowgood, and moved on to other executables. I found a vulnerable version of minishare, and exploited that, along with vulnserver. Do all you can, practice!

<H2>4. The Exam</H2>

I have switched from Chrome to FireFox being my daily web browser. I tested my webcam in FireFox, and when I got my browser going for the proctoring software, my CPU utilization jumped to 98%, and stayed steady through the entire exam. FireFox took all my CPU, and Chrome could not detect my webcam. It was already 40 minutes into my exam and did not want to waste any more exam time troubleshooting. Get your Chrome's working for the proctoring software, everyone I talked to on this said they used Chrome. Maybe stear clear of FireFox?

I used some basic scripts for my enumeration right at the start. I made some small scripts prior to the exam that took a file (i.e. IP.txt) with all the IP's for my machines in that file. I fed this into scripts like nikto_enum.sh, nmap_enum.sh, dirb_enum.sh, etc. These enum scripts were basic shell one-liners that performed a quick for loop, to loop though the IP's and perform a standard scan, and then save the output of each to a folder of it's own IP. I have heard of better suggestions, but these were easy and worked. I also built some other quick terminal line options for these scripts with pre-built scans for further enumeration if needed. I recommend doing this, and having rudimentary scripts to automate your standard scans, it will save time.

Once scanning kicked off, I got to work on the Buffer Overflow. I quickly got a working exploit in the first hour and a half. Slow to what others get it, but for this n00b I was proud of that time frame. I then went for the next box, an easier one, where I quickly rooted it. This was two hours in, and I had already received half the exam points needed. Halfway there ...

I then got to work on the remaining three boxes. All my scans were complete at this point, and so I began working on two of the medium ones. After some time and frustration I stepped away and ate lunch. I spent 30 minutes with my family and cooled off my jets.

When I returned, I took a peek at the harder machine. After reviewing the scan results, I found the way in through some light tinkering. With that, I performed some privilege escalation scans, and looked at another box. And, I found something I missed on my initial enumeration, that led me to finally getting in. I got onto the machine as a low-level user. I found something else interesting, and quickly got root.

At this point, it was dinner time, where I took another break. I walked away for another 30 minutes and took some time with my family. I must honestly say, the small breaks and stepping away, not getting caught in the rabbit holes, checked my sanity! Do not get lost on the rabbit holes, take a step back, and look at the problem. When I returned, I popped a shell on another box, and got the final find of my exam.

That is where it stayed, I was never able to privilege escalate on the final two machines. I was able to get onto all five, all without MetaSploit. I worked until the final five minutes of my exam, living off energy drinks and an hour and a half of sleep. But, at the end of my time, I took a small nap and got to work on my exam report. I finished by the evening, and 36 hours after my exam start time, my report was officially submitted...

<H2>5. Top Five Takeaways</H2>

Here are my top five takeways:

1. There is ALWAYS a way in, you just need to find it. It may not be apparent, it may require two or three vulnerabilities to get in, but EVERYTHING is hackable. Stay determined.
2. Versions. Always find them. They may be hidden from banner grabbing. You may need to do some digging, such as using WireShark and grabbing those packets to find the version. But finding exactly what you are working with is important.
3. Enumeration, enumeration, and then more enumeration. Enumeration is solid. Enumerate more.
4. When you think you are lost, take a break. Walk away, go and do something else. Towards the end of my 30 days of labs, I was burned out. It is good to step away, take a breath, go tell the family you love them, and get back to work when you can.
5. You can do anything you put your mind to. This is cliche, I KNOW. But honestly, I always wanted to try for OSCP, and a year ago it seemed daunting. But this exam attempt has made me realize that I can do things I never thought I could. It seemed out of grasp, maybe it still is. But getting here has been a journey! Pass or fail, I am proud at what I have accomplished.

<H2>6. End Rant</H2>

And this brings me to my closing comments. Even though I am unsure if I passed, I learned so much about myself and my determination to OSCP's motto of "try harder". I did something over the last few days that challenged me entirely. To those of you in my multiple Slack and Discrod servers and Reddit threads, thank you! Exabyt3, SurgicalMittens, Xaliom, nineninenine, Gridith, The XSS Rat, CyberTuna, The Mad Human, Cyptik, Axua, and anyone I forgot, you kept me sane and helped me through when times were tough with this. I will find out in 10 business days or less what the outcome is.

Thanks for reading!
