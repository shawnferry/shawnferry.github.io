---
layout: post
title: Netbeans will not start on OS X
date: '2009-01-05'
author: Shawn Ferry
tags:
- howto
- Untitled
- yakshaving
- NetBeans
- b.s.c.
- java
modified_time: '2010-04-29T10:17:26.781-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-1186803501794211184
blogger_orig_url: http://blog.shawnferry.com/2009/01/netbeans-will-not-start-on-os-x.html
---

Back from a two week break, finished with at least the first pass of mail and
attempting to get back to inbox zero I find that I cannot start NetBeans 6.1
or 6.5. It appears that the problem is an OS X update forcing the use of
64-bit data model when the 32-bit is required.

In various locations I found instructions to change to the 32-bit data model
with -d32. After adding this change to netbeans.conf (alternatively -d32 or
-J-d32) the problem was not resolved.  

An **ugly hack** to work around this issue (inspiration from [Sam
Cogan](http://samcogan.com/blog/?p=23)):

>

>     lipo
/System/Library/Frameworks/JavaVM.framework/Versions/1.5.0/Home/bin/java \

>        -remove x86_64 -output /tmp/java

>  
>  
>  
>     sudo mv
/System/Library/Frameworks/JavaVM.framework/Versions/1.5.0/Home/bin/java \

>        /System/Library/Frameworks/JavaVM.framework/Versions/1.5.0/Home/bin
/java-x86_64

>  
>  
>  
>     sudo mv /tmp/java
/System/Library/Frameworks/JavaVM.framework/Versions/1.5.0/Home/bin/java-32

>  
>  
>  
>     sudo ln -s
/System/Library/Frameworks/JavaVM.framework/Versions/1.5.0/Home/bin/java-32 \

>
/System/Library/Frameworks/JavaVM.framework/Versions/1.5.0/Home/bin/java

>  

This appears to have fixed the immediate symptom, there is no telling what I
have broken in some other insidious ways. **I also expect this problem to
return with the next update to java which replaces the java binary.**

Ways to identify this problem:

>

>     From the terminal.app: open /Applications/NetBeans/NetBeans\ 6.5.app

>  
>  
>  
>     LSOpenFromURLSpec() failed with error -10810 for the file
/Applications/NetBeans/NetBeans 6.5.app

>  
>  
>  
>     From CrashReporter files for java:

>  
>  
>  
>     Process:         java [5579]

>     Path:
/System/Library/Frameworks/JavaVM.framework/Versions/1.5/Home/bin/java

>     Identifier:      java

>     Version:         ??? (???)

>     Code Type:       X86-64 (Native)

>     Parent Process:  bash [5421]

>     <...>

>     Exception Type:  EXC_BAD_ACCESS (SIGSEGV)

>     Exception Codes: 0x000000000000000d, 0x0000000000000000

>     Crashed Thread:  0

>     <...>

>     Thread 0 crashed with X86 Thread State (64-bit):

>  
>  
>  
>     In: /var/log/system.log

>  
>  
>  
>     1/5/09 3:05:41 PM [0x0-0x133133].org.netbeans.ide[5610]
/Applications/NetBeans/NetBeans
6.5.app/Contents/MacOS/../Resources/NetBeans/bin/../platform9/lib/nbexec: line
493:  5768 Segmentation fault
"/System/Library/Frameworks/JavaVM.framework/Versions/1.5/Home/bin/java" ...  
>     >

>  

