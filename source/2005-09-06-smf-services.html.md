---
layout: post
title: SMF services
date: '2005-09-06'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:36.925-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-7283428933146235804
blogger_orig_url: http://blog.shawnferry.com/2005/09/smf-services.html
---

I will post the last part of my creating an SMF service saga shortly.  
  
However, I feel that I should comment on a very obvious difference  
between a couple of commands.  

smf: not a command  

svcs: smf service status command  

svn: a version control system and command  

Various combinations of svn -l "*fmri*", svcs commit, smf status are not  
helping me get work done.  

I find that I am having occasional issues trying to use svn track  
changes to smf services.  

