---
layout: post
title: Live Upgrade Rocks
date: '2005-07-19'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:36.939-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-846908518637936458
blogger_orig_url: http://blog.shawnferry.com/2005/07/live-upgrade-rocks.html
---

<p><br />      I know that live upgrade has been available for a while...What I don't <br />      know is why I never really used it before.<br />    </p><br />    <br /><br /><br /><br /><p><br />      Conceptually it makes sense but it was cool how easy it was to upgrade. <br />      I need to see it in larger environemnts with complex application <br />      deployments, but so far so good<br />    </p><br /><br /><br /><br /><pre><br /><br /># lucreate -c "Nevada16" -m /dev/dsk/c0d0t4:/:ufs -n "Nevada18"<br /># lustatus (see output below)<br /># lofiadm -a /var/tmp/solarisdvd.iso /dev/lofi/1<br /># mount -F hfsf  /dev/lofi/1 /mnt<br /># luupgrade -u -n "Nevada18" -s /mnt<br /># luactivate Nevada18<br /># init 6<br /># lustatus (See output below)<br /><br /><br /><br /><br />Before: <br /><br />lustatus<br />Boot Environment           Is       Active Active    Can    Copy     <br />Name                       Complete Now    On Reboot Delete Status   <br />-------------------------- -------- ------ --------- ------ ----------<br />Nevada16                   yes      yes    yes       no     -        <br />Nevada18                   yes      no     no        yes    -        <br /><br /><br /><br />After:<br /><br />lustatus<br />Boot Environment           Is       Active Active    Can    Copy     <br />Name                       Complete Now    On Reboot Delete Status   <br />-------------------------- -------- ------ --------- ------ ----------<br />Nevada16                   yes      no     no        yes    -        <br />Nevada18                   yes      yes    yes       no     -        <br /></pre><br /><br/><br />topic:{technorati}[Solaris]<br />topic:{technorati}[liveupgrade]<br />

