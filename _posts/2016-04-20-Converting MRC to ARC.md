---
layout: post
title: Converting MRC to ARC
---

<h1 class="post-center-title">Initial Conversion</h1>

0、 If there are a lot of third party code using MRC like ASI, just put *-fno-objc-arc* to them.

1、 **Preferences -> General**: Check *Continue building after errors*.

2、 **Edit -> Convert -> To Objective-C ARC...**: Select the target to be converting and click **Check** then Xcode will prompt 'Xcode found many issues that prevents conversion from proceeding. Fix all ARC readiness issues and try again.'.

3、 Fix all ARC readiness issues and remember to clean the project or change another simulator when meet the prompt 'lipo can't figure out the architecture type of...'.

4、 Repeat the step 2 until success then follow the guide.

5、 Redo the step 0 and delete some 'release' sentences manually.

6、 Run the App and test it really really carefully because some tricky code maybe rely on the feature of MRC.
