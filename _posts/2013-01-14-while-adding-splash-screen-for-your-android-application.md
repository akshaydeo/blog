---
title: While adding splash screen for your Android application
author: Deo Akshay
layout: post
permalink: /while-adding-splash-screen-for-your-android-application/
dsq_thread_id:
  - 2678628029
categories:
  - Android
---
Right now I am working on Android application for [AppSurfer][1]. And while designing a prototype for the application, I had to integrate it with a splash screen. Segmentation is a well known issue for Android developers, and hence we have to be careful about the design components. They should not break on devices which are very large or very small. So I did a search and found following set of dimensions that should be used for splash screen which accommodates almost all screen sizes.

1. LDPI  
Portrait : 200x320px  
Landscape : 320x200px  
2. MDPI  
Portrait : 320x480px  
Landscape : 480x320px  
3. HDPI  
Portrait : 480x800px  
Landscape : 800x480px  
4. XHDPI  
Portrait : 720px1280px  
Landscape : 1280x720px

Happy coding \m/

 [1]: appsurfer.com