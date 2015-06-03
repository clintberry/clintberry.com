---
title: JavaScript and the Brackets Shell Environment
author: Clint Berry
layout: post
date: 2013-07-22
url: /2013/native-desktop-javascript/
vp_ptemplate_settings:
  - 'a:2:{s:10:"categories";s:0:"";s:10:"blog_posts";i:6;}'
categories:
  - Desktop
  - HTML5
---
 <img src="http://clintberry.com/images/brackets-js.png" alt="brackets-js" width="300" height="120" class="alignleft size-full wp-image-1058" />This is the second post in my series on using the amazing <a href="https://github.com/adobe/brackets-shell/" title="Brackets Shell" target="_blank">bracket-shell</a> project for building native desktop applications with HTML and JavaScript. 

  * [Post 1 &#8211; Build Native Desktop Apps with Brackets Shell][1]
  * **Post 2 &#8211; JavaScript and the Brackets Shell Environment**

At this point you should have brackets-shell compiling/building with your own HTML app bundled inside of it. In this post I want to teach you how to interact with the shell via JavaScript.

Brackets shell has two ways to interact with the native environment via JavaScript. The first is via a set of functions created in native C++/Objective-C and then exposed in <a href="Chromium Embedded Framework" title="CEF" target="_blank">Chromium Embedded Framework</a>. The second method is to communicate with the Node.js process via websocket protocol and tell it to run commands on the native environment. This post will take a look at the first method.

#### The Brackets JavaScript Object

The brackets-shell team has already built several native functions and exposed them via JavaScript to your HTML application. To take a look, run your newly built brackets shell app and open the Chrome DevTools by right clicking anywhere in your app and selecting \`Show DevTools\`.

<img src="http://clintberry.com/images/Screen-Shot-2013-07-17-at-10.27.26-PM.png" alt="Show dev tools" width="554" height="308" class="alignnone size-full wp-image-1024" />

Once in DevTools, click on the console tab furthest to the right. Inside the console type \`brackets\` and hit enter. The console will echo the contents of that variable, but it will be minimized. Click on the little triangle arrow to the left of the response to expand the contents and you will see an \`app\` object and a \`fs\` object. Expand either of those objects to see an entire list of functions that brackets-shell gives you for free to use in your app. The \`brackets\` variable is injected into the browser via C++ and is available to your code when running in the shell. So cool.

<img src="http://clintberry.com/images/Screen-Shot-2013-07-17-at-10.24.19-PM.png" alt="brackets.app contents" width="727" height="560" class="alignnone size-full wp-image-1021" />

You can see there are some handy functions in there. To test one out, type in \`brackets.app.quit()\` into the console and your app should quit! Pretty neat that you did it from JavaScript, eh? Now let&#8217;s look at another function that doesn&#8217;t close your app. Run your app again and show devtools. In brackets.app there is a function called addMenu(). This adds a menu into the toolbar for you to use. Enter this string into the devtools console:

<pre class="wp-code-highlight prettyprint">brackets.app.addMenu(&#039;Parent&#039;, &#039;parent&#039;, &#039;&#039;, &#039;&#039;, function(){});
</pre>

The first argument is the string you want displayed for your new parent menu. The second is the ID. You should immediately see a new menu item appear for your app called &#8220;Parent&#8221;. (in OSX it appears at the top, on windows it appears right in the app itself)

<img src="http://clintberry.com/images/Screen-Shot-2013-07-17-at-11.03.58-PM.png" alt="New menu item" width="762" height="216" class="alignnone size-full wp-image-1032" />

Now let&#8217;s add a menu item into that parent menu called &#8216;My Action&#8217;. Enter in the following code in the console:

<pre class="wp-code-highlight prettyprint">brackets.app.addMenuItem(&#039;parent&#039;, &#039;My Action&#039;, &#039;myaction&#039;, &#039;&#039;, &#039;&#039;, &#039;&#039;, &#039;&#039;, function(){})
</pre>

The first argument is the ID of the parent menu you want to put this item in, the second argument is the string you want to display for your item, and the third argument is the ID of the menu item. The other blank strings are for positioning, but aren&#8217;t important for this demo.

You should now see your new menu item when you click on \`Parent\`. Now all we have to do is assign an action handler to the menu item when it is clicked. To catch events as they come in from the shell, you will need to define another object on the brackets variable called \`shellAPI\`, and within that object you must define a property called \`executeCommand\` as a function that accepts a string (commandId). The shell looks to see if that function exists and sends all commands to it so you can handle them in your code. For example, if you clicked on your new menu item with the ID \`myaction\`, the shell would call brackets.shellAPI.executeCommand(&#8216;myaction&#8217;)

Instead of doing this in the console, let&#8217;s go ahead and add this script right into our index.html file (along with some jQuery for cheating later):

<pre class="wp-code-highlight prettyprint">&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Awesome&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;

&lt;h2&gt;My Awesome App&lt;/h2&gt;
&lt;p&gt;Brackets-shell allows me to make this amazing native app with HTML. WOW!&lt;/p&gt;
&lt;script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js" &gt;&lt;/script&gt;
&lt;script&gt;
$(function(){
    brackets.app.addMenu(&#039;Parent&#039;, &#039;parent&#039;, &#039;&#039;, &#039;&#039;, function(){});
    brackets.app.addMenuItem(&#039;parent&#039;, &#039;My Action&#039;, &#039;myaction&#039;, &#039;&#039;, &#039;&#039;, &#039;&#039;, &#039;&#039;, function(){})
    brackets.shellAPI = {}
    brackets.shellAPI.executeCommand = function(command) {
        if(command == &#039;myaction&#039;) {
            alert(&#039;You definitely clicked on your new menu item!&#039;);
        }
    }
})

&lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>

Now re-open your index.html file in the brackets-shell (if you have DevTools open, and in focus, hit Command-R on mac, or Ctrl-R on windows and your app will refresh with your updated index.html file) Go ahead and click on your new menu item &#8216;My Action&#8217; and you should see the following alert box pop up:

<img src="http://clintberry.com/images/Screen-Shot-2013-07-18-at-12.31.31-AM.png" alt="clicked-alert" width="437" height="166" class="alignnone size-full wp-image-1039" />

Awesome!

Go ahead and play around with the other functions that brackets makes available to you. Especially the file system commands which really allow you to do some cool stuff. If you have trouble, make sure to check out the <a href="https://github.com/adobe/brackets-shell/blob/master/appshell/appshell_extensions.js" title="JavaScript functions for shell" target="_blank">detailed comments for each function in the source code</a>.

#### Adding Custom Native JavaScript Functions

So you want to add your own native functions, eh? If you are brave enough to venture into the dark realms of C++/Objective-C then the world is your oyster with brackets-shell. You can do pretty much anything you want to do. To illustrate, let&#8217;s add a function that minimizes the window (overly simple, but a good starting point). 

There are three files we will need to change to add custom functions:

  * **appshell/appshell\_extensions\_platform.h** &#8211; This is where we define our new C++ function
  * **appshell/appshell\_extensions\_win.cpp (or _mac.mm if you are working with OSX)** &#8211; Where the actual C++ function is written
  * **appshell/appshell_extensions.js** &#8211; This file bridges the C++ function to the JavaScript one

First let&#8217;s define our function in appshell\_extensions\_platform.h

<pre class="wp-code-highlight prettyprint">#if defined(OS_WIN)

void MinimizeWindow(CefRefPtr&lt;CefBrowser&gt; browser);

#endif
</pre>

First we add the #if statement to make sure this function only get&#8217;s defined on windows. If we were implementing on both systems, we could remove the if statement completely.
  
Then we define our function called MinimizeWindow and pass it the main browser object as a parameter.

Now we implement our new function in the appshell\_extensions\_win.cpp file:

<pre class="wp-code-highlight prettyprint">void MinimizeWindow(CefRefPtr&lt;CefBrowser&gt; browser, std::string value){
    CefWindowHandle hWnd = browser-&gt;GetHost()-&gt;GetWindowHandle();
    ShowWindow(hWnd, "SW_MINIMIZE");
}
</pre>

Obviously you need some Windows programming experience for this function. Thankfully, I have my co-worker, <a href="http://bigdevblog.com" title="Jordan C's Blog" target="_blank">Jordan</a>, who is excellent at figuring out this stuff, and so I &#8220;borrowed&#8221; some of his code. <img src="http://clintberry.com/wp-includes/images/smilies/simple-smile.png" alt=":-)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

Lastly, we need to add the function in appshell_extensions.js. We add it right after the DragWindow function (contributed by me and Jordan&#8230; yes, shameless, but I love that I got a C++ pull request accepted)

<pre class="wp-code-highlight prettyprint">/**
     * Minimize the window
     *
     * @return None. This is an asynchronous call that sends all return information to the callback.
     */
    native function MinimizeWindow();
    appshell.app.minimizeWindow = function () {
        MinimizeWindow();
    };
</pre>

Save your progress and run your build on Windows. Run your new build and open up DevTools and enter \`brackets.app.minimizeWindow()\` in the console and your window should minimize! With JavaScript!!! Excellent. 

Some other things we have added to brackets-shell at my company are:

  * &#8211; Removing the window frame
  * &#8211; Adding transparency and rounded corners
  * &#8211; Adding tray icons
  * &#8211; Customizing popup windows

We will get these pieces cleaned up and added to a forked repo for all to enjoy soon.

Good luck with your native app adventures! Hit me up with any questions or comments you might have. There are more posts to come!

 [1]: http://clintberry.com/2013/html5-desktop-apps-with-brackets-shell/ "Native Desktop Apps with HTML"