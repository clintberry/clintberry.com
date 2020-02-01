---
title: Why I Switched to Symfony 2 Framework
author: Clint Berry
layout: post
date: 2011-11-10
url: /2011/switched-symfony-2-framework/
seo_follow:
  - 'false'
seo_noindex:
  - 'false'
categories:
  - PHP
  - Symfony
---
I have been working with the Zend Framework for the last 3 years. I like it. It is flexible, heavily object oriented, and organized. However, one thing that has always bothered me is that modules in ZF have been <a title="Zend Framework Modules" href="http://weierophinney.net/matthew/archives/234-Module-Bootstraps-in-Zend-Framework-Dos-and-Donts.html" target="_blank">second-class citizens</a>. Granted, in Zend Framework 2 this is not the case, but I needed to start a project right away, and since ZF2 is still in beta, I decided to go checkout some other frameworks again.
  
<!--more-->


  
It has been a while since I looked at different PHP frameworks and I was plesently surprised by the maturity of many of them. Ultimately, though, I was most impressed with <a href="http://symfony.com/" target="_blank">Symfony 2</a> (SF2). Here are some reasons why:

## 1. Modules (Bundles)

I am a sucker for modules. For some reason I like the idea of being able to create a small library of code, with all the corresponding models/views/controllers, and then being able to drop it any application and have it work (some say I should switch to Python, which inherently works this way). It would mean all sorts of portability for my code. Symfony2 has this ability in the form of <a href="http://symfony.com/doc/current/book/page_creation.html#the-bundle-system" target="_blank">Bundles</a>. I started using them this week, and I fell in love. Did I mention there is already a <a href="http://symfony2bundles.org/" target="_blank">big library of bundles</a> that you can drop in your application? This is the future of PHP development.

## 2. Defining Routing Using Annotations

Symfony2 allows you to add <a href="http://symfony.com/doc/2.0/bundles/SensioFrameworkExtraBundle/annotations/routing.html" target="_blank">Annotations</a> (doc blocks) to controller actions that tell the application when to route to this action. You can put any regular expression pattern to match the URL. For some reason this seems so much more elegant than creating a separate routing file.

## 3. Community

Some of my <a title="SPF13" href="http://spf13.com/post/symfony2" target="_blank">favorite developers</a> are big contributors to the symfony project. Also, it is the number 1 watched PHP project on <a href="http://github.com/languages/PHP/most_watched" target="_blank">GitHub</a>. That is saying something.

## 4. Documentation &#038; Learning Curve

Zend Framework has a pretty steep learning curve and the documentation does not support beginners very well (although I know they want to [remedy this][1] for ZF2).Symfony, however, has a great getting started guide and a great online book to help you get going. It has been so easy to get started that it was hard not to smile.
  
&nbsp;

It has only been a week, but I am loving Symfony2. I keep running into new things that make me love SF2 more and more. I&#8217;m sure the honeymoon won&#8217;t last forever, though, so I&#8217;ll of course be posting more on this as I run into issues, find out the negatives, and have more comments in general. Let me know if you like Symfony or prefer something else.

**Update: I found another great link that is helpful if you are looking to use Symfony2 for your next project** &#8211; **[How OpenSky chose Symfony as their framework][2]
  
** 

&nbsp;

 [1]: http://framework.zend.com/wiki/display/ZFDEV2/Zend+Framework+2.0+Requirements#ZendFramework2.0Requirements-Easethelearningcurve
 [2]: http://engineering.opensky.com/post/how-opensky-chose-symfony2-as-our-web-framework "Sensio chooses symfony2"