---
layout: post
title: Install Apps on iPhone Devices
---

### 1. Install the WWDR Intermediate Certificate

### 2. Request a certificate from a certificate authority
1. Applications -> Utilities -> Keychain Access ->
Certificate Assistant -> Request Certificate from A Certificate Authority
2. Upload the Certificate to create Development and Production
certificates.

### 3. Create an App ID

### 4. Create Provisioning Profile

Refer to: <http://www.cnblogs.com/85538649/archive/2011/10/28/2227270.html>  
Learn more: <http://www.cocoachina.com/bbs/read.php?tid-330302.html>  

<h1 class="post-center-title">Detail about Enterprise</h1>
1.　**plist:**  
{% highlight plist %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>items</key>
	<array>
		<dict>
			<key>assets</key>
			<array>
				<dict>
					<key>kind</key>
					<string>software-package</string>
					<key>url</key>
					<string>https://www.example.com/apps/foo.ipa</string>
				</dict>
				<dict>
					<key>kind</key>
					<string>display-image</string>
					<key>url</key>
					<string>https://www.example.com/image.57x57.png</string>
				</dict>
				<dict>
					<key>kind</key>
					<string>full-size-image</string>
					<key>url</key>
					<string>https://www.example.com/image.512x512.png</string>
				</dict>
			</array>
			<key>metadata</key>
			<dict>
				<key>bundle-identifier</key>
				<string>com.example.foo</string>
				<key>bundle-version</key>
				<string>1.0</string>
				<key>kind</key>
				<string>software</string>
				<key>title</key>
				<string>foo</string>
			</dict>
		</dict>
	</array>
</dict>
</plist>
{% endhighlight %}  

2.　**Download address:**  
itms-services://?action=download-manifest&url=https://www.example.com/apps/foo.plist  

<h1 class="post-center-title">Detail about Review of AppStore</h1>
Pay attention things: <http://i.bufan.com/article/201405/45189.html>  
Tel of Apple: <https://developer.apple.com/contact/phone.php>

<h1 class="post-center-title">Search info of App</h1>
Refer to: <http://blog.csdn.net/kesalin/article/details/6605934>  
Remember to add *cn* to the address when the app is aimed to China.  
Search in browser, see the following format:  
https://itunes.apple.com/cn/app/xiao-yuan-ying-xiao-zhu-shou/id985559724?mt=8  
https://itunes.apple.com/cn/lookup?id=985559724
