---
layout: post
title: |-
  So Not Funny: Shmoo Group International Domain Name Translation
  Spoof/Hack
date: '2005-02-06'
author: Shawn Ferry
tags:
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:24:37.121-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-7190338401098269598
blogger_orig_url: http://blog.shawnferry.com/2005/02/so-not-funny-shmoo-group-international.html
---

The "Shmoo Group":http://www.shmoo.com/ has published an example of an  
exploit which takes advantage of IDN to spoof sites for both "PayPal  
http":http://www.pаypal.com and "PayPal https":https://www.pаypal.com  
  
The links above should take you to "PayPal". Original "Shmoo  
Example":http://www.shmoo.com/idn/ and  
"explanation":http://www.shmoo.com/idn/homograph.txt Using firefox and  
setting "network.enableIDN" to false in about:config, will prevent  
firefox from following the link, but the error is non-descriptive  
resulting in a "[Translated Name] site could not be found, please check  
the name and try again"  

Update: I should mention that it does not work in IE by default, as  
apparently IDN translation code is only available as a plugin.  

