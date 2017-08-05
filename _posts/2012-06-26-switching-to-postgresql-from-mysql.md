---
title: Switching to Postgresql from MySql
author: Deo Akshay
layout: post
permalink: /switching-to-postgresql-from-mysql/
fb_mentioned_pages:
  - 'a:0:{}'
fb_mentioned_pages_message:
  - 
fb_mentioned_friends:
  - 'a:0:{}'
fb_mentioned_friends_message:
  - 
fb_author_post_id:
  - 3887160149369
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3887160149369" target="_blank">your Facebook Timeline</a>";s:5:"error";s:0:"";}}'
dsq_thread_id:
  - 
categories:
  - Uncategorized
---
I have been using MySQL database as back end since my first Django web application. But as my applications started getting more and more complex, MySQL started giving me a lot of issues. One of the biggest issue was while performing south migrations. Usually migrations used to affect/break relationships and Django ORM used to return None objects. But a lot of times these migrations used to run correctly on my test PostgreSQL db. So finally I decided to switch onto PostgreSQL db.

But initially I faced a lot of problems while setting up PostgreSQL on my Ubuntu box. Basically its way different than the way we use MySQL. So gathered a lot of information over web and aggregating those here. This was for me, but probably useful for few of you, so making it public.

**Install PostgreSQL**

PostgreSQL server

<pre>sudo apt-get install postgresql</pre></p> 

Pgadmin tool for managing databases

<pre>sudo apt-get install pgadmin</pre></p> 

**Add password for default superuser postgres**

The default superuser, called ‘postgres’, does not have a password by default. So we need to add password for this user

<pre>sudo su postgres -c psql template1
postgres=# ALTER USER postgres with PASSWORD '&lt;your password>';
postgres=# q
</pre></p> 

**Crate a user and database under that user**

Create user

<pre>sudo -u postgres createuser -d -R -P &lt;new username>
</pre>

Create database

<pre>sudo -u postgres createdb -O &lt;usename> &lt;database name>
</pre></p> 

These are the basic things one should know who wants to switch their Django apps from MySql to PostgreSQL. Hope this post will help you. Do correct me if I made some wrong comments here in this post :).  
Happy coding m/