---
title: Custom User Providers in Symfony2
author: Clint Berry
layout: post
date: 2011-12-21
url: /2011/custom-user-providers-symfony2/
categories:
  - PHP
  - Symfony
---
#### Why Custom User Providers?

At my current job all the database systems are managed by DB admins and the developers have to connect to the database via web services. We do not connect to the databases directly. This allows for better <a title="Separation of Concerns" href="http://en.wikipedia.org/wiki/Separation_of_concerns" target="_blank">separation of concerns</a> and allows experts to focus on their respective specialties. But if you are programming a Symfony2 app, that means you don&#8217;t get to use Doctrine or any other ORM, which in turn means you create your own models/entities.

When you want to develop the authentication parts of your new app you will quickly find that there is plenty of documentation for Doctrine/ORM based apps, but if you are using your own custom models then you run into pages <del datetime="2012-01-20T21:30:13+00:00"><a href="http://symfony.com/doc/current/cookbook/security/custom_provider.html" target="_blank">like this one</a></del>.  <del datetime="2012-01-20T21:30:13+00:00">(when I finish this post I will submit an article to the docs and see if they approve it)</del> **UPDATE: Someone beat me to it, there is now a good tutorial in the Symfony docs for custom user providers.**

<!--more-->


  
So, after a few hours of googling-reading-tinkering I figured out how to use the Symfony authentication system  with my own custom models. Keep in mind I am still new to Symfony2, so many of these concepts will be beginner level.

#### Your User Entity

For this example, I am assuming you are using custom entities for your project. I have created a custom User entity to manage users in my application. It extends a base class that handles most of the getting, setting and the calls to my database REST service, but that is optional depending on how you setup your own entities.

<pre class="wp-code-highlight prettyprint">namespace CB\WebsiteBundle\Entity;

use Clint\Model\Base;
use Symfony\Component\Security\Core\User\UserInterface;

