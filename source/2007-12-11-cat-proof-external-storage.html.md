---
layout: post
title: Cat Proof External Storage
date: '2007-12-11'
author: Shawn Ferry
tags:
- Leopard
- storage
- ZFS
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:17:26.858-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-4042865872973828706
blogger_orig_url: http://blog.shawnferry.com/2007/12/cat-proof-external-storage.html
---

What happens when you disconnect the drive on the fly?  
(e.g. set up a tripping hazard for your cat)  
  
You get this message  

![The device you removed was not properly put away.Data might have been lost
or damaged. Before youunplug your device, you must first select its icon inthe
Finder and choose Eject from the File
menu.](http://lalartu.smugmug.com/photos/231335372-L.jpg)

What happens when you do the same thing to a ZFS pool? This is no different  
than what you get on Solaris, except it is on my Mac Book Pro.  

>

>       pool: p1  
>     >    state: DEGRADED  
>     >   status: One or more devices could not be opened.  Sufficient
replicas exist for  
>     >       the pool to continue functioning in a degraded state.  
>     >   action: Attach the missing device and online it using 'zpool
online'.  
>     >      see: http://www.sun.com/msg/ZFS-8000-D3  
>     >    scrub: scrub completed with 0 errors on Sat Dec  8 01:44:00 2007  
>     >   config:  
>     >  
>     >       NAME         STATE     READ WRITE CKSUM  
>     >       p1           DEGRADED     0     0     0  
>     >         mirror     DEGRADED     0     0     0  
>     >           **disk7s1  UNAVAIL      0   114     0  cannot open**  
>     >           disk0s3  ONLINE       0     0     0

Fortunately for me, my system keeps merrily chugging along.  
  
Unfortunately (sort of) and this is much more of an issue on my Mac than on  
any other system I am adding and removing devices much more frequently.  
  
When I last connected the external disk and imported the pool I had 6  
more "disks" visible to the system (really a usb thumb drive) another FW  
device and a few SW RAID devices. This caused my nice portable external  
FW disk to appear as disk7.  

>

>       diskutil list  
>     >   /dev/disk0  
>     >      #:                       TYPE NAME                    SIZE
IDENTIFIER  
>     >      0:      GUID_partition_scheme                        *149.1 Gi
disk0  
>     >      1:                        EFI                         200.0 Mi
disk0s1  
>     >      2:                  Apple_HFS noroute                 88.9 Gi
disk0s2  
>     >      3:                  Apple_HFS                         57.0 Gi
disk0s3  
>     >      4:                  Apple_HFS noroute_2_1_2           2.6 Gi
disk0s4  
>     >   /dev/disk1  
>     >      #:                       TYPE NAME                    SIZE
IDENTIFIER  
>     >      0:     FDisk_partition_scheme                        *55.9 Gi
disk1  
>     >      **1:                  Apple_HFS                         55.9 Gi
disk1s1**  
>     >

The difference between the zpool status and diskutil list output  
is the device configured for the pool is disk7s1 not the currently visible  
disk1s1.  
  
I don't know how to recover from this situation live. If the device has not  
renumbered you just zpool online p1 disk7s1 to reconnect on a different  
device number I export and import the pool again.  
  
To state that another way, I don't know how to effectively do:  
zpool online p1  
similar to  
zpool replace p1  
(the replace, even with -f, failed telling me that  was busy)  
  
I have just requested a pre-release tarball that is reported to fix some bugs  
in the current beta. We will see how it goes.  
  
Other Observations:  
  
Once you load the RW kext, it appears that anyone can do anything to the ZFS
pools  
and filesystems. On my single user system this is OK. I am the only user and
guest  
accounts are disabled. e.g. Initiate a scrub as a non-admin guest account or
create a  
snpashot as nobody (sudo -u nobody zfs snapshot p1/Music@nobody)  
  
You CANNOT download music from the iTunes store in the current beta. you get a  
permission error. I can however add music from local files.  
  
Edit: Cleaned up the block quotes a bit, still not really rendering as one
might hope.  

