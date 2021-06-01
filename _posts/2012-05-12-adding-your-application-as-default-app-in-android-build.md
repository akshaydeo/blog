---
title: Adding your application as default app in Android build
date: 2012-05-12 00:00:00 Z
permalink: '/adding-your-application-as-default-app-in-android-build/'
tags:
  - Android
  - Android source code
author: Deo Akshay
layout: post
dsq_thread_id:
  - 3183294632
---

I have been working on Android source code for past few weeks. I was never a systems&#8217; guy or a C programmer for that matter except whatever was required for college assignments. So I had to struggle a lot to get more and more information about changing Android source code the way I want, and sadly very limited information is available over internet about changing Android source code (If I am wrong, I would really appreciate your help links as comment :) ).

Ok so in this post I would discuss how to add your Android application as default application in Android build. Whenever you are customizing ROM mostly you want some of the application to be installed in /system/app folder i.e. as default application so that no one could uninstall unless they have root access.

I followed following steps to achieve this :  
( Note: All paths are relative to top root of Android source code )

1. Add your android source code to the folder /packages/apps. If you have developed app using eclipse then remember to remove gen and bin folders as we are going to build is using make command.
2. Write Android.mk file for your application and add it to top level of your application source. I have given a sample Andorid.mk file
<pre>LOCAL_PATH := $(call my-dir)
include $(CLEAR_VARS)

LOCAL_MODULE_TAGS := optional

LOCAL_STATIC_JAVA_LIBRARIES := libarity android-support-v4

LOCAL_SRC_FILES := $(call all-java-files-under, src)

LOCAL_SDK_VERSION := current

LOCAL_PACKAGE_NAME := &lt;your package name>

include $(BUILD_PACKAGE)
##################################################
include $(CLEAR_VARS)
include $(BUILD_MULTI_PREBUILT)

# Use the folloing include to make our test apk.

include $(call all-makefiles-under,$(LOCAL_PATH))

</pre>

3. Add your applications entry into /build/target/product/core.mk&#8217;s PRODUCT_PACKAGE list. The file looks like this
<pre># Copyright (C) 2007 The Android Open Source Project

#

# Licensed under the Apache License, Version 2.0 (the "License");

# you may not use this file except in compliance with the License.

# You may obtain a copy of the License at

#

# http://www.apache.org/licenses/LICENSE-2.0

#

# Unless required by applicable law or agreed to in writing, software

# distributed under the License is distributed on an "AS IS" BASIS,

# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

# See the License for the specific language governing permissions and

# limitations under the License.

#

PRODUCT_BRAND := generic
PRODUCT_DEVICE := generic
PRODUCT_NAME := core

PRODUCT_PROPERTY_OVERRIDES :=
ro.config.notification_sound=OnTheHunt.ogg
ro.config.alarm_alert=Alarm_Classic.ogg

PRODUCT_PACKAGES :=
ApplicationsProvider
BackupRestoreConfirmation
Browser
Contacts
ContactsProvider
DefaultContainerService
DownloadProvider
DownloadProviderUi
HTMLViewer
Home
KeyChain
MediaProvider
PackageInstaller
PicoTts
SettingsProvider
SharedStorageBackup
TelephonyProvider
UserDictionaryProvider
VpnDialogs
apache-xml
bouncycastle
bu
cacerts
com.android.location.provider
com.android.location.provider.xml
core
core-junit
dalvikvm
dexdeps
dexdump
dexlist
dexopt
dmtracedump
dx
ext
filterfw
framework-res
hprof-conv
icu.dat
installd
ip
ip-up-vpn
ip6tables
iptables
libOpenMAXAL
libOpenSLES
libaudiopreprocessing
libcrypto
libdvm
libexpat
libfilterfw
libfilterpack_imageproc
libgabi++
libicui18n
libicuuc
libnativehelper
libnfc_ndef
libpowermanager
libspeexresampler
libsqlite_jni
libssl
libstagefright_soft_h264dec
libstagefright_soft_aacdec
libstagefright_soft_amrdec
libstagefright_soft_g711dec
libstagefright_soft_mp3dec
libstagefright_soft_mpeg4dec
libstagefright_soft_vorbisdec
libstagefright_soft_vpxdec
libvariablespeed
libwebrtc_audio_preprocessing
libwilhelm
libz
screencap
sensorservice

# host-only dependencies

ifeq ($(WITH_HOST_DALVIK),true)
PRODUCT_PACKAGES +=
apache-xml-hostdex
bouncycastle-hostdex
core-hostdex
dalvik
endif

</pre>

4. Build new system image and see magic :)

Hope this post will help you guys :). Any suggestions and corrections/amendments are welcome :).
