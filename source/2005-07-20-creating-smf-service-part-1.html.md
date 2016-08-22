---
layout: post
title: Creating an SMF service (Part 1)
date: '2005-07-20'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:36.937-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-6571725129119953356
blogger_orig_url: http://blog.shawnferry.com/2005/07/creating-smf-service-part-1.html
---

[![](http://blogs.sun.com/roller/resources/yakshaving/2005_07_20_23-03-06-318_n1.small.jpeg)](http://blogs.sun.com/roller/resources/yakshaving/2005_07_20_23-03-06-102_n0.large.jpeg)  
  
This is Part 1 of an attempt to document creating a new service in SMF.

Resources:  
  
smf(5) and other related manpages  
  
/usr/share/lib/xml/dtd/service_bundle.dtd.1  
  
[BigAdmin SelfHealing Site](http://www.sun.com/bigadmin/content/selfheal)  
  
[BigAdmin Developer SMF
Intro](http://www.sun.com/bigadmin/content/selfheal/sdev_intro.html)  
  
[BigAdmin SMF QuickStart](http://www.sun.com/bigadmin/content/selfheal/smf-
quickstart.html)  
  
[docs.sun.com Solaris 10 Admin
Guide](http://docs.sun.com/app/docs/doc/817-1985/6mhm8o5n6?a=view)  
  
[Ben Rockwood's SMF Manifest
Cheatsheet](http://www.cuddletech.com/blog/pivot/entry.php?id=182)

The goal is to create a SMF manifest to start a monitoring daemon. (maybe one
of those mugs as well)

Before going doing much poking around. I was expecting that I would create a
binary instance and a script instance. I would like to be able to have a
single manifest that contains all of the services/instances/bits I need.

There are two complementary functions a binary and a script. Recently the
script has been modified it now has two operation modes. A "Full" mode and a
compatability mode.

I will try to document the problems I run into and the solutions I find.

[Part
2](http://blogs.sun.com/roller/page/yakshaving?entry=creating_an_smf_service_part1)  
  
topic:{technorati}[Solaris]  
topic:{technorati}[SMF]  

