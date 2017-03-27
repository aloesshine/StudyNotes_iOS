# UIWebView

## 简介

1.什么是UIWebView
- UIWebView是iOS内置的浏览器控件- 系统自带的Safari浏览器就是通过UIWebView实现的- UIWebView不但能加载远程的网页资源，还能加载绝大部分的常见文件	- html\htm	- pdf、doc、ppt、txt	- mp4	- ......2.UIWebView常用的加载资源的方法
	- (void)loadRequest:(NSURLRequest *)request;3.常用属性和方法

```objc
// 重新加载（刷新）- (void)reload;// 停止加载- (void)stopLoading;// 回退- (void)goBack;// 前进- (void)goForward;// 需要进行检测的数据类型(比如电话，网站)@property(nonatomic) UIDataDetectorTypes dataDetectorTypes

// 是否能回退@property(nonatomic,readonly,getter=canGoBack) BOOL canGoBack;// 是否能前进@property(nonatomic,readonly,getter=canGoForward) BOOL canGoForward;// 是否正在加载中@property(nonatomic,readonly,getter=isLoading) BOOL loading;// 是否伸缩内容至适应屏幕当前尺寸@property(nonatomic) BOOL scalesPageToFit;
```

4.UIWebViewDelegate 监听UIWebView的加载过程

```objc
// 开始发送请求（加载数据）时调用这个方法- (void)webViewDidStartLoad:(UIWebView *)webView;// 请求完毕（加载数据完毕）时调用这个方法- (void)webViewDidFinishLoad:(UIWebView *)webView;// 请求错误时调用这个方法- (void)webView:(UIWebView *)webView didFailLoadWithError:(NSError *)error;

// UIWebView在发送请求之前，都会调用这个方法，如果返回NO，代表停止加载请求，返回YES，代表允许加载请求
// 通过这个方法完成JS调用OC- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType;
```

## JavaScript

1.什么是JavaScript
- JavaScript是一门脚本语言，简称JS- JS的常见作用有	- 给HTML网页添加动态功能，比如响应用户的各种操作	- 操纵HTML元素，比如添加、删除、修改网页元素2.常见的JavaScript函数
	alert(10);  // 弹框	document.getElementById(‘test’); // 根据ID获得某个DOM元素3.OC中调用JavaScipt```js[webView stringByEvaluatingJavaScriptFromString:@"alert(100);"];
    
// 利用JS获得当前网页的标题
self.title = [webView stringByEvaluatingJavaScriptFromString:@"document.title;"];
    
NSString *result = [webView stringByEvaluatingJavaScriptFromString:@"login();"];
NSLog(@"%@", result);```

4.JavaScipt中调用OC

- 通过代理方法

	```objc
	-(BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType;
	```
- 第三方框架：WebViewJavaScriptBridge	
## 补充- iOS : Xcode
- Java : eclipse\MyEclipse
- Android : eclipse\MyEclipse\Android Studio
- 网页（前端）：Dreamweaver（美工）、Sublime（前端）、WebStorm（前端）
 
- iOS App == Native + HTML5
 
- 完整的网页组成：
	- HTML：内容（文字、图片）
 	- CSS：美化、样式
 	- JS：动态效果、事件处理、跟用户进行交互
