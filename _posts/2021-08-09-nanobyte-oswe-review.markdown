---
layout: post
title:  "nanobytes OSWE Review"
author: nanobyte
date:   2021/08/09
description: Post Exam Writeup for my Offensive Security Web Expert (OSWE)
tags: post-exam
---

1. Introduction & Lookbacks
2. Preperation
3. The Labs
4. The Exam - Attempt 1
5. The Exam - Attempt 2
6. The Exam - Attempt 3
7. Top Five Takeaways
8. Final Thoughts

<h2>1. Introduction & Lookbacks</h2>

I knew that I wanted to be a penetration tester after earning my Offensive Security Certified Professional (OSCP). To aide in this goal, I wanted to become more familiar with penetration testing web applications. This is what kicked off my <a href="https://www.offensive-security.com/awae-oswe/" target="_blank">Offensive Security Web Expert (OSWE)</a> certification interest. Offensive Securities Advanced Web Attacks and Exploitation (AWAE) course, which is required for the OSWE exam, was an intense course. It consisted of several modules that carries you through vulnerability discovery within source code to remote code execution on a variety of web applications in several different languages. AWAE is a white box course, meaning you have access to all the web application's source code. This is not a black box course. For me, the most difficult task during this course was being able to follow the source code and identify vulnerabilities in the various languages. I have heard that a heavy development background is required to pass this course. On the contrary, if you are able to read source code, then you should be able to complete this course.

My background in development is severely limited - learning two high-level languages in college (Java and C++) and some Python scripting. I have never developed anything in production (or even close) in the real world. However, while taking this course and knocking off the rust, I was able to follow along.

Looking back, during your preparation for AWAE, if you are not familiar with some of the languages, I would urge you to take some time to review common web languages. This includes PHP, Java, JavaScript and .Net. I took time to touch up on Python scripting, but avoided brushing up on anything else. This was definitely a mistake. If you are not familiar with these languages, take some time to learn them. You won't need the ability to write fully functional web apps - but rather have the ability to follow the logic, the functions, which parameters are useful, etc.

<h2>2. Preparation</h2>

I began to prepare for AWAE shortly after earning my OSCP. I prepared for around 6 months before starting my labs. I read every review I could find, but with the updated 2020 content, many of the reviews were on the old course. For those like me who have never penetration tested web applications, <a href="https://portswigger.net/web-security" target="_blank">Burp Suite's PortSwigger</a> labs were an amazing resource for diving into AWAE. The ability to better understand common web vulnerabilities, why they were vulnerable, and how to exploit them, were very useful in understanding the attacks in the course. Some of the labs I completed were:

-SQL injection<br>
-Cross-site scripting<br>
-XML external entity (XXE) injection<br>
-Insecure deserialization<br>

Looking back through these labs, I wish I would have also focused on a few others, such as cross-site request forgery (CSRF). Coming into this course, I only had an idea of web attacks from OSCP and <a href="https://www.hackthebox.eu/" target="_blank">Hack the Box</a>. Working through PortSwigger's labs, especially how they provided instructions on these attacks when stuck, was amazing for me. I highly recommend PortSwigger to anyone looking to just better familiarize themselves with common web attacks.

Another key piece to my preparation was <a href="https://www.pentesteracademy.com/course?id=11" target="_blank">PentesterAcademy's JavaScript for Pentesters</a> course This went deep into JavaScript (JS), and also Cross-site scripting (XXS) attacks. This was a core understanding of looking at a web page's source code, and thinking outside the box on how to leverage a vulnerability to exploit it. I really enjoyed this course, and highly recommend it for anyone just getting comfortable with JS or XXS attacks.

Finally, I got back into the groove of practicing Python. I had learned how to read and edit Python exploits I discovered while in Penetration Testing with Kali Linux (PWK) course for OSCP, and some Python scripting courses years ago. But I knew I would be crafting Python exploits by hand, as my language of choice for exploits, and learning how to quickly script is key in this course for the exam. I used <a href="https://edabit.com/" target="_blank">edabit</a> to familiarize myself with Python once again.

<h2>3. The Labs</h2>

I decided to purchase the 60 days of labs. I read many comments that stated one could easily do the entire course in 30 days. However I have a family, with kids and dogs and cats. I knew I would not be at my PC every night, and that I needed to take breaks, especially since I took booked this course over the holidays. Take whatever you are comfortable with, but the breaks between modules helped keep mine and my family’s sanity. I read the PDF, going through each module, and then would play the videos and follow along with my labs. I completed most extra miles - but should have completed every extra mile at this stage. The extra miles were key to help me learn to go the extra step and discover vulnerabilities on my own, or exploit something differently. Take the time, and do every extra mile you can.

