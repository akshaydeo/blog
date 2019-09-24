---
layout: post
title: "Integrating Push notifications inside your Android app using FCM in 4 simple steps"
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

# How to test this? 🤔

## Long way 🤯 but we will have to do it before shipping this feature.

1. Setup Firebase account for this app. 
2. Refer: https://firebase.google.com/docs/android/setup?authuser=0


## But there is one more way to test these push notifications independently! 😲

- I have built a Android dev tool for helping Android developers. One of it's features is **Testing Push Potifications without Server Side integration**

### Tell me more