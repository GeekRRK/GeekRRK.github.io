---
layout: post
title: Converting MRC to ARC
---

<h1 class="post-center-title">Initial Conversion</h1>

### 0、If there are a lot of third party code using MRC like ASI, just put *-fno-objc-arc* to them. Solve the issues in our own code according to the suggestions of Xcode.

### 1、**Preferences -> General**: Check “Continue building after errors”.

### 2、**Edit -> Convert -> To Objective-C ARC...**: Select the target to be converting and click **Check**.

### 3、Review automatic changes and click next.
