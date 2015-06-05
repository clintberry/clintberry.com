---
title: Installing Magento on OS X
author: Clint Berry
layout: post
date: 2010-02-25
url: /2010/installing-magento-on-os-x/
categories:
  - Magento
---
Mark Hopwood has created a great post on [installing Magento stand-alone on OS X][1]. I know there have been several posts in the Magento forums with questions on this. Just remember if you are installing Magento on OS X with Zend Server, you need to open your TCP/IP sockets in the mysql.conf file so that Mysql can be connected to using localhost or the local IP address.

 [1]: http://blog.markhopwood.com/2010/02/22/installing-magento-enterprise-stand-alone-on-os-x/