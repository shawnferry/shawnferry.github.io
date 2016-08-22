---
layout: post
title: 'CEC: RBAC Demystified'
date: '2006-10-06'
author: Shawn Ferry
tags:
- CEC 2006
- Security
- Sun CEC
- RBAC
- b.s.c.
modified_time: '2010-04-29T10:20:48.072-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-4459082625069957800
blogger_orig_url: http://blog.shawnferry.com/2006/10/cec-rbac-demystified_4519.html
---

Brian Bianquart and Darren Moffat  

style="font-weight: bold;">Role Based Access Control  

What is a Role: An account on the system  
  
Cannot directly login  
  
Could be root (or any user)  

What is a Privilege: An attribute of a process  
  
Checked by Kernel  

Authorization: given to users directly or through profile  

_...Cutting back on  
following/outlineing until I see  
something that I am less sure is readily available online and in docs..._  

One exec_attr table can be  
used across Solaris 8 and 9,  
Trusted Solaris (8) and Solaris 10  

_Here we have a  
graphic I have never seen  
before...took a picture but it will probably be lame._  
  
_I think maybe hand drawings scanned and added to the slides._

_ href="http://www.flickr.com/photos/shawnferry/261252689/"  
title="Photo Sharing"> style="border: 0px solid ; width: 800px; height:
600px;"  
src="http://static.flickr.com/113/261252689_892fac0221_o.jpg"  
alt="A picture of an RBAC slide">_

style="font-weight: bold;">Q:  
Can we make it such that user and role profiles can be modified while  
the user is logged in or the role is in use.  
  
A: Yes, that  
is a bug fixed in update 3...changes may not take effect until next  
login, but you will be able to make the change.  

Standard RBAC  
example:  
  
Execute with elevated  
privileges...Start Apache as a regular user on port 80  
  
_(As opposed to start as root and drop privs)_

_I think I was  
hoping for more in depth technical details, still time yet we will see_  

/usr/bin/pfexec is the  
closest thing to sudo only without authentication (yet)  

pfexec will use the first  
profile found....that is the ALL role should be last, otherwise don't  
bother to define other profiles.  

SMF demo: Allow a user to  
change the running state of a service but not the boot state  
  
e.g.  
  
ALLOWED: svcadm enable/disable -t  
  
DISALOWED: svcadm enable/disable (no -t)  

DO NOT MODIFY SYSTEM  
SUPPLIED PROFILES  
  
File a bug if you think it should be changed  
  
OR  
  
Create your own profiles  

**Privileges**  
  
Kernel no longer only  
checks for UID==0  
  
48+ privileges checked instead  

_Now privilege  
sets, next how the privileges flow not really going to note that  
down...I know it is well documented I have read it._  

_Note: Dark Red on  
black...hard to see, shouldn't do colors that evaluate to black_  

Use ppriv -D to debug  
privilege access. _(Yes this is commonly known)_

**ACLs**  
  
Solaris 10 NFSv4/ZFS ACLs  
now match those as implemented in Windows NT/XP=  
  
More info  
  
There is a RBAC and SUDO  
comparison slide  
  
Strengths and weaknesses  
on both sides the most common requested deltas are being addressed.  
  
Authentication and Netgroups are on/near the top of the list.

security-discuss@opensloaris.org  
 and Sun blueprints

