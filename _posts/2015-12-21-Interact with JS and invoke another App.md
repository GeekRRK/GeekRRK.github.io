# Interact with JS and invoke another App
## The html which UIWebView will jump into
```html
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
    <a href="Protocol://param">Click here, invoke method of OC.</a>
    <br/>
    <br/>
    <a href="http://m.baidu.com">Inject JS into the html of baidu.</a>
</body>
</html>
```

## 1. Invoke JS in OC
```objective-c
- (void)webViewDidFinishLoad:(UIWebView *)webView {
	//1.Invoke the document object.
  //We can preview 'document.title' in the console of browser.
	NSLog(@"%@", [self.webView
  stringByEvaluatingJavaScriptFromString:@"document.title"]);

	//2.Invoke JS
	[self.webView stringByEvaluatingJavaScriptFromString:@"clickme()"];
}
```
## 2. Invoke OC in JS
```objective-c
//YES if the web view should begin loading content; otherwise, NO,
//will invoke JS.
- (BOOL)webView:(UIWebView *)webView
shouldStartLoadWithRequest:(NSURLRequest *)request
            navigationType:(UIWebViewNavigationType)navigationType {
	NSLog(@"%@", request.URL.absoluteString);
	NSString *urlStr = request.URL.absoluteString;

	if ([urlStr hasPrefix:@"Protocol://"]) {
		NSString *urlContent = [urlStr substringFromIndex:[@"Protocol://" length]];
		NSLog(@"%@", urlContent);

		NSArray *urls = [urlContent componentsSeparatedByString:@"/"];
		NSLog(@"Components:%@", urls);

		if (urls.count != 2) {
			return NO;
		}
		NSString *funName = [NSString stringWithFormat:@"%@:", urls[0]];

		SEL callFun = NSSelectorFromString(funName);
# pragma clang diagnostic push
# pragma clang diagnostic ignored "-Warc-performSelector-leaks"
	  [self performSelector:callFun withObject:urls[1]];
# pragma clang diagnostic pop
		NSLog(@"MethodName:%@, Param:%@", funName, urls[1]);

		return NO;
	}

	return YES;
}

- (void)loadUrl:(NSString *)urlStr {
	NSLog(@"Param: %@", urlStr);

	NSURL *url = [NSURL URLWithString:[NSString
  stringWithFormat:@"http://%@", urlStr]];
	NSURLRequest *request = [NSURLRequest requestWithURL:url];

	[self.webView loadRequest:request];
}
```
## 3. Inject JS
```objective-c
- (void)jsClick {
	[self.webView
   stringByEvaluatingJavaScriptFromString:
   @"var script = document.createElement('script');"
	 "script.type = 'text/javascript';"
	 "script.text = \"function myFunction() { "
	 "var field = document.getElementsByName('word')[0];"
	 "field.value='WWDC2014';"
	 "document.forms[0].submit();"
	 "}\";"
	 "document.getElementsByTagName('head')[0].appendChild(script);"];

	[self.webView stringByEvaluatingJavaScriptFromString:@"myFunction();"];
}
```

**Original website is:** http://blog.csdn.net/xn4545945/article/details/36487407
