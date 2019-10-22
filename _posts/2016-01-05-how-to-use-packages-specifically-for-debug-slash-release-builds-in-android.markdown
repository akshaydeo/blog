---
layout: post
title: "How to use packages specifically for Debug/Release builds in Android"
date: 2016-01-05 12:39:06 +0530
comments: true
categories: 
  - Android 
  - Gradle 
  - Android Studio
  - Build
  - Android build flavors
image: /public/images/2.png
---

## Preface
Currently I am working on an Android app for one of the most interesting startups in Fintech. I have been really choosy about the packages that are getting shipped with this app, simply because it involves a lot of money related functionalties. During the development, I came across a requirement that debug builds should have instabug integrated for reporting UI issues easily. APK size matters a lot, so I wanted to achieve this without shipping Instabug SDK in production builds. 

## How to do this?

### Gradle file
{% highlight groovy %}
...
dependencies {
    ...
    compile appDependencies.rateUs
    compile appDependencies.markdownJ
    debugCompile(appDependencies.instabug) {
        exclude group: 'com.mcxiaoke.volley'
    }
}
...    
{% endhighlight %}
I changed the way gradle compiles instabug dependency. Now it's done during debug builds only. Release builds will not consider this dependency.

### How to use this conditional dependency in code?
![](/public/images/1.png)
![](/public/images/2.png)

Create debug and release folders inside the src folder of your app. Create the exact same package structure (same as main folder) in both of these folders. Create a class with overriding nature i.e. same name and methods. Now write add instabug initialization inside the debug flavour, while release flavour won't have this bit. Update your main Application class to include the initialization of this newly created class.

Application class in main.
{% highlight java %}
    @Override
    public void onCreate() {
        super.onCreate();        
        ...
        SimplAppInitializer.init(this);
        ...
    }
{% endhighlight %}

And you are done, now the instabug will be only compiled with the debug builds and not with your production builds.

Let me know if there are some issues/suggestions related to this post.
Happy coding \m/