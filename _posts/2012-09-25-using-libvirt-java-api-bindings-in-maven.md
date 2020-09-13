---
title: Using libvirt Java API bindings in maven
date: 2012-09-25 00:00:00 Z
permalink: "/using-libvirt-java-api-bindings-in-maven/"
categories:
- Uncategorized
author: Deo Akshay
layout: post
fb_social_plugin_settings_box_recommendations_bar:
- default
fb_mentioned_pages:
- a:0:{}
fb_mentioned_pages_message:
- 
fb_mentioned_friends:
- a:0:{}
fb_mentioned_friends_message:
- 
fb_author_post_id:
- 4287319553104
fb_status_messages:
- a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/4287319553104"
  target="_blank">your Facebook Timeline</a>";s:5:"error";s:0:"";}}
dsq_thread_id:
- 2075428690
---

Hello coders, I am writing this quick post just to help a small bunch of people who are trying to use libvirt JAVA API bindings. I was trying to build .jar from git repo mentioned on the [Libvirt site][1], but 

<pre>ant build</pre>

command was giving a bunch of errors, to be precise, 100 errors. Then I decided to go with maven but there was no documentation. There is no rocket science in understanding maven architecture but for the coders trying to find ready template here it is :

<pre>&lt;repositories>
        &lt;repository>
            &lt;id>libvirt repo&lt;/id>
            &lt;url>http://www.libvirt.org/maven2/&lt;/url>
        &lt;/repository>
    &lt;/repositories>

    &lt;dependencies>
        &lt;dependency>
            &lt;groupId>org.libvirt&lt;/groupId>
            &lt;artifactId>libvirt&lt;/artifactId>
            &lt;version>0.4.9&lt;/version>
        &lt;/dependency>

    &lt;/dependencies>
</pre>

Add this in your pom and you are ready to go.  
Thanks :)

 [1]: http://libvirt.org/java.html