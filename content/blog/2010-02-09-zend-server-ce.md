---
title: Zend Server CE on OS X
author: Clint Berry
layout: post
date: 2010-02-09
excerpt: 'Installing Zend Server CE on OSX - dumbified for those just switching to Mac'
url: /2010/zend-server-ce/
categories:
  - Zend Server
---
I finally did it. I bought a mac. And after navigating around OS X for ten minutes using the multi-touch pad, I can honestly tell you I don&#8217;t think I&#8217;ll ever go back. As I use it more I am curious to see if developing on my new, sleek, aluminum toy will continue to bring the feelings of ecstasy.

The first thing I did was install Zend Server CE to develop and test locally on my machine. Here are my notes on the Zend Server CE installation for the OS X, which didn&#8217;t seem as user friendly as it&#8217;s Windows Installer counterpart. But I am such a noob with Macs it is probably just me. This is for all you windows developers making the switch.
  
<!--more-->

### _Zend Server CE Configuration
  
_ 

When installing applications on OS X, you don&#8217;t usually get to specify what directory to install to. Since OS X is a linux based environement Zend Server installs itself to the same folder as most Linux distros.

You can find ZS in &#8216;/usr/local/zend&#8217; after installation. Wait, you can&#8217;t see that folder in your finder? That is because it is a hidden system folder. You can access all folders in Terminal, and if you are like some of my co-developers who love editing files in VIM still, then that is probably okay with you. But if you want to browse your system folders in the finder, you will need to unhide your system files. Open up a terminal window and type:

<pre class="wp-code-highlight prettyprint">defaults write com.apple.finder AppleShowAllFiles TRUE
killall Finder</pre>

Now you can see all your hidden system files in your finder. If that is annoying or scary to you, you can create an alias (shortcut) to your local Zend Server install, and any other sytem files you want and then use the same command with FALSE to re-hide all your system files.

By default, the ZS apache listens on port 10088 which is great if you are worried about conflicting with other programs. Personally, I don&#8217;t want to have to type in :10088 after the web address every time I want to test my software, so I want to set Apache to listen on port 80. To do this, you have to stop apache. Here is the terminal command:

<tt>sudo /usr/local/zend/bin/zendctl.sh stop-apache</tt>

Then edit the apache config file in <tt>/usr/local/zend/apache2/conf/httpd.conf<br /> Change "Listen 10088" to Listen 80" on Line 43</tt>

### _Changing the Web Root_

While you&#8217;re in the httpd.conf file, you can change any of your directives just like any other apache server. I decided to change my web root to &#8220;/Users/clint/sites&#8221;. To change yours, just edit the line with the &#8220;DocumentRoot&#8221; directive.

Re-start apache with

<tt>sudo /usr/local/zend/bin/zendctl.sh start-apache</tt>

And you are done-zo!

### _MySQL Configuration_

When I install ZSCE on Windows machines, all my third party database applications would connect to the included MySQL installation right out of the box by using &#8220;localhost&#8221; as the domain. For some reason with ZSCE on OS X you cannot connect to mysql over TCP/IP, so you will have to configure your applications to connect using mysql sockets. By default the socket file is at &#8216;/usr/local/zend/mysql/tmp/mysql.sock&#8217;

I hope that helps!