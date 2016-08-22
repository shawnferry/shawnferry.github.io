---
layout: post
title: 'CEC: RAS Trends From Andromeda to ZFS'
date: '2006-10-03'
author: Shawn Ferry
tags:
- CEC 2006
- Sun CEC
- b.s.c.
modified_time: '2010-04-29T10:20:48.090-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-68003758129293474
blogger_orig_url: http://blog.shawnferry.com/2006/10/cec-ras-trends-from-andromeda-to-zfs_1163.html
---

Richard Elling, PAE  
  
Performance, Availability &amp; Architecture Engineering  

He remembered to do the session recording start  
information...So far the second session I have attended to do so.  
  
(We forgot to do it, bad presenters)  

Internal changes to the process in the past few years have  
made a big difference in both pre-release quality and post-release  
support.  

**MTBF, Heat, Moving Parts**  

Starting with some  
  
MTBSI == Mean Time Between Systems Interruption  
  
Similar to previous usage of MTBF, now Failure does not have to mean  
System/Service Interruption  

More Parts --&gt; lower MTBF (Bad)  

[1] Integration == fewer parts == Higher MTBF

Most common failures Fans, Power Supplies, Disks, Memory  
  
(glad he said that, I say it too...woot validation)  

PowerSupply -- Built in surge supressor  
  
Designed for 1M hours MTBF (Niagara and Galaxy vs PC at ~83K hours)  
  
Use fewer Better more expensive power supplies  

Fans -- Fail too often moving parts  
  
Hard to implement fans in small systems  

System policies can affect MTBSI...e.g. T1000 firmware starts  
a shutdown on fan failure, not on system temperature  
  
This may not be the preferred method but it is in the spec.  

Heat Kills, run cool, run for a long time  

Disk numbers from Seagate, rule of thumb  

+15deg C == MTBF / 2  

Smaller disks run cooler and have faster seek (seagate  
website)  

**Boot Disk Trends**  
  
Nearly 100% of Sun's customers mirror boot disks in the DC  

Software raid is popular but built in HW raid is becoming more  
popular, ZFS boot disks coming

Long term: solid state, CF slot in CP3010 and CP3020 also  
Moore's law  

SAS vs Parallel SCSI...system now knows if the disk disappears  
before it tries to access it.  

In case you weren't sure More parts == lower MTBF  
  
See 1  

**Memory**  
  
Memory Page Retirement -- FMA  

What about mirrored memory?  
  
Will anyone use it? Hey double memory sales!  
  
(Probably only in the highest RAS demand environments)  

See 1  

**Processors**  
  
More transistors -- more self test circuits -- More redundancy  

See 1  

**OPL**  
  
Mainframe class reliability in a Solaris system  

**Software**  
  
Solaris FMA (Fault Management Architecture)  
  
(_Really cool, useful messages, diagnosis codes, get a system  
and break things to see it  
  
you could pull a disk to see an example however this is not Richard's  
top choice in terms of display)  
_

**_SMF  
  
_**Before: Panic  
  
SMF: Restart  
  
Ability to define relationships  
  
(See my previous posts and/or [Liane  
Praza's Weblog](http://blogs.sun.com/lianep/))

**_ZFS_**_  
  
_Data reliability...we know if we get corrupted data before  
we try to use it.  
  
(_This is also cool, I have been running ZFS at home for at  
least 9 months, although I don't own any huge storage hardware for the  
complex case)  
  
_  
  
Data recovery time based on data space used not size of FS.  
  
Not yet fully integrated with FMA, depends on driver, disks and system  
implementation.  

**Virtulization  
  
Good**  
  
Really fast boot! == lower MTTR  
  
Guest OS portability  
  
Hide faults from OS

**Bad  
  
**Hides faults from OS (If the OS knows there is a fault  
it can respond appropriately)  
  
Scheduling...Real Time app or time-sensitive devices  

**Fail  
in Place  
  
**Possibly a better term "Deferred Repair  
Strategy"  
  
_(We use this in Grid...with 1000s of nodes do you really need  
a service call for a single failure? We say no)_  
  
From here we go from MTBF to MTBSI, non-service interrupting failure  

**Scrubbing  
  
**We scrub just about everything, looking for  
faults and corruption before we try to use bad data.  

**Reduction  
in Parts**  
  
X4100 Rack  

78 Power supplies  

E25K  

6 Power supplies  

Blade 8000  

6 Power Supplies  

(Similar for fans)  

Why then would you deploy a rack of X4100s instead of Blades  
unless you have a specific need.  

**Get  
More Information  
  
**OpenSolaris Forums...Participate 

b.s.c/relling  

