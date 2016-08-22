---
layout: post
title: Solaris DHCP and Replay TV
date: '2005-05-13'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:36.998-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-6715744280849405741
blogger_orig_url: http://blog.shawnferry.com/2005/05/solaris-dhcp-and-replay-tv.html
---

I have a ReplayTV, I have had it for a couple of years. Until now I  
haven't had any problems to speak of. I recently installed Solaris 10 on  
my firewall. As part of that process, I moved DHCP to a different Solaris  
10 server in my network. The ReplayTV (named Bob), sends dhcp requests  
asking for addresses with the host name "RTV Bob". 42856e77: Datagram  
received on network device: rtls0(limited broadcast) 42856e77:  
select_offer: hostname request for RTV Bob 42856e77: name_avail(F):  
gethostbyname_r failed, err 2 42856e77: select_offer: name_avail false or  
no address for RTV Bob This is a problem. So now I have manually assigned  
it an address, but it appears hung. I think it might be running an update,  
but I can't be sure. I am tempted to powercycle it, but instead I think I  
will go to bed. It might get kicked in the morning. On another note, the  
email support link specifically says "Not for technical problems"...WTF I  
guess it would be silly to send them mail about hostnames with spaces  
then. Or not, it just started responding right before I was about to  
submit this entry  

