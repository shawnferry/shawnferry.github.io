---
layout: post
title: |-
  CEC: Enterprise Level Role Based Access Control and the Coming Perfect
  Storm
date: '2007-10-09'
author: Shawn Ferry
tags:
- Sun CEC
- CEC 2007
- Sun
- b.s.c.
modified_time: '2010-04-29T10:17:26.918-07:00'
blogger_id: tag:blogger.com,1999:blog-496684037280688885.post-6786334753946688049
blogger_orig_url: http://blog.shawnferry.com/2007/10/cec-enterprise-level-role-based-access.html
---

IdM and RBAC are the next "new thing" Manage roles not users.

Why is it a perfect storm. SOX, Periodic Access Review. larger numbers of
users, LDAP has good penetration. RBAC clarification in the industry from
NIST.

NIST RBAC

  1. Level 1, flat
  2. Level 2 hierarchial 
    1. Inherited
    2. Activated
  3. Level 3, constrained  

    1. must enforce separation of duties at the role level
    2. static and dynamic (check at session creation and deny)
  4. Level 4, symetrical with permission review  

    1. SOD inspection of permissions granted by roles in addition to role conflicts
    2. performance must be roughly equiv

Federation/Extranet: Some interesting concepts gaining traction. Sun Managed
Operations could use this (theoretical) to centralize synamic user management
without requiring customers to add our users to their systems. (all dependent
on customer requirements, this is not a solution that we support now and may
never support :) this is a forward looking random note)  
  
