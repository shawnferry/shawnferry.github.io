---
layout: post
title: 'CEC: Dtrace Approaches to Real-World Problems'
date: '2006-10-06'
author: Shawn Ferry
tags:
- CEC 2006
- Sun CEC
- Dtrace
- b.s.c.
modified_time: '2010-04-29T10:20:48.074-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-6067789979367558061
blogger_orig_url: http://blog.shawnferry.com/2006/10/cec-dtrace-approaches-to-real-world_9600.html
---

Jim Fiori  

Apparently the presentation  
is normally a 3+ hour presentation and Jim requested 3 hours, but all  
the slots are 60min.  
  
This is a presentation on approach not learning D.  
  
This entry is a horrible hack job on content, too much and I am  
learning more than I  
  
can easily condense/digest usefully on the fly.  

**INTRO  
  
**

Everyone needs  
dtrace...but it is not the first thing to run.  
  
Identify a possible issue use Dtrace to figure out what is going on.  
  
Advice: Practice, Practice, Practice**  
**

**Approach  
  
**

use the manual all  
examples are in /usr/demo/dtrace  
  
use quantize(), min()/max()/avg() can hide data**  
**  
  
****  
  
Be careful using the PID provider it can impose load on a highly active  
process.  
  
****  
  
****Normal system tools still have their  
place.  

New tools: intrstat _(some  
others I haven't used)  
  
_****

**Privs** \-- Root level or RBAC...**  
**

**Zero Probe Effect** \-- via  
instruction replacement**  
**

**Scenarios  
**

High User Time

hotuser.sh dtrace toolkit  
  
C++ Apps  
  
Watch for small  
allocations and short allocations  
  
High System Calls  
(&gt;100s)  
  
use aggregation  
  
use pfiles to determine target of File descriptor  
  
System time (&gt;10%  
or user:sys near 2:1)  
  
prsatst to  
find it, dtrace to examine it  
  
Threaded App.  
  
prstat to find it  
  
plockstat ... to see it, single process  
  
Java  
  
use jstack  
  
Java 1.6 has static dtrace providers  
  
Oracle  

Look at I/O and File  
systems first  
  
ONLY after regular investigation by DBA (statspack etc)  
  
Sybase  

Watch for TCP Nagle  
(buffering requests before sending)  
  
Try TCP no delay on client and server  

File system  
  
Watch for  
periodic pauses check autoup in large memory (&gt;8G) systems  

**  
  
Hints and Tools  
**

  * Use a sample rate at not quite 1000, to help keep the Dtrace  
probe func from running with kernel actions  
  
  * Dtrace is running in kernel context...you can't just dump  
the memory location, you need to dump the user memory space  
  
  * .mul() and .div() are SPARC V7...the application should be  
recompiled**  
**  
  
  * -c flag...use pid$target ... woot  
  
  * look for system call errors, when things are not working as  
expected  
  
  * see atomic_ops(3c) on mutex_lock/unlock for simple variable  
updates**  
**  
  
  * High ctx...look at FX scheduler (good for some apps)  
  
  * High Migrations...look at binding to a specific processor**  
**  
  
  * libumem for memory leaks  
  
  * Chime (GUI...uses Sparklines!!!...I like sparklines)  
OpenSolaris**  
**
  
**Coming  
**

  * RFE 6311947 svn_43...fun/mod/ufunc/umod  
  
  * Dtrace in a zone  
  
  * aggregation printing  
  
****
  
