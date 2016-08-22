---
layout: post
title: Automated Jumpstart Finally!
date: '2005-04-13'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:37.036-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-7821841088973334052
blogger_orig_url: http://blog.shawnferry.com/2005/04/automated-jumpstart-finally.html
---

After cleaning up some out of date macros, interpreting some errors from  
JET that were actually from add_install_client and creating a new macro  
specifically for the host, things appear to be going well. It is still  
confusing to me that the initial boot image appears to be Solaris Express  
build 72. It makes me wonder if I didn't clean something up, or if I am  
otherwise confused about the state of my jumpstart server. Once this is  
done I will make the tiny little laptop into a jumpstart sever, at some  
point I need to rebuild my firewall/gateway to Solaris as well. I will  
also be able to play with live update without worrying about destroying my  
production boxes, and without having to worry about trying to get them  
back up if I do something strange. It's a shame that it is late and I am  
tired, it would be interesting to see if I can figure out why my root disk  
is blocking so much when the jumpstart root, boot and packages are all in  
a JBOD. This sounds like a job for Dtrace! I need to see about some more  
hardware.  

