---
title: Build a Scalable, Redundant, Open-source Phone System - Part 1 - Architecture
draft: true
author: Clint Berry
layout: post
date: 2015-09-01
url: /2015/build-a-scalable-phone-system-with-freeswitch-and-kamailio-part-1/
categories:
  - Telephony
  - FreeSWITCH
  - Kamailio
---

# Intro

After working at [Weave][1] for three years, I am finally getting comfortable with the Session Initiation Protocol (SIP). Even working with this awkward protocol day-in and day-out (mostly), it has taken my feeble mind until now to feel like I am knowledgable enough to blog on the subject. And while these posts are not exclusively about the protocol itself, they are about the open source projects that use SIP, and in turn, require a working knowledge of the protocol. The intent of this series of posts is to walk through the installation and basic configuration a of a scalable and redundant phone system. 

# Who is this For?

This series of posts will only appeal to a very niche group of people. The kind of people that are interested in building a scalable, redundant, communications platform. So why even blog about it at all? Only this reason: To help the next generation of open-source communication technologists build upon this knowledge, much like I did with the brilliant developers that have worked on these open source projects before me. 

Some of the terms I use in these posts will be very VoIP industry related and some working knowledge of the industry will probably be required to make full sense of the information.

Hopefully the information here will help someone on their startup journey! 


# The Problem

At Weave, one of our core products is a fully hosted voice over IP solution. Our minimum viable product relied on FreeSWITCH to handle all SIP endpoint registration, and also all media. We put a SIP proxy ([OpenSIPS][2]) in the mix to handle dynamic routing to the correct FreeSWITCH registrars from trunk providers (connection to the PSTN or the traditional land-line phone system we all know and love). In my opinion it is the absolute best MVP that could have been built and kudos to our consultants ([Izeni][3]) who built it. 

When I took it over three years ago we had less than 100 offices using our phone service. Compare that to now where we have 1700 offices using our phone system. Growing that fast has pushed our MVP to the limit and I have the scars to prove it. We have had a service outage caused by any and every weak point in our architecture. Some weaknesses we knew about and wanted to fix, others were complete surprises. Here are some of our findings:

* FreeSWITCH is the best open-source media server around
* FreeSWITCH can't handle SIP at scale
* Putting your registrar and your media server on the same box is not a good idea at scale
* Kamailio (formerly OpenSER) is brilliant at handling large amounts of SIP traffic
* Kamailio is not nearly as beginner friendly as FreeSWITCH and is much more difficult to configure
* It used to be hard to make dynamic dial plan decisions in Kamailio at scale
* Distributed systems are hard, distributing data reading and writing is even harder


NOTE: I know some people will disagree with those statements, especially with regard to FreeSWITCH and SIP. While I do have some years under my belt, there is no doubt that we could have missed something along the way. Configurations, modules, or something else entirely may have created a world where FreeSWITCH could handle the amount of SIP traffic we needed. We spent weeks and months trying to solve it, but couldn't. In particular presence was a major issue that we couldn't get FreeSWITCH to handle at scale.

# The Architecture

After learning all of the above, we set out to re-architect our entire system from scratch in a way that was horizontally scalable, supported multi-data center redundancy, and was rock solid stable and secure. What we came up with is by no means the only way to achieve our goals, but it is worked for us and could work for you.

NOTE: I will mostly be focussed on the scalability and redundancy aspects of the system in these posts. The configurations won't be production ready, more imporantly, they won't be security minded. That I will leave up to you, as there are many ways to get that information.

<img src="/images/phone-network-diagram.png" alt="Phone Network Diagram" style="max-width:100%;height:auto;"/>


## The Pieces

First I will describe each piece in that architecture image.

### SIP Registar

The SIP registrar is where are all endpoints will register to (duh). It is powered by Kamailio with the registartion module and the new and improved jannson-rpc module (which we tested heavily) for dynamic loading of users and other load balancer information. 

NOTE: Dynamic data loading in Kamailio proved to be problematic for us for a long time. The options available to us either required us to add another piece of tech to our stack (mod_kazoo) or weren't stable at all when tested under load (jsonrpc-c). Lucky for us, Flowroute released a new and improved JSON RPC module using the JANSSON C library and it has performed beautifully.

### SIP Proxy

The sip proxy is our gateway to the PSTN and is also powered by Kamailio. It will be configured to support multiple trunk providers and will use the dynamic routing module to decide which PSTN to send outbound traffic to. 

NOTE: We opted to use two different boxes for the proxy and registar, but you could easily combine them into one box.

### Media Servers

These babies are powered by FreeSWITCH and get their dialplan configuration via HTTP using the mod\_xml_curl module. Most call routing instructions will be handled in FreeSWITCH because of the ease of configuration. 

### Manager

The manager will serve all routing instructions, user data, and any other data the media, proxy, and registrar may need. It supports access via json-rpc for kamailio instances, and HTTP for FreeSWITCH boxes.

### Database

Scalable, redundant, multi-datacenter databases are a hot topic right now, in particular NoSQL solutions like Couchbase. Most of the data we are dealing with, however, is relational data (e.g. users are related to devices, devices are related to locations, locations are related to accounts). Others have gone the route of putting relational data into NoSQL solutions, which can work fine. We, however, opted to stick with a tried and true, performant, relational database: Postgresql. 

I can hear you say it... what about master-master replication, especially across data centers? Introducing Postgres Bi-directional replication (BDR). It is in beta, but is slated to go into production Postgres in the next version. We are using the beta to much success. I have a blog post drafted on installing and configuring BDR that I will release soon.








[1]:http://getweave.com
[2]:http://opensips.org