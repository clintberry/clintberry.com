---
title: Build a Scalable Phone System - Part 1 - The Registrar
draft: true
author: Clint Berry
layout: post
date: 2015-09-01
url: /2015/build-a-scalable-phone-system-with-freeswitch-and-kamailio-part-1/
categories:
  - Telephony
  - FreeSWITCH
  - Kamailio
  - SIP
---

# TLDR

This is the first of a series of posts designing an open-source, scalable, redundant, hosted VoIP platform. This post is focused on the setup of the SIP Registrar.

# Intro

After working at [Weave][1] for four years, I am finally getting comfortable with the Session Initiation Protocol (SIP). Even working with this awkward protocol for a good chunk of my time, it has taken my feeble mind until now to feel like I am knowledgeable enough to blog on the subject. And while these posts are not exclusively about the protocol itself, they are about the open source projects that use SIP, and in turn, require a working knowledge of the protocol. The intent of this series of posts is to walk through designing a scalable, multi-tenant, hosted phone system using open source technologies. 

# Who is this For?

This series of posts will only appeal to a very niche group of people. The kind of people that are interested in building a scalable, redundant, communications platform. So why even blog about it at all? Only this reason: To help the next generation of open-source communication technologists build upon this knowledge, much like I did with the brilliant developers that have worked on these open source projects before me. 

Some of the terms I use in these posts will be very VoIP specific and some working knowledge of the industry will probably be required to make full sense of the information.

Hopefully this will help someone on their start-up journey! 


# The Problem

At Weave, one of our core products is a fully hosted voice over IP solution. Our minimum viable product relied on FreeSWITCH to handle all SIP endpoint registration, and also all media. We put a SIP proxy ([OpenSIPS][2]) in the mix to handle dynamic routing to the correct FreeSWITCH registrars from trunk providers (connection to the PSTN or the traditional land-line phone system we all know and love). In my opinion it is the absolute best MVP that could have been built and kudos to our consultants ([Izeni][3]) who built it. 

When I began to take it over four years ago we had less than 100 offices using our phone service. Compare that to now where we have over 2300 offices using our phone system with over 16,000 active devices connected to the system. Growing that fast has pushed our MVP to the limit and I have the scars to prove it. We have had a service outage caused by any and every weak point in our architecture. Some weaknesses we knew about and wanted to fix, others were complete surprises. Here are some of our findings:

* FreeSWITCH is the best open-source media server around
* FreeSWITCH struggles to handle SIP at scale
* SIP and Media scale in a completely different way and should be separate for optimal structure
* Kamailio (formerly OpenSER) is brilliant at handling large amounts of SIP traffic
* Kamailio is not nearly as beginner friendly as FreeSWITCH and is much more difficult to configure
* It used to be hard to make dynamic SIP routing decisions in Kamailio at scale
* Distributed systems are hard


NOTE: I know some people will disagree with some of those statements, especially with regard to FreeSWITCH and SIP. While I do have some years under my belt, there is no doubt that we could have missed something along the way. Configurations, modules, or something else entirely may have created a world where FreeSWITCH could handle the amount of SIP traffic we needed. We spent weeks and months trying to solve it, but couldn't. In particular presence was a major issue that we couldn't get FreeSWITCH to handle at scale for prolonged periods of time.

After learning all of the above, we set out to re-architect our entire system from scratch in a way that was horizontally scalable, supported multi-data center redundancy, and was rock solid stable and secure. This is, and always will be, a work in progress. But we have made large strides and these posts will outline the direction we have gone.

NOTE: I will mostly be focused on the scalability and redundancy aspects of the system in these posts. The configurations won't be production ready, more importantly, they won't be secure. That I will leave up to you, as there are many ways to get that information.

# The Registrar

One of our first goals at making our system more scalable and stable, was to remove registrations out of FreeSWITCH. We were familiar with the [Kamailio][4] project and knew that it was touted to handle TONS of SIP traffic. So we opted to try and use it for our registrar.

## Configuring Kamailio

_**NOTE**: I don't cover Kamailio installation here, but instead will provide a Docker container with Kamailio pre-installed that will allow you to test the configuration rapidly._

Out of the box, Kamailio is a stateless SIP proxy. But its modular design allows it to function as so much more. Configuring it as a registrar is fairly trivial. First, we will add a database to store all the registrations in. I prefer postgresql so will add a connection string in my configuration. Then I add 


[1]:http://getweave.com
[2]:http://opensips.org
[3]:http://izeni.com
[4]:http://www.kamailio.org
