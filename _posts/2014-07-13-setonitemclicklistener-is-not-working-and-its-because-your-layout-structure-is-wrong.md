---
title: 'setOnItemClickListener() is not working&#8230;and its&#8217; because your layout structure is wrong'
author: Deo Akshay
layout: post
permalink: /setonitemclicklistener-is-not-working-and-its-because-your-layout-structure-is-wrong/
post_to_facebook_timeline:
  - 1
dsq_thread_id:
  - 2842471565
categories:
  - Android
---
This is gonna be a quick post. I spent like last 45 mins solving a very dumb issue, related to setOnItemClickListener of a custom class extending GridView. 

Layout of GridView item:
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:layout_width="@dimen/fragment_application_grid_item_width"
                android:layout_height="@dimen/fragment_application_grid_item_height"
                android:background="@drawable/fragment_application_list_item_background_drawable"
                android:orientation="vertical">

        

<!-- Some Elements -->

        <View
            android:id="@+id/application_price_indicator"
            android:layout_width="0dp"
            android:layout_height="fill_parent"
            android:layout_weight="0.05"
            android:background="@drawable/fragment_application_grid_item_free_indicator"/>

        <ImageButton
            android:id="@+id/add_to_list_menu"
            android:layout_width="0dp"
            android:layout_height="@dimen/fragment_application_grid_item_menu_dimen"
            android:layout_weight="1"
            android:background="@android:color/transparent"
            android:src="@drawable/fragment_application_grid_item_add_to_list_icon"/>

        <ImageButton
            android:id="@+id/install_app_icon"
            android:layout_width="0dp"
            android:layout_height="@dimen/fragment_application_grid_item_menu_dimen"
            android:background="@android:color/transparent"
            android:layout_weight="1"
            android:src="@drawable/fragment_application_grid_item_install_icon"/>

        <ImageButton
            android:id="@+id/share_app_icon"
            android:layout_width="0dp"
            android:layout_height="@dimen/fragment_application_grid_item_menu_dimen"
            android:background="@android:color/transparent"
            android:layout_weight="1"
            android:src="@drawable/fragment_application_grid_item_share_icon"/>
        </LinearLayout>
    </RelativeLayout>
{% endhighlight %}

The issue here is, ImageButton is the part of this view. So whenever onClick event happens, Android system will try to find onClickListener related to ImageButtons in the view. So There are only two solutions to this :

1. Either change ImageButton to ImageView  
2. Override onTouch of ImageButton and return False.