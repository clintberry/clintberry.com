---
title: Why Plivo Cloud Is Better Than Twilio
author: Clint Berry
layout: post
date: 2012-07-19
url: /2012/why-plivo-cloud-is-better-than-twilio/
categories:
  - Plivo
  - Telephony
---
<a href="http://plivo.com" title="Plivo Cloud" target="_blank"><img src="/images/plivologo.png" alt="" title="plivo" class="alignleft size-full wp-image-603" style="margin:15px 10px 10px 10px" /></a>
  
Last year around this time Plivo.org was announced to the public, allowing anyone to develop telephony applications with the web language of their choice on top of their _OWN_ FreeSWITCH based phone system. Plivo.org is an amazing project, and if you aren&#8217;t familiar with it I suggest you read [my getting started post][1].

Now the founders of Plivo.org have built on that technology to create a new telephony platform, <a href="http://plivo.com" title="Plivo Cloud" target="_blank">Plivo Cloud</a>. I know what you are thinking&#8211; another Twilio. And while the general idea (building awesome telephony apps in any web language) is still the same, there are a few things that make Plivo Cloud much different, and much better.
  
<!--more-->

<div class="alert alert-info">
  <strong>Disclaimer:</strong> I know the founders of Plivo and I am big fan of the open source version, so much so that I wrote the NodeJS helper library. I am hardly un-biased, but that is simply because I think the idea is awesome.
</div>

When I first learned about Twilio I was blown away. &#8220;You mean I can create awesome telephone applications in PHP? Sweet!&#8221; Having ties to entrepreneurs in the call center industry, I immediately pitched to them how easy it would be to create amazing call center applications with phone functions built right in. They were impressed, but I couldn&#8217;t get any of them to bite the bullet. The same issues came up again and again. Plivo Cloud is a game-changer because it addresses some of these issues. Here are some of the biggest differences:

### Direct Endpoint Integration

One of the biggest pain points for companies thinking about switching to a Twilio-based solution is cost. Conversations with customers would go something like this:

**Customer:** &#8220;So you mean I pay 2 cents per minute on top of my internal VOIP system costs?&#8221;
  
**Me:** &#8220;Well, yes&#8230;&#8221;
  
**Customer:** &#8220;No thanks&#8230;&#8221;

Plivo Cloud allows you to connect SIP-based phones directly to the cloud, meaning you can use Plivo as your phone system! This means you don&#8217;t pay for VOIP in your office on top of platform costs. You simply pay per minute for the platform, and it acts as your VOIP service as well. Amazing!

This also means getting a new office setup with a phone system is relatively painless. Buy SIP phones, point them to your Plivo account, and you are off to the races.

### Carrier Choice

Another pain point of services like Twilio/Voxeo is that you are locked-in to whatever carriers they are using for their cloud. If your business is under contract with a different carrier, it can be a pain to migrate. But what if you could tie your new, shiny platform with your current carrier? That would be awesome. Well, Plivo Cloud is awesome. They will help you tie in your current carrier to their FreeSWITCH based system and then charge a flat rate of 0.4 cents per minute for platform usage. This provides a level of freedom unheard of for this type of company.

### Self-Hosted Cloud

The last key difference with Plivo Cloud is the ability to host your own cloud. While this hasn&#8217;t been announced or priced yet, I have spoken to Venky about it and this is a key part of the Plivo strategy. Imagine not being held hostage by a third party platform with all their Amazon Cloud outages and anything else they choose to inflict on your application. For mission critical telephony applications, it is crucial to have redundancy, and Plivo Cloud will offer a level of control that competitors won&#8217;t.

### I&#8217;m Not a Hater

To be clear, I love Twilio. I think it has changed the web by making phone system integration into web apps easier than ever. But Plivo Cloud offers many freedoms and pricing advantages that separate it from the competition.

In an upcoming post I will give tips and tricks on how to migrate your app to Plivo Cloud.

Also, read more about Plivo on the [TechCrunch announcement][2]

 [1]: /2011/getting-started-with-plivo/ "Getting started with Plivo"
 [2]: http://techcrunch.com/2012/07/09/yc-backed-plivo-launches-its-scalable-api-platform-for-voice-sms-apps/ "Plivo Cloud Rocks!"