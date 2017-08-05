---
title: Be careful while using Executors.newCachedThreadPool()
author: Deo Akshay
layout: post
permalink: /be-careful-while-using-executors-newcachedthreadpool/
post_to_facebook_timeline:
  - 0
dsq_thread_id:
  - 1831980417
categories:
  - Executors
  - Java
comments: true
---
I am currently writing a scalable backend server for <a href="http://appsurfer.com" title="AppSurfer" target="_blank">AppSurfer</a>, which does routing work for different components associated with a session. Recently I went into a trouble with the new version of the server. Even for a single connection CPU load was almost 100%. I was clueless about the issue and started digging into it. 

**My setup**

* netty4x using our home cooked wrapper <a href="http://rainingclouds.github.com/transporter" title="Transporter" target="_blank">Transporter</a>.  
* JDK with latest update number 40.  
* CentOS. 

**CaseStudy**

We at [AppSurfer][1] solve a very tricky problem on our technology side. Our servers need to respond almost real time with minimilstic load on the server as it might affect other sessions. For better scaling we shifted onto [netty][2] (i.e nio) from standard io. It really helped us to improve overall response time as well as efficient usage of resources. But during shift we faced one critical issue related to 100% CPU usage. Some initial googling introduced me to this [JVM issue][3]. But this issue was tackled very well in netty3x as well as netty 4x. Then I started digging into this issue more.

CPU usage shown by HTOP command \[4\] :  
![](/public/images/Screenshot-from-2013-09-28-210624-1024x110.png)
CPU usage shown by TOP command \[5\] : 
![](/public/images/Screenshot-from-2013-09-28-211248.png)

I was clueless about what was going wrong after adding all the fixed related to nio. Then yourkit output indicated one strange thing related to garbage collection. There was a lot of garbage collection happening during session runs. Then digging into it more revealed that the main issue was related with Executors.newCachedThreadPool(). 

Garbage collection output \[6\]: 
![](/public/images/Screenshot-from-2013-10-06-233615-1024x231.png)

What I read about newCachedThreadPool in **Java documentation** was:

> &#8220;Creates a thread pool that creates new threads as needed, but will reuse previously constructed threads when they are available. These pools will typically improve the performance of programs that execute many short-lived asynchronous tasks. Calls to execute will reuse previously constructed threads if available. If no existing thread is available, a new thread will be created and added to the pool. Threads that have not been used for sixty seconds are terminated and removed from the cache. Thus, a pool that remains idle for long enough will not consume any resources. Note that pools with similar properties but different details (for example, timeout parameters) may be created using ThreadPoolExecutor constructors.&#8221; 

But I missed one of the important crux of this line. **If no existing thread is available, a new thread will be created and added to the pool**. This means if my tasks are getting created at a very faster rate for lets say 2-3 mins (and in our case its throughout the session run), you will end up with thousands of threads created and destroyed (to be precise during our single session for 50 sec, number of threads shown by yourkit was around 10500). I always knew that, in our case there are going to be a lot of short duration tasks will be submitted to the workers but in the java documentation they have mentioned that **These pools will typically improve the performance of programs that execute many short-lived asynchronous tasks**. But what they mean by that is, when you know the number of tasks are under control and they not virtually concurrently submitted. 

**Solution**

We wrote our custom thread pool executor to control the number of threads getting spawned at the runtime, and now cpu usage is like 1% or 2% throughtout the lifetime. 

{% highlight java %}
public static ThreadPoolExecutor get(final int coreSize, final int maxSize, final int idleTimeout, final TimeUnit timeUnit, final int queueSize, final String namePrefix) {
    return new EfficientThreadPoolExecutor(coreSize, // core size
            maxSize, // max size
            idleTimeout, // idle timeout
            timeUnit,
            new ArrayBlockingQueue(queueSize), // queue with a size
            new PriorityThreadFactory(namePrefix, Thread.NORM_PRIORITY));
}
{% endhighlight %}    

Thanks to [Norman Maurer][7], most active contributor of netty, for helping me out in solving this issue.

 [1]: http://appsurfer.com
 [2]: http://netty.org
 [3]: http://bugs.sun.com/view_bug.do?bug_id=6403933
 [4]: /images/Screenshot-from-2013-09-28-210624.png
 [5]: /images/Screenshot-from-2013-09-28-211248.png
 [6]: /images/Screenshot-from-2013-10-06-233615.png
 [7]: https://github.com/normanmaurer
