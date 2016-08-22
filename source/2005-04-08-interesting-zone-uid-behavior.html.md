---
layout: post
title: Interesting Zone UID behavior
date: '2005-04-08'
author: Shawn Ferry
tags:
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:24:37.048-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-1915257998183644387
blogger_orig_url: http://blog.shawnferry.com/2005/04/interesting-zone-uid-behavior.html
---

I am playing with zones and instead of maintaining a centralized UID list,  
I am adding users as needed to each zone(As I suspect would be the normal  
behavior). Not surprisingly there are differences in UIDs. As a note it is  
strongly recommended that you never directly access files in other zones  
from the global zone.  
  
    sort -t: -k3 -n /etc/passwd /stripe/zones/apache01/root/etc/passwd /stripe/zones/apache02/root/etc/passwd | uniq -c  
      
       3 root:x:0:0:Super-User:/:/sbin/sh  
       3 daemon:x:1:1::/:  
       3 bin:x:2:2::/usr/bin:  
       3 sys:x:3:3::/:  
       3 adm:x:4:4:Admin:/var/adm:   
       3 uucp:x:5:5:uucp Admin:/usr/lib/uucp:  
       3 nuucp:x:9:9:uucp Admin:/var/spool/uucppublic:/usr/lib/uucp/uucico  
       3 smmsp:x:25:25:SendMail Message Submission Program:/:  
       3 listen:x:37:4:Network Admin:/usr/net/nls: 3 gdm:x:50:50:GDM Reserved UID:/:  
       3 lp:x:71:8:Line Printer Admin:/usr/spool/lp:  
       3 webservd:x:80:80:WebServer Reserved UID:/:  
       1 apache2:x:100:1::/zone/local/apache2:/bin/sh  
       1 yak:x:100:1::/export/home/yak:/bin/zsh  
       1 srs:x:101:1::/export/home/srs:/bin/sh  
       2 mysql:x:101:1::/zone/local/mysql:/bin/sh  
       1 mysql:x:102:1::/home/mysql:/bin/sh  
       1 yak:x:102:1::/home/yak:/bin/sh  
       1 torrus:x:102:1::/zone/local/etc/torrus:/bin/sh  
       1 yak:x:103:1::/home/yak:/bin/sh  
       1 torrus:x:103:1::/home/torrus:/bin/sh  
       3 nobody:x:60001:60001:NFS Anonymous Access User:/:  
       3 noaccess:x:60002:60002:No Access User:/:  
       3 nobody4:x:65534:65534:SunOS 4.x NFS Anonymous Access User:/:  
      
As you can see above, starting in the 100s we have conflicting uid  
assignments. The result is some confusing ps and prstat output in the  
global zone which indicated in my case that mysql was running the torrus  
collector process.  
  
        ZONE     UID   PID  PPID   C    STIME TTY         TIME CMD  
    apache01   mysql 29869     1   0 09:48:57 ?           0:10 /usr/local/bin/perl /zone/local/torrus/bin/collector --tree=yakshaving  
      
    Total: 138 processes, 447 lwps, load averages: 0.48, 0.47, 0.41    
    PID USERNAME  SIZE   RSS STATE  PRI NICE      TIME  CPU PROCESS/NLWP          
    11642 daemon   2096K  616K sleep   60  -20   0:05:16  13% nfsd/4   
    29869 mysql      16M 8404K sleep   59    0   0:00:10 2.0% collector/1     
    160 root     4404K 4200K cpu0    59    0   0:00:00 0.4% prstat/1    

Knowing that mysql was not running the collector made this simple to  
diagnose, but in a less controlled environment I think it could be quite a  
bit more confusing.  

