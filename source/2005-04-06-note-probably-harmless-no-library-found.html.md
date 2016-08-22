---
layout: post
title: 'Note (probably harmless): No library found for -ldb-4.2'
date: '2005-04-06'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:37.054-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-4223438904790432846
blogger_orig_url: http://blog.shawnferry.com/2005/04/note-probably-harmless-no-library-found.html
---

Why is it that "probably harmless" really isn't? I am compiling mod_perl 2  
from the bleeding edge source. Thinking that as usual that is a lie, I  
went ahead and compiled it anyway. Although I didn't see any messages  
indicating that something was wrong 'make test' informed me otherwise. The  
problem is that I have BerkeleyDB installed in /usr/local/BerkeleyDB.4.2  
and I don't see a why to fix the library path to use that location. crle  
-u -l /usr/local/BerkeleyDB.4.2/lib added the path to my library search   
list, but that didn't seem to do it, but starting down the road to a  
configuration nightmare I added symbolic links from  
/usr/local/BerkeleyDB.4.2/lib/* to /usr/lib Now that it is compiled I  
should see if I can remove the links, the actual execution should respect  
the library search path.  

