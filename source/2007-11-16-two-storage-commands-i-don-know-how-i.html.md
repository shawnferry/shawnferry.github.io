---
layout: post
title: Two Storage Commands I Don't Know How I Lived Without
date: '2007-11-16'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:17:26.877-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-4648204068086707513
blogger_orig_url: http://blog.shawnferry.com/2007/11/two-storage-commands-i-don-know-how-i.html
---

I am working on an issue which involves a 3510 and two dual connected hosts
(Home grown Active/Active configuration). The customer's equipment has just
been moved within the cage. When the systems were rebooted one of them
reported multipath failures and both SCSI errors.

While I was investigating the problems I used cfgadm, luxadm and sccli the
first two are common to Solaris the last is an additional package for
management of arrays including the 3510. These commands are not new to me;
while I was searching for alternative solutions to my problems I found
[fcinfo](http://docs.sun.com/app/docs/doc/816-5166/6mbb1kq0m?l=en&a=view&q=fcinfo
"fcinfo man page" ) and
[mpathadm](http://docs.sun.com/app/docs/doc/816-5166/6mbb1kq8u?l=en&a=view&q=mpathadm
"man page of mpathadm" ) two fairly new (and definitely new to me) commands.

Using luxadm to display the state of the ports:  
(One of the ports was not connected but I wasn't thinking about blogging it so
I missed my chance to capture the output.)

>

>     luxadm -e port  
>     > /devices/pci@1d,0/pci1022,7450@1/pci1077,100@1/fp@0,0:devctl
CONNECTED  
>     > /devices/pci@1d,0/pci1022,7450@2/pci1077,100@1/fp@0,0:devctl
CONNECTED  
>     >

Using luxadm to show link errors:

>

>     luxadm -e rdls /dev/es/ses0  
>     >  
>     > Link Error Status information for loop:/dev/es/ses0  
>     > al_pa   lnk fail    sync loss   signal loss   sequence err   invalid
word   CRC  
>     > 9e      0           1           1             0              2794
0  
>     > 9f      0           0           0             0              243
0  
>     > 1       0           0           0             0              0
0  
>     >  
>     > Link Error Status information for loop:/dev/es/ses0  
>     > al_pa   lnk fail    sync loss   signal loss   sequence err   invalid
word   CRC  
>     > a3      0           2           2             0              65535
0  
>     > a5      0           0           0             0              28481
0  
>     > 1       0           0           0             0              0
0  
>     >

Using cfgadm to see the configuration of the devices:  
(In the original investigation c2 was displaying type fc and Occupant
unconfigured)  

>

>     cfgadm -al  
>     > Ap_Id                          Type         Receptacle   Occupant
Condition  
>     > c0                             scsi-bus     connected    configured
unknown  
>     > c0::dsk/c0t0d0                 disk         connected    configured
unknown  
>     > c0::dsk/c0t2d0                 disk         connected    configured
unknown  
>     > c0::dsk/c0t3d0                 disk         connected    configured
unknown  
>     > c0::es/ses1                    processor    connected    configured
unknown  
>     > c1                             fc-private   connected    configured
unknown  
>     > c1::256000c0ffc86cfb           disk         connected    configured
unknown  
>     > c1::256000c0ffd86cfb           ESI          connected    configured
unknown  
>     > c2                             fc-private   connected    configured
unknown  
>     > c2::226000c0ffa86cfb           ESI          connected    configured
unknown  
>     > c2::226000c0ffb86cfb           ESI          connected    configured
unknown  
>     >

