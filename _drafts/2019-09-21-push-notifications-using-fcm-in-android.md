---
layout: post
title: "Integrating Push notifications inside your Android app using FCM in 5 simple steps"
date: 2019-09-21 18:30:46 +0530
comments: true
categories: [Android,FCM,Firebase,Push Notifications,Kotlin]
---

Push notifications is the inherent part of any Android app. I assume that everybody understands what are [Push Notifications](https://www.airship.com/resources/explainer/push-notifications-explained/?utm_source=googleplus_sumo_share&utm_medium=website&utm_campaign=ua_web) and [How FCM Works](https://firebase.google.com/docs/cloud-messaging). Lets start 👨‍💻.

## Step 1: Integrate Firebase with your Android app 

- Goto project level `build.gradle`.
- And update the file to have following entries

```groovy

buildscript {
    repositories {
        ...
        google()     
        ...   
    }
    dependencies {
        ...
        // Add this line
        classpath 'com.google.gms:google-services:4.3.2'
    }
    ...
}

allprojects {
    repositories {
        ...
        google()
        ...
        
    }
}
```

- Then goto your `app/build.gradle` and add following dependency
```
implementation 'com.google.firebase:firebase-messaging:${latestVersion}'
```

- And add this plugin at the bottom of the same gradle file

```
apply plugin: 'com.google.gms.google-services'
```

## Step 2: Updating the manifest file

- Add following service tag into your `AndroidManifest.xml` file for the app.

```groovy
<service
    android:name=".services.FCMHandler"
    android:exported="false">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT" />
    </intent-filter>
</service>

```

## Step 3: Monitoring changes to FCM token
- FCM creates a unique token for each device to identify it independently.
- This ID is important to be attached with the user profile on your server side.
- Use following code to track the changes to the FCM device id.
  
> Keep this code in either Application Class or in an activity which is shown every time the app is opened (for example: Splash screen).

```kotlin
FirebaseInstanceId.getInstance().instanceId
    .addOnCompleteListener(OnCompleteListener { task ->
        if (!task.isSuccessful) {
            Log.w(TAG, "getInstanceId failed", task.exception)
            return@OnCompleteListener
        }

        // Get new Instance ID token
        val token = task.result?.token

        // Send this token to your server
        Apis.sendToken(token);
    })
```

## Step 4: Add `FCMHandlerService` as mentioned in the manifest file in Step 2
- We use this service to detect new device id (token) generated for your device and send it to the server.

```kotlin
class FCMHandler : FirebaseMessagingService() {
    override fun onNewToken(token: String) {
        // Send this to the server
        Apis.sendToken(token);
    }
}
```

## Step 5: Add firebase config to the app

1. Setup Firebase account for this app. 
2. Goto https://console.firebase.google.com
3. Create a new project, if you haven't already created.
4. Add a new Android app.
5. Fill up the following form

![img1](https://raw.githubusercontent.com/akshaydeo/blog/master/public/images/firebase_form_add_app.png)

6. Download the config file and save inside `app` folder of the project.
7. Rerun the app and you have successfully 

# How to test this? 🤔

## Long way 🤯

- Configure a server / API endpoint that will trigger a push notification into your device. 


## But there is one more way to test these push notifications independently! 😲

- I have built an Android development tool for helping Android developers. One of it's features is **Testing Push Potifications without Server Side integration** 

![img2](https://raw.githubusercontent.com/akshaydeo/blog/master/public/images/viwr_introduction.png)



### Tell me more
