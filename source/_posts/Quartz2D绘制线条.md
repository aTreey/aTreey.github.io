---
title: WKWebiView 使用
date: 2016-11-22 11:10:31
tags: WKWebView
---


#### WKWebiView iOS8.0 出现，性能要远远好于 iOS 2.0时出现的 UIWebView,主要记录WKWebView 的创建和代理方法


## 创建WKWebView

	WKWebView *webView = [[WKWebView alloc] initWithFrame:self.view.bounds];
	self.webView = webView;
	[self.view addSubview:webView];
	webView.UIDelegate = self;
	webView.navigationDelegate = self;
	[webView loadRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:@"https://www.baidu.com"]]];
	
遵守协议 WKNavgationDelegate WKUIDelegate

## 代理方法

- WKNavgationDelegate 方法
	
	- 发送请求之前决定是否跳转

	```
	- (void)webView:(WKWebView *)webView decidePolicyForNavigationAction:(WKNavigationAction *)navigationAction decisionHandler:(void (^)(WKNavigationActionPolicy))decisionHandler {
	
		decisionHandler(WKNavigationActionPolicyAllow);
		// 不允许跳转
	   decisionHandler(WKNavigationActionPolicyCancel);
		NSLog(@"decidePolicyForNavigationAction");
	}
	
	```
	- 收到相应之后决定是否跳转

	```
	- (void)webView:(WKWebView *)webView decidePolicyForNavigationResponse:(WKNavigationResponse *)navigationResponse decisionHandler:(void (^)(WKNavigationResponsePolicy))decisionHandler {
	
		decisionHandler(WKNavigationResponsePolicyAllow);
		
		decisionHandler(WKNavigationResponsePolicyCancel);
		
		NSLog(@"decidePolicyForNavigationResponse");
	
	}
	```

	- 开始加载时调用

	```
	- (void)webView:(WKWebView *)webView didStartProvisionalNavigation:(null_unspecified WKNavigation *)navigation {
	
	NSLog(@"didStartProvisionalNavigation");
	}
	```

	- 接受到服务器的跳转请求时调用

	```
	- (void)webView:(WKWebView *)webView didReceiveServerRedirectForProvisionalNavigation:(null_unspecified WKNavigation *)navigation {
		NSLog(@"didReceiveServerRedirectForProvisionalNavigation");
	}
	```
	
	

	- 页面加载失败时调用 **存在缓存问题**

	```
	- (void)webView:(WKWebView *)webView didFailProvisionalNavigation:(null_unspecified WKNavigation *)navigation withError:(NSError *)error {
			
		NSLog(@"error= %@", error);
	}
	```

	
	- 内容开始返回时调用
	
	```
	- (void)webView:(WKWebView *)webView didCommitNavigation:(null_unspecified WKNavigation *)navigation {
	
		NSLog(@"didCommitNavigation");
	}
	```

 - 内容返回成功后调用

	```
	- (void)webView:(WKWebView *)webView didFinishNavigation:(null_unspecified WKNavigation *)navigation {
	
		NSLog(@"didFinishNavigation");
	}
	
	```
 - 开始返回错误时调用
	
	```
	- (void)webView:(WKWebView *)webView didFailNavigation:(null_unspecified WKNavigation *)navigation withError:(NSError *)error {
	
		NSLog(@"didFailNavigation");
	}
	```
	
 - 目前不知道什么时候调用

	```
	- (void)webView:(WKWebView *)webView didReceiveAuthenticationChallenge:(NSURLAuthenticationChallenge *)challenge completionHandler:(void (^)(NSURLSessionAuthChallengeDisposition disposition, NSURLCredential * _Nullable credential))completionHandler {
		NSLog(@"didReceiveAuthenticationChallenge");
	}
	```
	- 目前不知道什么时候调用

	```
	- (void)webViewWebContentProcessDidTerminate:(WKWebView *)webView API_AVAILABLE(macosx(10.11), ios(9.0)) {
	
		NSLog(@"webViewWebContentProcessDidTerminate");
	}
	```
