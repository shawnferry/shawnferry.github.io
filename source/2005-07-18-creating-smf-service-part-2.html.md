---
layout: post
title: Creating an SMF service (Part 2)
date: '2005-07-18'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:36.942-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-8341949842012171609
blogger_orig_url: http://blog.shawnferry.com/2005/07/creating-smf-service-part-2.html
---

If you haven't alreay read it you might be interested in statrting with
[Creating an SMF service (Part
1)](http://blogs.sun.com/roller/page/yakshaving?entry=creating_an_smf_service_part
"Creating an SMF Service Part 1" )

The first major problem...the DTD

Or maybe not so much a problem with the DTD as with my understanding of the  
DTD.

Which generally means that I have either been lucky before when writing  
Validated XML or I knew and forgot it.

I spent a good 30 minutes trying to figure out what I was doing wrong.  
  
I had valid XML but it didn't validate.  
  
My first thought was that I was miss-remembering the DTD occurrence syntax.

[W3 Schools: DTD Elements](http://www.w3schools.com/dtd/dtd_elements.asp "DTD
Elements" ) Indicates:

>  
>

>

> When children are declared in a sequence separated by commas,  
>  the children must appear in the same sequence in the document.

>

>  
>

Error message line breaks for readability.

    svccfg validate sysedge.xml  
    sysedge.xml:94: element service: validity error :  
    Element service content does not follow the DTD, expecting (create_default_instance? , single_instance? ,  
     restarter? , dependency* , dependent* , method_context? , exec_method* , property_group* , instance* ,  
     stability? , template?), got (instance create_default_instance instance stability )  
    svccfg: Document is not valid.  
    
Now, that error is clear. I have my children out of order (The above error is
synthetic).

I have come up with the following "services":

>  
>

>

> svc:/application/monitoring/sysedge_deps:plus  
>  svc:/application/monitoring/sysedge_deps:default  
>  svc:/application/monitoring/sysedge:default  
>  svc:/application/monitoring/sysedge:concord  
>  svc:/application/monitoring/sysedge:compat  
>  svc:/application/monitoring/sysedge:plus

>

>  
>

sysedge_deps:* are classified as milestones

I don't understand why sysedge:default exists with  
`<create_default_instance enabled='false'/>` set. Or is that just "The default
instance will be created but not enabled"?

I may try and reduce the possible confusion and break the regular and plus  
code into different services instead of instances. Particularly since I  
want/need to set `<single_instance/>` for sysedgeplus.

It also seems that "application property groups" might do some good things.

I may also see if I can figure out how [Dan Price](http://blogs.sun.com/dp/)  
did his dependency graphing. Stephen Hahn has a cool example of [smf
dependency  
graphing](http://blogs.sun.com/roller/page/sch/20040917#smf_5_a_view_from1)

[Part
1](http://blogs.sun.com/roller/page/yakshaving?entry=creating_an_smf_service_part)  
  
[Part
3](http://blogs.sun.com/roller/page/yakshaving?entry=creating_an_smf_service_part2)  
  
* * *

Since I wrote this I have split the services and done a bunch of package
integration.  
  
I will go into details of the design and implementation in future
installments.

topic:{technorati}[Solaris]  
topic:{technorati}[SMF]  

