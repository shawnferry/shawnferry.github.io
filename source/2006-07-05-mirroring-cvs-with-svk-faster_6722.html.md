---
layout: post
title: Mirroring CVS with SVK (Faster)
date: '2006-07-05'
author: Shawn Ferry
tags:
- SVN
- SCM
- yakshaving
- CVS
- SVK
- b.s.c.
modified_time: '2010-04-29T10:24:36.912-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-4691863585248273421
blogger_orig_url: http://blog.shawnferry.com/2006/07/mirroring-cvs-with-svk-faster_6722.html
---

As I stated
href="http://blogs.sun.com/roller/page/yakshaving?entry=version_control_with_svk">previously  
SVN and SVK have filled the void in my heart related to Distributed  
Version Control.  
  
SVK "plays well with others", the company formerly known as SevenSpace  
used CVS to track changes to more than 20K configuration files.  
Unfortunately the CVS server is horribly IO bound and even working on a  
fast link is painfully slow.  
  
[MirrorVCP](http://svk.elixus.org/?MirrorVCP)  
describes the general use of SVK to mirror other repositories. The  
problem I faced was my general level of impatience waiting on  
synchronization with the CVS server. Originally I thought the problem  
was strictly on the server side, but I later realized that a cvs update  
was significantly faster.  
  
While poking around to try and figure out what was going on to make it  
so slow I found [FasterMirrorCVS](http://svk.elixus.org/?FasterMirrorCVS)  
which drastically decreased the time to pull changes from the CVS  
repository.  The FasterMirrorCVS page indicated that one of  
the outstanding issues with the change was a lack of support for more  
than one module from the same CVSROOT.  So I extended the
href="http://svk.elixus.org/plugin/attachments/FasterMirrorCVS/cvs.pm.patch">patch  
a bit and  added support for multiple modules, informational  
and error logging and more error checking.  
  
The quick overview from FasterMirrorCVS  
  
Given the following two repositories with max symmetric network transit  
speeds of ~330KB/s  
on a single file scp.  

The same sync operation was performed both with and without caching  
enabled. No updates were pending in either direction.  
The CVS server is highly IO bound and I would expect greater  
improvements from a faster source.  

389MB 20322 files  
//mirror/cvs/ops cvs::ext:sferry@cvs:_cvsroot:ops_...  

    No Cache: svk sync --all //mirror/cvs/ops 176.94s user 59.24s system 13% cpu 28:26.70 total  
       Cache: svk sync --all //mirror/cvs/ops 29.81s  user 17.10s system 4% cpu 18:37.83 total  
    
6.2MB 178 files  
//mirror/cvs/dev cvs::ext:sferry@cvs:_cvsroot:dev_...  

    No Cache: svk sync --all //mirror/cvs/dev 7.28s user 1.14s system 20% cpu 40.908 total  
       Cache: svk sync --all //mirror/cvs/dev 5.59s user 0.65s system 30% cpu 20.502 total  
    
Technorati Tags: [](http://technorati.com/tag/SVK) rel="tag">SVK,
href="http://technorati.com/tag/SVN" rel="tag">SVN,  
[CVS](http://technorati.com/tag/CVS),  
[SCM](http://technorati.com/tag/SCM)  

