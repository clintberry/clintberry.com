---
title: Wildcard Sub-Domains on OSX, Web Development on Localhost
author: Clint Berry
layout: post
date: 2011-05-30
url: /2011/wildcard-sub-domains-on-osx-web-development-on-localhost/
quote-author:
  - Unknown
quote-url:
  - http://
quote-copy:
  - Unknown
audio:
  - http://
link-url:
  - http://
seo_follow:
  - 'false'
seo_noindex:
  - 'false'
categories:
  - Development Environment
---
Today&#8217;s post will be on setting up wildcard subdomains in OSX. This allows you to map folders in your web root to sub-domains on your local box. Why would you want to do this? It is nice to be able to type use _http://myproject.clint.dev_ for the URL and have it map to the &#8216;myproject&#8217; folder on your local box. No need to add a line to the etc/hosts file, and no need to add a virtual host in apache for each sub-domain. The method I will be outlining here will allow you to map x.yourdevdomain.dev to any folder &#8216;x&#8217; using Bind and a wildcard virtual host in apache. This tutorial assumes you already have Apache setup on your local box.
  
<!--more-->

### **Bind DNS Setup (OSX 10.6+)**

Bind comes pre-installed on OSx 10.6. All we have to do is configure it and run it. I took these Bind setup instructions from <a href="http://jessedearing.com/nodes/9-setting-up-wildcard-subdomains-on-os-x-10-6" target="_blank">Jesse Dearing&#8217;s blog</a>. I hope he doesn&#8217;t mind.

<a href="http://www.isc.org/software/bind" target="_blank">BIND</a> is the little piece of software the runs the internet. BIND is a DNS server and it works in a distributed fashion. It’s really fascinating <a href="http://www.howstuffworks.com/dns1.htm" target="_blank">how DNS works</a> but that’s outside the scope of this post. There are a few steps to get BIND set up to serve domains and they are crucial.

  1. `sudo -s`
  
    This will let you run a shell as root. I suggest doing this because most of the next commands you will execute need to be on privileged files and directories.
  2. Create the `rndc.conf` and `rndc.key`
  
    You can easily do this by running the following commands:
  
    `rndc-confgen > /etc/rndc.conf`
  
    `head -n5 /etc/rndc.conf |tail -n4 > /etc/rndc.key`
  3. Add the zone to `/etc/named.conf` I am going to use .dev for all my examples. I have used .local on my machine but I have found that this can be troublesome with the OS X default Multicast being .local. You can make this whatever top level domain you would like (.dev, .rails, .php, etc.) In `/etc/named.conf `add: <pre class="wp-code-highlight prettyprint">&lt;code&gt; zone "dev" IN { type master; file "dev.zone"; allow-update { none; }; }; &lt;/code&gt;</pre>

  4. Create `/var/named/dev.zone`
  
    Use this template for your zone file:</p> <pre class="wp-code-highlight prettyprint">$TTL 60 $ORIGIN dev. @ 1D IN SOA localhost. root.localhost. ( 
    42 ; 
    serial (d. adams) 3H ; 
    refresh 15M ; 
    retry 1W ; 
    expiry 1D ) ; 
minimum 1D IN NS localhost 1D IN A 127.0.0.1 localhost.dev. 60 IN A 127.0.0.1 *.dev. 60 IN A 127.0.0.1</pre>
    
    The only thing you should have to change is the serial (42) to any random number and if you decided to go with something else other than .dev: everthing that has .dev in it. Note that the dots (.) after the dev entries are intentional and required.</li> 
    
      * `launchctl load -w /System/Library/LaunchDaemons/org.isc.named.plist`
  
        This will cause named to load everytime the system restarts.
      * `named`
  
        This loads named now. It doesn’t give you any input if it fails to load so I recommend running `named -g` first to make sure it parses you zones and configs. `-g` will tell named to run in the forground and write all logging to STDOUT. Once you’ve verified that it starts then Ctrl-c and run `named` as normal.</ol> 
    
    #### Create the resolver {#create-the-resolver}
    
    Next we have to create an entry in the /etc/resolver folder. This folder is checked every time a DNS entry is looked up. If a file exists here then it will use the special resolv configuration to look up DNS names. Here you can specify custom domains, search orders, and in this case nameservers. The resolver folder may not exist in the /etc directory. If it doesn&#8217;t, create it.
    
    Create a file with the root domain you want to use for your projects.. I.e. if your root domain is `clint.dev` then create the file `clint.dev` and inside it put:
    
    <pre class="wp-code-highlight prettyprint">&lt;code&gt;nameserver 127.0.0.1 &lt;/code&gt;</pre>
    
    Now when you go to http://clint.dev your root web directory should load, but you can also go to http://www.clint.dev or http://theskyisblue.clint.dev and it should still go to your root web directory.
    
    Log out of the root shell by typing exit.
    
    Your wildcard DNS settings are now ready to go.
    
    ## Apache Setup
    
    Now we need to get apache to route to the correct folder based on the sub-domain used. Using the example from before, we want http://myproject.clint.dev to go to the folder &#8216;myproject&#8217; in my web root. To accomblish this we setup a vhost in the apache configuration file (httpd.conf). Depending on your Apache setup, the file can be in different locations. Because I use ZendServer community edition, I add a separate file in the `/usr/local/zend/apache2/conf.d` directory. I call my file dev.conf and add the following lines:
    
    <pre class="wp-code-highlight prettyprint">NameVirtualHost *:80 
 ServerAdmin webmaster@local.web 
 DocumentRoot "/Users/clint/Sites" 
 ServerName clint.dev ServerAlias *.clint.dev 
 RewriteEngine on 
 RewriteCond %{HTTP_HOST} !^www.* [NC] 
 RewriteCond %{HTTP_HOST} ^([^\.]+)\.clint\.dev 
 RewriteCond /Users/clint/Sites/%1/public -d 
 RewriteRule ^(.*) /%1/public/$1 [L] 
 RewriteCond %{HTTP_HOST} !^www.* [NC] 
 RewriteCond %{HTTP_HOST} ^([^\.]+)\.clint\.dev 
 RewriteCond /Users/clint/Sites/%1 -d 
 RewriteRule ^(.*) /%1/$1 [L]</pre>
    
    This virtual host checks to make sure the folder exists for that project, and checks to see if a public directory exists (Zend Framework apps). If the public directory exists, it uses that directory as the document root for that subdomain. If public doesn&#8217;t exist, it just uses the project folder as the document root. If niether exists, then it will just use the Sites directory as the document root (the default document root).
    
    **Note: Make sure to change any &#8216;/Users/clint/Sites&#8217; to whatever directory your document root is to be. Also change the .clint.dev to your custom development domain.**
    
    After that, all your subdomains will map to project folders in your document root. Let me know if you have any questions.