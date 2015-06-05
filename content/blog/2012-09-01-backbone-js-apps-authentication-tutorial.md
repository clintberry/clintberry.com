---
title: Backbone.js apps with Authentication Tutorial
author: Clint Berry
layout: post
date: 2012-09-01
url: /2012/backbone-js-apps-authentication-tutorial/
vp_ptemplate_settings:
  - 'a:2:{s:10:"categories";s:0:"";s:10:"blog_posts";i:6;}'
categories:
  - Backbone
  - JavaScript
  - PHP
---
<img src="/images/backbonelocked.png" alt="backbonelocked" title="backbonelocked" width="568" height="111" class="alignnone size-full wp-image-670" />
  
At my current company I am working on my first large-scale production backbone.js app and I couldn&#8217;t be happier. After using backbone.js for a few months I have caught the vision and I am becoming more and more proficient. But every once and a while I still run into problems I would consider basic, but I can&#8217;t seem to find much help on the interwebs. Authentication with backbone.js apps was one of those problems. So I am posting the solution I came up with in hopes it will benefit someone else, and hopefully will garner some feedback or potentially better ways to solve authentication with Backbone.js.
  
<!--more-->

#### Starting Code Base

To start this tutorial, I will be using an already created backbone.js application called <a href="https://github.com/ccoenraets/backbone-directory" title="Backbone Directory" target="_blank">Backbone Directory</a>, created by <a href="http://coenraets.org/blog/2012/02/sample-app-with-backbone-js-and-twitter-bootstrap/" title="Sample Backbone Bootstrap app" target="_blank">Christophe Coenraets</a> who has some great tutorials and information about backbone on his blog. He has some mobile versions of the app in the code base as well, but we will be working in the &#8220;web&#8221; directory.

#### Project Overview

Backbone Directory uses the Slim PHP framework on the server to communicate with backbone, but the principles we will be going over are language agnostic. In addition, Slim is based on the Sinatra (Ruby) methodology which in turn translates to Express.js framework for Node.js (JavaScript), and Tornado (Python).

#### Setting Up (Very) Basic Server Side Authentication

To get this started, we need to setup the server side login functions, and also a way to protect API requests so no data goes to anyone that isn&#8217;t authenticated. First, let&#8217;s add a login function to the api/index.php in the web directory:

<pre class="wp-code-highlight prettyprint">// file: api/index.php
session_start(); // Add this to the top of the file

/**
 * Quick and dirty login function with hard coded credentials (admin/admin)
 * This is just an example. Do not use this in a production environment
 */
