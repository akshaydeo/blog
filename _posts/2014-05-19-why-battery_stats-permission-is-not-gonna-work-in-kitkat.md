---
title: Why BATTERY_STATS permission is not gonna work in KITKAT
date: 2014-05-19 00:00:00 Z
permalink: '/why-battery_stats-permission-is-not-gonna-work-in-kitkat/'
tags:
  - Android
  - Android source code
author: Deo Akshay
layout: post
post_to_facebook_timeline:
  - 1
dsq_thread_id:
  - 2839537827
---

I am working on an internal project which requires battery stats. A quick search would guide you to use android.permission.BATTERY_STATS, and you would be able to access battery stats directly. But you would notice that this code wont work in KITKAT.

Why ?

If you are coding in Android you must be familiar with the concept of permission group. Every of the android.permission.\* is associated with a permissions group. And each of these permission groups have a protection level associated with them. There are six types of the protection levels.

<table>
  <colgroup align="left"> </colgroup> <colgroup align="left"> </colgroup> <colgroup align="left"> </colgroup> <tr>
    <th>
      Constant
    </th>
    
    <th>
      Value
    </th>
    
    <th>
      Description
    </th>
  </tr>
  
  <tr>
    <td>
      <code>normal</code>
    </td>
    
    <td>
    </td>
    
    <td>
      A lower-risk permission that gives an application access to isolated<br /> application-level features, with minimal risk to other applications,<br /> the system, or the user. The system automatically grants this type<br /> of permission to a requesting application at installation, without<br /> asking for the user&#8217;s explicit approval (though the user always<br /> has the option to review these permissions before installing).
    </td>
  </tr>
  
  <tr>
    <td>
      <code>dangerous</code>
    </td>
    
    <td>
      1
    </td>
    
    <td>
      A higher-risk permission that would give a requesting application<br /> access to private user data or control over the device that can<br /> negatively impact the user. Because this type of permission<br /> introduces potential risk, the system may not automatically<br /> grant it to the requesting application. For example, any dangerous<br /> permissions requested by an application may be displayed to the<br /> user and require confirmation before proceeding, or some other<br /> approach may be taken to avoid the user automatically allowing<br /> the use of such facilities.
    </td>
  </tr>
  
  <tr>
    <td>
      <code>signature</code>
    </td>
    
    <td>
      2
    </td>
    
    <td>
      A permission that the system is to grant only if the requesting<br /> application is signed with the same certificate as the application<br /> that declared the permission. If the certificates match, the system<br /> automatically grants the permission without notifying the user or<br /> asking for the user&#8217;s explicit approval.
    </td>
  </tr>
  
  <tr>
    <td>
      <code>signatureOrSystem</code>
    </td>
    
    <td>
      3
    </td>
    
    <td>
      A permission that the system is to grant only to packages in the<br /> Android system image <em>or</em> that are signed with the same<br /> certificates. Please avoid using this option, as the<br /> signature protection level should be sufficient for most needs and<br /> works regardless of exactly where applications are installed. This<br /> permission is used for certain special situations where multiple<br /> vendors have applications built in to a system image which need<br /> to share specific features explicitly because they are being built<br /> together.
    </td>
  </tr>
  
  <tr>
    <td>
      <code>system</code>
    </td>
    
    <td>
      0x10
    </td>
    
    <td>
      Additional flag from base permission type: this permission can also<br /> be granted to any applications installed on the system image.<br /> Please avoid using this option, as the<br /> signature protection level should be sufficient for most needs and<br /> works regardless of exactly where applications are installed. This<br /> permission flag is used for certain special situations where multiple<br /> vendors have applications built in to a system image which need<br /> to share specific features explicitly because they are being built<br /> together.
    </td>
  </tr>
  
  <tr>
    <td>
      <code>development</code>
    </td>
    
    <td>
      0x20
    </td>
    
    <td>
      Additional flag from base permission type: this permission can also<br /> (optionally) be granted to development applications.
    </td>
  </tr>
</table>

Earlier BATTERY_STATS was having protection level as dangerous but from kitkat they have changed it to signature/system so unless your app is running as system app or your app is signed with same certificate as system apps, your app wont be able to access battery stats !!

Have a good day coders :)
