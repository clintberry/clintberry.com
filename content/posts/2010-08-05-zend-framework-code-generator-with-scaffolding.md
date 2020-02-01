---
title: 'Zend Framework Code Generator With Scaffolding &#8211; ZFcodo'
author: Clint Berry
layout: post
date: 2010-08-05
url: /2010/zend-framework-code-generator-with-scaffolding/
categories:
  - Zcodo
  - Zend Framework
---
**Update:** This project is now hosted at my [GitHub account][1].

While I am a huge fan of Zend Framework, I miss having the code generation that is bundled with other frameworks I use. Since my favorite codegen/ORM is from the Qcubed project, I decided to take the Qcubed code generator and customize it for Zend Framework.
  
<!--more-->


  
A little background on Qcubed: [Qcubed][2] is a framework branched off of the [Qcodo][3] project. Qcubed code generation connects to your already built database and generates your models, views, and controllers based on your database schema. Qcubed has a built-in ORM that uses the Active Record model and also a custom querying language similar to Doctrine ORM. [(Go here for more information on the Qcubed project)][2]

The advantage to using Qcubed over Doctrine is that I have it generating not only my models, but also basic forms, controllers, and views. I will also be running some benchmark tests because I think that out of the box, Qcubed ORM performs better than Doctrine. I have dubbed this project &#8220;ZFcodo&#8221;, which references the origins of the Qcubed project: Qcodo. So far I have a hacked together proof of concept that I think is interesting. Here is what I have with some example code so you can see why this will save you time setting up your Zend Framework Projects.

To get this going, I started with a basic Zend Framework application structure. Then I copied the entire Qcubed project into my custom library folder &#8220;ZFcodo&#8221;. I stripped out anything I could see from Qcubed that was specifically for the framework, and not related to the ORM or code generation and altered some files a little to match a Zend Framework application. I then created a ZFcodo manager file that would set all the constants needed and added the following to /application/configs/application.ini:

<pre class="wp-code-highlight prettyprint">; ---
; Database and ZFcodo settings
; ---

zq.db.host = "localhost"
zq.db.username = "sample"
zq.db.password = "sample"
zq.db.name = "sample"

; ZFcodo Model Settins
zq.modelFolder = "/models"
zq.modelBaseFolder = "/Base"

; ZFcodo Form Settings
zq.generateForms = 0
zq.formFolder = "/forms"

; ZFcodo Controller Settings
zq.generateControllers = 0
zq.controllerFolder = "/controllers"
</pre>



My next step was to add an init function to my /application/Bootstrap.php file that initialized the ZFcodo ORM:

<pre class="wp-code-highlight prettyprint">protected function _initQcubed()
{
    require APPLICATION_PATH . &#039;/../library/ZFcodo/Codegen/Manager.php&#039;;

    $zqConfig = $this-&gt;getOption(&#039;zq&#039;);
    $manager = new Zcodo_Manager($zqConfig);
    $manager-&gt;loadOrm();
}
</pre>

And finally, I modified the Qcubed code generation PHP script to work from a CLI and put it into /application/scripts/zf-codo.php.

Now I can run zf-codo.php from the command line and **it generates my models (including base models), basic controllers, basic views, and basic forms.**

![ZFcodo Code Generation From Command Line][4]

For my example, I created a simple &#8220;customer&#8221; table with &#8220;first name&#8221; and &#8220;last name&#8221;. The code generator added the following files for the table:

<pre class="wp-code-highlight prettyprint">application/controllers/CustomerController.php
application/forms/Customer.php
application/models/Customer.php
application/models/Base/Customer.php
application/views/scripts/customer/list.phtml
application/views/scripts/customer/create.phtml
application/views/scripts/customer/edit.phtml
</pre>

Then to list the customers, all I do is go to http://projectlocation/customer/list and I see this page:

<img src="http://clintberry.com/images/screenshot1.png" alt="Zend Framework Code Generator" style="border:1px solid gray;" />

The codegen alone can get you off to a great start on your next ZF project, but with the built-in ORM, you can save hours on your project. ZFcodo will turn the 80/20 rule into the 90/10 rule&#8230; <img src="http://clintberry.com/wp-includes/images/smilies/simple-smile.png" alt=":-)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

Right now this is more of a proof of concept (a nice way of saying it is a hack job) but I will continue to work on this, and if there seems to be a demand for this, I will work on it even more. I have implemented the ORM into a few live projects already and it seems to be working great.

You can get the entire project source code here: [Zend Framework ORM][1]
  
**Update:** This project is now hosted at my [GitHub account][1]
  
To use it, just edit the application.ini file with your database info, and then run the script application/scripts/zcodo.php from the CLI.
  
That easy&#8230; 

The future plans for this project are to:
  
&#8211; organize the directory structure the Zend way
  
&#8211; add Zend_Paginator ability to the ORM
  
&#8211; style the generated forms
  
&#8211; and allow module based codegen

Let me know if you have any questions.

 [1]: https://github.com/clintberry/zf-codo
 [2]: http://qcu.be/
 [3]: http://www.qcodo.com/
 [4]: http://clintberry.com/images/codegenscreenshot.png