---
title: Getting Started With Plivo
author: Clint Berry
layout: post
date: 2011-08-24
url: /2011/getting-started-with-plivo/
seo_follow:
  - 'false'
seo_noindex:
  - 'false'
categories:
  - Plivo
  - Telephony
---
The open-source telephony development world has been changing drastically over the last 2 years. Projects like FreeSWITCH allow you to create simple phone applications with XML files, but until recently, advanced applications required a solid knowledge of the Erlang language. Then enters <a title="Twilio" href="http://www.twilio.com/" target="_blank">Twilio</a>&#8212; which allows any web developer to make advanced telephony apps in the web language of their choice. But with Twilio&#8217;s pricing structure, you are looking at anywhere from 2 to 4 cents per minute **IN ADDITION** to your VOIP service while also being locked-in to their cloud. **Must I learn Erlang if I want to make cool telephony apps for free?**
  
<!--more-->

## Enter Plivo

Then on May 26, 2011, the [Plivo project was announced][1]. Plivo is essentially an open source Twilio extension that works with FreeSWITCH. What does it mean? It means you can build advanced telephony apps using whatever web programming language you want via the Plivo Framework&#8230; FOR FREE! Yes, that is right, you can build a simple PBX system for your company&#8217;s phone system with PHP. You can create a skill-based routing server in python. And my personal favorite, you can create real-time web-phones with NodeJS. It truly is amazing. <a title="Plivo Website" href="http://plivo.org" target="_blank">Read more about Plivo.</a> But enough of my babbling. Let&#8217;s get to the good stuff.

## Getting Started

I have encountered a few people on the #plivo IRC chat having trouble getting Plivo going. They usually are web developers, new to FreeSWITCH like me. The beautiful part about Plivo is you don&#8217;t need to be a FreeSWITCH expert (although it doesn&#8217;t hurt).

#### Step 1 &#8211; Prepare your Box

You need a place to install FreeSWITCH and Plivo. If you don&#8217;t have an extra box lying around I recommend using a virtual Ubuntu Linux machine for development. I prefer to use the free program <a title="Virtual Box" href="http://www.virtualbox.org/" target="_blank">Virtual Box</a> to power my virtual machines. I also suggest grabbing the <a title="Turnkey Linux" href="http://www.turnkeylinux.org/core" target="_blank">Turnkey linux ISO</a> which is a basic Ubuntu ISO image ready to go. If you need extra help getting your virtual machine setup, follow these instructions: <a title="Turnkey Linux on VirtualBox" href="http://www.turnkeylinux.org/docs/installation-appliances-virtualbox" target="_blank">How to Setup Turnkey Linux on Virtual Box</a>.

If you are planning to give public access to your app then I recommend using [VPS.net][2] for a cheaper entry into cloud hosting.

#### Step 2 &#8211; Install FreeSWITCH

A big thanks to the <a title="Guys at Plivo" href="http://www.plivo.org/about-2/" target="_blank">guys at Plivo</a> for providing a simple shell script for installing FreeSWITCH. Switch to your home directory and enter these commands into your shell (taken directly from Plivo website):
  
