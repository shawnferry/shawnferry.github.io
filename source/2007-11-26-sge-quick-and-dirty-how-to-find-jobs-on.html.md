---
layout: post
title: SGE quick and dirty how to find jobs on 'bad' slots
date: '2007-11-26'
author: Shawn Ferry
tags:
- howto
- SGE
- N1GE
- b.s.c.
modified_time: '2010-04-29T10:17:26.873-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-5625782500302416001
blogger_orig_url: http://blog.shawnferry.com/2007/11/sge-quick-and-dirty-how-to-find-jobs-on.html
---

I occasionally have a need to find queues in Sun Grid Engine that are in one
of the possibly problematic states which have an occupied slot. It is just
infrequent enough that I don't remember exactly how I did it the last time.  
  
>

>     qstat -f | awk '$6~/[cdsuE]/ && $3!~/^[0]/'  
>     > queuename                      qtype used/tot. load_avg arch
states  
>     > zone.q@r130c24z0.network.com   BIP   1/1       -NA-     sol-amd64
adu  
>     > zone.q@r130c24z1.network.com   BIP   1/1       -NA-     sol-amd64
adu  
>     >

>

>  
>

An alternate is "qstat -f | awk '$6~/[cdsuE]/ && $3~/^[1-9]/'" which also
avoids printing the header line. In the example above 'state' in $6 matches
's' and 'used' does not begin with '0'.  
  
The possibly more elegant 'qstat -f -qs cdsuE' still requires a second
comparison in awk of '$0!~/--/' to filter out the queue separator lines.
(qstat -f -qs acduE | awk '$0!~/--/ && $3!~/^[0]/')  
  
Finally because I can never remember what exactly all the queue states are and
the qstat man page doesn't have the nice table:

>  
>  aoACD #8211 Number of queue instances that are in at least one of the
following states:  
>  a #8211 Load threshold alarm  
>  o #8211 Orphaned  
>  A #8211 Suspend threshold alarm  
>  C #8211 Suspended by calendar  
>  D #8211 Disabled by calendar

>

>  
>

> cdsuE #8211 Number of queue instances that are in at least one of the
following states:  
>  c #8211 Configuration ambiguous  
>  d #8211 Disabled  
>  s #8211 Suspended  
>  u #8211 Unknown  
>  E #8211 Error

>

>

>

> Job State/Status:

>

> d(eletion),  E(rror), h(old), r(unning), R(estarted), s(uspended),
S(uspended), t(ransfering), T(hreshold) or w(aiting).  
>

References: [SGE (N1GE 6.0) -- Monitoring and Controlling
Queues](http://docs.sun.com/app/docs/doc/817-6117/i1001318?a=view)  

Edit: Added Job Status, literally couldn't find that in any of the online docs
(notwithstanding ~40% through the
[qstat(1)](http://gridengine.sunsource.net/nonav/source/browse/~checkout~/gridengine/doc/htmlman//htmlman1/qstat.html
"qstat\(1\) man page at sunsource" ) man page, targeted google searches do a
poor job finding the link)  
  
