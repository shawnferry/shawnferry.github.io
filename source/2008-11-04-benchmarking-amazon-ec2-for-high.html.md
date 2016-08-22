---
layout: post
title: Benchmarking Amazon EC2 for High-Performance Scientific Computing
date: '2008-11-04'
author: Shawn Ferry
tags:
- computing
- grid
- Untitled
- cloud
- performance
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:17:26.785-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-6897935951377302008
blogger_orig_url: http://blog.shawnferry.com/2008/11/benchmarking-amazon-ec2-for-high.html
---

My copy of ;Login: arrived ~~yesterday~~ last week. As I was reading it over a
quick breakfast of oatmeal and coffee the article "[Benchmarking Amazon EC2
for High-Performance Scientific Computing
(PDF)](http://www.usenix.org/publications/login/2008-10/openpdfs/walker.pdf
"PDF of Benchmarking Amazon EC2 for High-Performance Scientific Computing" )"
by Edward Walker caught my eye. Having worked with our own grid services at
Network.com I understand that there is a tradeoff in terms of cost/value and
performance and didn't find the gist of the results to be surprising.

In my opinion, a purpose built high bandwidth, low latency grid/cluster is
generally going to outperform the lower cost more generic and I believe
possibly more flexible solution. On the other hand, in the utility model the
consumer of the service/resources doesn't really have to worry about the TCO
of the service or the cost of management and up front implementation.

In short: if you have the time, money, talent and desire feel free to build
your own. If not it may be worth while to trade off some of that performance
for convenience.

