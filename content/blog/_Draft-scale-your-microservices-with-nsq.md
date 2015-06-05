---
title: Scale Your Micro-services With NSQ
draft: true
author: Clint Berry
layout: post
date: 2015-07-01  
url: /2015/scale-your-microservices-with-nsq/
categories:
  - Go
---

At <a href="http://getweave.com" target="_blank">Weave</a> we have bought into the micro-service architecture and we have been loving it. It allows us to silo groups of developers into fully autonomous groups that can develop and release faster without waiting on other groups. It also makes developers happier because they have more ownership of a project.

<strong>Note: Check out these links for inspiration on some good micro-service thoughts: </strong>
<ul>
    <li><a title="Spotify Engineering Culture" href="https://labs.spotify.com/2014/03/27/spotify-engineering-culture-part-1/" target="_blank">Spotify Engineering Culture</a></li>
    <li><a title="Microservices at Netflix" href="http://nginx.com/blog/microservices-at-netflix-architectural-best-practices/" target="_blank">Microservices at Netflix</a></li>
    <li><a title="Immutable Infrastructure with Docker" href="http://tech.gilt.com/post/90578399884/immutable-infrastructure-with-docker-and-ec2-gilt-at" target="_blank">Immutable Infrastructure With Gilt</a></li>
</ul>
When you switch from one (or a handful of) monolithic application(s) to dozens of small micro-services it adds some complexity to the system. One of those complexities is facilitating communication between all the micro-services. To solve that we looked as several messaging systems to broker that communication. The obvious first choice was RabbitMQ. RabbitMQ is very powerful and has tons of features. Setup for a single box instance was relatively simple as well and it seemed like a good choice. But when we looked at making RabbitMQ more distributed with no single point of failure it became a bit more complicated. (See <a title="Distributed RabbitMQ" href="https://www.rabbitmq.com/distributed.html" target="_blank">https://www.rabbitmq.com/distributed.html</a>

Granted, distributed RabbitMQ setups are not impossible, but we thought we would search for a messaging system that was designed to be distributed from its foundation. That is when I came across <a title="NSQ" href="http://nsq.io" target="_blank">NSQ</a>. Despite a crappy logo, the project has been awesome for us. Here are some of the reasons why NSQ was a good choice for us and why it may be the right choice for you.
<h4>Distributed Architecture</h4>
If you like tools with no single point of failure out of the box, then you will like NSQ. Spin up 1, or 100 different consumers/producers and they all are aware of eachother via one or more nsqlookup processes. (I have seen several commits for using the Gossip protocol, so no need for nsqlookup processes soon)
<h4>Handles an Insane Amount of Messages</h4>
While Weave has only pushed a few million messages through NSQ, I have talked to several companies on IRC pushing many millions per day through NSQ in production environments. Check out @lxfontes NSQ counter... https://twitter.com/lxfontes/status/569660077868773376 Yup, 3 commas in that sucker. And those quantities are possible with little to no tweaking.
<h4>Simple to Get Started</h4>
It is so easy to get going with NSQ. Just install the lightweight nsqd process on any machine that will be generating messages (producer). One of the huge advantages to using Go (the language NSQ is programmed in) is that there are no dependencies. It is just a single executable. Next, create an HTTP Post to localhost on the nsqd listening port and you are off to the races. Now you can create as many consumers as you want to handle load balancing, or publish to all consumers for pub-sub architecture. Which brings me to the elegance of the design.
<h4>Elegant Design</h4>
NSQ uses topics and channels. Every producer when posting to NSQ posts to a Topic. Every consumer that connects to NSQ connects to a Topic and a Channel. You don't need to create a topic or channel from any sort of config or admin, you just subscribe and it exists. For pub-sub, have all your consumers subscribe to a different channel name (we use host names) and all messages will be sent to each consumer. For load balancing have all your consumers connect to the same channel and all messages will only go to one consumer. Image taken from the NSQ site:

<img src="https://f.cloud.github.com/assets/187441/1700696/f1434dc8-6029-11e3-8a66-18ca4ea10aca.gif" alt="NSQ Design" />
<h4>Admin and Visualization</h4>
The last reason I would recommend using NSQ is because of the simplicity of the NSQ admin tool, and also the built-in integration with Statsd/graphite. When you have dozens of micro-services communicating with each other, monitoring can get difficult. But if you standardize on using NSQ as the default way to communicate between services, then the admin and the graphite integration allow you to create a nice dashboard for easy visualization of the status of your system. At Weave we use NSQ to load balance handling phone call records so, by extension, we can use the built in metrics to track how many calls our system places and create a nice dashboard for our TV.

&nbsp;