function login() {
    if(!empty($_POST[&#039;email&#039;]) && !empty($_POST[&#039;password&#039;])) {
        // normally you would load credentials from a database. 
        // This is just an example and is certainly not secure
        if($_POST[&#039;email&#039;] == &#039;admin&#039; && $_POST[&#039;password&#039;] == &#039;admin&#039;) {
            $user = array("email"=&gt;"admin", "firstName"=&gt;"Clint", "lastName"=&gt;"Berry", "role"=&gt;"user");
            $_SESSION[&#039;user&#039;] = $user;
            echo json_encode($user);
        }
        else {
            echo &#039;{"error":{"text":"You shall not pass..."}}&#039;;
        }
    }
    else {
        echo &#039;{"error":{"text":"Username and Password are required."}}&#039;;
    }
}
</pre>

This is a very basic login function that is obviously not secure, but will do the job for us, since our focus is really on the backbone side of things. The key thing to note here, is that since we are using backbone, even the login function works as a JSON api request. We don&#8217;t generate any HTML, we simply send back JSON data with a user identity, or an error if something went wrong. Now we need to associate this function with a route in Slim, so add the following code under the other defined routes in index.php:

<pre class="wp-code-highlight prettyprint">// file: api/index.php
// I add the login route as a post, since we will be posting the login form info
$app-&gt;post(&#039;/login&#039;, &#039;login&#039;);

</pre>

Now we also need to make sure no data gets sent to anyone that isn&#8217;t authorized. So now we define an authorize function to check that a user has the right permissions to get the data:

<pre class="wp-code-highlight prettyprint">// File: api/index.php
/**
 * Authorise function, used as Slim Route Middlewear
 */
function authorize($role = "user") {
    return function () use ( $role ) {
        // Get the Slim framework object
        $app = Slim::getInstance();
        // First, check to see if the user is logged in at all
        if(!empty($_SESSION[&#039;user&#039;])) {
            // Next, validate the role to make sure they can access the route
            // We will assume admin role can access everything
            if($_SESSION[&#039;user&#039;][&#039;role&#039;] == $role || 
                $_SESSION[&#039;user&#039;][&#039;role&#039;] == &#039;admin&#039;) {
                //User is logged in and has the correct permissions... Nice!
                return true;
            }
            else {
                // If a user is logged in, but doesn&#039;t have permissions, return 403
                $app-&gt;halt(403, &#039;You shall not pass!&#039;);
            }
        }
        else {
            // If a user is not logged in at all, return a 401
            $app-&gt;halt(401, &#039;You shall not pass!&#039;);
        }
    };
}
</pre>

The authorize function uses some PHP closure Kung Fu, but the key is to return HTTP error codes to backbone. In our case we are going to return a 401 error (unauthorized) if a user is trying to access something they need to be logged in for, and a 403 (forbidden) if the user is logged in but doesn&#8217;t have enough privs to get the data he wants.

<div class="alert alert-info">
  <strong>More Info:</strong> Check out <a href="http://www.slimframework.com/documentation/stable#routing-middleware" title="Slim Route Middlewear" target="_blank">Slim Route Middleware</a> and <a href="http://php.net/manual/en/functions.anonymous.php" title="PHP Closures" target="_blank">PHP Closures</a>
</div>

The last thing we need to do in our server-side code is add the middleware to the routes we want to protect:

<pre class="wp-code-highlight prettyprint">// File: api/index.php
$app-&gt;get(&#039;/employees&#039;, authorize(&#039;user&#039;), &#039;getEmployees&#039;);
$app-&gt;get(&#039;/employees/:id&#039;,	authorize(&#039;user&#039;),&#039;getEmployee&#039;);
$app-&gt;get(&#039;/employees/:id/reports&#039;,	authorize(&#039;admin&#039;),&#039;getReports&#039;);
$app-&gt;get(&#039;/employees/search/:query&#039;, authorize(&#039;user&#039;),&#039;getEmployeesByName&#039;);
$app-&gt;get(&#039;/employees/modifiedsince/:timestamp&#039;, authorize(&#039;user&#039;), &#039;findByModifiedDate&#039;);
</pre>



#### Setting Up Backbone Views

Now let&#8217;s get to the good stuff: Setting up our backbone views. For authentication we will of course need a login view:

<pre class="wp-code-highlight prettyprint">// File: web/js/views/login.js
window.LoginView = Backbone.View.extend({

    initialize:function () {
        console.log(&#039;Initializing Login View&#039;);
    },

    events: {
        "click #loginButton": "login"
    },

    render:function () {
        $(this.el).html(this.template());
        return this;
    },

    login:function (event) {
        event.preventDefault(); // Don&#039;t let this button submit the form
        $(&#039;.alert-error&#039;).hide(); // Hide any errors on a new submit
        var url = &#039;../api/login&#039;;
        console.log(&#039;Loggin in... &#039;);
        var formValues = {
            email: $(&#039;#inputEmail&#039;).val(),
            password: $(&#039;#inputPassword&#039;).val()
        };

        $.ajax({
            url:url,
            type:&#039;POST&#039;,
            dataType:"json",
            data: formValues,
            success:function (data) {
                console.log(["Login request details: ", data]);
               
                if(data.error) {  // If there is an error, show the error messages
                    $(&#039;.alert-error&#039;).text(data.error.text).show();
                }
                else { // If not, send them back to the home page
                    window.location.replace(&#039;#&#039;);
                }
            }
        });
    }
});

</pre>

This view is pretty straight forward. It renders the login template, and put a click event handler on the login button. The event handler fires the login function when the button is clicked and sends an ajax request to our php login function. If an error comes back, we put it in the error div and show that div. 

Here is the login template code:

<pre class="wp-code-highlight prettyprint">&lt;!-- File: web/tpl/Login.html --&gt;
&lt;h1&gt;Login&lt;/h1&gt;
&lt;div class="alert alert-error" style="display:none;"&gt;
&lt;/div&gt;
&lt;form class="form-horizontal"&gt;
  &lt;div class="control-group"&gt;
    &lt;label class="control-label" for="inputEmail"&gt;Email&lt;/label&gt;
    &lt;div class="controls"&gt;
      &lt;input type="text" id="inputEmail" placeholder="Email"&gt;
    &lt;/div&gt;
  &lt;/div&gt;
  &lt;div class="control-group"&gt;
    &lt;label class="control-label" for="inputPassword"&gt;Password&lt;/label&gt;
    &lt;div class="controls"&gt;
      &lt;input type="password" id="inputPassword" placeholder="Password"&gt;
    &lt;/div&gt;
  &lt;/div&gt;
  &lt;div class="control-group"&gt;
    &lt;div class="controls"&gt;
      &lt;button type="submit" class="btn" id="loginButton"&gt;Sign in&lt;/button&gt;
    &lt;/div&gt;
  &lt;/div&gt;
&lt;/form&gt;
</pre>

#### Telling Backbone How to Handle 401 &#038; 403 Errors (ajaxSetup)

Now here comes the kicker. We need backbone/jquery to catch any requests that return a 401 or 403 error and handle those requests appropriately. The method I have used to do this is to call the jquery function ajaxSetup which allows us to watch for certain status codes and to handle them appropriately.

<pre class="wp-code-highlight prettyprint">// File: web/js/main.js
// Tell jQuery to watch for any 401 or 403 errors and handle them appropriately
$.ajaxSetup({
    statusCode: {
        401: function(){
            // Redirec the to the login page.
            window.location.replace(&#039;/#login&#039;);
         
        },
        403: function() {
            // 403 -- Access denied
            window.location.replace(&#039;/#denied&#039;);
        }
    }
});
</pre>

Now all 401s and 403s will be redirected to appropriate place. (I haven&#8217;t implemented the &#8220;denied&#8221; view yet, but you get the idea)

Lastly we update the backbone routing to include the login url and login view:

<pre class="wp-code-highlight prettyprint">// File: web/js/main.js
window.Router = Backbone.Router.extend({

    routes: {
        "": "home",
        "contact": "contact",
        "employees/:id": "employeeDetails",
        "login" : "login"
    },

// ...

    login: function() {
        $(&#039;#content&#039;).html(new LoginView().render().el);
    }
}
</pre>

#### The Final Word (and source code)

That is it! You should now have a password protected REST API for BackboneJS. I have posted the <a href="https://github.com/clintberry/backbone-directory-auth" title="Backbone Authentication" target="_blank">project to github (here)</a>, so feel free to check out the code and see it in action. Currently, you will need PHP/Apache with MySQL setup and the database imported. I am working on a Vagrant file for the project so you will be able to see it in action without setting up your own server.

As always, let me know if you have any questions or suggestions.

Source Code: <a href="https://github.com/clintberry/backbone-directory-auth" title="Backbone Authentication" target="_blank">https://github.com/clintberry/backbone-directory-auth</a>