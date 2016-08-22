---
layout: post
title: Solaris 10 ipfilter ipnat and PPTP
date: '2005-05-25'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:36.991-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-7314918115898744195
blogger_orig_url: http://blog.shawnferry.com/2005/05/solaris-10-ipfilter-ipnat-and-pptp.html
---

I have been having a problem setting up PPTP tunnels since I upgraded my  
firewall to Solaris 10 (nv_12).  
Some basic tests clearly  
indicated that the problem was with my configuration.  
  
  1.   
My BSD firewall with ipfilter worked  

  2.   
My laptop direct worked  

As part of the migration I copied the ipf.conf and ipnat.conf files that I  
had been using.  
Once the firewall was up on Solaris 10, I  
installed the files and changed the interface names to match.  
  
After installing the rules, I had to edit pfil.ap and add a new  
interface type. svcadm start ipfilter and everything started  
working...almost. All of my web browsing, inbound/outbound mail, inbound  
http and ssh worked. The only thing that I couldn't do was create a PPTP  
tunnel.  

I have been poking the config for a few weeks never making the time to  
sit down and really think about the problem. Last night I took some time  
to start at the beginning and see if I could work it out.  

After reading through the Section 4 of the ipf and ipnat man pages a few  
more times to make sure I wasn't doing anything obviously wrong. I  
practiced my googlescholar skills and looked at a bunch of mailing-list  
posts, the pptp rfc and piles of other stuff. The trigger was seeing a  
post indicating that all GRE traffic needed to be redirected to the PPTP  
server.  

Kicking off a number of snoops an ipmon and finally (and I don't know  
why I didn't do this a while ago) I ran a tcpdump for proto gre on my  
laptop.  

  *   
From the external snoop I was able to see the inbound and outbound  
traffic  

  *   
From the ipmon I was able to see the inbound and outbound traffic  

  *   
From my laptop, I could only see the outbound gre  

The "fix" is to specifically route all gre traffic to the address of my  
laptop.  
  
I need to see if I can do it without the hard coding of the IP addresses  
that part is lame.  
  
The rules that make everything work are:  

    :::::::: ipf.conf ::::::::  
    pass out quick on extint proto tcp from any to any port = 1723 flags S keep state  
    pass out quick on exitint proto 47 from any to any  
    pass in  quick on extint proto 47 from any to any keep state  
         
    :::::::: ipnat.conf ::::::::  
    rdr extint PPTPserverip/32 port 0 -> laptopip port 0 gre   
            
