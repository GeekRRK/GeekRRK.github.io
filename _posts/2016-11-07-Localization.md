---
layout: post
title: Localization
---

<h1 class="post-center-title">Preparation</h1>

Add languages under Project->Info->Localizations.
![Add languages]({{site.baseurl}}/assets/Localization/add_languages.png)  

<h1 class="post-center-title">Localize App name</h1>

0、Create InfoPlist.
![Create InfoPlist]({{site.baseurl}}/assets/Localization/create_infoplist.png)  

1、Select InfoPlist and click Localize... on the File inspector.
![Localize in the File inspector]({{site.baseurl}}/assets/Localization/file_inspector_localize.png)  

2、Select the language we want on the alert view to localize InfoPlist.
![Localize in the File inspector]({{site.baseurl}}/assets/Localization/localize_Infoplist.png)  

3、Check all the languages we want on the File inspector.
![Localize in the File inspector]({{site.baseurl}}/assets/Localization/check_languages.png)  

4、Add localized App name to each InfoPlist.strings.
![Localize in the File inspector]({{site.baseurl}}/assets/Localization/add_app_name_to_each_infoplist.png)  

<h1 class="post-center-title">Localize strings in code</h1>

0、Steps are same as Localizing App name except the file name is Localizable.strings.
![Localize in the File inspector]({{site.baseurl}}/assets/Localization/localize_strings_in_code.png)  

1、Use NSLocalizedString to assign strings.
`NSString *title = NSLocalizedString(@"click", nil);`

Tip: Edit->Scheme->Run->Arguments Passed On Launch ->-AppleLanguages (zh-Hans) to change the run language environment. (Won't work for InfoPlist.strings)

<h1 class="post-center-title">When code in team</h1>

`NSString *title = NSLocalizedStringFromTable(@"click", @"myLocalizable", nil);`

<h1 class="post-center-title">Localize image</h1>

Way 1: Localize image name like previous steps.

Way 2:
0、Localize the image on the File inspector.
1、Show the image in Finder and put another image with the same name to another .lproj directory then drag the image to project under the previous image.
![Localize in the File inspector]({{site.baseurl}}/assets/Localization/localize_image.png)  
![Localize in the File inspector]({{site.baseurl}}/assets/Localization/localized_images.png)  

<h1 class="post-center-title">View/Switch localized languages</h1>

Remember call the method in application:didFinishLaunchingWithOptions:
{% highlight objc %}
- (void)switchLocalizedLanguage {
    NSArray *langArr1 = [[NSUserDefaults standardUserDefaults] valueForKey:@"AppleLanguages"];
    NSString *language1 = langArr1.firstObject;
    NSLog(@"Before switch：%@", language1);

    NSArray *lans = @[@"zh-Hans"];
    [[NSUserDefaults standardUserDefaults] setObject:lans forKey:@"AppleLanguages"];

    NSArray *langArr2 = [[NSUserDefaults standardUserDefaults] valueForKey:@"AppleLanguages"];
    NSString *language2 = langArr2.firstObject;
    NSLog(@"After switch：%@", language2);
}
{% endhighlight %}

Refer to: <http://www.jianshu.com/p/88c1b65e3ddb>
