---
title: Switching branch with repo for Android Source code
author: Deo Akshay
layout: post
comments: true
permalink: /switching-branch-with-repo-for-android-source-code/
dsq_thread_id:
  - 2678420378
categories:
  - Android
  - Android source code
---
I find working with repo as difficult as working with Android source code. We faced a lot of trouble while working with different versions of Android Source code. So I googled for repo command help but didnt find much help. So I experimented a bit and found a good way of switching branch.</br></br>

  1. git reset command for removing changes that you have made 
    <pre>$ repo forall -c git reset --hard</pre>

  2. then initialize repo with new branch.Suppose you have checked out version 4.0.4\_r1.2 and you want to revert to 4.0.1\_r1 (which was my case actually :P) then 
    <pre>$ repo init -u https://android.googlesource.com/platform/manifest -b android-4.0.1_r1</pre>

  3. Sync repo 
    <pre>$ repo sync</pre>

  4. It&#8217;s applicable for all combinations

And magic happens :).  
Hope you will find this useful. Cheers and happy coding m/.
