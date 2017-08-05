---
title: Installing sun-java on Ubuntu system (12.04)
author: Deo Akshay
layout: post
permalink: /installing-sun-java-on-ubuntu-system-12-04/
fb_social_plugin_settings_box_recommendations_bar:
  - default
dsq_thread_id:
  - 1845474012
categories:
  - Ubuntu
tags:
  - java
  - sun-jdk
  - sun-jre
  - ubuntu
  - ubuntu-12.04
---
I work on Andorid source code which specifically needs sun-jdk-6 for building the source code. Whenever I start with fresh copy of Ubuntu, I start facing this problem of installing sun-jdk-6.

After a lot of failed attempts I found this solution (It works well with my env : Ubuntu 12.04 64bit desktop)

  1. Download jre/jdk source from [oracle website][1]
  2. Extract files to jdk/jvm_version
<pre>chmod +x jdk-&lt;build>-linux-x64.bin&lt;/br>
./jdk-&lt;build>-linux-x64.bin</pre>

  3. Move this folder to /usr/lib/jvm/
<pre>sudo mv jdk1.6.0_32/ /usr/lib/jvm/jdk1.6.0</pre>

  4. And let your system know that you have a new java alternative
<pre>sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.6.0_32//bin/javac 1

sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.6.0_32/bin/java 1</pre>

And then update your default javac and java

<pre>akshay@akshay-Inspiron-N4010 ~/Downloads &gt; sudo update-alternatives --config javac
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
Java HotSpot(TM) 64-Bit Server VM (build 20.7-b02, mixed mode)</pre>

 [1]: http://www.oracle.com/technetwork/java/javase/overview/index.html