---
title: HTML5 Apps on the Desktop in 2013
author: Clint Berry
layout: post
date: 2013-05-11
url: /2013/html5-apps-desktop-2013/
vp_ptemplate_settings:
  - 'a:2:{s:10:"categories";s:0:"";s:10:"blog_posts";i:6;}'
categories:
  - Desktop
  - HTML5
---
[<img src="http://clintberry.com/images/html-apps-on-desktop.jpg" alt="html-apps-on-desktop" width="379" height="286" class="alignleft size-full wp-image-923" />][1]
  
No doubt we are in the middle of the mobile revolution. The mobile-first mentality is prevalent. I feel like I see these types of posts every day lately:

<img src="http://clintberry.com/images/Screen-Shot-2013-04-22-at-10.34.21-PM.png" alt="Screen Shot 2013-04-22 at 10.34.21 PM" title="Screen Shot 2013-04-22 at 10.34.21 PM" width="520" height="91" class="alignnone size-full wp-image-793" />

<img src="http://clintberry.com/images/Screen-Shot-2013-04-22-at-10.35.34-PM.png" alt="Screen Shot 2013-04-22 at 10.35.34 PM" title="Screen Shot 2013-04-22 at 10.35.34 PM" width="521" height="84" class="alignnone size-full wp-image-795" />

While there is an obvious trent towards mobile in the consumer world, there is one place desktop is king: **Business.**<!--more-->


  
We are a long way off from getting our work done on mobile devices. Accountants, day traders, developers, and a whole litany of other careers require desktop computing of some sort. I know I sure don&#8217;t want to develop apps on my 4&#8243; phone screen. On an ipad maybe if I was traveling, but desktop size increases my productivity and I don&#8217;t see many developers dumping their desktops/laptops for ipad only development.

At my current company we develop <a href="http://getweave.com" title="Dental Phone Systems" target="_blank">phone systems for dental offices</a>. I can tell you that desktop computers will be in the dental industry for years to come.

**But why run HTML apps on the desktop?**

**Control the browser** &#8211; Bundling your app into an executable allows you to dictate what rendering engine (webkit) will be used for your app.

**Cross Platform** &#8211; HTML runs on all operating systems, so it can be much easier to create a cross platform desktop application.

**Build once, run everywhere&#8230;** &#8211; I am a huge fan of the build-once-run-everywhere methodology. While it won&#8217;t work for some projects, many applications can see a huge benefit to sharing code between the desktop, web, and mobile space.

**Window sizing and control** &#8211; If you want your app to run at a certain size, or do some more advanced things with popups, you get that control on the desktop. Most solutions also provide a way to access the file system and allow other more advanced controls you wouldn&#8217;t get with a regular web app.

Here are all the different options I have researched for creating HTML/JS applications on the desktop, with their pros and cons. Please leave comments if you know of other solutions I should look at.

### TideSDK

<a href="http://www.tidesdk.org/" title="TideSDK" target="_blank">TideSDK</a> has so much potential. They have a great looking website and a good set of tools. Of all the solutions I looked at, TideSDK has the easiest method for packaging your app. You open up the TideSDK builder and choose your HTML files, and click the package button. It runs basic web applications with ease. But then I tried running my first AngularJS app and&#8230; FAIL. So I checked the web inspector and saw a litany of errors that I knew didn&#8217;t exist when I ran my app in chrome. Turns out the webkit version used in TideSDK is over 3 years old and Angular doesn&#8217;t like it. They haven&#8217;t had a solid update for a while as well and are <a href="http://www.tidesdk.org/blog/2013/04/11/tidesdk-in-numbers/" title="TideSDK needs money" target="_blank">pleading for money</a> from the community. 

**The Good**
  
* Great Packaging Tools
* Run PHP, Python, or Ruby code on the &#8220;backend&#8221; of your app (Awesome!)
* Documentation is thorough with good getting started guides

**The Bad**

* No version bumps in a long time
* The webkit version is OLD
* They need more money to keep going
  
        
### AppJS
        
