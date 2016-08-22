---
layout: post
title: 'XSH: XML Editing Shell'
date: '2005-03-09'
author: Shawn Ferry
tags:
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:24:37.105-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-8033145796097753274
blogger_orig_url: http://blog.shawnferry.com/2005/03/xsh-xml-editing-shell.html
---

Just fixed a small [xsh](http://xsh.sourceforge.net)  
script that I wrote months ago in my spare time to walk a [  
Torrus](http://www.torrus.org) configuration snapshot.  
  
A coworker asked me If I had a way to easily provide an overview/outline  
of the data that was being collected via Torrus for a customer.  
Fortunately the answer was yes, and it didn't involve the complex, ugly  
and not quite functional grep and awk statements I used the first time I  
tried to get quick overview of the XML tree.  

The problem was that in order to get the comment I was stepping into the  
comment parameter to get the value when I should have been doing  
  
    $comment = string(param[attribute::name='comment']/attribute::value);        

Then I wasn't stepping back out, prematurely ending the loop and missing  
additional trees and leaves.  

The Final output looks a bit like this, but goes on to enumerate all of  
the data that is collected and displayed.  
  
      Subtree = Devices ''  
          Subtree = router-name '2651XM chassis, Hw Serial#: XXXXXXXXX (XXXXXXXX), Hw Revision: 0x301'  
              Subtree = Buffer_Usage 'Buffer usage statistics'  
                  Subtree = Small_Buffers 'Buffer usage statistics'  
                      Leaf = Free   'Number of Free Small Buffers'  
                      Leaf = Max    'Max Small Buffers'  
                      Leaf = Hits   'Small Buffer Hits'  
                      Leaf = Misses 'Small Buffer Misses'  
                      Leaf = Creates        'Small Buffer Creates'  
                      Leaf = Trims  'Small Buffer Trims'        

The snapshot XML in part looks like  

    >  
            >  
              
