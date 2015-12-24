---
layout: post
title: Framework
---

<h1 class="post-center-title">Dynamic Framework</h1>
### 0. Create a Cocoa Touch Framework
![Cocoa Touch Framework]({{site.baseurl}}/assets/framework/cocoatouch_framework.png)  
Write some class and put their headers into the header of framework while must
place them into the public zone.
![Public header]({{site.baseurl}}/assets/framework/public_header.png)

### 1. Build twice for simulator and iPhones
These steps are same as [Static Library](http://geekrrk.github.io/Blog/2015/12/22/Static%20Library.html)
except combining the two frameworks into one. So make the aggregate framework.  
If use *lipo* command to combine them, remember to go into the direcotry
of framework and find the same name file which is the lib.  
Create Aggregate Target in the existing framework project.  
File -> New -> Target  
![Aggregate Target]({{site.baseurl}}/assets/framework/aggregate_target.png)  
Choose the target and click the **+** to add the **New Run Script Phase**  
Put the following script into it.  
{% highlight sh %}
# Sets the target folders and the final framework product.
# If the name of project is different from the target name of the
# framework, we should define the FMKNAME, for example,
# FMK_NAME = "MyFramework"
FMK_NAME=${PROJECT_NAME}
# Install dir will be the final output to the framework.
# The following line create it in the root folder of the current project.
INSTALL_DIR=${SRCROOT}/Products/${FMK_NAME}.framework
# Working dir will be deleted after the framework creation.
WRK_DIR=build
DEVICE_DIR=${WRK_DIR}/Release-iphoneos/${FMK_NAME}.framework
SIMULATOR_DIR=${WRK_DIR}/Release-iphonesimulator/${FMK_NAME}.framework
# -configuration ${CONFIGURATION}
# Clean and Building both architectures.
xcodebuild -configuration "Release" -target "${FMK_NAME}" -sdk iphoneos clean build
xcodebuild -configuration "Release" -target "${FMK_NAME}" -sdk iphonesimulator clean build
# Cleaning the oldest.
if [ -d "${INSTALL_DIR}" ]
then
rm -rf "${INSTALL_DIR}"
fi
mkdir -p "${INSTALL_DIR}"
cp -R "${DEVICE_DIR}/" "${INSTALL_DIR}/"
# Uses the Lipo Tool to merge both binary files (i386 + armv6/armv7) into one Universal final product.
lipo -create "${DEVICE_DIR}/${FMK_NAME}" "${SIMULATOR_DIR}/${FMK_NAME}" -output "${INSTALL_DIR}/${FMK_NAME}"
rm -r "${WRK_DIR}"
open "${INSTALL_DIR}"
{% endhighlight %}
![Run Script]({{site.baseurl}}/assets/framework/run_script.png)  
Choose the target and build. It will popup the aggregate framework in the finder if everything is ok.

### 2. Use the framework in another project
Remember to drag the framework into Embedded Binaries zone that is only
available in Xcode6+.  
![embedded_binaries]({{site.baseurl}}/assets/framework/embedded_binaries.png)  

<h1 class="post-center-title">Static Library available in Xcode5-</h1>
![Static Library]({{site.baseurl}}/assets/framework/static_library.png)  
Now we don't need to drag the framework into the Embedded Binaries zone
that doesn't exist in Xcode5-.  

Refer to: <http://www.cnblogs.com/zhw511006/p/4155930.html>  

<h1 class="post-center-title">Use framework dynamically</h1>
### 0. Set Build Phases
Put the framework into the following zones.  
Targets -> Build Phases -> Link Binary With Libraries  
Targets -> Build Phases -> Copy Bundle Resources  
![Build Phases]({{site.baseurl}}/assets/framework/build_phases.png)  
If want to link the framework automatically when the App launches, we can set
the dependent path of framework.  
Targets -> Build Setting -> Linking -> Runpath Search Paths  
![Runpath Searth Paths]({{site.baseurl}}/assets/framework/runpath.png)  
*@executable_path/* represents the directory of .app in sandbox, don't miss the
*/*.  
If don't want to load the framework when the App launches, we can set the
*Status* to Optional in *Link Binary With Libraries* or delete it.
![Optional]({{site.baseurl}}/assets/framework/optional.png)  

### 1. Load the framework
Use *dlopen* to load the framework, the real executable code is
 *MyFramework.framework/MyFramework*, so don't forget *MyFramework*.
{% highlight objective-c %}
#import <dlfcn.h>
#import <mach-o/dyld.h>

//Be careful with the path of the framework we want to load with which method.
#define BUNDLEPATH_DLOPEN [[NSBundle mainBundle] pathForResource:@"MyFramework.framework/MyFramework" ofType:nil];
#define BUNDLEPATH_NSBUNDLE [[NSBundle mainBundle] pathForResource:@"MyFramework.framework" ofType:nil];

// This is for Documents path of sandbox when we download the framework
// to the sandbox.
// #define DOCUMENTSPATH_DLOPEN [NSString stringWithFormat:@"%@/Documents/MyFramework.framework/MyFramework", NSHomeDirectory()];
// #define DOCUMENTSPATH_NSBUNDLE [NSString stringWithFormat:@"%@/Documents/MyFramework.framework", NSHomeDirectory()];

- (IBAction)onDlopenLoadAtPathAction:(id)sender
{
    [self dlopenLoadDylibWithPath:BUNDLEPATH_DLOPEN];
}

- (void)dlopenLoadDylibWithPath:(NSString *)path
{
    void *libHandle = NULL;
    libHandle = dlopen([path cStringUsingEncoding:NSUTF8StringEncoding], RTLD_NOW);
    if (libHandle == NULL) {
        char *error = dlerror();
        NSLog(@"dlopen error: %s", error);
    } else {
        NSLog(@"dlopen load framework success.");
    }
}
{% endhighlight %}

Use *NSBundle* to load the framework, this time we should forget *MyFramework*,
just use *MyFramework.framework* as the path.
{% highlight objective-c %}
- (IBAction)onBundleLoadAtPathAction:(id)sender
{
    [self bundleLoadDylibWithPath:BUNDLEPATH_NSBUNDLE];
}

- (void)bundleLoadDylibWithPath:(NSString *)path
{
    NSError *err = nil;
    NSBundle *bundle = [NSBundle bundleWithPath:path];
    if ([bundle loadAndReturnError:&err]) {
        NSLog(@"bundle load framework success.");
    } else {
        NSLog(@"bundle load framework err:%@",err);
    }
}
{% endhighlight %}

### 2. Invoke the method of the framework  
{% highlight objective-c %}
- (IBAction)onTriggerButtonAction:(id)sender
{
    Class rootClass = NSClassFromString(@"MyUtil");
    if (rootClass) {
        id object = [[rootClass alloc] init];

        //If the class in the framework we already known
        [(MyUtil *)object showMsg];

        //If the method of the class in the framework we already known
        # pragma clang diagnostic push
        # pragma clang diagnostic ignored "-Warc-performSelector-leaks"
        SEL callFun = NSSelectorFromString(@"showMsg");
        [object performSelector:callFun withObject:nil];
        # pragma clang diagnostic pop
    }
}
{% endhighlight %}

### 3. Monitoring the adding and removing of frameworks
We can register the callback for the adding or removing of the framework.  
{% highlight objective-c %}
static void image_added(const struct mach_header *mh, intptr_t slide)
{
    NSLog(@"add");
}

static void image_removed(const struct mach_header *mh, intptr_t slide)
{
    NSLog(@"remove");
}

+ (void)load
{
  _dyld_register_func_for_add_image(&image_added);
  _dyld_register_func_for_remove_image(&image_removed);
}
{% endhighlight %}

Refer to: <http://foggry.com/blog/2014/06/12/wwdc2014zhi-iosshi-yong-dong-tai-ku/>

<h1 class="post-center-title">Package resources into framework</h1>
### 0. Drag xib and image to *Copy Bundle Resources* of framework project.
Remember to drag them to *Embedded Binaries* if the framework is dynamic.  
![Copy Bundle Resources]({{site.baseurl}}/assets/framework/copybundle.png)  

### 1. Choose aggregate and build.
We can find the xib and image in the directory of framework  
![Directory of framework]({{site.baseurl}}/assets/framework/framework_directory.png)  

### 2. Load the xib and image from main bundle
{% highlight objective-c %}
[[NSBundle mainBundle]
loadNibNamed:@"MyFramework.framework/View"
       owner:nil
     options:nil];

[[NSBundle mainBundle] pathForResource:@"MyFramework.framework/img"
                                ofType:@"jpg"];
{% endhighlight %}

### 3. Load resource from other bundle.
Create a directory and put the image into it then name the direcotry as
 *myBundle.bundle*. Drag the bundle to our project.  

{% highlight objective-c %}
+ (NSString *)pathForResource:(NSString *)name ofType:(NSString *)type {
    NSBundle *myBundle = [NSBundle
      bundleWithPath:[[NSBundle mainBundle]
     pathForResource:@"myBundle"
              ofType:@"bundle"]];
    [myBundle load];
    NSString* path = [myBundle pathForResource:name ofType:type];
    return path;
}
{% endhighlight %}

Refer to: <http://blog.csdn.net/xyxjn/article/details/42527341>  

When finish this blog, we might wonder that can we use this tech as dynamic
plugin and bypass the review of AppStore. People say it won't work. And Apple Inc.
only allow the *lua* to update App dynamically.
See this site: <http://www.cocoachina.com/bbs/read.php?tid=129723>
