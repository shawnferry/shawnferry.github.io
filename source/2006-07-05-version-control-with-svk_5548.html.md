---
layout: post
title: Version Control with SVK
date: '2006-07-05'
author: Shawn Ferry
tags:
- SVN
- SCM
- yakshaving
- CVS
- SVK
- b.s.c.
modified_time: '2010-04-29T10:24:36.914-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-2755452181419234992
blogger_orig_url: http://blog.shawnferry.com/2006/07/version-control-with-svk_5548.html
---

 I was a big fan of BitKeeper when it was free for open source  
development, and I still miss a number of it's features. Oh well. At  
this point however it's place in my heart has been replaced with
href="http://subversion.tigris.org/">SVN and  
further [SVK](http://svk.elixus.org/).  
SVK allows for one of the things I miss the most, decentralized version  
control. With SVN I was commonly frustrated when I wanted to push back  
code but wasn't:  

  * online
  
  * finished
  
I commonly found myself in the position of having code that I was done  
with, or having made some changes I was (at least relatively) sure  
about and needing/wanting to commit.  With the goal being to  
move on to another change, modification or new function. Sometimes  
being quite sure that what I really wanted to do was poke around but  
not have to worry about taking a snapshot of the current code to get  
back to it after I took a horribly wrong approach.  
  
SVK fills that void, by using SVN as a base but keeping a local copy  
and allowing mirroring and syncronization I can commit at will  
effectively check pointing code whenever I think I have completed  
something or when I know I am about to go off the rails with some wild  
idea.  
  
SVK also makes rapid development/testing easier, since the workspace in  
SVK is a flat filesystem using rsync to push changes to a test system  
is quite effective without the  overhead of copying the local  
.svn structure to the test host.  
  
A bit more on mirroring particularly CVS later.  
  
See also: [The SVK Wiki](http://svk.elixus.org/),  
[The  
Subversion Project Page](http://subversion.tigris.org/)  
  
Technorati Tags: [](http://technorati.com/tag/SVK) rel="tag">SVK,
href="http://technorati.com/tag/SVN" rel="tag">SVN,  
[CVS](http://technorati.com/tag/CVS),  
[SCM](http://technorati.com/tag/SCM)  

