---
title: Custom Error Messages on Zend Form Validators
author: Clint Berry
layout: post
date: 2010-07-08
url: /2010/zend-form-email-validator-customizing-error-messages/
categories:
  - Zend Framework
---
Zend Form is extremely powerful, and I love most of the built in validators. But some of the validators are overkill for many projects. Take the EmailAddress validator for instance. I have never worked on a web-form where I wanted 3 error messages to appear if the Email address entered was invalid. (To see what I mean, just type in &#8220;a@a&#8221; for your email address and see what Zend\_Validate\_EmailAddress displays). I have seen several questions and complaints about this problem ([Example 1][1] or [Example 2][2]) and thought I would offer up my fix.
  
<!--more-->


  
I have found that the quickest way to to control your error messages with the EmailAddress Validator is to create your own email validator that extends from Zend\_Validate\_EmailAddress, and then override the isValid function. Here is the shortest version I have come up with:

<pre class="wp-code-highlight prettyprint">class Clint_Validate_EmailAddress extends Zend_Validate_EmailAddress
{
    public function isValid($value)
    {
       $response = parent::isValid($value);
       if(!$response){
           $this-&gt;_messages =
                array(self::INVALID =&gt; "Please enter a valid email address");
       }
       return $response;
    }
}</pre>

This class simply calls the parent function isValid(), and if it returns false, it sets the _messages array to have only one error message of your choice. Call it a hack if you want, but it works, and it works without putting logic in the controller which means I can re-use this form wherever I want. As always, let me know if you have a different and/or better way of doing this.

 [1]: http://framework.zend.com/issues/browse/ZF-2224
 [2]: http://www.zfforums.com/zend-framework-general-discussions-1/general-q-zend-framework-2/how-can-i-get-just-1-error-message-emailaddress-validator-instead-multiple-2582.html