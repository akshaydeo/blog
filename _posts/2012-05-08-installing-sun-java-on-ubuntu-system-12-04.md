---
title: Installing sun-java on Ubuntu system (12.04)
date: 2012-05-08 00:00:00 Z
permalink: "/installing-sun-java-on-ubuntu-system-12-04/"
categories:
- Ubuntu
tags:
- java
- sun-jdk
- sun-jre
- ubuntu
- ubuntu-12.04
author: Deo Akshay
layout: post
fb_social_plugin_settings_box_recommendations_bar:
- default
dsq_thread_id:
- 1845474012
---

I work on Andorid source code which specifically needs sun-jdk-6 for building the source code. Whenever I start with fresh copy of Ubuntu, I start facing this problem of installing sun-jdk-6.

After a lot of failed attempts I found this solution (It works well with my env : Ubuntu 12.04 64bit desktop)

* Download jre/jdk source from [oracle website][1]
* Extract files to jdk/jvm_version
{% highlight bash %}chmod +x jdk-&lt;build>-linux-x64.bin&lt;/br>
./jdk-&lt;build>-linux-x64.bin{% endhighlight %}
* Move this folder to /usr/lib/jvm/
{% highlight bash %}sudo mv jdk1.6.0_32/ /usr/lib/jvm/jdk1.6.0{% endhighlight %}
 * And let your system know that you have a new java alternative
{% highlight bash %}sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.6.0_32//bin/javac 1

sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.6.0_32/bin/java 1{% endhighlight %}

And then update your default javac and java

{% highlight bash %}akshay@akshay-Inspiron-N4010 ~/Downloads &gt; sudo update-alternatives --config javac
There are 2 choices for the alternative javac (providing /usr/bin/javac).

Selection Path Priority Status
------------------------------------------------------------
* 0 /usr/lib/jvm/java-6-openjdk-amd64/bin/javac 1061 auto mode
1 /usr/lib/jvm/java-6-openjdk-amd64/bin/javac 1061 manual mode
2 /usr/lib/jvm/jdk1.6.0_32//bin/javac 1 manual mode

Press enter to keep the current choice[*], or type selection number: 2
update-alternatives: using /usr/lib/jvm/jdk1.6.0_32//bin/javac to provide /usr/bin/javac (javac) in manual mode.
akshay@akshay-Inspiron-N4010 ~/Downloads &gt; sudo update-alternatives --config java
There are 2 choices for the alternative java (providing /usr/bin/java).

Selection Path Priority Status
------------------------------------------------------------
* 0 /usr/lib/jvm/java-6-openjdk-amd64/jre/bin/java 1061 auto mode
1 /usr/lib/jvm/java-6-openjdk-amd64/jre/bin/java 1061 manual mode
2 /usr/lib/jvm/jdk1.6.0_32/bin/java 1 manual mode

Press enter to keep the current choice[*], or type selection number: 2
update-alternatives: using /usr/lib/jvm/jdk1.6.0_32/bin/java to provide /usr/bin/java (java) in manual mode.
akshay@akshay-Inspiron-N4010 ~/Downloads &gt; java -version
java version "1.6.0_32"
Java(TM) SE Runtime Environment (build 1.6.0_32-b05)
Java HotSpot(TM) 64-Bit Server VM (build 20.7-b02, mixed mode){% endhighlight %}

 [1]: http://www.oracle.com/technetwork/java/javase/overview/index.html