I tried using 'cfgadm -c configure c2' and 'cfgadm -f -c configure c2' and
finally 'cfgadm -o force_update -c configure c2' none of which succeeded in
letting me recover the path. I just now found a [bug for path shows NOT
CONNECTED](http://bugs.opensolaris.org/view_bug.do;jsessionid=afcfa74218c1bae3a88e6ca4a2a?bug_id=6329824).
It appears that I might have been able to recover using 'luxadm -e forcelip'.
Since I needed to clear the 3510 error counters it was decided to take the
systems down and power cycle the 3510.  
  
Now on to the hook for this post (fcinfo and mpathadm). While looking at
various documentation I found fcinfo and mpathadm!  
fcinfo was added in S10u1. Using fcinfo I saw the some of the same information
that I got from 'luxadm -e rdls' and more.  
Using fcinfo to see local hba-port info:

>

>     fcinfo hba-port -l  
>     > HBA Port WWN: 210000e08b1cdb34  
>     >         OS Device Name: /dev/cfg/c1  
>     >         Manufacturer: QLogic Corp.  
>     >         Model: QLA2340  
>     >         Firmware Version: 3.3.117  
>     >         FCode/BIOS Version: N/A  
>     >         Type: L-port  
>     >         State: online  
>     >         Supported Speeds: 1Gb 2Gb  
>     >         Current Speed: 2Gb  
>     >         Node WWN: 200000e08b1cdb34  
>     >         Link Error Statistics:  
>     >                 Link Failure Count: 0  
>     >                 Loss of Sync Count: 0  
>     >                 Loss of Signal Count: 0  
>     >                 Primitive Seq Protocol Error Count: 0  
>     >                 Invalid Tx Word Count: 0  
>     >                 Invalid CRC Count: 0  
>     > HBA Port WWN: 210000e08b1124bf  
>     >         OS Device Name: /dev/cfg/c2  
>     >         Manufacturer: QLogic Corp.  
>     >         Model: QLA2340  
>     >         Firmware Version: 3.3.117  
>     >         FCode/BIOS Version: N/A  
>     >         Type: L-port  
>     >         State: online  
>     >         Supported Speeds: 1Gb 2Gb  
>     >         Current Speed: 2Gb  
>     >         Node WWN: 200000e08b1124bf  
>     >         Link Error Statistics:  
>     >                 Link Failure Count: 0  
>     >                 Loss of Sync Count: 0  
>     >                 Loss of Signal Count: 0  
>     >                 Primitive Seq Protocol Error Count: 0  
>     >                 Invalid Tx Word Count: 0  
>     >                 Invalid CRC Count: 0  
>     >

Nothing extremely interesting on the hba-port side, however fcinfo also shows
remote-port information.  
Using fcinfo to see remote-port info: (the -p option shows information visible
from the hba-port WWNs seen above)  

>

>     fcinfo remote-port -l -p 210000e08b1124bf  
>     > Remote Port WWN: 226000c0ffb86cfb  
>     >         Active FC4 Types:  
>     >         SCSI Target: yes  
>     >         Node WWN: 206000c0ff086cfb  
>     >         Link Error Statistics:  
>     >                 Link Failure Count: 0  
>     >                 Loss of Sync Count: 2  
>     >                 Loss of Signal Count: 2  
>     >                 Primitive Seq Protocol Error Count: 0  
>     >                 Invalid Tx Word Count: 65535  
>     >                 Invalid CRC Count: 0  
>     > Remote Port WWN: 226000c0ffa86cfb  
>     >         Active FC4 Types:  
>     >         SCSI Target: yes  
>     >         Node WWN: 206000c0ff086cfb  
>     >         Link Error Statistics:  
>     >                 Link Failure Count: 0  
>     >                 Loss of Sync Count: 0  
>     >                 Loss of Signal Count: 0  
>     >                 Primitive Seq Protocol Error Count: 0  
>     >                 Invalid Tx Word Count: 28481  
>     >                 Invalid CRC Count: 0  
>     >

>  
>  
>  
>  
>     >

>  
>  
>  
>     fcinfo remote-port -l -p 210000e08b1cdb34  
>     > Remote Port WWN: 256000c0ffd86cfb  
>     >         Active FC4 Types:  
>     >         SCSI Target: yes  
>     >         Node WWN: 206000c0ff086cfb  
>     >         Link Error Statistics:  
>     >                 Link Failure Count: 0  
>     >                 Loss of Sync Count: 1  
>     >                 Loss of Signal Count: 1  
>     >                 Primitive Seq Protocol Error Count: 0  
>     >                 Invalid Tx Word Count: 2794  
>     >                 Invalid CRC Count: 0  
>     > Remote Port WWN: 256000c0ffc86cfb  
>     >         Active FC4 Types:  
>     >         SCSI Target: yes  
>     >         Node WWN: 206000c0ff086cfb  
>     >         Link Error Statistics:  
>     >                 Link Failure Count: 0  
>     >                 Loss of Sync Count: 0  
>     >                 Loss of Signal Count: 0  
>     >                 Primitive Seq Protocol Error Count: 0  
>     >                 Invalid Tx Word Count: 243  
>     >                 Invalid CRC Count: 0  
>     >  
>     >

Fcinfo and luxadm are clearly showing me that there are problems reported for
the remote port in the 'Invalid Tx Word Count'.  
The primary recommendation is to reseat the cables SPFs and blow out the
ports. We are moving along with the process now, having replaced one of the
cables, reseated everything and blown out the ports.  
  
On to mpathadm, it was added in S10u3 and lets you discover and manage
multipathing (shocking given its name).  
I am using mpathadm to display information about the current configuration.
Prior to the reboot the system with only one link in CONNECTED state showed
only one path to all devices.  
Output from 'mpathadm list lu':

>

>     mpathadm list lu  
>     >         /scsi_vhci/enclosure@g600c0ff000000000086cfb0000000000  
>     >                 Total Path Count: 3  
>     >                 Operational Path Count: 3  
>     >         /dev/rdsk/c3t600C0FF000000000086CFB359771241Bd0s2  
>     >                 Total Path Count: 1  
>     >                 Operational Path Count: 1  
>     >         /dev/rdsk/c3t600C0FF000000000086CFB359771241Ad0s2  
>     >                 Total Path Count: 1  
>     >                 Operational Path Count: 1  
>     >         /dev/rdsk/c3t600C0FF000000000086CFB3597712419d0s2  
>     >                 Total Path Count: 1  
>     >                 Operational Path Count: 1  
>     >         /dev/rdsk/c3t600C0FF000000000086CFB3597712418d0s2  
>     >                 Total Path Count: 1  
>     >                 Operational Path Count: 1  
>     >         /dev/rdsk/c3t600C0FF000000000086CFB3597712417d0s2  
>     >                 Total Path Count: 2  
>     >                 Operational Path Count: 2  
>     >         /dev/rdsk/c3t600C0FF000000000086CFB3597712416d0s2  
>     >                 Total Path Count: 2  
>     >                 Operational Path Count: 2  
>     >  
>     >

Specific detail from 'mpathadm show lu' on path:  

>

>     mpathadm show lu /dev/rdsk/c3t600C0FF000000000086CFB3597712416d0s2  
>     > Logical Unit:  /dev/rdsk/c3t600C0FF000000000086CFB3597712416d0s2  
>     >         mpath-support:  libmpscsi_vhci.so  
>     >         Vendor:  SUN  
>     >         Product:  StorEdge 3510  
>     >         Revision:  415G  
>     >         Name Type:  unknown type  
>     >         Name:  600c0ff000000000086cfb3597712416  
>     >         Asymmetric:  no  
>     >         Current Load Balance:  round-robin  
>     >         Logical Unit Group ID:  NA  
>     >         Auto Failback:  on  
>     >         Auto Probing:  NA  
>     >  
>     >         Paths:  
>     >                 Initiator Port Name:  210000e08b1cdb34  
>     >                 Target Port Name:  256000c0ffc86cfb  
>     >                 Override Path:  NA  
>     >                 Path State:  OK  
>     >                 Disabled:  no  
>     >  
>     >                 Initiator Port Name:  210000e08b1124bf  
>     >                 Target Port Name:  226000c0ffb86cfb  
>     >                 Override Path:  NA  
>     >                 Path State:  OK  
>     >                 Disabled:  no  
>     >  
>     >         Target Ports:  
>     >                 Name:  256000c0ffc86cfb  
>     >                 Relative ID:  0  
>     >  
>     >                 Name:  226000c0ffb86cfb  
>     >                 Relative ID:  0  
>     >

This is in no way a full exploration of the capabilities of fcinfo and
mpathadm.  
I hope that the next time you are (or I am) looking at FC or multipath issues
these commands will be helpful.  
Please see the links to the manual pages below for more specific information
and examples from the fcinfo and mpathadm commands.  
  
References:  
[fcinfo #8211 Fibre Channel HBA Port Command Line
Interface](http://docs.sun.com/app/docs/doc/816-5166/6mbb1kq0m?l=en&a=view&q=fcinfo
"fcinfo man page" )  
[mpathadm #8211 multipath discovery and
administration](http://docs.sun.com/app/docs/doc/816-5166/6mbb1kq8u?l=en&a=view&q=mpathadm
"man page of mpathadm" )  

EDIT: Fixed some strange formatting issues  
EDIT1: A bit more touch up

