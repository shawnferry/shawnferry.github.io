---
layout: post
title: OpenSolaris/Indiana first thoughts
date: '2007-11-01'
author: Shawn Ferry
tags:
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:17:26.890-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-8433998239952101353
blogger_orig_url: http://blog.shawnferry.com/2007/11/opensolarisindiana-first-thoughts.html
---

(See Update 1 for how to get functional networking in Indiana under Parallels)  
  
As ThinGuy just mentioned on twitter, no registration or login to download the
image. Woot!  

I am running the installer iso on top of Parallels on Mac OS X 10.5 (leopard)  

  * Boot speed is fabulous
  * Installer is straight forward 
    * so few steps it seems like I must be forgetting something
  * Base install is FAST

On the down side:

  * I can't seem to get an external network interface to plumb, but Parallels seems a bit flaky on Leopard
  * single disk contention is the long pole install is not as fast as it could be, should have burned a CD

So now as long as I can get a network interface working one way or another I
am set. Fingers crossed. I have already nuked my other local install so I had
space to play. I would rather not have to recover it.  
  
Update 1:

Networking: It helps if you remember to install the Parallels provided network
interface driver.

See the comment about so few steps I must be forgetting something (like
installing the driver)  
  
Parallels: Installing the Beta got rid of some VM related network error
messages and appears to have fixed shared networking (my default for my simple
test instance).  
  
I guess I could have provided a link:
<http://opensolaris.org/os/project/indiana/resources/getit/>  
  
Issues with the base slim install No /usr/ccs/bin/make (or any make as far as
I can tell in the default install). To install the required driver the
following manual steps are required.

  * untar the ni*.tgz into /tmp
  * in the /tmp/ni*/i386 directory
  *     * cp ni dp8390 to /kernel/drv
    * cd ..
    * addni.sh  
(or you can change /etc/path_to_inst yourself)

    * modload /kernel/drv/ni
  * Assuming you are using dhcp wait a moment and get the popup telling you that you have an address.  
  
![](http://lalartu.smugmug.com/photos/215941972-L.png)  

Appendix C: is way out of date although it would have reminded me to install
the network driver.  

