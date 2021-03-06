---
layout: post
title: Converting MRC to ARC
---

<h1 class="post-center-title">Initial Conversion</h1>

0、 If there are a lot of third party code using MRC like ASI or JSONKit, just put *-fno-objc-arc* to them.  
![Put -fno-objc-arc to third party MRC code]({{site.baseurl}}/assets/convert2arc/asi_fno_objc_arc.png)  

1、 **Preferences -> General**: Check *Continue building after errors* to reveal all errors.  
![Continue build after errors]({{site.baseurl}}/assets/convert2arc/continue_error.png)  

2、 **Edit -> Convert -> To Objective-C ARC...**: Select the target to be converting and click **Check** then Xcode will prompt 'Xcode found many issues that prevents conversion from proceeding. Fix all ARC readiness issues and try again.'  
![Convert2ARC]({{site.baseurl}}/assets/convert2arc/convert2arc.png)  
![Prompt many readiness issues]({{site.baseurl}}/assets/convert2arc/readiness_issues.png)  

3、 Fix all ARC readiness issues and repeat step 2 until prompt the 'Convert to Automatic Reference Counting' Dialog.
![Prompt start 2 convert]({{site.baseurl}}/assets/convert2arc/start2convert_prompt.png)  
![Generating preview]({{site.baseurl}}/assets/convert2arc/generating_preview.png)  
![Save]({{site.baseurl}}/assets/convert2arc/save.png)  

4、Build and run. Redo the step 0 and delete some 'release' code manually when meet some errors like 'ARC forbids explicit message send of ...'.

5、Remember to clean the project or change another simulator when meet the prompt 'lipo can't figure out the architecture type of...'.  

6、 Run the App and test it really really carefully because some tricky code may rely on the feature of MRC.  

Refer to: <https://objectpartners.com/2013/07/17/converting-an-ios-project-to-use-arc-automatic-reference-counting/>