`The open-source telephony development world has been changing drastically over the last 2 years. Projects like FreeSWITCH allow you to create simple phone applications with XML files, but until recently, advanced applications required a solid knowledge of the Erlang language. Then enters <a title="Twilio" href="http://www.twilio.com/" target="_blank">Twilio</a>&#8212; which allows any web developer to make advanced telephony apps in the web language of their choice. But with Twilio&#8217;s pricing structure, you are looking at anywhere from 2 to 4 cents per minute **IN ADDITION** to your VOIP service while also being locked-in to their cloud. **Must I learn Erlang if I want to make cool telephony apps for free?**
  
<!--more-->

## Enter Plivo

Then on May 26, 2011, the [Plivo project was announced][1]. Plivo is essentially an open source Twilio extension that works with FreeSWITCH. What does it mean? It means you can build advanced telephony apps using whatever web programming language you want via the Plivo Framework&#8230; FOR FREE! Yes, that is right, you can build a simple PBX system for your company&#8217;s phone system with PHP. You can create a skill-based routing server in python. And my personal favorite, you can create real-time web-phones with NodeJS. It truly is amazing. <a title="Plivo Website" href="http://plivo.org" target="_blank">Read more about Plivo.</a> But enough of my babbling. Let&#8217;s get to the good stuff.

## Getting Started

I have encountered a few people on the #plivo IRC chat having trouble getting Plivo going. They usually are web developers, new to FreeSWITCH like me. The beautiful part about Plivo is you don&#8217;t need to be a FreeSWITCH expert (although it doesn&#8217;t hurt).

#### Step 1 &#8211; Prepare your Box

You need a place to install FreeSWITCH and Plivo. If you don&#8217;t have an extra box lying around I recommend using a virtual Ubuntu Linux machine for development. I prefer to use the free program <a title="Virtual Box" href="http://www.virtualbox.org/" target="_blank">Virtual Box</a> to power my virtual machines. I also suggest grabbing the <a title="Turnkey Linux" href="http://www.turnkeylinux.org/core" target="_blank">Turnkey linux ISO</a> which is a basic Ubuntu ISO image ready to go. If you need extra help getting your virtual machine setup, follow these instructions: <a title="Turnkey Linux on VirtualBox" href="http://www.turnkeylinux.org/docs/installation-appliances-virtualbox" target="_blank">How to Setup Turnkey Linux on Virtual Box</a>.

If you are planning to give public access to your app then I recommend using [VPS.net][2] for a cheaper entry into cloud hosting.

#### Step 2 &#8211; Install FreeSWITCH

A big thanks to the <a title="Guys at Plivo" href="http://www.plivo.org/about-2/" target="_blank">guys at Plivo</a> for providing a simple shell script for installing FreeSWITCH. Switch to your home directory and enter these commands into your shell (taken directly from Plivo website):
  
` 

Let it run for a few minutes and you will have a working copy of FreeSWITCH ready to go.

