---
layout: post
title: Regular Expression
---

 "^one"  
 "a dog$"  
 "^apple$"  
 "banana"  
 "ab*"  
 "ab+"  
 "ab?"  
 "a?b+$"  
 "ab{4}"  
 "ab{1,}"  
 "ab{3,4}"  
 "a|b"  
 "(a|bcd)ef"  
 "(a|b)c*"  
 "[ab]"  
 "[a-d]"  
 "^[a-zA-Z]"  
 "[0-9]a"  
 "[a-zA-Z0-9]$"  
 "a.[a-z]"  
 "^.{5}$"  
 "(.)\1"  
 "10\{1,2\}"  
 "0\{3,\}"  
 "@[^a-zA-Z]4@"  
 "\d"  
 "\D"  
 "\w"  
 "\W"  

 @"^\\d\+$"  

{% highlight objc %}
 - (BOOL)validateNumber:(NSString *)textString {
     NSString *number = @"^[0-9]+$";
     NSPredicate *numberPre = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", number];
     return [numberPre evaluateWithObject:textString];
 }

 - (void)viewDidLoad {
     [super viewDidLoad];

     NSString *searchText = @"you want to match";
     
     NSString *searchText = @"rangeOfString";
     NSRange range = [searchText rangeOfString:@"^[0-9]+$" options:NSRegularExpressionSearch];
     if (range.location != NSNotFound) {
        NSLog(@"range ：%@", [searchText substringWithRange:range]);
     }

     NSError *error = NULL;
     NSRegularExpression *regex = [NSRegularExpression regularExpressionWithPattern:@"^[0-9]+$" options:NSRegularExpressionCaseInsensitive error:&error];
     NSTextCheckingResult *result = [regex firstMatchInString:searchText options:0 range:NSMakeRange(0, [searchText length])];
     if (result) {
         NSLog(@"%@", [searchText substringWithRange:result.range]);
     }
 }
 {% endhighlight %}

Refer to: <http://www.admin10000.com/document/5944.html>  
Learn more: <http://www.cocoachina.com/programmer/20160513/16243.html>
