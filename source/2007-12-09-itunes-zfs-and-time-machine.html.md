---
layout: post
title: iTunes, ZFS and Time Machine
date: '2007-12-09'
author: Shawn Ferry
tags:
- Leopard
- ZFS
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:17:26.865-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-8647498334306562103
blogger_orig_url: http://blog.shawnferry.com/2007/12/itunes-zfs-and-time-machine.html
---

I have been blogging about ZFS on OS X recently  
[RW ZFS and Leopard first
thoughts](http://blogs.sun.com/yakshaving/entry/rw_and_zfs_leopard_my "RW ZFS
and Leopard first thoughts" )  
[How to get experimental rw ZFS support AFTER upgrading to
10.5.1](http://blogs.sun.com/yakshaving/entry/how_to_get_experimental_rw "How
to get experimental rw ZFS support AFTER upgrading to 10.5.1" )  
  
As a way to start using ZFS on OS X on a daily basis I have migrated the
contents of  
my Music folder to a ZFS mirrored pool composed of an external disk and a
matching  
internal partition.  
  
After a slightly rocky start including reinstalling my laptop so I could do
the repartitioning  
I wanted and a fairly high number of initial crashes things currently seems to
be going well.  
  
One of the things that I have noticed is that my iTunes Music folder and
Library are polluted.  
Now embedded in my library with the same name as a number of songs I have
'.htm' files.  
(./path/to/foo.mp3 also has a matching ./path/to/foo.htm in a large number of
cases)  
The files are part of some O'Reilly books I have on CD, as such I know "where"
they came from, sort of.  
(My library from my pre-experimentation CCC backup also has the same
corruption)  
  
A restore from my Time Machine backup is in order to get things back to a
known good state. Or a recovery  
from my slightly older CCC backup which is also not corrupted.  
  
I am not particularly impressed with the Time Machine interface. It is novel
and seems relatively functional  
for finding the most recent backup by using the timeline on the right.  
  
[Time Passes]  
  
The Time Machine restore failed after ~8GB on a couldn't read/write a file
error aborting the whole restore.  
I do have to admit that the backups are working I can browse the backup
structure. The use of hard links and  
metadata is cool. Restoring via rsync does the trick, possibly faster as well
since it doesn't copy unchanged files.  
  
Even better I don't see a log, I am feeling somewhat less than impressed with
the feedback from and utility  
of Time Machine from a administrator's point of view. Am I asking for too
much? A log of the restore with an  
indication of the failure?  
  
I have a feeling that the restore is really just a copy in Finder. An
automator type action passing the  
selection to Finder and executing a copy.  
  
Again what finally worked for me was a combination of methods. I used the Time
Machine interface to find the  
backup I wanted and CMD-I to see the path to the directory (although
Terminal.app would have been just as good).  
Once I found the backup I wanted rsync worked quite well restoring 48GB of
Music in 12,273 files.  
(rsync --archive -P /Volumes/TimeMachine/.../Music/iTunes
~sferry/Music/iTunes)  
  
