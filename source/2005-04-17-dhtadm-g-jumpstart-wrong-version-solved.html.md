---
layout: post
title: dhtadm -g; jumpstart wrong version, solved
date: '2005-04-17'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:37.018-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-2571587900149303786
blogger_orig_url: http://blog.shawnferry.com/2005/04/dhtadm-g-jumpstart-wrong-version-solved.html
---

The moral of this story might just be: If it keeps failing between 23:00  
and 02:00 try it during the day after you get some sleep. I need to thank  
Eric Noriega, for his summary post a year ago in which he states that the  
Sun DHCP server does not pick up changes to macros with out a restart. I  
need to blame my apparent inability to read (see moral) on missing the  
following in the man page for dhtadm.  
  
    dhtadm(1M)  
         After you make changes  with  dhtadm,  you  should  issue  a   SIGHUP  to  the  DHCP server,   
         causing it to read the dhcptab and pick up the changes. Do this using the -g option.  
    ....  
              -g           
          Signal the DHCP daemon to reload the dhcptab after  successful completion of the operation.    

Quite obvious when you go back and look for it thinking; that is stupid,  
why doesn't dhtadm tell in.dhcpd that something changed? So remember,  
after you make changes to your macros and you can't figure out why nothing  
appears to have changed, you forgot 'dhtadm -g'. 'svcadm restart  
dhcp-server' will do it after the fact.  