[box type=&#8221;note&#8221; style=&#8221;rounded&#8221; border=&#8221;full&#8221;]Keep in mind that this install script will only work for CentOS 5.5+ and Debian-based Distros (Ubuntu)[/box]

#### Step 3 &#8211; Install Plivo

Yet another easy installation thanks to the Plivo guys&#8230;
  
``The open-source telephony development world has been changing drastically over the last 2 years. Projects like FreeSWITCH allow you to create simple phone applications with XML files, but until recently, advanced applications required a solid knowledge of the Erlang language. Then enters <a title="Twilio" href="http://www.twilio.com/" target="_blank">Twilio</a>&#8212; which allows any web developer to make advanced telephony apps in the web language of their choice. But with Twilio&#8217;s pricing structure, you are looking at anywhere from 2 to 4 cents per minute **IN ADDITION** to your VOIP service while also being locked-in to their cloud. **Must I learn Erlang if I want to make cool telephony apps for free?**
  
<!--more-->

## Enter Plivo

Then on May 26, 2011, the [Plivo project was announced][1]. Plivo is essentially an open source Twilio extension that works with FreeSWITCH. What does it mean? It means you can build advanced telephony apps using whatever web programming language you want via the Plivo Framework&#8230; FOR FREE! Yes, that is right, you can build a simple PBX system for your company&#8217;s phone system with PHP. You can create a skill-based routing server in python. And my personal favorite, you can create real-time web-phones with NodeJS. It truly is amazing. <a title="Plivo Website" href="http://plivo.org" target="_blank">Read more about Plivo.</a> But enough of my babbling. Let&#8217;s get to the good stuff.

## Getting Started

I have encountered a few people on the #plivo IRC chat having trouble getting Plivo going. They usually are web developers, new to FreeSWITCH like me. The beautiful part about Plivo is you don&#8217;t need to be a FreeSWITCH expert (although it doesn&#8217;t hurt).

#### Step 1 &#8211; Prepare your Box

You need a place to install FreeSWITCH and Plivo. If you don&#8217;t have an extra box lying around I recommend using a virtual Ubuntu Linux machine for development. I prefer to use the free program <a title="Virtual Box" href="http://www.virtualbox.org/" target="_blank">Virtual Box</a> to power my virtual machines. I also suggest grabbing the <a title="Turnkey Linux" href="http://www.turnkeylinux.org/core" target="_blank">Turnkey linux ISO</a> which is a basic Ubuntu ISO image ready to go. If you need extra help getting your virtual machine setup, follow these instructions: <a title="Turnkey Linux on VirtualBox" href="http://www.turnkeylinux.org/docs/installation-appliances-virtualbox" target="_blank">How to Setup Turnkey Linux on Virtual Box</a>.

If you are planning to give public access to your app then I recommend using [VPS.net][2] for a cheaper entry into cloud hosting.

#### Step 2 &#8211; Install FreeSWITCH

A big thanks to the <a title="Guys at Plivo" href="http://www.plivo.org/about-2/" target="_blank">guys at Plivo</a> for providing a simple shell script for installing FreeSWITCH. Switch to your home directory and enter these commands into your shell (taken directly from Plivo website):
  
`The open-source telephony development world has been changing drastically over the last 2 years. Projects like FreeSWITCH allow you to create simple phone applications with XML files, but until recently, advanced applications required a solid knowledge of the Erlang language. Then enters <a title="Twilio" href="http://www.twilio.com/" target="_blank">Twilio</a>&#8212; which allows any web developer to make advanced telephony apps in the web language of their choice. But with Twilio&#8217;s pricing structure, you are looking at anywhere from 2 to 4 cents per minute **IN ADDITION** to your VOIP service while also being locked-in to their cloud. **Must I learn Erlang if I want to make cool telephony apps for free?**
  
<!--more-->

## Enter Plivo

Then on May 26, 2011, the [Plivo project was announced][1]. Plivo is essentially an open source Twilio extension that works with FreeSWITCH. What does it mean? It means you can build advanced telephony apps using whatever web programming language you want via the Plivo Framework&#8230; FOR FREE! Yes, that is right, you can build a simple PBX system for your company&#8217;s phone system with PHP. You can create a skill-based routing server in python. And my personal favorite, you can create real-time web-phones with NodeJS. It truly is amazing. <a title="Plivo Website" href="http://plivo.org" target="_blank">Read more about Plivo.</a> But enough of my babbling. Let&#8217;s get to the good stuff.

## Getting Started

I have encountered a few people on the #plivo IRC chat having trouble getting Plivo going. They usually are web developers, new to FreeSWITCH like me. The beautiful part about Plivo is you don&#8217;t need to be a FreeSWITCH expert (although it doesn&#8217;t hurt).

#### Step 1 &#8211; Prepare your Box

You need a place to install FreeSWITCH and Plivo. If you don&#8217;t have an extra box lying around I recommend using a virtual Ubuntu Linux machine for development. I prefer to use the free program <a title="Virtual Box" href="http://www.virtualbox.org/" target="_blank">Virtual Box</a> to power my virtual machines. I also suggest grabbing the <a title="Turnkey Linux" href="http://www.turnkeylinux.org/core" target="_blank">Turnkey linux ISO</a> which is a basic Ubuntu ISO image ready to go. If you need extra help getting your virtual machine setup, follow these instructions: <a title="Turnkey Linux on VirtualBox" href="http://www.turnkeylinux.org/docs/installation-appliances-virtualbox" target="_blank">How to Setup Turnkey Linux on Virtual Box</a>.

If you are planning to give public access to your app then I recommend using [VPS.net][2] for a cheaper entry into cloud hosting.

#### Step 2 &#8211; Install FreeSWITCH

A big thanks to the <a title="Guys at Plivo" href="http://www.plivo.org/about-2/" target="_blank">guys at Plivo</a> for providing a simple shell script for installing FreeSWITCH. Switch to your home directory and enter these commands into your shell (taken directly from Plivo website):
  
` 

Let it run for a few minutes and you will have a working copy of FreeSWITCH ready to go.

[box type=&#8221;note&#8221; style=&#8221;rounded&#8221; border=&#8221;full&#8221;]Keep in mind that this install script will only work for CentOS 5.5+ and Debian-based Distros (Ubuntu)[/box]

#### Step 3 &#8211; Install Plivo

Yet another easy installation thanks to the Plivo guys&#8230;
  
`` 
  
Again, it should run for a bit and when it is done it should be ready to go.

#### Step 4 &#8211; Configure Your Dialplans

Since most web developers don&#8217;t come from a Telephony background, the whole FreeSWITCH thing is new. On the #plivo IRC channel, I see new people, usually web developers like me, getting stuck at this spot all the time.

###### Dialplan Basics

The dialplan in freeswitch essentially tells freeswitch how to route calls when they come in. An out-of-the box freeswitch setup installs 20 extensions (just like an office phone system) with passwords to login. The extensions are 1000-1020 and all have the same password &#8211; 1234. With the Plivo FreeSWITCH installer, the default dialplan is overwritten to intercept all calls and those extensions don&#8217;t work very well anymore. If you are just getting started with the whole freeswitch thing, I have provided an alternate version of the dialplan file to allow you to do testing of your freeswitch install as well as develop Plivo apps.

[Download Dialplan Here &#8211; default.xml][3]

Copy that dialplan into the freeswitch/conf/dialplan directory. This dialplan is the same as the out of the box FreeSWITCH dialplan, except I added the plivo config and restricted it to extension 1005. This means the rest of your extensions will work like normal so you can see how awesome freeswitch is. Restart freeswitch by typing &#8216;shutdown&#8217; in the freeswitch terminal.

#### Step 5 &#8211; Get a Free SoftPhone For Testing

Now things start to get fun. Get a free soft phone. For osx I got [Blink Lite][4]. Once you get a free soft phone you can login to your FreeSWITCH install by extension and IP address. If you installed freeswitch on 192.168.1.5 then for your credentials on your softphone you would use 1000@192.168.1.5 as the user, and 1234 as the password. You should get logged right in and now you can call any other extension. Try dialing 1001 and you should get taken to voicemail. Pretty sweet right? Try dialing 1005 and right now you should get nothing but a disconnected call, since 1005 is now connected to plivo.

#### Step 6 &#8211; Configure Plivo

The last step before you take over the world with your new telephony app is configuring plivo. This is extremely easy. Go to your plivo install directory (typically /usr/local/plivo) and go to the etc/plivo directory. Open the default.conf file and look for these directives:

<pre class="wp-code-highlight prettyprint">DEFAULT_ANSWER_URL = http://127.0.0.1:3000/answer/
DEFAULT_HANGUP_URL = http://127.0.0.1:3000/hangup/</pre>

Typically your web server won&#8217;t be tied to port 3000. You need to change these values to point to your web server IP address and port (typically 80). Also, change the URL to point wherever your app will be. I will leave mine pointing to /answer. Then make sure FreeSWITCH is started and go ahead and start plivo:

<pre class="wp-code-highlight prettyprint">/usr/local/plivo/bin/plivo start</pre>

#### Step 7 &#8211; Hello World

FINALLY, TIME FOR AN AWESOME PHONE APP! Your system is all setup, and plivo will be answering all calls on extension 1005. Let&#8217;s write our first app. I will make this first app in PHP, but I won&#8217;t be doing anything special, it will work on any web language that can serve an xml file. Since plivo is pointing to answer/ I will create answer/index.php and inside that file I will put plivo response xml code:

<pre class="wp-code-highlight prettyprint">&lt;?xml version="1.0" encoding="UTF-8" ?&gt;
&lt;Response&gt;
     &lt;Speak&gt;Hello World&lt;/Speak&gt;
&lt;/Response&gt;</pre>

Now open your soft phone and dial 1005. You should hear a pleasant voice, saying &#8220;hello world&#8221;. It should be an exciting moment for you.

Now your limits are endless. Write some amazing telephony apps in a web language and let me know some of the cool projects you are building. I&#8217;ve got several more posts planned about building plivo apps coming in the next few weeks, so be ready for some exciting developments.

 [1]: http://www.plivo.org/2011/05/26/launch-of-plivo-an-open-source-alternative-to-twilio/ "Plivo Project"
 [2]: http://manage.aff.biz/z/146/CD15515/&dp=40017
 [3]: http://clintberry.com/download/default.xml
 [4]: http://itunes.apple.com/us/app/blink-lite/id431473881?mt=12