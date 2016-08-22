---
layout: post
title: Nonexistant Diskette0
date: '2005-04-01'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:37.063-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-6934483163501410421
blogger_orig_url: http://blog.shawnferry.com/2005/04/nonexistant-diskette0.html
---

I am trying to re-jump a box at home. It is currently running Solaris 10  
b72. I am trying to jump it to Solaris 10 GA, but the boot keeps panicking  
because there is no /dev/diskette0. At first I was hoping that the problem  
was DHCP handing out the wrong address. Thinking that maybe because hosts  
and ethers didn't agree with address something was missing from the boot.  
Unfortunately after I fixed that no apparent changes. Next Step, look at  
the old JET template that I copied into a new 3.7.3 install, or GASP maybe  
read the docs to see if something changed. [  
Blueprints](http://www.sun.com/blueprints/0205/819-1692.pdf)(I should read
this one), [  
Jumpstart](http://docs.sun.com/app/docs/doc/817-5506) and [Jumpstart  
Enterprise Toolkit](http://www.sun.com/bigadmin/content/jet/) really are quite
useful.  

