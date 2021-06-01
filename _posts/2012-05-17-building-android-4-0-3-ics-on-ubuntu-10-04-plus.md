---
title: Building Android source code on Ubuntu 10.04+
date: 2012-05-17 00:00:00 Z
permalink: '/building-android-4-0-3-ics-on-ubuntu-10-04-plus/'
tags:
  - Android
  - Android source code
author: Deo Akshay
layout: post
comments: true
dsq_thread_id:
  - 1810775465
---

Fortunately or unfortunately I faced a lot of problems while building Android 4.0.3 on Ubuntu 12.04 LTS. I searched a bit about the same, because Android officially supports two platforms for building source code : Linux and Mac. But the thing is that Google supports LTS versions and since last LTS version of Ubuntu i.e. 10.04 a lot of changes has been made which leads to build failures.

I had to solve these errors by manually changing some of the files and removing some of the constraints from various .mk files. But then I realized that most of the errors are due to gcc compatibility. So while executing make command I passed which version to be used for make like

<pre>make CC=gcc-4.4 CXX=g++-4.4
</pre>

And I didn&#8217;t get any of the errors that I used to get when I used to execute just make.

Hope this will help you Android developers :).
