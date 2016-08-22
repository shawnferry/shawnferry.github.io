---
layout: post
title: Entry Titles in Permalink pages
date: '2007-01-28'
author: Shawn Ferry
tags:
- yakshaving
- b.s.c.
modified_time: '2010-04-29T10:17:26.954-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-1551794035917977101
blogger_orig_url: http://blog.shawnferry.com/2007/01/entry-titles-in-permalink-pages.html
---

Thanks to [Alexis and
Dave](http://blogs.sun.com/alexismp/entry/better_html_title_handling_in "html
title from premalink" ) I have updated my template to use the title of the
entry being viewed from a permalink.

>  Using Roller 2.x: "Preferences" -&gt; "Templates" -&gt; "Weblog" Edit -&gt;
locate the `<title> </title>` tags and replace them with the following:  
>  
>  ` #if ($pageModel.getWeblogEntry())  
>    <title>#showWebsiteTitle(): $pageModel.getWeblogEntry().title</title>  
> #else  
>    <title>#showWebsiteTitle(): #showWebsiteDescription()</title>  
> #end`

