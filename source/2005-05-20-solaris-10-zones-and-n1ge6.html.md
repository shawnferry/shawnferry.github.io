---
layout: post
title: Solaris 10 Zones and N1GE6
date: '2005-05-20'
author: Shawn Ferry
tags:
- grid
- solaris
- zones
- b.s.c.
- solaris10
modified_time: '2010-04-29T10:24:36.996-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-8614481306427795353
blogger_orig_url: http://blog.shawnferry.com/2005/05/solaris-10-zones-and-n1ge6.html
---

I am trying to decide if it is cool or if I have no life(this is generally  
rhetorical) I recently (last night) created some more zones on one of my  
machines. Subsequently I installed N1 Grid Engine 6. The install was  
surprisingly easy. Literally 1) Install Packages 2) run  
$SGE_ROOT/install_qmaster 3) share $SGE_ROOT via nfs 4) mount shared  
$SGE_ROOT at $SGE_ROOT on each node 5) run $SGE_ROOT/install_execd on each  
node 6) run jobs $SGE_ROOT/examples/jobs/pascal.sh 200 Things I have found  
out: 1) 50000 jobs in simple queuing results in horrible io wait on an  
underpowered PC         e.g. qstat may as well never respond for how long  
it takes at 99% io wait 2) 20000 jobs in BerkeleyDB queuing isn't to bad,  
but it will be a while before they are done running.         e.g. qstat  
takes 3s to return the list (15727 entries currently) Things to try: 1)  
add the little PCG-U3 laptop as an execution host 2) add my powerbook as  
an execution host 3) add C-'s ibook as an execution host 4) pascal.sh 500,  
just to see if 125250 jobs will kill it Remaining Jobs at 60s + qstat run  
time intervals Fri May 20 17:40:56 EDT 2005 | 15688 Fri May 20 17:42:04  
EDT 2005 | 15673 Fri May 20 17:43:14 EDT 2005 | 15661 Fri May 20 17:44:26  
EDT 2005 | 15646 Fri May 20 17:45:37 EDT 2005 | 15631 Fri May 20 17:46:44  
EDT 2005 | 15616 Fri May 20 17:47:56 EDT 2005 | 15604 Fri May 20 17:49:07  
EDT 2005 | 15589 Fri May 20 17:50:15 EDT 2005 | 15574 Fri May 20 17:51:27  
EDT 2005 | 15562 About the server: s10_69 (still haven't gotten around to  
the upgrade to Nevada Build 14 I want to see New Boot) System  
Configuration: Sun Microsystems i86pc Memory size: 768 Megabytes AMD: K6  
600MHz Currently Running 5 zones(3 execution hosts, apache, torrus  
collector) Technorati Tag: [  
grid](http://technorati.com/tag/grid) Technorati Tag: [  
solaris](http://technorati.com/tag/solaris) Technorati Tag: [  
solaris10](http://technorati.com/tag/solaris10) Technorati Tag: [  
zones](http://technorati.com/tag/zones)  

