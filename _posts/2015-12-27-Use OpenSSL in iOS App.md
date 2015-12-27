---
layout: post
title: Use OpenSSL in iOS App
---

### 0. Download source code
<http://www.openssl.org/source>  
Get the latest version, for example, openssl-1.0.2e.

### 1. Modify ui_openssl.c
Replace *static volatile sig_atomic_t intr_signal;* with
*static volatile int intr_signal;** in **crypto/ui/ui_openssl.c*
for preventing building error.  

### 2. Execute configure
`$ cd Desktop; mkdir ssllibs; cd ssllibs`  
`$ mkdir openssl_i386 openssl_x86_64 openssl_armv7 openssl_armv7s openssl_arm64`  
`$ cd openssl-1.0.2e`  
`$ ./configure BSD-generic32 --openssldir=/Users/GeekRRK/Desktop/ssllibs/openssl_i386`  

### 3. Modify Makefile and Compile
Replace *CC= gcc* with *CC= /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang -miphoneos-version-min=7.0 -arch i386*  
Append *-isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk* to *CFLAG=*  
`$ make`  
`$ make install`  

### 4. Clean configure, and repeat step 2 and 3 for other platforms
`$ make clean`  
Remember replace *i386* with other platforms, and replace *iPhoneSimulator* with *iPhoneOS* for iPhones.

### 5. Combine different architectures into one
Go into directory */Users/GeekRRK/Desktop/ssllibs/openssl_xxx/lib* to find *libcrypto.a* and *libssl.a* and combine each of them as the steps in blog
[Static library](http://geekrrk.github.io/Blog/2015/12/22/Static%20Library.html)

### 6. Use OpenSSL in project
Drag the two *.a* files and *include* directory into project.  
Set *Always Search User Paths* to *YES* and *User Header Search Paths* to
*$(SRCROOT)/include*  
Add the following code to our code and build run.  
{% highlight objective-c %}
#include <openssl/md5.h>

void Md5( NSString * string){
    unsigned char *inStrg = ( unsigned char *)[[string dataUsingEncoding : NSASCIIStringEncoding ] bytes];
    unsigned long lngth = [string length ];
    unsigned char result[ MD5_DIGEST_LENGTH ];
    NSMutableString *outStrg = [NSMutableString string];
    MD5 (inStrg, lngth, result);
    unsigned int i;
    for (i = 0 ; i < MD5_DIGEST_LENGTH ; i++)
    {
        [outStrg appendFormat : @"%02x" , result[i]];
    }
    NSLog ( @"input string:%@" ,string);
    NSLog ( @"md5:%@" ,outStrg);
}

- (void)viewDidLoad {
    [super viewDidLoad];

    Md5(@"12345");
}
{% endhighlight %}  

Refer to:  
<http://blog.csdn.net/kmyhy/article/details/6534067>  
<http://m.blog.csdn.net/blog/lsq19871207/46535597>  
This is a script to compile the OpenSSL for all architectures:  
<https://github.com/x2on/OpenSSL-for-iPhone/blob/master/build-libssl.sh>  

<h1 class="post-center-title">Functions of OpenSSL</h1>
Common interface of X509: <http://blog.csdn.net/wanjie518/article/details/6570141>  
