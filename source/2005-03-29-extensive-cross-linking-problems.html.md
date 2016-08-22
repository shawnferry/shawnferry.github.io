---
layout: post
title: Extensive Cross Linking Problems
date: '2005-03-29'
author: Shawn Ferry
tags:
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:24:37.070-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-183757867532738005
blogger_orig_url: http://blog.shawnferry.com/2005/03/extensive-cross-linking-problems.html
---

So I just ran into an initially non-obvious problem. Given: foohost1  
/usr/local/foo -&gt; /realfoo bar: /usr/local/foo -&gt; /realfoo Where the  
contents of /realfoo are superficially identical on bar mount: foohost1:/  
/tmp/foohost1 foohost1:/realfoo /tmp/foohost1/realfoo So knowing about the  
links everything is clear, but if you are not thinking about them or are  
unaware that they exist you get confusion. The problem: /tmp/foohost1 ==  
foohost1:/ /tmp/foohost1/realfoo/one/two == foohost1:/realfoo/one/two  
/tmp/foohost1/usr/local/foo/one/two == bar:/realfoo/one/two so if you were  
to rsync /tmp/foohost1/realfoo/one/two AND  
/tmp/foohost1/usr/local/foo/one/two In reality you are smashing the two  
trees together the result in this case being strange application behavior  

