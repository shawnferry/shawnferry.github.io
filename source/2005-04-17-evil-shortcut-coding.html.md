---
layout: post
title: Evil Shortcut Coding
date: '2005-04-17'
author: Shawn Ferry
tags:
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:24:37.016-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-4213496850652606226
blogger_orig_url: http://blog.shawnferry.com/2005/04/evil-shortcut-coding.html
---

osver = `uname -r` if [ $osver -ge "5.10" ]; then zonename =  
`/usr/bin/zonename` fi Unfortunately 5.10.1 isn't a number. -x  
'/usr/bin/zonename' is probably better form in any case.  

