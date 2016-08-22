---
layout: post
title: How to get experimental rw ZFS support AFTER upgrading to 10.5.1
date: '2007-11-27'
author: Shawn Ferry
tags:
- howto
- apple
- Leopard
- ZFS
- b.s.c.
modified_time: '2010-04-29T10:17:26.871-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-5130906921335366223
blogger_orig_url: http://blog.shawnferry.com/2007/11/how-to-get-experimental-rw-zfs-support.html
---

The alternate title of this entry is "Force install of ZFS Beta Seed v1.1 on
Leopard"

This is performed at your own risk. The steps described remove any logical
restriction for installation of the package.  

There is more than one way to implement this particular hack. This method uses
the package installer (a cleaner more friendly hack than manually copying
files). Other recommendations included installing 10.5 on a different
partition and subsequently installing the patch and copying the package files.  
  
A simple alternative left to the reader would be extract the files from the
package Payload an manually copy the files into place (cat
/tmp/ZFSseed1/ZFSBetaSeed1.pkg/Payload | pax -z -r -v).  

On to the actual Implementation:  

Download the dmg from developer.apple.com

Mount the DMG:

> open ~/Desktop/Inbox/leopard_9a559_zfsbetaseed1_0613523123.dmg

Expand the package:

> pkgutil --expand /Volumes/ZFS\ 1/ZFSBetaSeed1.pkg /tmp/ZFSseed1

Edit the Distribution file and comment out the line that actually checks the
requirements (and "causes" the failure):  

> vi /tmp/ZFSseed1/Distribution  
>  ~~// ~~

>

>  
>  
>

Flatten the edited expanded package directory back into package format:

> pkgutil --flatten /tmp/ZFSseed1 /tmp/ZFSrw.pkg  
>

Open the package installer:

> open /tmp/ZFSrw.pkg

Install the package and reboot.

Again this is performed at your own risk. The steps described remove any
logical restriction for installation of the package and may cause you system
to explode or you cat to catch fire.  
  
References:

[ZFS Beta Seed v1.1 will not install on Leopard
(10.5.1)](http://synesius.wordpress.com/2007/11/18/zfs-beta-seed-v11-will-not-
install-on-leopard1-1051/)  
  
Edit: I guess I should mention that it does actually appear to work :) Next
I'm going to try and switch access between Leopard and Solaris under Parallels  

Edit1:  Changed comment marks from // to ; // works if you are commenting in
the embedded script part not the XML part. Thanks to [Colin
Seymor](http://www.lildude.co.uk/2007/11/force-install-zfs-beta-seed-11-on-
leopard/) for catching that.[  
](http://www.lildude.co.uk/2007/11/force-install-zfs-beta-seed-11-on-leopard/)

