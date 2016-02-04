---
layout: post
title: Interaction between OC and JS
---

**Remember url ignores case**

### 0. The html page

{% highlight html %}
<html>
<head>
    <meta xmlns="http://www.w3.org/1999/xhtml" http-equiv="Content-Type"
    content="text/html; charset=utf-8" />
    <title>The html which UIWebView will jump into</title>
    <script Type='text/javascript'>
        function clickme() {
            alert('Click me');
        }
    </script>
</head>
<body>
    <h1>Interact between OC and JS</h1>
    <!-- Customize the protocol that calling OC -->
    <a href="protocol://ocmethod/geekrrk.github.io">Click here,
    invoke method of OC.</a>
    <br/>
    <br/>
    <a href="http://m.baidu.com">Inject JS into the html of baidu.</a>
</body>
</html>
{% endhighlight %}

### 1. Setup index.html

{% highlight objc %}
- (void)viewDidLoad {
    [super viewDidLoad];

    NSString *url = @"<html>\
    <head>\
    <meta xmlns=\"http://www.w3.org/1999/xhtml\" http-equiv=\"Content-Type\"\
    content=\"text/html; charset=utf-8\" />\
    <title>The html which UIWebView will jump into</title>\
    <script Type='text/javascript'>\
    function clickme() {\
        alert('Click me');\
    }\
    </script>\
    </head>\
    <body>\
    <h1>Interact between OC and JS</h1>\
    <!-- Customize the protocol that calling OC -->\
    <a href=\"protocol://ocmethod/geekrrk.github.io\">Click here, \
    invoke method of OC.</a>\
    <br/>\
    <br/>\
    <a href=\"http://m.baidu.com\">Inject JS into the html of baidu.</a>\
    </body>\
    </html>";

    [self.webView loadHTMLString:url baseURL:nil];

    self.webView.delegate = self;
}
{% endhighlight %}

### 2. Invoke JS from OC or inject JS into html

{% highlight objective-c %}
- (void)injectJSintoBaidu {
	[self.webView
   stringByEvaluatingJavaScriptFromString:
   @"var script = document.createElement('script');"
	 "script.type = 'text/javascript';"
	 "script.text = \"function myFunction() { "
	 "var field = document.getElementsByName('word')[0];"
	 "field.value='WWDC2015';"
	 "document.forms[0].submit();"
	 "}\";"
	 "document.getElementsByTagName('head')[0].appendChild(script);"];

	[self.webView stringByEvaluatingJavaScriptFromString:@"myFunction();"];
}

- (void)webViewDidFinishLoad:(UIWebView *)webView {
    //Invoke JS
    [self.webView stringByEvaluatingJavaScriptFromString:@"clickme()"];

    //Inject JS into Baidu
    [self injectJSintoBaidu];
}
{% endhighlight %}

### 3. Invoke OC from html（Response the invocation from hyperlinks）

{% highlight objective-c %}
//YES if the web view should begin loading content; otherwise, NO.
- (BOOL)webView:(UIWebView *)webView
shouldStartLoadWithRequest:(NSURLRequest *)request
            navigationType:(UIWebViewNavigationType)navigationType {
	NSString *urlStr = request.URL.absoluteString;

	if ([urlStr hasPrefix:@"protocol://"]) {
		NSString *urlContent = [urlStr substringFromIndex:[@"protocol://" length]];
		NSArray *urls = [urlContent componentsSeparatedByString:@"/"];

		if (urls.count != 2) {
			return NO;
		}

		NSString *funName = [NSString stringWithFormat:@"%@:", urls[0]];
		SEL callFun = NSSelectorFromString(funName);
# pragma clang diagnostic push
# pragma clang diagnostic ignored "-Warc-performSelector-leaks"
	  [self performSelector:callFun withObject:urls[1]];
# pragma clang diagnostic pop
		return NO;
	}

	return YES;
}

- (void)ocmethod:(NSString *)urlStr {
	NSURL *url = [NSURL URLWithString:[NSString
  stringWithFormat:@"http://%@", urlStr]];
	NSURLRequest *request = [NSURLRequest requestWithURL:url];

	[self.webView loadRequest:request];
}
{% endhighlight %}

Refer to: <http://blog.csdn.net/xn4545945/article/details/36487407>