class User extends Base
implements UserInterface
{

    public static $modelName = &#039;User&#039;;
    public static $modelUrl = &#039;/user&#039;;

    /**
     * Returns the roles granted to the user.
     *
     * @return Role[] The user roles
     */
    public function getRoles(){
        return array(&#039;ROLE_USER&#039;);
    }

    /**
     * Returns the password used to authenticate the user.
     *
     * @return string The password
     */
    public function getPassword(){
        return $this-&gt;password;
    }

    /**
     * Returns the salt.
     *
     * @return string The salt
     */
    public function getSalt(){
        return null;
    }

    /**
     * Returns the username used to authenticate the user.
     *
     * @return string The username
     */
    public function getUsername(){
        return $this-&gt;username;
    }

    /**
     * Removes sensitive data from the user.
     *
     * @return void
     */
    public function eraseCredentials(){
        $this-&gt;password = null;
    }

    /**
     * The equality comparison should neither be done by referential equality
     * nor by comparing identities (i.e. getId() === getId()).
     *
     * However, you do not need to compare every attribute, but only those that
     * are relevant for assessing whether re-authentication is required.
     *
     * @param UserInterface $user
     * @return Boolean
     */
    public function equals(UserInterface $user){
        return ($this-&gt;getUsername() === $user-&gt;getUsername());
    }
}</pre>

&nbsp;

The key to making your User entity compatible with Symfony2 authentication, is to implement the User Interface as you can see in above class. These are the functions required by that interface:

  * getRoles() &#8211; for now I am simply returning a hard-coded role, but you could implement to get from the user object
  * getPassword() &#8211; Retrieve the password from the user object
  * getSalt() returns the <a title="Salt" href="http://en.wikipedia.org/wiki/Salt_(cryptography)" target="_blank">salt for your password encryption</a>
  * getUsername()
  * eraseCredentials() which is used to erase sensitive data from the session object
  * equals(UserInterface $user) which is used to make sure the right user is authenticated

You will need to define all of these functions to correctly load your users from your web-service (or however you are doing it). Once you have all the required function defined, you are ready to move on to the User Provider Service.

#### The User Provider

To use your own custom entities in Symfony2 authentication, you will need to have a basic understanding of <a href="http://symfony.com/doc/current/book/service_container.html" title="Symfony Services" target="_blank">Symfony services</a> and the <a href="http://symfony.com/doc/current/book/security.html" title="Symfony Authentication" target="_blank">Symfony authentication</a> system. Read those links if you haven&#8217;t yet. To reiterate what the user provider is, from the docs:

<blockquote cite="http://symfony.com/doc/current/book/security.html#where-do-users-come-from-user-providers">
  <p>
    In Symfony2, users can come from anywhere &#8211; a configuration file, a database table, a web service, or anything else you can dream up. Anything that provides one or more users to the authentication system is known as a &#8220;user provider&#8221;. Symfony2 comes standard with the two most common user providers: one that loads users from a configuration file and one that loads users from a database table.
  </p>
</blockquote>

Again, since we are NOT using doctrine, we will create our own User Provider as a service in Symfony. Based on the documentation, it seems that the best place to put this is in YourBundle/Security folder.

<pre class="wp-code-highlight prettyprint">namespace CB\WebsiteBundle\Security;

use Symfony\Component\Security\Core\User\UserInterface;
use Symfony\Component\Security\Core\User\UserProviderInterface;
use Symfony\Component\Security\Core\Exception\UsernameNotFoundException;

use CB\WebsiteBundle\Entity\User;

class Provider implements UserProviderInterface {

    protected $user;
    public function __contsruct (UserInterface $user) {
        $this-&gt;user = $user;
    }

    /**
     * Loads the user for the given username.
     *
     * This method must throw UsernameNotFoundException if the user is not
     * found.
     *
     * @throws UsernameNotFoundException if the user is not found
     * @param string $username The username
     *
     * @return UserInterface
     */
    function loadUserByUsername($username) {
        $user = User::find(array(&#039;username&#039;=&gt;$username));
        if(empty($user)){
            throw new UsernameNotFoundException(&#039;Could not find user. Sorry!&#039;);
        }
        $this-&gt;user = $user;
        return $user;
    }

    /**
     * Refreshes the user for the account interface.
     *
     * It is up to the implementation if it decides to reload the user data
     * from the database, or if it simply merges the passed User into the
     * identity map of an entity manager.
     *
     * @throws UnsupportedUserException if the account is not supported
     * @param UserInterface $user
     *
     * @return UserInterface
     */
    function refreshUser(UserInterface $user) {
        return $user;
    }

    /**
     * Whether this provider supports the given user class
     *
     * @param string $class
     *
     * @return Boolean
     */
    function supportsClass($class) {
        return $class === &#039;CB\WebsiteBundle\Entity\User&#039;;
    }
}</pre>

&nbsp;

The important thing to note about this class is that it implements the Symfony User Provider Interface. Note the three functions I implemented from the interface definition:

  * loadUserByUsername() &#8211; Make sure to implement this with however your custom user entity loads users by username
  * refreshUser() &#8211; I don&#8217;t completely understand the purpose of this function yet. I will update when I do.
  * supportsClass() &#8211; A check to see if a certain type of user class is supported, in our case we use our custom user class definition

I also added a constructor that takes a UserInterface object and stores it in a property when initialized. This will be done as a symfony service.

#### Configuration

The final step is to create the configuration for your newly build User Provider and User Entity. First, we must add our new entity and provider as a symfony service in our bundle&#8217;s service configuration in YourBundle/Resources/config/services.xml :

<pre class="wp-code-highlight prettyprint">&lt;container xmlns="http://symfony.com/schema/dic/services"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd"&gt;

    
    &lt;parameters&gt;
        &lt;parameter key="cb_security_user.class"&gt;CB\WebsiteBundle\Entity\User&lt;/parameter&gt;
        &lt;parameter key="cb_security_provider.class"&gt;CB\WebsiteBundle\Security\Provider&lt;/parameter&gt;
    &lt;/parameters&gt;

    &lt;services&gt;
        &lt;service id="cb_security_user" class="%cb_security_user.class%" /&gt;
        &lt;service id="cb_security_provider" class="%cb_security_provider.class%"&gt;
            &lt;argument type="service" id="cb_security_user" /&gt;
        &lt;/service&gt;
    &lt;/services&gt;
    
&lt;/container&gt;

</pre>

&nbsp;

I define two parameters with the name of my custom user class and my custom provider class. I then add two services, one for the user entity, and the other for the user provider class. One thing to note is that I actually pass the user entity service as an argument to the provider service when initialized.

And lastly, you need to update your security configuration for your application. Here is my configuration with form-based validation:

<pre class="wp-code-highlight prettyprint">security:
    encoders:
        CB\WebsiteBundle\Entity\User:
            algorithm: sha1
            iterations: 1
            encode_as_base64: false

    role_hierarchy:
        ROLE_ADMIN:       ROLE_USER
        ROLE_SUPER_ADMIN: [ROLE_USER, ROLE_ADMIN, ROLE_ALLOWED_TO_SWITCH]

    providers:
        main:
            id: cb_security_provider

    firewalls:
        dev:
            pattern:  ^/(_(profiler|wdt)|css|images|js)/
            security: false

        login:
            pattern:  ^/login$
            security: false

        secured_area:
            pattern:    ^/secure/
            form_login: ~
            logout: ~
            #anonymous: ~
            #http_basic:
            #    realm: "Secured Demo Area"

    access_control:
        - { path: ^/secure, roles: ROLE_USER }
        #- { path: ^/secure, roles: IS_AUTHENTICATED_ANONYMOUSLY, requires_channel: https }
        - { path: ^/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
        #- { path: ^/login, roles: IS_AUTHENTICATED_ANONYMOUSLY, requires_channel: https }
        #- { path: ^/_internal, roles: IS_AUTHENTICATED_ANONYMOUSLY, ip: 127.0.0.1 }</pre>

&nbsp;

Key things to note are:

  * The encoders section is configured to use your custom user entity
  * The providers section is configured to use your user provider service (we used the ID from the services xml)

Now you just need to make sure your login routes and forms are all setup and you have a newly created User Provider using custom entities!

Let me know if you have any questions.