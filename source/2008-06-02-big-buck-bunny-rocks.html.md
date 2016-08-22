---
layout: post
title: Big Buck Bunny Rocks
date: '2008-06-02'
author: Shawn Ferry
tags:
- blender
- SGE
- N1GE
- Untitled
- Movie
- rendering
- yakshaving
- b.s.c.
- OSS
modified_time: '2010-04-29T10:17:26.803-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-2225679122228871935
blogger_orig_url: http://blog.shawnferry.com/2008/06/big-buck-bunny-rocks.html
---

Way back when (June 19, 2007 at ~4am) I sent mail to Ton at Blender. At the
time I allowed that we had this compute grid that already ran Blender and that
while I couldn't commit any time I could if he was interested put him in touch
with the people who could. Happily he allowed that indeed they were looking
for a render farm sponsor.

Woot! On May 29, 2008 I received my copy of the Big Buck Bunny DVD. I brought
it inside and we paused the Stanley Cup Playoffs so we could watch it. It
looks really good and I think it was worth the occasional pain on our part in
supporting the rendering (also because my name is in the credits as I bought a
pre-release copy of the DVD to further support the development effort :) ).

[Big Buck Bunny](http://www.bigbuckbunny.org) was rendered on
[Network.com](http://www.network.com/) ([press
release](http://www.sun.com/aboutsun/pr/2008-06/sunflash.20080602.1.xml))
using [Blender](http://www.blender.org/) which is part of the Network.com
[application catalog](http://www.network.com/apps/blender.html)

![](http://peach.blender.org/wp-content/uploads/big_big_buck_bunny.jpg)  

To focus on the things that I think most people reading Sun blogs would find
interesting the [rendering process](http://www.bigbuckbunny.org/index.php/our-
renderfarm-and-how-it-works/) from the Big Buck Bunny blogs. My role was down
in the Sun Grid - Network.com bubble.

![Renderfarm overview](http://peach.blender.org/wp-
content/uploads/renderfarm_overview.png)  

If you look at the blogs there is plenty of
[information](http://www.bigbuckbunny.org/index.php/our-renderfarm-and-how-it-
works/) describing the development process, the [rendering
process](http://wiki.blender.org/index.php/Bf-institute/renderfarm) and the
fact that there were some complications in the execution. Interestingly one of
the most common problems from my perspective were a large number of core files
from the various Blender processes. These cores would at times fill up /var in
the zones they were running in causing the Blender jobs to fail. Because of
the implementation a single sub-task failure would cascade resulting in
stopping the whole rendering process.

I "fixed" the core issue (coreadm is your friend) and cores have been disabled
by default, now requiring that the end user make a local directory in their
job setup scripts to hold their own cores if they would like to capture them.
I believe that the cascading stop has been made into an option that is
controllable in the job definition through the portal as well.

Go take a look at [Big Buck
Bunny](http://www.bigbuckbunny.org/index.php/download/) and marvel at the
power of open source software. Then realize how cool it is that a company such
as Sun would spend/donate real time, money and resources on a creative commons
animated movie. I'm glad we did it and I hope we can continue to do so in the
future. (Maybe the next one as well)

