---
layout: post
title: 'CEC: Managing Systems at Grid Scale (Our Presentations)'
date: '2006-10-08'
author: Shawn Ferry
tags:
- CEC 2006
- Education
- Sun CEC
- grid
- SunGrid
- puppy
- Sun
- b.s.c.
modified_time: '2010-04-29T10:20:48.069-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-6249433896216821049
blogger_orig_url: http://blog.shawnferry.com/2006/10/cec-managing-systems-at-grid-scale-our_8314.html
---

Shawn Ferry (ME) and Brian Smith  
  
In general our  attendance was a little bit lower than I had  
been hoping for. I think we got in the neighborhood of 80 total...I  
think I should be able to find out exactly somewhere.  
  
Both instances went fairly well we had the required Managed Operations  
content. In the future we need to go straight to an elevator pitch and  
be done with it. I watched as people got bored hearing about how  
managed operations does things. Certainly they were interested in the  
more technical details CTA, encrypted transport and such. Unfortunately  
in our general Managed Operations presentation the less technically  
interesting details are more prevalent and this is a geek  
conference.  
  
Both presentations had a few Managed Operations staff in attendance for  
solidarity. As well as a couple of new Managed Operations staff from  
APAC and EMEA.  
  
The first presentation had a slight timing problem, the 15 min max  
planned managed operations specific content ran about 30min, in the end  
I got  
my content in with about a minute to spare and Brian got the last bits  
in about 5 after the scheduled end.  
  
The second presentation went more to plan. I was able to go into  
more depth on the technical aspects of the monitoring. For those who  
attended the presentation. We send an alert.  
  
The only real failure for the whole thing was the attempt for a demo in  
the extra 5min before the scheduled end of the second presentation  
during the question and answer period. I had maintained what looked  
like good network connectivity until I tried to use it, at which point  
it all went [pear  
shaped](http://en.wikipedia.org/wiki/Pear_shaped) and I lost network access
again.  
  
In brief we covered monitoring methodology:  
  
We have a top down and bottom up approach.  
  
Top Down(A Holistic View):  
  
We start measuring from a user experience, can you reach the web site.  
  
Then a complex web site walking script/library(That I  
wrote) performs a:  
  
  * login
  
  * job submission
  
  * waits for completion
  
  * downloads and analyzes the results
  
  * deletes the job
  
  * logs out
  
At various points in the process if we don't get specific results  
(Welcome to the Grid,Job X submitted, Job X Successful, Logout  
Complete)  
or the process is taking too long we send an alert.  
  
Really in the Middle Here(Business Process Monitoring):  
  
The from a Managed Operations Point of view what we want to know is  
what went wrong.  
  
A user can get an login failure for a number of reasons:  
  
  * External authentication service could not be functional(Sun  
Grid does not maintain direct authentication data, you must have a  
sunsolve/mysun/store.sun.com account...Go
href="http://www.sun.com/identity">Federated Identity  
(
href="http://www.sun.com/software/media/flash/demo_federation/index.html">Silly  
Flash IDM) , see also:  href="http://blogs.sun.com/saragates" rel="co-
worker">Sara  
Gates Sun's VP IDM)

  * Your name may have been found on the  href="http://www.sun.com/sales/its/export/DRPL/index.html">Denied  
Restricted Persons List

  * You may have moved to Brazil (This is a presentation In  
joke, I was hoping for some Brazilians to jump up and down and cheer)  
or really you are coming from outside the US.

A job can fail for a number of reasons:  
  
  * Malformed Job
  
  * Portal Submission Failure
  
  * Grid Engine Failure
  
Some of these things we can see cause from the middle and some we need  
a lower level view.  
  
Bottom Up (A small sampling, we monitor a very wide range):  
  
  * System level KPIs(Memory, CPU)
  
  * processes (sge_qmaster,sge_execd)
  
  * services(SMF)
  
  * logfiles(errors, failures)
  
  * disk utilization
  
  * network devices (firewalls, switches, routers)
  
Examples:  

  * If the qmaster is not running the job will never actually  
get scheduled.

  * If there are no execds running (slots available) the job  
will never get executed(or really scheduled because the qmaster won't  
schedule it unless it should be able to run.

  * If queues are in an Error state they cannot run jobs.
  
  * If the file system is full it is impossible for a job to  
write it's output

Presentation Things I learned:  
  
  * People want to know why they can't use the grid from  
outside the US and  
don't quite understand how it is that Sun can have a gird and Sun  
employees can't play with it.

    * We knew this was an issue, we could have just addressed  
it straight from the slides

  * Few Sun employees (who attended our presentation have ever  
used the  
SunGrid, of course I know that a good number of Sun Employees have used  
the Grid it would seem that we just didn't get a lot of them.

    * A demo would have been well received, but if we were  
working with a demo we could have talked for hours

  * I still need to remember to breathe fully. I find that I  
will talk but  
forget to inhale full breaths if I am not paying attention.

    * I just need to pay attention to my breathing. A pause to  
take a breath is ok...also it lets people absorb your output

