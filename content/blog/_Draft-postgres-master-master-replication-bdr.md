---
title: Postgres Master-Master Replication with BDR
draft: true
author: Clint Berry
layout: post
date: 2015-07-01 
url: /2015/postgres-master-master-replication-bdr/
categories:
  - Postgres
---


PostgreSQL is an absolutely amazing relational database. And with its newer JSON data type, it can even function as a decent document store with non-structured data, while still keeping full transaction support and all the other great things you would expect from a good relational database.

But one thing that has been lacking with PostgreSQL is replication, so when I heard that Postgres was enhancing its core with <a href="http://2ndquadrant.com/en/resources/bdr/" title="BDR" target="_blank">BDR (bi-directional replication)</a>, I was super excited. This is a tutorial to get BDR setup with a <a href="http://en.wikipedia.org/wiki/Multi-master_replication" title="Multi-Mater Replication" target="_blank">master-master</a> architecture on two servers with Ubuntu 14.04.

I have provided a github repo here with a Vagrant file that will spin up two virtual machines for you. It will also install Postgres with the BDR plugin/patch and connect the two servers for replication, but here are the step by step instructions to get you going on your own boxes.

1. First, you need two Ubuntu 14.04 servers.
2. Add the PostgresBDR and BDR plugin apt repository
Create /etc/apt/sources.list.d/pgdg.list and add this line:
<pre>
# /etc/apt/sources.list.d/pgdg.list
deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main
</pre>

Run this command to add the key:

<pre language="bash">
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
</pre>

Next create /etc/apt/sources.list.d/2ndquadrant.list and add this line:
deb http://packages.2ndquadrant.com/bdr/apt/ trusty-2ndquadrant main

Then run this command to add the 2nd quadrant key:
wget --quiet -O - http://packages.2ndquadrant.com/bdr/apt/AA7A6805.asc | sudo apt-key add -

Now update your apt repos and install postgres with BDR enabled:

sudo apt-get update
sudo apt-get install postgresql-bdr-9.4
sudo apt-get install postgresql-bdr-9.4-bdr-plugin
sudo apt-get install postgresql-bdr-*