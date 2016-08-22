---
layout: post
title: 'CEC: Concerning Capacity'
date: '2007-10-10'
author: Shawn Ferry
tags:
- Sun CEC
- CEC 2007
- Sun
- b.s.c.
modified_time: '2010-04-29T10:17:26.909-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-6946976822167990009
blogger_orig_url: http://blog.shawnferry.com/2007/10/cec-concerning-capacity.html
---

Bob Sneed

Bob is a fount of knowledge, I highly recommend any
course/session/conversation with him. Unfortunately we are trying to pack what
could be days of discussion into a tiny fraction of the time.

Bob was wondering if anyone had an LG phone charger, his is dead.

Why capacity: reduce capacity escalations, raise awareness

What is capacity: Submarine 100% underwater vs. at crush depth (the physical
metaphor is what people understand) CPU 100% vs. unacceptable application
performance.

Look for the Business problem not some easily observed numbers from the system
(CPU, IOPS)

Capacity done wrong: over-provisioning

  * HW is cheap, why not buy more (power, cooling)
  * good if you sell computers
  * better safe than sorry (problems pointed at insufficient HW(why didn't you buy more, it is cheap)

Bad QoS management in Small Iron == Bad QoS management on Big Iron

  * Over provisioning reduces the incentive to "do it right"
  * eco-reckless
  * Inefficiencies on small hardware are MORE inefficient on big hardware, only you can waste more before it is a problem

Not a problem when: Done wrong but no one cares (performance perception can be
a major factor in escalations)

Utilization has no "quality" dimension it is a measurement of busy.
Utilization does not reflect the performance of useful work.

See Adrian's blog or paper (search on) "utilization is a virtually useless
metric"

Without Business Metrics all you have are a bunch of numbers.

![](http://lalartu.smugmug.com/photos/206520410-S.jpg)  

[The whole gallery, being updated as frequently as
possible.](http://lalartu.smugmug.com/gallery/3612295#P-3-15 "Sun CEC 2007
Gallery" )  

