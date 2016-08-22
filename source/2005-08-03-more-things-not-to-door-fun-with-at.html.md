---
layout: post
title: More things not to do...(or fun with at)
date: '2005-08-03'
author: Shawn Ferry
tags:
- lame
- b.s.c.
modified_time: '2010-04-29T10:24:36.927-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-2252723566857428275
blogger_orig_url: http://blog.shawnferry.com/2005/08/more-things-not-to-door-fun-with-at.html
---

Adding another item of things not to do.  
  
at now+30m  
at> shutdown -g300 -i6 -y  
at> CTRL-D  
  
So I was testing some behavior to see if trying to allocate more swap than the
available space would generate the message I was expecting (It does).  
  
My next step was to use mkfile to eat up all but the smallest amount of space
in /tmp that I could. Before doing that I set up the at job to try and recover
if I ended up in a situation where all of my connections hung.  
  
The result of the whole situation...  
  
I was able to use almost all of my swap, the system was unresponsive for a
minute or so while I was filling up /tmp.  
  
While I continued to mess around seeing if normal tool use would also generate
the message (It doesn't) I forgot to remove the at job.  
  
So after making a note of my results and writing a message to that effect I
remembered that I had an at job... unfortunately about 30 seconds too late to
kill the running shutdown.  
  
So much for my uptime.  

