---
layout: post
title: 'Note to Self: Manually pulling the bits out of a flar'
date: '2007-01-30'
author: Shawn Ferry
tags:
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:17:26.952-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-8827461963224342578
blogger_orig_url: http://blog.shawnferry.com/2007/01/note-to-self-manually-pulling-bits-out.html
---

Found this randomly. I had wanted to do this at one point but had an alternate
solution at hand.  

4\. Split the flash image (`flar split machineX.flar`), then move the file
"archive" to `/export/zones/machineX/root/`, and unpack it with `cpio -i`.

  * Uncompress if necessary (`mv archive archive.Z; uncompress archive.Z`) 
  * `cd` to the ` machineX/root directory`: `cpio -i < /export/archive`

5\. Boot the machine with `zoneadm -z machineZ boot` and log in -- the devices
will be built at that time. Sysid information is normally required at this
point ...

Don't forget: send an invitation for coffee to
[D@vidSteed.com](mailto:D@vidSteed.com) and I will accept!