If you are a nodeJS fan, <a href="http://appjs.com/" title="Desktop Apps with HTML" target="_blank">AppJS</a> is the start of something awesome. It runs allows you to interact with desktop windows via JavaScript in a nodeJS application. Great idea, but it has some bugs and debugging issues can be a pain. Also, the issue list keeps piling up in github and it seems no progress is being made.

        
**The Good**

* NodeJS-based makes this pretty simple to get going
* Thousands of cool NodeJS libraries now available to run on the desktop
* Cool features like transparent windows, window dragging, and popups all built in

**The Bad**
            
* No recent updates and appears abandoned
* Enough bugs to make this not production worthy


### Node-webkit
                
Along the same lines as appjs, <a href="https://github.com/rogerwang/node-webkit" title="html apps on the desktop with node-webkit" target="_blank">node-webkit</a> is a desktop runtime that combines chromium with NodeJS. This project is also backed by Intel and has regular updates. Packaging apps into Exes is not trivial, however and we had some problems running our more advanced apps on windows (popup windows in particular).

**The Good**
          
* NodeJS based is awesome
* Backed by Intel, with regular updates
* Webkit and Node run in the same memory space which allows for some cool tricks

**The Bad**
                   
* Packaging apps isn&#8217;t simple
* Had trouble with popup windows on Windows

                        

### Sencha Desktop

Sencha&#8217;s solution is not free at almsot $700. It&#8217;s webkit version is outdated and it has a very limited feature set for controlling the windows (no transparency, no removing the chrome frame, etc).

* No breakdown. This baby isn&#8217;t worth the price, hands down.

                        
### Brackets Shell

The <a href="https://github.com/adobe/brackets-shell" title="Brackets Shell" target="_blank">Brackets Shell</a> is a customized version of Chromium Embedded Framework (CEF) that was created to run the <a href="http://brackets.io/" title="HTML JavaScript Editor" target="_blank">Brackets IDE</a> on the desktop. But this shell has some great features and is general enough to allow you to modify it and use it for your own project. Brackets Shell is backed by Adobe and is updated regularly, and they <a href="https://github.com/adobe/brackets-shell/pull/231" title="Pull Request for Drag capabilities" target="_blank">accept pull requests</a> for features that the Brackets IDE might not even use. Building the shell is easy thanks to the bundled Grunt file, and it is using CEF3 so the webkit version is up to date. We went with this solution at my current company and have been loving it. I intend to write a whole post on customizing and using the brackets shell soon.

<strong>UPDATE:</strong> I wrote a full post on <a href="http://clintberry.com/2013/html5-desktop-apps-with-brackets-shell/" title="Native desktop apps in HTML with Brackets Shell">getting started with brackets-shell</a>


**The Good**

* Backed by Adobe, with regular updates and an updated webkit version
* NodeJS was recently integrated into the shell for some awesome new capabilities
* Building and packaging apps is amazingly easy using GruntJS and runs on Windows and Mac

**The Bad**

* Lacking some features for customizing the main window, like transparency
* No linux support (yet)
* <a href="https://github.com/adobe/brackets/issues/2389 " title="no html5 audio in brackets shell" target="_blank">No HTML5 audio? (sad)</a>

### Other Solutions

Other solutions I haven&#8217;t looked into are Windows 8 which runs <a href="http://msdn.microsoft.com/en-us/library/windows/apps/br211385.aspx" title="Windows 8 web apps" target="_blank">HTML apps as first class citizens</a> directly on the desktop. Ubuntu desktop has a similar solution with <a href="http://developer.ubuntu.com/resources/technologies/webapps/" title="Ubuntu Web Apps" target="_blank">Ubuntu Web Apps</a>.
                                
As I see Windows and Ubuntu embracing HTML on the desktop, I can&#8217;t help but think the future is bright for desktop apps, even in a mobile first economy.

As always, let me know if you have any comments, corrections, or criticisms.


 [1]: http://clintberry.com/images/html-apps-on-desktop.jpg