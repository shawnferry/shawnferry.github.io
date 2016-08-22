---
layout: post
title: What not to do with your Lab v20z
date: '2005-07-01'
author: Shawn Ferry
tags:
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:24:36.949-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-383237549631165295
blogger_orig_url: http://blog.shawnferry.com/2005/07/what-not-to-do-with-your-lab-v20z.html
---

...at least when someone starts using it while you are messing with the  
Service Processor Sensor settings.  
  
 `sensor set -i ambienttemp -c 29.23 -w 29.24 -W 29.26 -C 29.27`  
  
Particularly when the operating temprature is ~29 - 31 deg C. We got a  
bunch of SNMP traps as the temp. bounced between the thresholds. The  
goal was to make sure we were receiving traps properly. Then we got a  
platform state change trap...system power off.  

A coworker started working with the system during this time and after  
turning it on began to create a flar. Naturally after a short time the  
system shut down again.  

He came over and asked if I shut the system down. My answer: No  

I told him that if he would give me a moment I would reset the sensors  
so it would shut itself down again.  

In any case it all worked as expected.  

topic:{technorati}[v20z]  

