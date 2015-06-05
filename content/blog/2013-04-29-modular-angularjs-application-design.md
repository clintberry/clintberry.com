---
title: Modular AngularJS App Design
author: Clint Berry
layout: post
date: 2013-04-29
url: /2013/modular-angularjs-application-design/
standard_seo_post_meta_description:
  - Organize your Angular app into modules for code reusability
vp_ptemplate_settings:
  - 'a:2:{s:10:"categories";s:0:"";s:10:"blog_posts";i:6;}'
categories:
  - Angular
---
<img src="http://clintberry.com/images/angular-module1.png" alt="angular module" width="225" height="200" class="alignleft size-full wp-image-836" />
  
I am a sucker for modules. Something about bundling a set of functionality and display logic into a module that can be easily redistributed for many apps makes my skin tingle. So when I saw that &#8220;modules&#8221; were a big part of the AngularJS methodology, I was super excited to try it out. I quickly realized however, that most of the examples online used one module for the entire application, which didn&#8217;t seem all that modular to me.
  
<!--more-->


  
At my current company we are developing an app that is ideal for the modular approach. We essentially have separate &#8220;apps&#8221; within our application, each of which we have bundled into full angular modules. This methodology could be used for many other applications as well so I thought I would share our approach.

<div class="alert alert-info">
  Note: Credit for this structure design goes to my coworker and friend <a href="https://twitter.com/tphalp" title="Tyson" target="_blank">@tphalp</a> who is rapidly becoming an angular expert and teaching me his ways.
</div>

#### Directory Structure

Here is the way we setup our directory structure:

<img src="http://clintberry.com/images/Screen-Shot-2013-04-23-at-7.23.24-PM.png" alt="App Structure" width="315" height="363" class="alignnone size-full wp-image-807" />

You can see we have the standard app directory similar to angular seed or a yeoman generated project. But we add a modules directory within the app directory. Each module then has its own sub-directory and a file for directives, controllers, filters and services and a directory for views. 

I am still somewhat conflicted on wether I should be creating a new file for each directive/controller/filter/service or if the current way is fine. Sometimes the files get to be too large an unwieldy and I want them broken up.

here is a shot further down the tree:
  
<img src="http://clintberry.com/images/Screen-Shot-2013-04-23-at-7.23.56-PM.png" alt="More app structure" width="220" height="170" class="alignnone size-full wp-image-817" />

We still maintain the scripts folder which houses app.js and our routes.js file for handling the routing.

#### Defining the Modules

Let&#8217;s have a look at the app.js file where we setup our modules and define dependencies.

<pre class="wp-code-highlight prettyprint">// Define all your modules with no dependencies
angular.module(&#039;BirthdayApp&#039;, []);
angular.module(&#039;CollectionApp&#039;, []);
angular.module(&#039;DashboardApp&#039;, []);
angular.module(&#039;LoginApp&#039;, []);
angular.module(&#039;MessageApp&#039;, []);
angular.module(&#039;PatientApp&#039;, []);
angular.module(&#039;PhoneApp&#039;, []);
angular.module(&#039;ReportsApp&#039;, []);

// Lastly, define your "main" module and inject all other modules as dependencies
angular.module(&#039;MainApp&#039;,
  [
    &#039;BirthdayApp&#039;,
    &#039;CollectionApp&#039;,
    &#039;DashboardApp&#039;,
    &#039;LoginApp&#039;,
    &#039;MessageApp&#039;,
    &#039;PatientApp&#039;,
    &#039;PhoneApp&#039;,
    &#039;ReportsApp&#039;,
    &#039;templates-main&#039;,
  ]
);

</pre>

##### &nbsp;

Creating a &#8220;MainAppModule&#8221; allows us to inject all our other modules which in turn allows each of our other modules to access each other. So if the Reports module needs to access something from the Patient module, it will be able to with this method without having to inject the Patient module directly into the Reports module.

The MainApp also allows us to do routing and use ng-view in our index.html file:

<pre class="wp-code-highlight prettyprint">&lt;html&gt;
// ...
&lt;body ng-app="MainApp"&gt;
    &lt;div ng-view&gt;&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>

##### &nbsp;

Then you define your routes on the MainApp module

<pre class="wp-code-highlight prettyprint">// scripts/routes.js
angular.module(&#039;MainApp&#039;)
  .config(
    [&#039;$routeProvider&#039;,
      function($routeProvider) {
        $routeProvider
          .when(&#039;/&#039;, {
            templateUrl: &#039;modules/dashboard/views/index.html&#039;,
            action: &#039;DashboardApp.DashboardCtrl&#039;
          })
   // ...

</pre>

#### Implementing Your Modules

Now when we go to add a directive/service/filter/controller to a module, we open up the appropriate file within that module and simply use the appropriate module name when we define it.

<pre class="wp-code-highlight prettyprint">// app/modules/patient/controllers.js
angular.module(&#039;PatientApp&#039;).controller(&#039;PatientCtrl&#039;, function($scope) {
 $scope.patients = "something";
});
</pre>

##### &nbsp;

Since we inject all modules into a &#8220;global&#8221; module you will need to take care to namespace your angular elements appropriately so you don&#8217;t run into name conflicts. Take a look at <a href="https://github.com/angular-ui/bootstrap" title="Angular Bootstrap Github" target="_blank">angular bootstrap code</a> too see how they use multiple modules and namespace them.

And that is it! Now you can make reusable modules for angular that you can use in different apps, or even <a href="http://briantford.com/blog/angular-bower.html" title="angular modules in bower" target="_blank">contribute your module to Bower</a> if it is something that will benefit the community.

Hope this helps!