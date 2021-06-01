---
title: 'Lesson learnt : Switching from Desktop Application to Web Application development'
date: 2012-08-03 00:00:00 Z
permalink: '/lesson-learnt-switching-from-desktop-application-to-web-application-development/'
tags:
  - Uncategorized
author: Deo Akshay
layout: post
comments: true
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
  - 4047879927263
fb_status_messages:
  - a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/4047879927263"
    target="_blank">your Facebook Timeline</a>";s:5:"error";s:0:"";}}
dsq_thread_id:
  - 1810775467
---

Just to give an introduction, I work on core technology of [AppSurfer][1]. Part of which is done in Django. Before starting my own venture I was a part of a MNC where I used to work on desktop applications written in C#.net. So before starting my first major web application, I was never part of any web application. And hence I struggled a lot to decide architecture of my web application. I am going to discuss one aspect of web application here, Database transactions during request processing.

**The problem**  
When we are developing a desktop application, what we mainly concentrate on is modularity of code. We take out entities, implement them and go on expanding the system depending upon responsibilities of every entity. We really dont care about (mostly as it will be running locally and now a days normal desktop is quite fast so that we can rely on optimisation made by compiler, databases and database connectors) number of db writes and pull, subroutine calls and number of indirections. Our main concern is to make system more lossely coupled, and modular.

**What goes wrong**  
In the process of making my web application more modular and lossely coupled, I ended up making a lot of modules and a lot of persisting data which means more interaction with database. Now usually the business logic of your web application takes on an average say 10% (its based on statistics of my web application) and 90% of the time is for db queries roughly. So obviously less number of db queries, less response time hence faster execution, less load on server etc etc.

**What we can do**  
One of the thing I did was, I removed unnecessary modules from my web application. And wrote most part of the apploication as a script. Secondly once an object is taken from db, perform all operations related to it and hence bring most of the operations related to it at a place and then store that back into the db. Use other db and UDP based logging system so as to make your application independent of logging and error catching. This small change brought a huge difference in db response and overall app response time.

Consider marked points, as snapshot is from production and other is from staging server.(Using new relic analytics)  
**Before my changes overall db response time**  
![](/public/images/main21-e1343984465378.png)\[2\]  
**After my changes overall db response time**  
![](/public/images/pre21-e1343984580321.png)\[3\]

Hope you will find this post useful.  
Thanks and keep coding :)

[1]: http://appsurfer.com
[2]: http://rashcoder.com/wp-content/uploads/2012/08/main21-e1343984465378.png
[3]: http://rashcoder.com/wp-content/uploads/2012/08/pre21.png
