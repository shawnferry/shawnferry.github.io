---
layout: post
title: First try with Sun Service Tags and SXDE 09/07
date: '2007-11-05'
author: Shawn Ferry
tags:
- opensolaris
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:17:26.885-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-1610909256028543253
blogger_orig_url: http://blog.shawnferry.com/2007/11/first-try-with-sun-service-tags-and.html
---

I just installed SXDE 09/07 and decided to give Service Tags another shot. The
installation of the Service Tags packages doesn't take a special effort.

Unfortunately I can't get the product registration agent to find anything. I
checked with one of my co-workers to make sure that he had success before
spending any time on the issue.  
  
He confirmed that he was getting discovery for OS installs on systems without
servicetag supported products.

![Service Tag Discovery: No Products
Found](http://lalartu.smugmug.com/photos/217779427-L.png)  

So clearly I have just installed the packages. This is obnoxious.

So under preferences I enabled FINEST logging and tried again.

>

>     FINE: Checking ip addresses: 10.211.55.10

>     Nov 5, 2007 11:57:17 AM com.sun.scn.client.ui.RegClient getSystems

>     FINE: Getting ip addresses: 10.211.55.10

>     Nov 5, 2007 11:57:17 AM com.sun.scn.client.ui.RegClient getSystems

>     FINE: Checking: 10.211.55.10

>     Nov 5, 2007 11:57:17 AM com.sun.scn.client.ui.RegClient checkIPAddress

>     FINE: Checking if valid ip address: 10.211.55.10

>     Nov 5, 2007 11:57:17 AM com.sun.scn.client.comm.TCPProbe run

>     FINER: sending message to: 10.211.55.10

>     Nov 5, 2007 11:57:17 AM com.sun.scn.client.comm.Communicator$1 run

>     FINER: communicating with: /10.211.55.10:6481

>     Nov 5, 2007 11:57:17 AM com.sun.scn.client.comm.Communicator
getFromAgent

>     FINE: Getting agent: http://10.211.55.10:6481/stv1/agent/

>  

That look like communication to me, but wait a URI...trying in a browser
returns:  

> ld.so.1: in.stlisten: fatal: libcrypto.so.0.9.7: open failed: No such file
or directory

When I look on my system I see libcrypto.so.0.9.8. I know an easy first try to
"fix" that.

> ln -s /usr/sfw/lib/ibcrypto.so.0.9.8 /usr/sfw/lib/ibcrypto.so.0.9.7

It seems to me that we could do with some sort of error detection or a host
level smf service failure.  
e.g. We got a response from the polled host but it wasn't anything that we
were expecting.  
  
Trying again in a browser returns a result that looks a lot like I would
expect given my understanding of Service Tags.

>

>  
>  
>       urn:st:28e87b4a-a625-c5ec-b64e-a5c64a1a9f65

>       1.1

>       1.0

>  
>         SunOS

>         splat

>         5.11

>         i386

>         i86pc::snv_70b

>         Parallels Software International Inc.

>         GenuineIntel

>         Parallels-18 F2 11 FF 3E 85 43 C5 B4 CC D7 85 04 84 A9 AD

>         39721385

>  
>  
>  
>  

![](http://lalartu.smugmug.com/photos/217779542-S.png)  
  
Now everything is working as expected. That is what I am expecting to see!  
  
Taking a look at the Sun Connection Inventory Channel I can see the host I
just registered!  
![Sun Connection: Viewing Registered
Hosts](http://lalartu.smugmug.com/photos/217779456-L.png)  
  
Now everyone should go and install the Service Tags agent and register their
devices (It's tied to an extra bonus for us next year :) ).  
  
References: [Sun Connection on
BigAdmin](http://www.sun.com/bigadmin/hubs/connection/ "Sun Connection on
BigAdmin" )

