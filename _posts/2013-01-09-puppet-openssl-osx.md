---
layout: post
title: Fixing HTTPS error in puppet 3 on Mac OS X 10.8.2 with RVM and Ruby 1.9.3
---
<h2 style="margin-bottom:0">
  <a href="{{ page.url }}">{{ page.title }}</a>
</h2>
<sub>Written on {{ page.date | date_to_long_string }}</sub><hr />

So, I wanted to try out puppet 'cause I heard it's awesome.

As I work on Mac OS X most of the time, I installed it there.
But when I tried to use it, I quickly came to the following error:
<pre><code>
└─⚡︎ puppet module search stdlib
Notice: Searching https://forge.puppetlabs.com ...
Error: Could not connect via HTTPS to https://forge.puppetlabs.com
  Unable to verify the SSL certificate
    The certificate may not be signed by a valid CA
    The CA bundle included with OpenSSL may not be valid or up to date
Error: Try 'puppet help module search' for usage
</code></pre>

Asking in [#puppet](irc://irc.freenode.org/puppet) on freenode didn't directly bring me to a solution, although user beefsalad told me that those kinds of errors pop up when openssl/libssl is out of date.
Seeing that I use Mac OS X 10.8, which ships with version 0.9.7 of openssl with no easy update possibility.

A little googling for Mac OS X, openssl and ruby brought me to [this blog post](http://www.rojotek.com/blog/2012/01/20/how-to-get-openssl-in-ruby-1-9-3-working-on-osx-10-7-fixing-the-segmentation-fault-with-ruby-openssl/) mentioning a segfault with Ruby 1.9.3 and openssl.
The author suggested the following commands:
<pre><code>
rvm pkg install openssl
rvm remove 1.9.3
rvm install 1.9.3 --with-openssl-dir=$rvm_path/usr --with-gcc=clang
</code></pre>

And to my delight after running these commands, puppet finally worked.
<pre><code>
└─⚡︎ puppet module search stdlib
Notice: Searching https://forge.puppetlabs.com ...
NAME                 DESCRIPTION                                            AUTHOR        KEYWORDS                  
offline-stdlib       Standard Library for Puppet Modules                    @offline                                
puppetlabs-activemq  This module configures Apache ActiveMQ                 @puppetlabs   java amqp stomp stdlib    
puppetlabs-java      Manage the Java runtime for use with other applica...  @puppetlabs   java stdlib jdk jre       
puppetlabs-stdlib    # Puppet Labs Standard Library #                       @puppetlabs   library stdlib stages   
</code></pre>
