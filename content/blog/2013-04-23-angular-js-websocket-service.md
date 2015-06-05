---
title: AngularJS WebSocket Service Example
author: Clint Berry
layout: post
date: 2013-04-23
url: /2013/angular-js-websocket-service/
vp_ptemplate_settings:
  - 'a:2:{s:10:"categories";s:0:"";s:10:"blog_posts";i:6;}'
categories:
  - Angular
---
<img src="http://clintberry.com/images/angular_logofull.png" alt="angular_logofull" title="angular_logofull" width="382" height="99" class="alignnone size-full wp-image-787" />

At my curent company we are using Angular.js for a new desktop application (yes, a desktop application, but I won&#8217;t get into that). Our app gets its data and events from a web service via a WebSocket connection. Angular comes bundled with some great tools to connect to REST servers, but it doesn&#8217;t come with anything to help you with real-time data (and it probably shouldn&#8217;t).

Here is an example of an Angular service (factory) that uses WebSockets to get data:
  
<!--more-->

<pre class="wp-code-highlight prettyprint">angular.module(&#039;MyApp&#039;).factory(&#039;MyService&#039;, [&#039;$q&#039;, &#039;$rootScope&#039;, function($q, $rootScope) {
    // We return this object to anything injecting our service
    var Service = {};
    // Keep all pending requests here until they get responses
    var callbacks = {};
    // Create a unique callback ID to map requests to responses
    var currentCallbackId = 0;
    // Create our websocket object with the address to the websocket
    var ws = new WebSocket("ws://localhost:8000/socket/");
    
    ws.onopen = function(){  
        console.log("Socket has been opened!");  
    };
    
    ws.onmessage = function(message) {
        listener(JSON.parse(message.data));
    };

    function sendRequest(request) {
      var defer = $q.defer();
      var callbackId = getCallbackId();
      callbacks[callbackId] = {
        time: new Date(),
        cb:defer
      };
      request.callback_id = callbackId;
      console.log(&#039;Sending request&#039;, request);
      ws.send(JSON.stringify(request));
      return defer.promise;
    }

    function listener(data) {
      var messageObj = data;
      console.log("Received data from websocket: ", messageObj);
      // If an object exists with callback_id in our callbacks object, resolve it
      if(callbacks.hasOwnProperty(messageObj.callback_id)) {
        console.log(callbacks[messageObj.callback_id]);
        $rootScope.$apply(callbacks[messageObj.callback_id].cb.resolve(messageObj.data));
        delete callbacks[messageObj.callbackID];
      }
    }
    // This creates a new callback ID for a request
    function getCallbackId() {
      currentCallbackId += 1;
      if(currentCallbackId &gt; 10000) {
        currentCallbackId = 0;
      }
      return currentCallbackId;
    }

    // Define a "getter" for getting customer data
    Service.getCustomers = function() {
      var request = {
        type: "get_customers"
      }
      // Storing in a variable for clarity on what sendRequest returns
      var promise = sendRequest(request); 
      return promise;
    }

    return Service;
}])
</pre>

##### &nbsp;

#### The Details

To explain this code in detail I will walk you through a usage scenario and step through each function and talk about what it does. Assume we have an angular controller called &#8220;customerList&#8221;. We need to access customer data in our new controller and our customer data comes from a websocket service somewhere in Canada. So you inject your new websocket service into the scope of your controller and you are able to call getCustomers(). Quick and dirty example for illustration purposes:

<pre class="wp-code-highlight prettyprint">angular.module(&#039;MyApp&#039;)
  .controller(&#039;customerList&#039;, [&#039;MyService&#039;, function(MyService){
    $scope.customers = MyService.getCustomers();
  }]);
</pre>

##### &nbsp;

So the getCustomers function is called and we see that the getCustomers function creates a request object literal and passes that to the sendRequest() function:

<pre class="wp-code-highlight prettyprint">// Define a "getter" for getting customer data
    Service.getCustomers = function() {
      var request = {
        type: "get_customers"
      }
      // Storing in a variable for clarity on what sendRequest returns
      var promise = sendRequest(request); 
      return promise;
    }
