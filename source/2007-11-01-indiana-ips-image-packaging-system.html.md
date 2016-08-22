---
layout: post
title: Indiana IPS (Image Packaging System)
date: '2007-11-01'
author: Shawn Ferry
tags:
- opensolaris
- yakshaving
- b.s.c.
- indiana
modified_time: '2010-04-29T10:17:26.888-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-7140902992765344817
blogger_orig_url: http://blog.shawnferry.com/2007/11/indiana-ips-image-packaging-system.html
---

Boldly forward with minimal reading of the docs.

I can't obviously find where I would be downloading additional packages from.
I feel like I am running in circles. It would seem that nearly the first thing
I should be able to find would be download the rest from HERE. The single CD
installer rocks, but we are up to 1 DVD for the normal full install.  
  
I want the Firefox default home to prominently show me:  
"Now that you have completed the Slim install get the rest of the packages.
Use pkg list/status/something someargs"  
  
>  
>

>

> The Preview includes the Image Packaging System. With IPS, you can select
versioned builds of components to manage or create your own custom OpenSolaris
distribution.

>

>  
>

>

> IPS packages that are not included in the Slim Install installation image,
such as developer tools, can be downloaded after the installation. This
prototype uses new IPS commands to access packages from the network
repositories. Both IPS packages and SVR4 packages are supported.

>

>  
>

>

> The OpenSolaris Project: Image Packaging System project page contains man
pages for the new IPS commands and a link to the IPS download site.

OK, pkg(5) has some indications of a repository (or authority)
pkg://opensolaris.org. No examples, is pkg://opensolaris.org actually a valid
and running authority? OK http://opensolaris.org/os/project/pkg/documents/
says pkg.opensolaris.org. Looking at the list of available packages I now see
that they are basically ALL installed. (maybe pkg(5) should reference
pkg.opensolaris.org)  
  
I can also see that when I pkg uninstall SUNWbind and pkg install SUNWbind the
counters on the site increment. When I drop ni0 the install fails and with it
enabled and snoop running there is http traffic downloading the package.
Clearly I am hitting the remote authority. Somewhere I think I should be able
to see what the default authority is or where to find it (without digging in
/var/pkg and guessing that cfg_cache should be the source, or looking at snoop
to see where my traffic is going)  
  
pkg install/uninstall is fast and also easy (somewhat dependent on network
bandwidth I would guess). It wouldn't suffer from the some sort of optional
feedback that work is progressing.  
  
Errors from pkg are quite ugly and straight out of python.  
  
At this point I call the install a success simple, easy, fast. The post
install experience is still missing something. The packages that are not
included on the live CD, how did they get installed on my instance when I had
no net during the install.  
  
So we are down to

  1. Is pkg.opensolaris.org the only authority?
  2. are all the packages that are available installed by default?
  3. If pkgadd can't add packages (or is this a bug) how do I add standard SYSV packages now

In the end this was fun and interesting, I have a much better understanding of
where we are going but it doesn't look like Indiana is going to be my everyday
system for a while yet,

