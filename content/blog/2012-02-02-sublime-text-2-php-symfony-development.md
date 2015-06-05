---
title: 'Sublime Text 2 for PHP & WordPress Development'
author: Clint Berry
layout: post
date: 2012-02-02
url: /2012/sublime-text-2-php-symfony-development/
categories:
  - Development Environment
  - PHP
  - Wordpress
---
Sublime Text 2 is an amazing code editor that I started using on the recommendation of a friend. I fell in love and haven&#8217;t looked back. Here are some recomendations for setting up Sublime Text 2 for PHP and WordPress development.
  
<!--more-->

### Why is Sublime Text 2 Awesome?

Where do I start&#8230; Here are some of my favorite features:

  * It is light weight. The download size is under 10 megs and it runs fast
  * It is VERY customizable. Every key binding and setting imaginable is available to customize
  * Plugins, plugins, plugins. ST2 has a built in python interpreter which allows web developers to create custom plugins. (The only thing that would make it better is if it had node.js bundled in for plugins, but I&#8217;ll take Python)
  * vim key bindings. Yes, if you love vim, or want to love vim, then you will love sublime text 2, which comes bundled with &#8220;vintage mode&#8221;, which allows you to use vim keybindings. 
      * It runs on Windows, OSX, and Linux, so I can use ST2 on almost any computer. </ul> 
        There are **MANY** more reasons. Check out this link for some good ones: <a href="http://net.tutsplus.com/tutorials/tools-and-tips/sublime-text-2-tips-and-tricks/" title="Sublime Text Tips and Tricks" target="_blank">Sublime Text Tips and Tricks</a>
        
        Now you should be fully convinced that Sublime Text is the greatest thing since sliced bread  <img src="http://clintberry.com/wp-includes/images/smilies/simple-smile.png" alt=":-)" class="wp-smiley" style="height: 1em; max-height: 1em;" />Let&#8217;s get it setup for developing PHP applications.
        
        ### Setting Up Package Management
        
        One of the coolest plugins for ST2 is the package manager. This should be the first plugin you install because it is the gateway to a big list of other great plugins for you. Installation is easy, simply open ST2 and press command+\` (on windows ctrl+\`) and it will open up the ST2 console. Copy and paste this code into the terminal:
        
        <pre class="wp-code-highlight prettyprint">import urllib2,os; pf=&#039;Package Control.sublime-package&#039;; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),&#039;wb&#039;).write(urllib2.urlopen(&#039;http://sublime.wbond.net/&#039;+pf.replace(&#039; &#039;,&#039;%20&#039;)).read()); print &#039;Please restart Sublime Text to finish installation&#039;
</pre>
        
        &nbsp;
        
        That&#8217;s it, you now have package control built into your editor. To access package control, press shift+command+p and start typing &#8220;package&#8221;. (shift+command+p opens the window of all commands in sublime text 2). You will see a list of commands related to package control. Sweet.
        
        ### Packages you need
        
        Let&#8217;s get this puppy setup for some PHP development!
        
        To install a package, type shift+command+p and start typing &#8220;package install&#8221; and hit enter when it is highlighted. Wait a few seconds and it will bring up a list of available packages. Select the package you want to install and then hit enter. That&#8217;s it! Now follow those steps for the following packages:
        
        ##### [Sublime Code Intel][1]
        
        What use would editing code be without some good autocompletion? CodeIntel provides a better code completion by parsing all the files in your project and creating a nice index of autocomplete options. It also provides click-to-definition functionality by clicking alt-click on a function or variable name. I have found that this plugin isn&#8217;t as consistent or as fast as full IDEs such as Eclipse, but in general is pretty good.
        
        ##### [Sublime Linter][2]
        
        This plugin gives you real-time error checking in your php code which is standard in most IDEs. Now it is standard in Sublime Text <img src="http://clintberry.com/wp-includes/images/smilies/simple-smile.png" alt=":-)" class="wp-smiley" style="height: 1em; max-height: 1em;" />
        
        ##### [Zen Coding][3]
        
        If you aren&#8217;t using Zen Coding to create your HTML, then you are missing out. This adds zen coding features into sublime text 2.
        
        ##### [Sublime JsDocs][4]
        
        Don&#8217;t let the name fool you, this plugin is a smart Doc Block generator for PHP as well as JavaScript. It is the best one of any I have tried. To use it, simply type /** in front of the function/variable you want to generate the docs for and hit tab. You will get a nice block generated for you.
        
        ##### [WordPress Plugin][5]
        
        This plugin adds a bunch of good snippets to have in your snippet/auto-complete library for WordPress. This helps speed up development significantly if you do a lot of wordpress sites.
        
        ##### [Git][6] &#038; also [Git Sidebar][7]
        
        Git commands integrated into git hub for your favorite version control. As an added bonus, add git functionality to the sidebar context menu as well.
        
        ### Syntax Specific Settings
        
        One of the greatest things about ST2 is how easy it is to customize. All the settings are simply JSON documents that you can change to meet your needs. Go ahead and open up Preferences->Global Preferences and you will see all settings that are available. But don&#8217;t change this file, since it gets overwritten during upgrades. Open up the User preferences file and put in any changes that you need. 
        
        But if you want to customize setting for only certain syntaxes (like PHP) you can do so by opening a PHP file in the editor and then going to Preferences->Syntax Specific and you will be able to change settings for only the PHP syntax. For instance I like it when I double click a variable in php for it to select the $ symbol as well, but by default ST2 doesn&#8217;t do this, so I change the PHP settings to not include $ in word separators.
        
        <pre class="wp-code-highlight prettyprint">{
	"extensions":
	[
		"php"
	],
  "word_separators": "./\\()\"&#039;-:,.;&lt;&gt;~!@#%^&*|+=[]{}`~?"
}</pre>
        
        &nbsp;
        
        ### Important Shortcut Keys
        
        Lastly, I want to include a short list of some short cut keys you will likely enjoy:
        
        **Alt+[1-9]** &#8211; Switch to a certain tab
  
        **Command+P** &#8211; Fast file switching, Line Number, Jump to definition (within file)
  
        **Alt+.** &#8211; Close a started HTML tag (although I suggest Zen Coding Plugin)
  
        **Esc** &#8211; Enter Vintage Mode (if enabled)
  
        **Command+D** &#8211; Multi-select same word. This is VERY COOL.
        
        Here are some other good <a href="http://www.sublimetext.com/forum/viewtopic.php?f=2&#038;t=4198" title="Sublime Text 2 Shortcut Keys" target="_blank">shortcut</a> <a href="http://www.sublimetext.com/docs/selection" title="Sublime Text 2 Shortcut Keys" target="_blank">keys</a>
        
        I hope you enjoy Sublime Text as much as I have!

 [1]: https://github.com/Kronuz/SublimeCodeIntel "Sublime Code Intel"
 [2]: https://github.com/Kronuz/SublimeLinter ""
 [3]: https://bitbucket.org/sublimator/sublime-2-zencoding "Zen Coding"
 [4]: https://github.com/spadgos/sublime-jsdocs "PHP Doc Blocks"
 [5]: https://github.com/purplefish32/sublime-text-2-wordpress ""
 [6]: https://github.com/kemayo/sublime-text-2-git ""
 [7]: https://github.com/SublimeText/SideBarGit ""