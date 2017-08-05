---
title: 'The night when we released our AppSurfer mobile app&#8230;'
author: Deo Akshay
layout: post
permalink: /the-night-when-we-released-our-appsurfer-mobile-app/
dsq_thread_id:
  - 1818779330
categories:
  - Uncategorized
---
Its one of the biggest day for us, as a company, as entrepreneurs, as technology lovers!! We released out the first version of mobile app <a href="https://play.google.com/store/apps/details?id=main.java.com.appsurfer" title="AppSurfer" target="_blank">AppSurfer</a> on 21st early morning (like 1.30 am early :P). We were happy, and equally nervous. And at the same time Google play was giving us a bit of hard time :D. We were kind of polling onto our expected web page on Google Play and it was returning 404 :-/. We were kind of sleepy, exhausted and equally excited to see our product page on Google play. We were too bored to click refresh again and again. And then the programmer within us resulted into this script 

<pre>import urllib
import urllib.request
import sys
from multiprocessing import Process
import time
 
def read_page():
    print("reading source")
    req = urllib.request.Request("https://play.google.com/store/apps/details?id=main.java.com.appsurfer")
    try:
        page_source = urllib.request.urlopen(req)
    except:
        print("no")
        return
    print("AppSurfer app is up \m/ yohoooooo !!")
 
if __name__ == "__main__":
    #sendmail()
    while(True):
        read_page()
        time.sleep(15)
</pre>

Output:

<pre>reading source
no
reading source
no
reading source
no
reading source
no
reading source
no
reading source
no
reading source
no
reading source
AppSurfer app is up \m/ yohoooooo !!
reading source
AppSurfer app is up \m/ yohoooooo !!
</pre>

And finally we saw AppSufer is up, it was almost 3.25 am. But nevertheless, its a big day for us. We are sure that you are gonna like AppSurfer service, and tune in with us for exciting upcoming updates.

P.S. If you liked inception, I am sure you are gonna like this <a href="http://appsurfer.com/get_app" title="page" target="_blank">page</a>