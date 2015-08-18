---
title: Easy SIP Capture With Flanders
draft: true
author: Clint Berry
layout: post
date: 2015-07-20
url: /2015/easy-sip-capture-with-flanders/
categories:
  - SIP
  - Telephony
---

If you are working with open source telephony (Asterisk, FreeSWITCH, Kamailio, etc.), you are probably familiar with the Homer project. Homer is critical for dignosing SIP related issues by capturing all SIP signals on your platform and putting an interface over them to allow you to search and export them for analysis. Homer was instrumental in helping us at Weave as we scaled our phone platform, but we always struggled with a few of the things in Homer:

* Installation was not trivial
* A separate capture agent was required (usually Kamailio with certain settings turned on)
* The user interface did not allow easy sharing of call data between co-workers
* It used MySQL by default which may not be the best DB for this kind of data
* We REALLY wanted real-time SIP capturing in the user interface.

So we set out to build the SIP capture tool we always wanted with those goals in mind

## Announcing "Flanders"

Flanders is an easy-to-use sip capture utility and user interface that satisfies our needs/wants at Weave. In particular, real-time capture monitoring in the GUI.
