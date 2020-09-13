---
title: Managing multiple versions of python on a Linux box
date: 2012-08-18 00:00:00 Z
permalink: "/managing-multiple-versions-of-python-on-a-linux-box/"
categories:
- Tools
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
- 4113032476036
fb_status_messages:
- a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/4113032476036"
  target="_blank">your Facebook Timeline</a>";s:5:"error";s:0:"";}}
dsq_thread_id:
- 2279648384
---

Hello coders, if you are working on Python or any of your development tool is based on Python then you must have faced a problem in using multiple versions of Python. For an instance I develop web applications using Django which has kind of beta support for Python3 so I have to be on Python27 and same for repo from Google which does not run on Python3. But Python3 is required in some cases for example I am running arch Linux which demands for Python3 for some of its components. I came across an amazing tool named pythonbrew.

Pythonbrew provides very simple commands to install, uninstall and switch between multiple versions of python.</br>  
**To install a python version**  
pythonbrew install 3.2  
**To remove a python version**  
pythonbrew uninstall 3.2  
**To switch to a particular version of Python**  
pythonbrew switch 2.7

**In action:**  
Installing a version

<pre>[akshay@amd_dev ~/Downloads/pythonbrew]$ pythonbrew install 3.2
Downloading Python-3.2.tgz as /home/akshay/.pythonbrew/dists/Python-3.2.tgz
######################################################################## 100.0%
Extracting Python-3.2.tgz into /home/akshay/.pythonbrew/build/Python-3.2

This could take a while. You can run the following command on another shell to track the status:
  tail -f "/home/akshay/.pythonbrew/log/build.log"

Patching Python-3.2
Installing Python-3.2 into /home/akshay/.pythonbrew/pythons/Python-3.2
Downloading distribute_setup.py as /home/akshay/.pythonbrew/dists/distribute_setup.py
######################################################################## 100.0%
Installing distribute into /home/akshay/.pythonbrew/pythons/Python-3.2
Installing pip into /home/akshay/.pythonbrew/pythons/Python-3.2

Installed Python-3.2 successfully. Run the following command to switch to Python-3.2.
  pythonbrew switch 3.2
[akshay@amd_dev ~/Downloads/pythonbrew]$ python
Python 2.7 (r27:82500, Aug 18 2012, 12:32:52) 
[GCC 4.7.1 20120721 (prerelease)] on linux3
Type "help", "copyright", "credits" or "license" for more information.
>>>
</pre>

Switch to version 3.2 which is just installed

<pre>[akshay@amd_dev ~/Downloads/pythonbrew]$ pythonbrew switch 3.2
Switched to Python-3.2
[akshay@amd_dev ~/Downloads/pythonbrew]$ python
Python 3.2 (r32:88445, Aug 18 2012, 12:56:01) 
[GCC 4.7.1 20120721 (prerelease)] on linux3
Type "help", "copyright", "credits" or "license" for more information.
>>> 
</pre>

Switch back to version 2.7

<pre>[akshay@amd_dev ~/Downloads/pythonbrew]$ pythonbrew switch 2.7
Switched to Python-2.7
[akshay@amd_dev ~/Downloads/pythonbrew]$ python
Python 2.7 (r27:82500, Aug 18 2012, 12:32:52) 
[GCC 4.7.1 20120721 (prerelease)] on linux3
Type "help", "copyright", "credits" or "license" for more information.
>>> 
[akshay@amd_dev ~/Downloads/pythonbrew]$ clear
</pre></p> 

Hope this will help some of you :)  
Happy coding !!