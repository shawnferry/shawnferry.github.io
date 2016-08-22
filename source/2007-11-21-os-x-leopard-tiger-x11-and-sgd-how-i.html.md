---
layout: post
title: |-
  OS X Leopard, Tiger X11 and SGD (How I downgraded to Tiger's X11 and
  got SGD working again)
date: '2007-11-21'
author: Shawn Ferry
tags:
- X11
- howto
- apple
- Leopard
- sgd
- Tiger
- b.s.c.
modified_time: '2010-04-29T10:17:26.875-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-5101048108142785146
blogger_orig_url: http://blog.shawnferry.com/2007/11/os-x-leopard-tiger-x11-and-sgd-how-i.html
---

We use SGD to provide access to few applications. After installing Leopard I
could focus and click with a mouse but all keyboard input was ignored for SGD
applications.  

Searching online I found some indications that X11 in Leopard has some
application interaction issues. The solution presented in a number of
different forums for various applications was to downgrade to Tigers X11.app.
I tried methods from a couple of posts and didn't have success. Instead I
mixed and matched the steps from a couple of suggestions and found a solution
that worked for me.

**I have not tried to recover from this change, You can PROBABLY re-install X from the leopard DVD.**

**When this is complete you will probably have two X icons in the Dock when X11.app is running.**

The steps can be summarized as:

  1. Download X11 Update 2006 1.1.3: "http://www.apple.com/support/downloads/x11update2006113.html"
  2. Destroy your current X11 installation
  3. Install X11 update 2006
  4. Change the path to your window manager in xinitrc
  5. reboot

> wget 'http://wsidecar.apple.com/cgi-bin/nph-
reg3rdpty2.pl/product=12045&amp;cat;=60&amp;platform;=osx&amp;method;=sa/X11Update2006.dmg'  
>  open X11Update2006.dmg  
>  sudo launchctl unload -w /System/Library/LaunchAgents/org.x.X11.plist  
>  sudo rm -R /usr/X11R6  
>  sudo ditto -Vx --noqtn /Volumes/X11\ Update\
2006/X11Update2006.pkg/Contents/Archive.pax.gz /  
>  sudo perl -i -p -e 's:exec quartz-wm:exec /usr/X11R6/bin/quartz-wm:g'

>

> The instructions I found online indicate that a log out/log in should do it.
I found that it didn't seem to start working until after I rebooted.  
>

The instructions I based the above steps on:  
[](http://aaroniba.net/articles/x11-leopard.html)[Bring Back Tiger's X11 to
Leopard in 3 Steps](http://aaroniba.net/articles/x11-leopard.html)  
[easier instructions to install Tiger's
X11.app](http://lists.apple.com/archives/x11-users/2007/Nov/msg00005.html)

