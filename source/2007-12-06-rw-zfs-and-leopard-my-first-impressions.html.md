---
layout: post
title: RW ZFS and Leopard, my first impressions
date: '2007-12-06'
author: Shawn Ferry
tags:
- Leopard
- ZFS
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:17:26.869-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-5942657632163518136
blogger_orig_url: http://blog.shawnferry.com/2007/12/rw-zfs-and-leopard-my-first-impressions.html
---

Not long ago I posted about [installing the RW ZFS beta seed on Leopard after
updating to
10.5.1](http://blogs.sun.com/yakshaving/entry/how_to_get_experimental_rw
"installing RW ZFS seed" ).  
  
As I said after the end of that post, It does work. On the one hand I haven't
had this many system  
panics on a mac ever. On the other hand I have repartitioned my laptop and now
have ZFS pools  
mirrored to an external USB stick and a firewire drive.  
Since the simple tests below I have re-installed to get more partitioning
flexibility. I couldn't do a live  
repartition to get the space I wanted. Every attempt to repartition after the
first was informing me that  
there wasn't enough space.  
  
After pulling the drive but before directing any traffic to the pool:  
  
> errors: No known data errors  
>  
>  pool: pool  
>  state: ONLINE  
>  scrub: resilver completed with 0 errors on Mon Dec 3 15:44:43 2007  
>  config:  
>  
>  NAME STATE READ WRITE CKSUM  
>  pool ONLINE 0 0 0  
>  mirror ONLINE 0 0 0  
>  disk0s3 ONLINE 0 0 0  
>  disk1s1 ONLINE 0 0 0  
>  
>  errors: No known data errors  
>

After initiating a scrub on the pool:  
  
> pool: pool  
>  state: DEGRADED  
>  status: One or more devices could not be opened. Sufficient replicas exist
for  
>  the pool to continue functioning in a degraded state.  
>  action: Attach the missing device and online it using 'zpool online'.  
>  see: http://www.sun.com/msg/ZFS-8000-D3  
>  scrub: scrub in progress, 11.12% done, 0h1m to go  
>  config:  
>  
>  NAME STATE READ WRITE CKSUM  
>  pool DEGRADED 0 0 0  
>  mirror DEGRADED 0 0 0  
>  disk0s3 ONLINE 0 0 0  
>  disk1s1 UNAVAIL 0 0 0 cannot open  
>  
>  errors: No known data errors  
>  
>  zpool online pool disk1s1

Disconnect while writing files to the volume via rsync:  

> pool: pool  
>  state: DEGRADED  
>  status: One or more devices could not be opened. Sufficient replicas exist
for  
>  the pool to continue functioning in a degraded state.  
>  action: Attach the missing device and online it using 'zpool online'.  
>  see: http://www.sun.com/msg/ZFS-8000-D3  
>  scrub: resilver completed with 0 errors on Mon Dec 3 15:56:10 2007  
>  config:  
>  
>  NAME STATE READ WRITE CKSUM  
>  pool DEGRADED 0 0 0  
>  mirror DEGRADED 0 0 0  
>  disk0s3 ONLINE 0 0 0  
>  disk1s1 UNAVAIL 0 595 0 cannot open  
>  
>  errors: No known data errors  
>

When the disk went offline the IO slowed (somewhat dramatically), when it came
back online it also slowed (again dramatically for ~1-2s). The resilver
started and writing data to the pool while resilvering slowed everything down
a bit but nothing horrible.  
  
More to come. On a day to day basis I am running iTunes and my music library
on ZFS.  
  
My first hint: I recommend turning off spotlight indexing on the ZFS volume it
seems SIGNIFICANTLY more stable.