I think the labs were great, for the most part. There were some instances of Offensive Security going above the novices (like me) and providing a statement, such as "And by reviewing the source code one could find this vulnerability...", or something along that line. However, I have never reviewed source code, and it was difficult to understand how anyone would identify that as a vulnerability! Read between the lines, and get the message before glancing over and continuing. This will help you come exam time.

Lastly, when you are taking notes during your labs, I recommend making a section for "Python Scripting Cheat Sheet", where you can put commonly used functions that you create through the course, as well as your Python skeleton script. This will speed up your exploit creation, so you are not struggling to find these in your notes or on the internet during your exam!

<h2>4. The Exam - Attempt 1</h2>

Walking into my first exam, I wanted to see where my strengths were, and what my weaknesses were. I knew I would not pass my first attempt. Nonetheless, I looked at this as a reconnaissance, to walk away with the intent to study on what I felt weak on. What I took away from my first attempt was my inability to utilize the tools taught in the course. I cannot go into any details, but in every module there are a variety of methods and tools that can be used. I recommend understanding all of these tools prior to your first exam, and how to use them.

I failed my first attempt with 0 points. This was hard to swallow, since I have never failed an exam in my life! I had an idea on one machine of what needed to be done, but never got a working exploit. However, instead of quitting, I purchased another 30 days of labs, completed some of the harder and more difficult extra miles I passed over on my first lab time, and regrouped. I was determined to not yet give up!

<h2>5. The Exam - Attempt 2</h2>

During my second attempt, I told myself I just wanted to do better than my previous attempt. This meant just getting a few points on the board. This time, I was able to identify one vulnerability and create a working Proof of Concept (POC) and got a few points! Ultimately I did not achieve enough, and still failed my second attempt. After failing twice, I was on the 60 day cool-off period for exams, and decided not to buy any more lab time. I did review my notes, re-read the PDF, and re-watched the videos for explanation in vulnerability discovery in source code, what I felt I lacked in the most.

Another thing I did during this time was create an in-depth checklist of what I should follow for a methodology in my next attempt. This is something I should have done for my first two, but failed to write. In my first two attempts, I followed whatever looked interesting, but did not systematically follow a check-list.

<h2>6. The Exam - Attempt 3</h2>

Finally, before I knew it, my cool-off period was over and my third attempt had arrived. During the 47 hour and 45 minute exam, I took a break every hour for at least five minutes. On the first day, I was able to fully exploit a machine, and earn more points than my second attempt. I gave myself a full eight hours of sleep, even though I felt like I was wasting time. I knew I needed the mental breaks. Coming back after that extended break, and looking at it with fresh eyes, I was able to complete enough objectives to earn the points required for passing!

Upon completion of my exam time, I wrote my exam report, and submitted it. I received a response way before the ten business days that I had earned my OSWE!

<h2>7. Top Five Takeaways</h2>

1. This course is tough, in my opinion much tougher than OSCP. Although, Offensive Security gives you the methodologies and tools that you need to complete this course. It will require reading and studying outside of the PDF or videos to complete some of the extra miles, but this prepares you for the exam.
2. My scripting abilities have grown ten-fold since entering this course. I am able to quickly craft a Python exploit from scratch.
3. I have a deep understanding of a variety of web application attacks. Not just how to perform them in a black box scenario, but also from a white box scenario with source code provided.
4. When you think you cannot go any further, take a break. Walk away from the computer, go spend five minutes with your family. Yes, even during the exam! Grab some water, and get back into it with fresh eyes. When you need to during your lab time, take a day or two off for another hobby. You don't need to overwork yourself!
5. If you stay persistent, you can accomplish amazing things. I thought I may have made a mistake after my first exam attempt - but staying persistent allowed me to achieve this amazing certificate.

<h2>8. Final Thoughts</h2>

This is the hardest exam I have ever taken in my life. I am thrilled to have been able to again pass another Offensive Security exam, and have OSWE as a part of my credentials. Although I am still not a penetration tester, I hope one day to apply the skills I have learned to a security position in my future. Thank you Offensive Security for a great course!

<div data-iframe-width="150" data-iframe-height="270" data-share-badge-id="5b89204b-da53-4f03-a6b9-b1e3f885c028" data-share-badge-host="https://www.credly.com"></div><script type="text/javascript" async src="//cdn.credly.com/assets/utilities/embed.js"></script>
