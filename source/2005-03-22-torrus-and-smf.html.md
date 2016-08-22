---
layout: post
title: Torrus and SMF
date: '2005-03-22'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:37.080-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-5281430922548134642
blogger_orig_url: http://blog.shawnferry.com/2005/03/torrus-and-smf.html
---

I was able to compile the current RRDtool 1.2 Release Canidate, install  
the required perl modules and Torrus without any issues. I was also able  
to setup the configs, do the device discoveries and database compilation.  
Instead of manually starting Torrus and setting it up as a legacy service  
I created my first SMF manifest and imported it. I was expecting it to be  
at least a little bit more complicated than editing an XML file, moving  
the start/stop script to the right place and running `svccfg -v  
import /var/svc/manifest/application/torrus.xml` The online and  
offline functions appear to be working quite well.... Unfortunately the  
RRDTool 1.2 Release Canidate perl shared bindings are another problem. I  
can however report that when the start method fails the system runs the  
stop method. ~~Unfortunately I don't think I will have any real time  
to work on it until next week. Maybe I will just try a slightly older  
rrdtool snapshot.~~ Trying an older snapshot is no good because I  
get an error from configure telling me that my compiler does not support  
IEEE math out of the box. So I am back to trying again later when I can  
think about the problems.  

