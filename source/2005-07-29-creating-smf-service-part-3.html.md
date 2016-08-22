---
layout: post
title: Creating an SMF service (Part 3)
date: '2005-07-29'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:36.933-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-8239845996196102903
blogger_orig_url: http://blog.shawnferry.com/2005/07/creating-smf-service-part-3.html
---

If you havn't already read them you might want to start with [Part
1](http://blogs.sun.com/roller/page/yakshaving?entry=creating_an_smf_service_part)
or continue with [Part
2](http://blogs.sun.com/roller/page/yakshaving?entry=creating_an_smf_service_part1).  
  
It took me a bit longer to get this written than I intended, now that I have
figured out that textile and code/pre tags don't play well this should easier.

In short however, it does what I need it to do now. The whole thing wouldn't
suffer from some more work though.

In particular it is currently very brittle. If my relocatable package is
relocated parts of this will break. I particularly don't know what to do with
installations on a root server.

The following is the first part of the manifest.

    <?xml version='1.0'?>  
    <!DOCTYPE service_bundle SYSTEM "/usr/share/lib/xml/dtd/service_bundle.dtd.1">  
    <!-- Service manifest for the Sysedge monitoring program(s) -->  
      
    <service_bundle type='manifest' name='SUNWsmcsysedge'>  
      
      <!-- A milestone to collect dependencies for both sysedge and sysedgeplus -->  
      <service   
        name="application/monitoring/sysedge_deps"  
        type="milestone"  
        version="1">  
      
          <!-- common sysedge dependencies -->  
          <instance name="default" enabled="false">  
              <dependency  
                  name='filesystem'  
                  grouping='require_all'  
                  restart_on='none'  
                  type='service'>  
                  <service_fmri value='svc:/system/filesystem/local' />  
              </dependency>  
              <dependency  
                  name='network'  
                  grouping='require_all'  
                  restart_on='refresh'  
                  type='service'>  
                  <service_fmri value='svc:/network/initial' />  
              </dependency>  
              <dependency  
                  name='sysedge_cf'  
                  grouping='require_all'  
                  restart_on='refresh'  
                  type='path'>  
                  <service_fmri  
                    value='file://localhost/etc/opt/SUNWsmcsysedge/sysedge.cf' />  
              </dependency>  
          </instance>  
      
          <!-- sysedgeplus dependencies -->  
          <instance name="plus" enabled="false">  
            <dependency  
                name='sysedgeplus'  
                grouping='require_all'  
                restart_on='refresh'  
                type='path'>  
                <service_fmri  
                  value='file://localhost/opt/SUNWsmcsysedge/plus/sysedgeplus' />  
            </dependency>  
          </instance>  
          <stability value='Unstable' />  
      </service>  
    
The brief rundown including how I generally read it to myself:

    <?xml version='1.0'?>  
    <!DOCTYPE service_bundle SYSTEM "/usr/share/lib/xml/dtd/service_bundle.dtd.1">  
    <!-- Service manifest for the Sysedge monitoring program(s) -->  
    
This is XML, and the DTD can be found here
_/usr/share/lib/xml/dtd/service_bundle.dtd.1_

    <service_bundle type='manifest' name='SUNWsmcsysedge'>  
    
This is a manifest called _SUNWsmcsysedge_

      <!-- A milestone to collect dependencies for both sysedge and sysedgeplus -->  
      <service   
        name="application/monitoring/sysedge_deps"  
        type="milestone"  
        version="1">  
    
The first "service" in the manifest is _application/monitoring/sysedge_dep_
and it is a milestone.  
  
(It is a milestone because somewhere the docs said roughly: A milestone is a
syntectic service that collects dependencies)

          <!-- common sysedge dependencies -->  
          <instance name="default" enabled="false">  
              <dependency  
                  name='filesystem'  
                  grouping='require_all'  
                  restart_on='none'  
                  type='service'>  
                  <service_fmri value='svc:/system/filesystem/local' />  
              </dependency>  
    
As stated in the comment this is an instance of the service/milestone called
default and it has a dependency called filesystem.  
  
The dependency is part of the **require_all** groups of dependencies it has
`restart_on=none` set which indicates that the local filesystems are reqiured
to start the service, but after it is started changes to the filesystem do not
require a restart to this service automatically.

This service has multiple instances with different instances being used or re-
used to fulfill the requirements pf various other services.

              <dependency  
                  name='network'  
                  grouping='require_all'  
                  restart_on='refresh'  
                  type='service'>  
                  <service_fmri value='svc:/network/initial' />  
              </dependency>  
    
The second dependency is a requirement for networking support to be enabled.
Again it is part of the **require_all** grouping, however in this case if the
network configuration is refreshed the dependent service is restarted.
Effectively I believe that this actually propagates down through the
dependencies.

          <dependency  
                  name='sysedge_cf'  
                  grouping='require_all'  
                  restart_on='refresh'  
                  type='path'>  
                  <service_fmri  
                    value='file://localhost/etc/opt/SUNWsmcsysedge/sysedge.cf' />  
          </dependency>  
    
The third dependency in _sysedge_defs:default_ or
_application/monitoring/sysedge_deps:default_ is also part of the
**require_all** grouping. Note that the type here is *path*. The path type
indicates that the *service_fmri* is pointing at a path that must exist to
meet the dependency requirement.

    </instance>  
    
The closing instance tag indicates that the first defined instance (In this
case named default) is finished.

          <!-- sysedgeplus dependencies -->  
          <instance name="plus" enabled="false">  
            <dependency  
                name='sysedgeplus'  
                grouping='require_all'  
                restart_on='refresh'  
                type='path'>  
                <service_fmri  
                  value='file://localhost/opt/SUNWsmcsysedge/plus/sysedgeplus' />  
            </dependency>  
          </instance>  
    
The second instance exists to validate the sysedgeplus instance.

          <stability value='Unstable' />  
      </service>  
    
The stability _Unstable_ indicates that I am still making up my mind about how
all of this should work and I am apt to change it at any time. (Sun has actual
definitions of the different stability levels)

Stay tuned, in our next episode I will define a service that actually runs a
process.

topic:{technorati}[Solaris]  
topic:{technorati}[SMF]  

