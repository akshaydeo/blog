---
layout: post
title: "Shipping your Android SDK anytime on live devices"
date: 2016-12-21 11:55:46 +0530
comments: true
categories: Android SDK Dex Classloader Dexloader
---

If your product depends upon a mobile SDK, then you must be knowing the real pain of shipping the latest version of your SDK onto the live devices through host apps. There is a way to tackle this issue with an interesting approach.

We are going to use dynamic class loading using classloaders provided by Android system.

### Class loader

The Java classloader is a part of the Java Runtime Environment that dynamically loads Java classes into the Java Virtual Machine. Usually, classes are only loaded on demand. The Java runtime system does not need to know about files and file systems because of classloaders.[2]

We are gonna use a special class loader from Android system called [PathClassLoader](https://developer.android.com/reference/dalvik/system/PathClassLoader.html), which provides a simple ClassLoader implementation that operates on a list of files and directories in the local file system, but does not attempt to load classes from the network. Android uses this class for its system class loader and for its application class loader(s).

### Chaining the ClassLoaders

{% img alignnone size-large wp-image-450 /public/images/classloading.jpg 600 400 %}[3]

So we can create a hierarchy of classloaders that can share the definition of the classes without any duplication. So in this case, classloader A has already loaded class A, so `loaderB.loadClass('A')` will delegate the request to `loaderA` instead of reloading it. This feature comes handy for our purpose. 

### Application lifecycle on Android OS

{% img alignnone size-large wp-image-450 /public/images/app_launch.jpg 600 400 %}[4]

When a user clicks on the app icon, a new application thread is started which contains a new instance of the dalvik vm. This dalvik vm has a classloader that loads the dex file (APK file) and kicks off the lifecycle of the app.

### What if we provide one more dex to load at runtime?
Consider the AAR file that we ship, contains one more DEX file which is loaded by the AAR at the time of initialization. This will allow us to change the functionality without having to force update the AAR on the devices.

### Structure of the project
{% img alignnone size-large wp-image-450 /public/images/project_structure_for_injection.png 200 64 %}

**app** : Host app where you will integrate the SDK
{% highlight groovy %}
dependencies {
  ...
  compile project(":lib")
  ...
}
{% endhighlight %}
**lib** : Our static SDK code that ships with the app. This has the secret sauce of loading the functionality runtime.

**dynamiclib** : SDK that does the implementation of the dynamic functionality of the SDK.

Let's divide our tasks and decode one by one.

#### Code architecture

To bring in the dynamic features, I am going with an interface approach. So our dynamic lib and lib will share one common interface. 

{% highlight java %}
    public interface IMethods {
        String getVersion();
        void log(String message);
    }
{% endhighlight %}

> Make sure that the package name for this interface will be the exactly same in both `lib` and `dynamiclib` module.

This `IMethods` is implemented by a class called `MethodsImpl` in dynamic lib.

{% highlight java %}
public class MethodsImpl implements IMethods {
  private static final String TAG = "##MethodsImpl##";

  @Override public String getVersion() {
    return "0.1.1";
  }

  @Override public void log(String message) {
    Log.d(TAG, message);
  }
}
{% endhighlight %}

Now build this dynamiclib to an APK (as we need a dex file), and host it. I use SimpleHttpServer. 
{% highlight bash %}
python -m SimpleHTTPServer 8000
{% endhighlight %}
This makes dynamic lib available on a link.

Let's build the last leg of the solution, downloading the APK and loading the classes using a PathClassLoader. 

##### Downloading the APK
Use any way to download the APK, I have used an AsyncTask 

##### Store the downloaded APK on internal storage sandboxed for our package
`context.getFilesDir()` gives the path to the internal sandboxed storage for the package. Store the downloaded APK in this folder. 

##### Load the downloaded APK and cast it to the IMethods
{% highlight java %}
// INTERNAL_DEX_PATH = context.getFilesDir() + FILE_NAME this is path to the file we have downloaded
private static void loadSdk(Context context) {    
    PathClassLoader pathClassLoader =
        new PathClassLoader(INTERNAL_DEX_PATH, context.getClassLoader());
    try {
      Log.d(TAG, "loading sdk class");
      // This step load the implementation of IMethods dynamically
      Class sdkClass = pathClassLoader.loadClass("net.media.dynamiclib.MethodsImpl");
      // This step creates the new instance of MethodsImpl and casts it to IMethods
      IMethods methods = (IMethods) sdkClass.newInstance();
      // This should log this message on logcat
      methods.log("testing this");
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
{% endhighlight %}

Logs when you run the app is
{% highlight log %}
12-22 17:16:29.036 10926-10926/net.media.injector D/## Injector ##: load()
12-22 17:16:29.131 10926-10926/net.media.injector W/gralloc_ranchu: Gralloc pipe failed
                                                                    
                                                                    [ 12-22 17:16:29.132 10926:10926 D/         ]
                                                                    HostConnection::get() New Host Connection established 0xad9f6d80, tid 10926
12-22 17:16:29.174 10926-10954/net.media.injector D/NetworkSecurityConfig: No Network Security Config specified, using platform default
12-22 17:16:29.222 10926-10954/net.media.injector D/## Injector ##: File output stream is => /data/user/0/net.media.injector/files/lib.apk
12-22 17:16:29.523 10926-10953/net.media.injector I/OpenGLRenderer: Initialized EGL, version 1.4
12-22 17:16:29.526 10926-10953/net.media.injector D/OpenGLRenderer: Swap behavior 1
12-22 17:16:30.064 10926-10926/net.media.injector D/## Injector ##: downloaded
12-22 17:16:30.064 10926-10926/net.media.injector D/## Injector ##: /data/user/0/net.media.injector/files/impl.dex
12-22 17:16:30.064 10926-10926/net.media.injector D/## Injector ##: /data/user/0/net.media.injector/files/impl.aar
12-22 17:16:30.065 10926-10926/net.media.injector D/## Injector ##: /data/user/0/net.media.injector/files/lib.apk
12-22 17:16:30.170 10926-10926/net.media.injector D/## Injector ##: loading sdk class
12-22 17:16:30.171 10926-10926/net.media.injector D/##MethodsImpl##: testing this
{% endhighlight %}

> This approach will allow you to update the functionality with a strong interface defined between shipped code and dynamic part of the SDK. This is the case where host app keeps interacting with your SDK using a set of functions. If your SDK is, initialize once and forget, then you can keep entire logic in dynamic part of the SDK.

### Security
Security is going to be one of the biggest concern in this approach. There are some standard ways to validate the dex file like using MD5 hashes. Once you securely download the DEX file in the internal storage then other concerns are as same as they are for shipped SDK.

[Github repo](https://github.com/akshaydeo/Injector) for the source code. 

Get in touch with me if you need any help or you find something wrong with this post. Happy coding :).

----
<br>
**References**

[1] [Android system fundamentals](http://www.electronicsweekly.com/blogs/eyes-on-android/what-is/the-dalvik-virtual-machine-2011-10/)

[2] [ClassLoaders Wiki](https://en.wikipedia.org/wiki/Java_Classloader)

[3] [How do classloaders work]((https://myprogressivelearning.wordpress.com/2014/10/28/class-loading-in-java-java-classloader-what-and-how/)

[4] [Android Application lifecycle](https://coltf.blogspot.in/p/android-os-processes-and-zygote.html)