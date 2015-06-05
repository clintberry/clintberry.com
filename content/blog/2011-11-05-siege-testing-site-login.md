---
title: Siege Testing Your Site Behind a Login
author: Clint Berry
layout: post
date: 2011-11-05
excerpt: I use Siege to test my web applications and get an idea of how much traffic they can sustain. A few days ago, I needed to test a part of my application that was behind a login...
url: /2011/siege-testing-site-login/
seo_follow:
  - 'false'
seo_noindex:
  - 'false'
categories:
  - Performance
---
I use <a href="http://www.joedog.org/index/siege-home" target="_blank">Siege</a> to test my web applications and get an idea of how much traffic they can sustain. It is a great tool and I suggest you read more about it <a href="http://www.joedog.org/index/siege-home" target="_blank">here</a>.

A few days ago, I needed to test a part of my application that was behind a login. I hit up UPHPU user group on IRC, and <a href="http://www.justincarmony.com/" target="_blank">Carmony</a> (a master website optimizer) suggested logging in on my browser and then hijacking the session in siege by setting header values. That is a great idea, but seems overly difficult. I doubted that I was the only one that ever wanted to do this. Isn&#8217;t there an easier way?
  
<!--more-->


  
Sure enough, I found the answer on Server Fault:
  
<a href="http://serverfault.com/questions/292679/stress-login-area-with-siege" target="_blank">http://serverfault.com/questions/292679/stress-login-area-with-siege</a>

Siege has a setting in the .siegerc file for this very scenario, although it isn&#8217;t documented.

Simply set the login-url value with a user and password and it will login for you and perform the tests.

<pre class="wp-code-highlight prettyprint">login-url = http://YourSite.com/login.php POST name=Clint&amp;pass=MyPassword</pre>

So cool!