---
layout: post
title: Dynamic Ipfilter Rules for RPC Services via SMF
date: '2006-03-17'
author: Shawn Ferry
tags:
- solaris
- b.s.c.
modified_time: '2010-04-29T10:24:36.916-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-2119888361652213019
blogger_orig_url: http://blog.shawnferry.com/2006/03/dynamic-ipfilter-rules-for-rpc-services_2127.html
---

How do you allow access to rpc services through ipf?

Use SMF with custom methods and dependencies to create Dynamic Ipfilter Rules
for RPC Services.

Searching found a number of people with  
the same questions and no good answers [Darren  
Reed: SunRPC
proxy](http://blogs.sun.com/roller/page/avalon?entry=sunrpc_proxy_for_ipfilter),[OpenSolaris  
Forums](http://www.opensolaris.org/jive/thread.jspa?messageID=25804&#25804) in
which Darren states "There is a proxy, of sorts, in  
the IPFilter source code at present, but it is of questionable  
integrity". Unfortunately questionable  
integrity is right out in this environment.

A simple solution was implemented, a  
startup script was written by Borgan Chu to parse `rpcinfo  
-p` and create ipfilter rules to allow traffic from the  
desired source addresses to the dynamic rpc service port. The script  
uses a configuration file with  
the following syntax, similar to the syntax of hosts.allow/hosts.deny.

> ` rpcservice: addrmask addrmask  
>  
>  
> rpcservice: addrmask addrmask ```

It created rules using the following  
logic:

> `  
>  
>  
> Split each line into service and source  
>  
>  
> For each source add a rule to ipf to allow the desired traffic  
>  
>  
> e.g. pass in quick proto _rpcproto_ from _source_  
> to _dest_ port = _rpcport_ keep  
> state | ipf -f -  
>  
>  
>  
>  
>  
> pass in quick proto udp from pool/1001 to any port = 32782 keep state  
>  
>  
>  ``pass in quick proto tcp from pool/1001 to  
> any port = 32808 keep state`  
>  
>  
>  `pass in quick proto udp from pool/1002 to any port =  
> 32782 keep state``  
>  
>  
>  ``pass in quick proto tcp from pool/1002 to  
> any port = 32808 keep state`  
>  
>  
>  `  
>  
>  
>  
>  
>  
>  `

The previous code acknowledged one _Major_  
problem. The script only runs at boot, any restart of rpcbind an rpc  
service could result in different port assignment invalidating the  
previous rules. From an operational standpoint I pictured repeatedly  
troubleshooting the same issue: A service mysteriously stops working,  
Tier 2 Engineers look into the problem and find that the  
service is running and can be reached locally and possibly from other  
hosts but not from the problem host.

One other issue with the script was  
apparent to me, future manual script execution would continue to add  
entries to the rules with no way to clean up without flushing the  
exiting rule set. This required a change to both the script and the  
default ipf.conf rules. With  the following changes  
the script supports both a stop and start method, as well as creating  
slightly different rules.

> `New base ipf rules:  
>  
>  
>  
>  
>  
> # Allow Dynamic RPC entries  
>  
>  
> pass in on bge0 all head 100  
>  
>  
> # useless rule to allow for deletion of all inbound dynamic pass rules  
>  
>  
> # i.e. you can't delete the first rule in a group  
>  
>  
> pass in on lo0 all group 100  
>  
>  
>  
>  
>  
>  
>  
>  
> start()  
>  
>  
> Split each line into service and source  
>  
>  
> For each source add a rule to ipf to allow the desired traffic  
>  
>  
> e.g. pass in quick proto _rpcproto_ from _source_  
> to _dest_ port = _rpcport_ keep  
> state group 100 | ipf -f -  
>  
>  
>  
>  
>  
> stop()  
>  
>  
> /usr/sbin/ipfstat -i | /usr/bin/grep "group 100" | /usr/sbin/ipf -r -f -  
>  
>  
>  
>  
>  
> pass in quick proto udp from pool/1002 to any port = 626 keep state  
> group 100  
>  
>  
> pass in quick proto tcp from pool/1002 to any port = 35913 keep state  
> group 100  
>  
>  
> pass in quick proto udp from pool/1001 to any port = 626 keep state  
> group 100  
>  
>  
> pass in quick proto tcp from pool/1001 to any port = 35913 keep state  
> group 100  
>  
>  
>  `

The major problem of maintaining dynamic  
rules can be resolved using [SMF](http://www.sun.com/bigadmin/content/selfheal
/smf-quickstart.html),  
creating a service for our ipf rules script with `require_all` dependencies  
on ipfilter causes the new service to run only when ipfilter is enabled.

> `dependency  
> require_all/refresh svc:/network/ipfilter:default (online)  
>  
>  
>  ``dependency  
> require_any/refresh  
> svc:/network/rpc/bind:default (online)`  
>  
>  
>

The execution can be further tuned by creating `require_any`  
dependencies  
on various rpc services.  
  
> `svcs "*rpc*"  
>  
>  
>  `

After the manifest is loaded the properties of the service can be bulk  
updated to cover most standard rpc services with the following command, or
manually updated to `require_any`  
other specific rpc services.  
  
> `svccfg -s ipfilter:rpcbind setprop  
> "rpc_services/entities = fmri:  
> (`svcs -H \*rpc\* \*nis\* \*nfs\* | awk '$NF !~ /ipfilter|bind:default/{
print $3 }'`)"  
>  
>  
>  
> Replaces the manifest defined rpc_services dependencies with the following:  
>  
>  `  
>

>

> `svc:/network/rpc/keyserv:default  
>  
> svc:/network/rpc/nisplus:default  
>  
> svc:/network/rpc/bootparams:default  
>  
> svc:/network/rpc/gss:default  
>  
> svc:/network/rpc/mdcomm:default  
>  
> svc:/network/rpc/metamed:default  
>  
> svc:/network/rpc/metamh:default  
>  
> svc:/network/rpc/rex:default  
>  
> svc:/network/rpc/rusers:default  
>  
> svc:/network/rpc/spray:default  
>  
> svc:/network/rpc/wall:default  
>  
> svc:/network/rpc-100235_1/rpc_ticotsord:default  
>  
> svc:/network/nfs/server:default  
>  
> svc:/network/rpc/meta:default  
>  
> svc:/network/rpc/smserver:default  
>  
> svc:/network/rpc/rstat:default  
>  
> svc:/network/nfs/rquota:default  
>  
> svc:/network/nfs/client:default  
>  
> svc:/network/nis/passwd:default  
>  
> svc:/network/nis/update:default  
>  
> svc:/network/nis/client:default  
>  
> svc:/network/nis/server:default  
>  
> svc:/network/nfs/cbd:default  
>  
> svc:/network/nfs/mapid:default  
>  
> svc:/network/nis/xfr:default  
>  
> svc:/network/nfs/status:default  
>  
> svc:/network/nfs/nlockmgr:default  
>  
>  ```  
>  
>  ``

>

>  
>  ``

Using a script similar to the one described above you could  
specifically limit the dependent services to the ones specified in your  
configuration file.  
  
The Service Manifest:  
  
> `  
> <?xml version='1.0'?>  
>  
>  
> <!DOCTYPE service_bundle SYSTEM  
> "/usr/share/lib/xml/dtd/service_bundle.dtd.1">  
>  
>  
> <!--  
>  
>  
> Shawn Ferry yakshaving <@> sun.com  
>  
>  
> Service manifest for maintaining dynamic ipfilter rules  
>  
>  
> -->  
>  
>  
>  
>  
>  
> <service_bundle type='manifest' name='ipfilter:dynamic'>  
>  
>  
>  
>  
>  
>   <service  
>  
>  
>  
> name='application/ipfilter/dynamic'  
>  
>  
>     type='service'  
>  
>  
>     version='1'>  
>  
>  
>  
>  
>     ``<!-- maybe more than one if this gets complex -->`  
>  
>  `  
>     <single_instance />  
>  
>  
>  
>  
>         <!-- Require ipfilter, without an online ipfilter, don't try to add
rules -->  
>  
>         <dependency  
>  
>  
>  
> name='ipfilter'  
>  
>  
>  
> grouping='require_all'  
>  
>  
>  
> restart_on='refresh'  
>  
>  
>  
> type='service'>  
>  
>  
>  
> <service_fmri value='svc:/network/ipfilter:default' />  
>  
>  
>  
> </dependency>  
>  
>  
>  
>  
>  
>  
>  
>  
>       <!--  
>  
>        An  
> instance for rpcbind, additional instances  
>  
>        could be created for  
> additional services requiring  
>  
>        dynamic rules (this would be the time to disable single_instance)  
>  
>       -->  
>  
>  
>       <instance  
> name="rpcbind" enabled="true">  
>  
>  
>  
> <!-- If rpcbind is offline, no rules to add, don't do anything -->  
>  
>               <dependency  
>  
>  
>  
> name='rpc_bind'  
>  
>  
>  
> grouping='require_all'  
>  
>  
>  
> restart_on='refresh'  
>  
>  
>  
> type='service'>  
>  
>  
>  
> <service_fmri value='svc:/network/rpc/bind:default' />  
>  
>  
>  
> </dependency>  
>  
>  
>  
>               <!-- If rule creation config file is missing, don't do
anything -->  
>  
>  
>  
> <dependency  
>  
>  
>  
> name='ipfilter_rpcbind_config'  
>  
>  
>  
> grouping='require_all'  
>  
>  
>  
> restart_on='refresh'  
>  
>  
>  
> type='path'>  
>  
>  
>  
> <service_fmri value='file://localhost/etc/ipf/ipfilter-dynamic_rpcbind.cfg'
/>  
>  
>  
>  
> </dependency>  
>  
>  
>  
>  
>               <!--  
>  
>                All of the dependencies in the rpc_services group  
>  
>                are "require_any".  
>  
>  
>  
>                With a "refresh" directive, on stop/start/refresh of those  
>  
>                services the ``ipfilter/dynamic:rpcbind service will be
restarted  
>  
>                keeping the ipf rules up to date  
>  
>               -->``  
>  
>  
>  
> <dependency  
>  
>  
>  
> name='rpc_services'  
>  
>  
>  
> grouping='require_any'  
>  
>  
>  
> restart_on='refresh'  
>  
>  
>  
> type='service'>  
>  
>  
>  
> <service_fmri value='svc:/network/nfs/server:default' />  
>  
>  
>  
> <service_fmri value='svc:/network/nfs/client:default' />  
>  
>  
>  
> </dependency>  
>  
>  
>  
>  
>         <!-- On "start" run the ipf rule script with the argument start -->  
>  
>  
>  
> <exec_method  
>  
>  
>  
> type='method'  
>  
>  
>  
> name='start'  
>  
>  
>  
> exec='/lib/svc/method/ipfilter-dynamic_rpcbind start'  
>  
>  
>  
> timeout_seconds='30' />  
>  
>  
>  
>  ``        <!-- On "stop" run the ipf rule script with the argument stop
-->`  
>  
>  `  
>  
> <exec_method  
>  
>  
>  
> type='method'  
>  
>  
>  
> name='stop'  
>  
>  
>  
> exec='/lib/svc/method/ipfilter-dynamic_rpcbind stop'  
>  
>  
>  
> timeout_seconds='60' />  
>  
>  
>  
>  
>         <!--  
>  
>           This is a transient service we are looking for a clean  
>  
>           exit code. i.e. a svcs -p shows no associated processes  
>  
>         -->  
>  
>  
>  
> <property_group name='startd' type='framework'>  
>  
>  
>  
> <propval name='duration' type='astring' value='transient'  
> />  
>  
>  
>  
> </property_group>  
>  
>  
>  
>  
>         <!--  
>  
>          Useful Info:  
>  
>  `  
>

>

>  
>

>

> `svcs -xv dynamic:rpcbind`  
>  
>  `svc:/application/ipfilter/dynamic:rpcbind (Dynamic rpc service rules for
ipfilter)`  
>  
>  ` State: online since Fri Mar 17 19:41:14 2006`  
>  
>  `   See: man -M /usr/share/man -s 1M ipf`  
>  
>  `   See: man -M /usr/share/man -s 1M ipfstat`  
>  
>  `   See: man -M /usr/share/man -s 4 ipf.conf`  
>  
>  `   See: /var/svc/log/application-sungrid-ipfilter:rpcbind.log`  
>  
>  `Impact: None.`  
>  
>

>

>  
>  ``

>

>  
>  `        -->  
>  
>  
>  
> <template>  
>  
>  
>  
> <common_name>  
>  
>  
>  
> <loctext xml:lang='C'>  
>  
>  
> Dynamic rpc service rules for ipfilter  
>  
>  
>  
> </loctext>  
>  
>  
>  
> </common_name>  
>  
>  
>  
> <description>  
>  
>  
>  
> <loctext xml:lang='C'>  
>  
>  
>  
> Add Dynamic rpc services rules to ipfilter refresh rules on  
>  
>  
>  
> restart/refresh of various services to maintain rules.  
>  
>  
>  
>  
>  
>  
> Manually clearing and reloading ipfilter rules with ipf will not  
>  
>  
>  
> trigger this service to restart/refresh.  
>  
>  
>  
> e.g. ipf -Fa -f /etc/ipf/ipf.conf  
>  
>  
>  
> </loctext>  
>  
>  
>  
> </description>  
>  
>  
>  
> <documentation>  
>  
>  
>  
> <manpage title='ipf' section='1M' manpath='/usr/share/man'  
> />  
>  
>  
>  
> <manpage title='ipfstat' section='1M' manpath='/usr/share/man'  
> />  
>  
>  
>  
> <manpage title='ipf.conf' section='4' manpath='/usr/share/man'  
> />  
>  
>  
>  
> </documentation>  
>  
>  
>  
> </template>  
>  
>  
>  
> </instance>  
>  
>  
>  
>  
>  
>       <stability  
> value='Unstable' />  
>  
>  
>   </service>  
>  
>  
>  
>  
>  
> </service_bundle>  
>  
>  
>  `

References:  
  
[Liane Praza's: smf(5) fault/retry
models](http://blogs.sun.com/roller/page/lianep?entry=smf_5_fault_retry_models)  
  
[Service Developer
Introduction](http://www.sun.com/bigadmin/content/selfheal/sdev_intro.html)  
  
[smf(5)](http://docs.sun.com/app/docs/doc/816-5175/6mbba7f3o?a=view)  
  
Notes:  
  
Any IPF rules that are actively in use when the service restarts or is
disabled are not removed. That particular aspect is not an issue for us as it
is assumed that if the rule is in use the service is still listening.  
  
topic:[Solaris]  
topic:[SMF]  
topic:[ipfilter]  

