---
title: Zend Studio and Git(Hub)
author: Clint Berry
layout: post
date: 2011-02-26
url: /2011/zend-studio-and-github/
seo_follow:
  - 'false'
seo_noindex:
  - 'false'
categories:
  - Git
  - Zend Studio
---
I have been using Zend Studio as my IDE for years, and for version control I have always used SVN. Zend Studio Eclipse plays very nicely with SVN out of the box, but lately it seems Git has gained a lot of momentum so I thought I would look into it. It turns out Git is awesome and is much better suited for many of the things I do. (See: [Why Git?][1])

**But will Git play nicely with Zend Studio 8?**
  
Thanks to eclipse plugins, you bet it will!<!--more-->

#### How to set it up

First you will need to install [EGit][2], the Git plugin for eclipse. The easiest way to do this is to add the EGit repository to Zend Studio. Open up Zend Studio and do the following:

  * Navigate to Help -> Install New Software
  * Click the &#8220;Add..&#8221; button to add the EGit repository (http://download.eclipse.org/egit/updates)![Adding the EGit Repository][3]
  * Agree to everything you see (of course read it first)
  * Zend Studio will restart and you will have git support

#### Adding Keys

To access git hub you need to use public and private SSH keys. If you have never done that before, here is how to create a key using Zend Studio:

  * Open up Zend Studio&#8217;s preferences window
  * Navigate to General->Network Connections->SSH2
  * Select the key management tab and click &#8220;Generate RSA key&#8221;
  * You will see that the text box is now filled with &#8220;ssh-rsa&#8221; and then a bunch of giberish. That is your new public key![Creating SSH Key][4]
  * Click the &#8220;Save Private Key&#8221; Function and confirm that you want to save it without a passphrase
  * Login to your GitHub account and go to Account Settings->SSH Public Keys
  * Click add a public key and enter the name (e.g. my laptop) and paste the public key text generated by Zend Studio into the box

**Now you will have access to github from Zend Studio**

My next post will be on how to clone projects in github and add them as Zend Studio projects.

 [1]: http://stackoverflow.com/questions/871/why-is-git-better-than-subversion
 [2]: http://www.eclipse.org/egit/
 [3]: http://clintberry.com/images/egit-repo.png
 [4]: http://clintberry.com/wp-content/assets/images/zend-git-key.png