---
title: Speed Up Your WordPress Development Cycle With Git
author: Clint Berry
layout: post
date: 2011-07-30
url: /2011/speed-up-your-wordpress-development-cycle-with-git/
seo_follow:
  - 'false'
seo_noindex:
  - 'false'
categories:
  - Git
  - PHP
  - Wordpress
---
If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
    `If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
` 
  
    Now merge in any updates from your current branch, or merge in a new branch altogether. This command merges the latest update from the 3.2 branch into your WordPress install:
  
    ``If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
    `If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
` 
  
    Now merge in any updates from your current branch, or merge in a new branch altogether. This command merges the latest update from the 3.2 branch into your WordPress install:
  
`` 
  
    Now you have the most recent version of WordPress. It is a beautiful thing.
  
    Don&#8217;t worry! If it breaks something, it is easy to go back. First enter:
  
    ```If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
    `If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
` 
  
    Now merge in any updates from your current branch, or merge in a new branch altogether. This command merges the latest update from the 3.2 branch into your WordPress install:
  
    ``If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
    `If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
` 
  
    Now merge in any updates from your current branch, or merge in a new branch altogether. This command merges the latest update from the 3.2 branch into your WordPress install:
  
`` 
  
    Now you have the most recent version of WordPress. It is a beautiful thing.
  
    Don&#8217;t worry! If it breaks something, it is easy to go back. First enter:
  
``` 
  
    This will output the most recent changes to your repo. Look for your last change which was merging. It will have a line like this:
    
    <pre class="wp-code-highlight prettyprint">Merge remote branch &#039;upstream/master&#039;
</pre>
    
    Take the commit sha (the long number right after the word commit) that is located on the commit before the Merge line. Take that sha and enter the command:
  
    ````If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
    `If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
` 
  
    Now merge in any updates from your current branch, or merge in a new branch altogether. This command merges the latest update from the 3.2 branch into your WordPress install:
  
    ``If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
    `If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
` 
  
    Now merge in any updates from your current branch, or merge in a new branch altogether. This command merges the latest update from the 3.2 branch into your WordPress install:
  
`` 
  
    Now you have the most recent version of WordPress. It is a beautiful thing.
  
    Don&#8217;t worry! If it breaks something, it is easy to go back. First enter:
  
    ```If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
    `If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
` 
  
    Now merge in any updates from your current branch, or merge in a new branch altogether. This command merges the latest update from the 3.2 branch into your WordPress install:
  
    ``If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
    `If you have developed WordPress sites for clients this process might sound familiar to you:

  * Get a new client that wants a wordpress site
  * Download the most recent wordpress
  * Find a good current blank wordpress theme to start implementing the design
  * Upload to a staging server to for client to see
  * Client requests changes
  * You make changes on local server, then upload to staging server
  * Repeat last three steps until finished and then you deploy the site to live

