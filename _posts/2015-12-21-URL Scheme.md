---
layout: post
title: URL Scheme
---

### 0. Return App when finish the tel calling
```objective-c
//Legal for AppStore
- (void) dialPhoneNumber:(NSString *)aPhoneNumber
{
    NSURL *phoneURL = [NSURL URLWithString:[NSString
    stringWithFormat:@"tel:%@", aPhoneNumber]];
    [self.phoneCallWebView loadRequest:[NSURLRequest requestWithURL:phoneURL]];
}

//Illegal for AppStore
[[UIApplication sharedApplication]
openURL:[NSURL URLWithString:@"telprompt://10086"]];
```

### 1. Customize URL Scheme
![URL Types]({{site.baseurl}}/assets/url_scheme/url_types.png)  
![URL Types1]({{site.baseurl}}/assets/url_scheme/url_types1.png)

### 2. Overide handler
```objective-c
//Priority is lower than below method
-(BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url
{
    if ([[url scheme] isEqualToString:@"Al"]) {
        NSLog(@"%@",url);
    }
    return YES;
}

//Priority is higher than above method
-(BOOL)application:(UIApplication *)application
           openURL:(NSURL *)url
 sourceApplication:(NSString *)sourceApplication
        annotation:(id)annotation
{
    if ([sourceApplication isEqualToString:@"com.Geek.Al"]) {
        NSLog(@"%@", sourceApplication);   //From which App（Bundle identifier）
        NSLog(@"scheme:%@", [url scheme]); //url scheme
        NSLog(@"query: %@", [url query]);  //query string  use“?param=ios”format
        return YES;
    } else {
        return NO;
    }
}
```

### 3. Invoke from another App
```objective-c
[[UIApplication sharedApplication]
openURL:[NSURL URLWithString:@"Al://method?param=ios"]];
```

### 4. LSApplicationQueriesSchemes in iOS9
If using *canOpenURL* in iOS9, we should add the *LSApplicationQueriesSchemes*
in *Info.plist*. The limitation is 50.  

```
<key>LSApplicationQueriesSchemes</key>  
<array>  
    <string>weixin</string>  
</array>
```
Refer to: <http://blog.sina.com.cn/s/blog_877e9c3c0102v0m5.html>
