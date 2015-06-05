---
title: MongoDB Base Model for Zend Framework
author: Clint Berry
layout: post
date: 2010-11-11
url: /2010/mongodb-base-model-for-zend-framework/
categories:
  - MongoDB
  - Zend Framework
---
I came accross [MongoDB][1] a few months ago and it seemed like a perfect fit for many of the projects I am working. Extremely fast inserts, map-reduce for complex queries, and most importantly, scaling is a breeze.

Since I am a Zend Framework guy I created a simple base model class for MongoDB. It is a very simple wrapper, but is effective for what I need. I usually create model classes for each &#8220;Collection&#8221; just like I would create models for each table in MySQL. Each model class extends from the new MongoDB base class and allows a low level &#8220;active directory&#8221; type access to MongoDB documents.
  
<!--more-->


  
**[Get The Source Code Here][2]**

Example: We want to create a new document for every visitor that comes into a website that we are tracking. We store those documents in the &#8220;visitor&#8221; collection.

The first thing we do is create a model for the visitor collection.

<pre class="wp-code-highlight prettyprint">class Model_Visitor extends Mongodb_ModelBase {

    // If you don&#039;t specify the collection name explicitly,
    // it will default to the name of the class minus the "Model_" part.
    protected static $_collectionName = "visitor";

}</pre>

Then we can create new documents for that collection

<pre class="wp-code-highlight prettyprint">$newVisitor = new Model_Visitor();
$newVisitor-&gt;ipAddress = &#039;5.5.5.5&#039;;
$newVisitor-&gt;save();</pre>

You can also query for visitors using static methods:

<pre class="wp-code-highlight prettyprint">$oneVisitor = Model_Visitor::findOne();
$allVisitors = Model_Visitor::find();
$someVisitors = Model_Visitor::find(array(&#039;ipAddress&#039;=&gt;&#039;5.5.5.5&#039;));</pre>

Use dot notation for nested values. These two commands do the same thing:

<pre class="wp-code-highlight prettyprint">$myVisitor-&gt;{&#039;referrer.url&#039;} = &#039;google.com&#039;;
$myVisitor-&gt;referrer = array(&#039;url&#039;=&gt;&#039;google.com&#039;);</pre>

I thought I would share this with the rest of the world in case someone needed it.

**[Get The Source Code Here][2]**

**NOTE: The base class makes use of [late static binding][3], which requires PHP 5.3**

 [1]: http://mongodb.org
 [2]: https://github.com/clintberry/zf-mongo-base
 [3]: http://php.net/manual/en/language.oop5.late-static-bindings.php