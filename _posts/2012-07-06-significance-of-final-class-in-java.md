---
title: Significance of final class in Java
date: 2012-07-06 00:00:00 Z
permalink: '/significance-of-final-class-in-java/'
tags:
  - Android
  - Java
author: Deo Akshay
layout: post
comments: true
fb_mentioned_pages:
  - a:0:{}
fb_mentioned_pages_message:
  -
fb_mentioned_friends:
  - a:0:{}
fb_mentioned_friends_message:
  -
fb_author_post_id:
  - 3933018375796
fb_status_messages:
  - a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3933018375796"
    target="_blank">your Facebook Timeline</a>";s:5:"error";s:0:"";}}
dsq_thread_id:
  - 2034414273
---

Hello guys, recently I have been working on Java for Android Application of <a href="http://appsurfer.com" target="_blank">AppSurfer</a>. And while coding, specially for mobile devices, you need to be concerned about speed of your application. So this post is related with improving speed of execution to some extent using final classes whenever it is applicable.

**When to use a final class :**  
When you are sure that the class is not going to be extended any further, then make that class as final class. When you make a class as final class, it does not allow any class to extend it.

**What is the advantage of that**  
When you make a class as final class, then implicitly all of the methods inside it are final. So Java compiler is sure that those methods are not going to get overridden anywhere else. Java compiler may be able to inline final method then.

<pre>final class SquareMaker{
                public int getSquare(int input) {return input*input;}
        }

        public class test {
                public static void main(String args[])
                {
                        SquareMaker sqMkr = new SquareMaker();
                        System.out.println(""+ sqMkr.getSquare(10));
                }
        }
</pre>

This would run about twice as fast when class Square maker is made final (http://www.glenmccl.com/).

Happy coding !! :)
