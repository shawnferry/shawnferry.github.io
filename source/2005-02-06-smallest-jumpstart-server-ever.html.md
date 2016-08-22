---
layout: post
title: Smallest JumpStart Server Ever
date: '2005-02-06'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:37.124-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-697100868159121354
blogger_orig_url: http://blog.shawnferry.com/2005/02/smallest-jumpstart-server-ever.html
---

I am planning on making the currently undisputed smallest JumpStart server  
ever.  
  
A "Sony PCG-U3":http://www.dynamism.com/u3/main.shtml

In breif: Crusoe 933 MHz processor 6.4" (XGA) TFT-LCD 20gb HDD 512mb RAM  
1.8 pounds 7.3" x 5.5"  
  
I understand that I could maybe make a smaller server with a nano-itx  
board, but not with a keyboard, display etc. in such a portable package.  
  
So far, I have Solaris 10 installed and running, from a PXE boot  
jumpstart install. That was neat, not as easy as booting a SPARC, but  
not bad once I got the proper DHCP/BOOTP settings in place.  
  
Unfortunately the box that did the initial jumpstart is currently unable  
to see it's root disk. So I can't use it to share the media to do the  
setup install server. I have all the hardware I need to recover one way  
or another, but I don't currently feel like scavaging from my firewall  
for another scsi card so I can see both sides of my old HP NetStore JBOD.  
  
So the next step is probably to nfs share from my laptop to the PCG-U3,  
possibly between commercials during the superbowl.  
  
