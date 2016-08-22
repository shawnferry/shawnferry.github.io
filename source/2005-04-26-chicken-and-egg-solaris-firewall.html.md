---
layout: post
title: Chicken and Egg? Solaris Firewall Install
date: '2005-04-26'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:37.004-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-6321559123429032839
blogger_orig_url: http://blog.shawnferry.com/2005/04/chicken-and-egg-solaris-firewall.html
---

Days old entry that never got out.... I am upgrading my firewall to  
Solaris 10. However the following problems exist... 1) My firewall is my  
gateway 2) My firewall/gateway does not support pxe boot 3) My  
firewall/gateway does not support console at boot (Anybody want to send a  
V20z my way? I understand that the employee discount is good, but not so  
good that I can just pick one up) 4) My Jumpstart + DHCP server want's to  
use my firewall as it's gateway 5) I am way to lazy to reconfigure so that  
it will all just work 4) Keeping the natives happy involves maintaining  
Internet access throughout this process. 6) I can't boot disk one from my  
SCSI DVD-ROM so I have to use the slower internal CD-ROM. Now the only  
problem is that I misremembered the type of network cards that I had in  
the system and I have no net from the box. Then after I grabbed the one  
that I thought it was, because I didn't have a pen handy to write down the  
vendor information from a prtconf -pv, it turns out that I have Netgear  
FA310 not FA311, ah well fortunately bulk CDs are cheep. Tonight I will  
find out if the tu driver from [  
http://homepage2.nifty.com/mrym3/taiyodo/eng/](http://homepage2.nifty.com/mrym3/taiyodo/eng/)
works for me. After the  
fact: Tonight was a little optomistic maybe by next week.  

