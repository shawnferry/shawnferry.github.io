---
layout: post
title: Encrypted FS on Solaris 10, Ugly Hack
date: '2006-09-10'
author: Shawn Ferry
tags:
- Security
- Sun
- opensolaris
- Encryption
- b.s.c.
modified_time: '2010-04-29T10:24:36.908-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-4105230214564786614
blogger_orig_url: http://blog.shawnferry.com/2006/09/encrypted-fs-on-solaris-10-ugly-hack_3784.html
---

This is an off the cuff solution to encrypted file systems on Solaris 10 in
response to [OpenSolaris
Adventures](http://blogs.sun.com/davidleetodd/entry/opensolaris_adventures)
which mentions concerns about file security given physical access to a device.  

Until zfs has crypto support or encrypted lofi is available, you could set a
bios password. Or create your own loopback file based fs. Of course if the
file is decrypted and the attacker steals your laptop you are out of luck. So
only having the decrypted data in /tmp would offer some protection.  

The poor man's version would be something like:  
  
1) Make a source file (Preferably in /tmp)  
  
2) Create a Loopback  
  
3) Layout a filesystem  
  
4) Add content  
  
5) Encrypt (To not /tmp)  
  
6) Delete source file  

Ongoing Usage scripted as:  
  
1) decrypt /var/tmp/encrypted.current to /tmp/decrypted  
  
2) create lofi and mount  
  
3) encrypt to /var/tmp/encrypted.new  
  
4) delete decrypted file  
  
5) Move encrypted.current to .bak and new to .current  

**Steps 1 - 4:**  

>  
> t2000-10# mkfile 10m /tmp/foo  
>  
> t2000-10# lofiadm -a /tmp/foo  
>  
> /dev/lofi/1  
>  
>  
>  
> t2000-10# newfs /dev/lofi/1  
>  
> newfs: construct a new file system /dev/rlofi/1: (y/n)? y  
>  
> /dev/rlofi/1: 20468 sectors in 34 cylinders of 1 tracks, 602 sectors  
>  
> 10.0MB in 3 cyl groups (16 c/g, 4.70MB/g, 2240 i/g)  
>  
>  
>  
> t2000-10# mkdir /tmp/foo_mnt  
>  
> t2000-10# mount /dev/lofi/1 /tmp/foo_mnt  
>  
> t2000-10# cat /usr/man/man1/* | nroff -man &gt; /tmp/foo_mnt/important.txt  
>

**Content is visible to the casual viewer:**  

t2000-10# cat /tmp/foo | strings | head -100  
  
...  
  
Moi2  
  
a subcommand and no arguments is  
  
an error. This guideline is provided to allow the  
  
common forms command --  
  
p, command -?  
  
?, command  
  
\--  
  
n, and command -V  
  
V to be accepted in the  
  
command-subcommand construct.  
  
Several of these guidelines are only of interest to the  
  
authors of utilities. They are provided here for the use of  

t2000-10# umount /tmp/foo_mnt  
  
t2000-10# lofiadm -d /dev/lofi/1  

**Step 5:**  

>  
> t2000-10# time encrypt -a 3des -v -i /tmp/foo -o /var/tmp/3des_encrypted  
>  
> Enter key:  
>  
>
[..................|...................|...................|...................]  
>  
> Done.  
>  
> encrypt -a 3des -v -i /tmp/foo -o /var/tmp/3des_encrypted 4.44s user 0.63s
system 60% cpu 8.434 total  
>  
>  
>  
> t2000-10# rm /tmp/foo  
>  
>  
>

**Simple check to see if data is still accessible:**  

>  
> t2000-10# lofiadm -a /var/tmp/3des_encrypted  
>  
> lofiadm: size of /var/tmp/3des_encrypted is not a multiple of 512  
>  
>  
>  
> t2000-10# file /var/tmp/3des_encrypted  
>  
> /var/tmp/3des_encrypted: data  
>  
>  
>  
> t2000-10# cat /var/tmp/3des_encrypted| strings  
>  
> ...  
>

**Accessing Encrypted Data:**  

>  
> t2000-10# decrypt -v -a 3des -i /var/tmp/3des_encrypted -o /tmp/decrypted_fs  
>  
> Enter key:  
>  
>
[..................|...................|...................|..................]  
>  
> Done.  
>  
>  
>  
> t2000-10# cat /tmp/decrypted_fs| strings | head -100  
>  
> ...  
>  
> VkQP  
>  
> a subcommand and no arguments is  
>  
> an error. This guideline is provided to allow the  
>  
> common forms command --  
>  
> p, command -?  
>  
> ?, command  
>  
> \--  
>  
> n, and command -V  
>  
> V to be accepted in the  
>  
> command-subcommand construct.  
>  
> Several of these guidelines are only of interest to the  
>  
> authors of utilities. They are provided here for the use of  
>  
>  
>  
> t2000-10# lofiadm -a /tmp/decrypted_fs  
>  
> /dev/lofi/1  
>  
>  
>  
> t2000-10# mount /dev/lofi/1 /tmp/foo_mnt  
>  
>  
>  
>  
>

**Checking Contents:**  

>  
> t2000-10# cd /tmp/foo_mnt  
>  
> t2000-10# head important.txt  
>  
>  
>  
> User Commands Intro(1)  
>  
>  
>  
> NAME  
>  
> Intro, intro - introduction to commands and application pro-  
>  
> grams  
>