</pre>

##### &nbsp;

You can see I am storing the response from sendRequest() in a variable called promise. I then return that promise. Let&#8217;s look at what sendRequest() actually does:

<pre class="wp-code-highlight prettyprint">function sendRequest(request) {
      var defer = $q.defer();
      var callbackId = getCallbackId();
      callbacks[callbackId] = {
        time: new Date(),
        cb:defer
      };
      request.callback_id = callbackId;
      console.log(&#039;Sending request&#039;, request);
      ws.send(JSON.stringify(request));
      return defer.promise;
    }
</pre>

##### &nbsp;

The sendRequest function first creates a defer object from the <a href="https://github.com/kriskowal/q" title="Q Library" target="_blank">Q library</a> that is bundled with Angular. (For more information on deferred objects and promises in angular I highly recommend the <a href="http://www.egghead.io/video/o84ryzNp36Q" title="Promises in Angularjs" target="_blank">egghead.io video on promises</a>) After that it creates a new callbackId variable and then adds an object literal to the callbacks object using the new callbackId as the index. 

#### So why have a callback ID and a callbacks object?

The callbacks variable is where I will store all requests that haven&#8217;t received a response yet. Because services implemented on the websocket side can be asynchronous, you could potentially send several requests to the websocket and the websocket could return responses in a different order than it received requests. This is where callback Ids come into play. Usually websocket servers will have a way for you to map responses from the websocket server to requests that you sent to it. Sending a user-generated callback_id to the websocket is one way to do this. In my case, I start at 0 and work my way up to 10000 then start over. You can see this in my getCallback() function:

<pre class="wp-code-highlight prettyprint">// This creates a new callback ID for a request
    function getCallbackId() {
      currentCallbackId += 1;
      if(currentCallbackId &gt; 10000) {
        currentCallbackId = 0;
      }
      return currentCallbackId;
    }
</pre>

##### &nbsp;

Now back to sendRequest. After the callbackId is generated, and the deferred is stored in the callbacks variable, we add the new callbackId to the request message:

<pre class="wp-code-highlight prettyprint">request.callback_id = callbackId;
</pre>

##### &nbsp;

Then we send the request object to the websocket and return a promise:

<pre class="wp-code-highlight prettyprint">ws.send(JSON.stringify(request));
    return defer.promise;
</pre>

##### &nbsp;

Now out in Canada somewhere, our websocket server processes the request and sends back a list of customers to us through the websocket. When data comes in from the websocket we call the listener function:

<pre class="wp-code-highlight prettyprint">ws.onmessage = function(message) {
        listener(message);
    };
</pre>

##### &nbsp;

The listener looks at the message coming in and sees that it looks something like this:

<pre class="wp-code-highlight prettyprint">{
  "result": true,
  "callback_id": 1,
  "data": [
    {
      first_name: Danny,
      last_name: Ocean
    },
    {
      first_name: Rusty,
      last_name: Ryan
    }
  ]
}
</pre>

##### &nbsp;

The listener() function sees the callback_id property and looks in our callbacks variable to see if we have a pending request waiting to be resolved. If there is one, it resolves the deferred object and deletes the callback object from the callbacks object-literal/dictionary/named-array:

<pre class="wp-code-highlight prettyprint">if(callbacks.hasOwnProperty(messageObj.callback_id)) {
      console.log(callbacks[messageObj.callback_id]);
      $rootScope.$apply(callbacks[messageObj.callback_id].cb.resolve(messageObj.data));
      delete callbacks[messageObj.callbackID];
    }
</pre>

And then, lo and behold, our scope variable, $scope.customers, is populated with our new customer list! And now you have a functioning websocket service. <img src="http://clintberry.com/wp-includes/images/smilies/simple-smile.png" alt=":-)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

I know this all can seem like a lot if you are new to angular or haven&#8217;t heard of promises before. Feel free to ask any questions in the comments or email me on my contact form if you need help. I am usually pretty good about getting back to you.