After lots of trial and error, I have come up with a process that is more efficient, and allows rapid development of wordpress sites. I use Git, Git Submodules, and WordPress child themes to get going quicker and keep code up to date with minimal effort. (If your not using a <a href="http://en.wikipedia.org/wiki/Revision_control" target="_blank">revision control </a>system like <a href="http://git-scm.com/" target="_blank">Git</a> or <a href="http://subversion.tigris.org/" target="_blank">SVN</a> for development, then check out <del datetime="2012-04-22T20:33:39+00:00"><a href="http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" target="_blank">this funny question and answer session on Stack Overflow</a></del> **Update:** Stack Overflow moderators removed the question&#8230; bummer. Check out the funny-ness in the <a href="http://web.archive.org/web/20100722082534/http://stackoverflow.com/questions/132520/good-excuses-not-to-use-version-control" title="Why to use version control" target="_blank">wayback machine internet archiver</a> instead)
  
<!--more-->


  
For this rapid WordPress development cycle, you will need to have Git installed.

## 1. Get the Most Current WordPress

The wordpress core dev team uses SVN to manage wordpress development. But I want to use Git for speed and easy branching. Fortunately there is a <a href="https://github.com/WordPress/WordPress" target="_blank">great WordPress git clone</a> that is updated every fifteen minutes. To get the most recent version of WordPress, simply clone that repo to your project directory.

<pre class="wp-code-highlight prettyprint">mkdir projectA
git clone https://github.com/markjaquith/WordPress.git ./projectA
cd projectA
git checkout origin/3.2-branch</pre>

Now you have the most recent version of wordpress, and it will be easier than ever to upgrade wordpress and revert if something goes wrong. But we will get into the upgrade process later.

## 2. Starting with a Quality Blank Theme

To help get your new WordPress project off the ground, let&#8217;s get a quality blank theme that updates regularly. I usually use the <a title="Roots WordPress Theme" href="http://www.rootstheme.com/" target="_blank">Roots theme</a>, which is a quality HTML5 wordpress theme that updates regularly. To make sure our theme stays up to date with the latest wordpress updates, we will add the Roots theme as a Git Submodule. This allows us to essentially add a git repository inside of another repository. Why do this? Because we can update our theme code to the latest release with a simple Git command, instead of downloading and merging on our own. To add the sub-module, change to your project directory and add the sub-module to the themes folder then initialize it:

<pre class="wp-code-highlight prettyprint">cd projectA
git submodule add https://github.com/retlehs/roots.git ./wp-content/themes/roots
git submodule init
git submodule update</pre>

<i class="icon-warning-sign"></i>**Update:** I have now forked mark&#8217;s wordpress git and added roots as a submodule. You can simply clone or fork my repo to save a step. Get it here: <a title="Clint Berry's Github Account" href="https://github.com/clintberry/WordPress" target="_blank">https://github.com/clintberry/WordPress</a> Make sure to clone recursive to get the roots theme. This repo updates nightly to the latest wordpress and roots release.

Now you have a quality blank theme installed into your git repo as a sub-module.

## 3. Adding Your Custom Design (WordPress Child Theme)

Since we have added the new blank theme as a git submodule, we don&#8217;t want to edit that theme directly. This could cause problems when/if we want to update our code base for the theme. To better avoid update problems, we can use the often-under-utilized-wordpress feature called child themes. Child themes allow you essentially &#8220;inherit&#8221; all the functionality of a &#8220;base&#8221; theme. In this case, we will create a child theme &#8220;MyTheme&#8221; that will have access to all the functionality of our base theme &#8220;Roots&#8221;.

Creating child themes in wordpress is simple:

  1. Add a new folder to your wordpress themes directory called &#8220;MyTheme&#8221;
  2. Create a file called &#8220;style.css&#8221; with the following text: <pre class="wp-code-highlight prettyprint">/*
Theme Name:     MyProject
Theme URI:      http://clintberry.com/
Description:    Roots Child Theme
Author:         Clint Berry
Author URI:     http://clintberry.com
Template:       roots
Version:        0.1.0
*/

@import url("../twentyten/style.css");
</pre>
    
    Take notice of the &#8220;Template: roots&#8221; field. That is what lets WordPress know that this is a child theme and to inherit all of the roots functionality. Also since this style.css file overwrites the roots css file, we import the css file from roots. </li> 
    
      * Create a functions.php file to add any custom WordPress functions. WordPress will load your new functions.php file along with the Roots functions.php file, so all is well.
      * Copy any template file you want to change from the Roots theme to your new child theme directory. Any template file found in your new directory will overwrite the template in Roots.</ol> 
    
    Now you can start your theme development with an HTML5 bang! Child themes make it much easier to get started and even keep your file structure a big cleaner. 
    
    ## Update WordPress Like A Pro
    
    Yes, you could update WordPress in the admin panel, but you want to do it in a much cooler, easier-to-revert way. With Git, now you can. First, backup your database. Then in the root wordpress directory, update your Git repo by using git fetch:
  
` 
  
    Now merge in any updates from your current branch, or merge in a new branch altogether. This command merges the latest update from the 3.2 branch into your WordPress install:
  
`` 
  
    Now you have the most recent version of WordPress. It is a beautiful thing.
  
    Don&#8217;t worry! If it breaks something, it is easy to go back. First enter:
  
``` 
  
    This will output the most recent changes to your repo. Look for your last change which was merging. It will have a line like this:
    
    <pre class="wp-code-highlight prettyprint">Merge remote branch &#039;upstream/master&#039;
</pre>
    
    Take the commit sha (the long number right after the word commit) that is located on the commit before the Merge line. Take that sha and enter the command:
  
```` 
    
    Now restore your database and you are back in action.
    
    ## Easier WordPress Deployment
    
    Deploying a WordPress site has always been a thorn in my side. Currently, I am developing a plugin for much easier deployment, but for now things are a bit hairy. The nice thing about git is keeping your code base up to date is much easier. Unfortunately, Git doesn&#8217;t help much with the database side of things, so we will focus on just the code base for now. 
    
    #### Deploy Using Git Clone
    
    Deploying with Git Clone is the easiest way to get your code installed on a different server. All you have to do is push your repo to a remote repository (like github) and then clone it directly on the live server. This allows easy updating in the future as well since a simple git pull is all that is required to get the most recent code base. If you don&#8217;t have SSH access or if the server you are deploying to doesn&#8217;t have Git, then this method won&#8217;t work.
    
    #### Old School FTP
    
    For a good guide on deploying the old fashioned way, as well as deploying the database, see <a href="http://www.codemyownroad.com/13-steps-to-deploy-wordpress-from-your-localhost-to-a-live-web-server/" target="_blank">This Post</a>
    
    I hope this helps getting your WordPress sites off the ground quicker and easier! As always, any comments or suggestions are welcome.