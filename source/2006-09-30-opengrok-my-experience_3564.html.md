---
layout: post
title: 'OpenGrok: My Experience'
date: '2006-09-30'
author: Shawn Ferry
tags:
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:24:36.898-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-8534347244143766443
blogger_orig_url: http://blog.shawnferry.com/2006/09/opengrok-my-experience_3564.html
---

A few months ago I decided to install OpenGrok on my laptop so I could search
the source and configuration files that we are using.  

It was simple, I downloaded [OpenSolaris Project:
OpenGrok/](http://www.opensolaris.org/os/project/opengrok/) and
[Glassfish](http://java.sun.com/javaee/community/glassfish/) and installed and
was up and running without any problems on my OS X 10.4.7 PowerBook. It was
wonderful, I could search to my hearts content. Finding random bits of code
that I wanted to reference in new work, all of the uses of "slapd" in a large
pile of old monitoring configuration files or files that I knew existed but
wasn't sure exactly where.  

The only problem was that as installed it was impractical to share, I could
point people to my http://mylaptop:8080/grok. But that was not so useful if I
wasn't in the office or hadn't started the server. To rectify this problem I
decided to install OpenGrok on our CVS/SVN/SCM server.  

The following are some observations that may already exist in the wild, but I
missed while poking around.  

As usual read the instructions Glassfish does not run on Solaris 8, you can
save maybe an hour or more of downloading and installation failure if you read
a bit more. Particularly if you are installing on a old overloaded Netra T1.  

  * Give the classifier more memory, you will run out on some of those large jar files that someone added I am setting max to 768MB.
  
  * OpenGrok really does a great job, I can find an old monitoring config based on a half remember comment
  
  * People really don't know what they are missing until they get a hold of it
  
  * paths.tsv...entries are relative to SRC_ROOT
  
  * Although it seems clear now, point to the checked out base of your source
  
