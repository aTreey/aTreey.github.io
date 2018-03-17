---
title: OC和JS交互
date: 2017-02-20 20:10:41
tags: OC, JS
---

### WebView相关属性

autoResizingMask 是UIView的属性,作用是实现子控件相对于父控件的自动布局

```
autoResizingMask 是UIViewAutoresizing 默认是 UIViewAutoresizingNone

UIViewAutoresizingNone = 0,
UIViewAutoresizingFlexibleLeftMargin = 1 << 0,
UIViewAutoresizingFlexibleWidth = 1 << 1,
UIViewAutoresizingFlexibleRightMargin = 1 << 2,
UIViewAutoresizingFlexibleTopMargin = 1 << 3,
UIViewAutoresizingFlexibleHeight = 1 << 4,
UIViewAutoresizingFlexibleBottomMargin = 1 << 5
```
各属性解释：

```
UIViewAutoresizingNone
不会随父视图的改变而改变

UIViewAutoresizingFlexibleLeftMargin

自动调整view与父视图左边距，以保证右边距不变

UIViewAutoresizingFlexibleWidth
自动调整view的宽度，保证左边距和右边距不变

UIViewAutoresizingFlexibleRightMargin
自动调整view与父视图右边距，以保证左边距不变

UIViewAutoresizingFlexibleTopMargin
自动调整view与父视图上边距，以保证下边距不变

UIViewAutoresizingFlexibleHeight
自动调整view的高度，以保证上边距和下边距不变

UIViewAutoresizingFlexibleBottomMargin
自动调整view与父视图的下边距，以保证上边距不变
```
### OC 调用js

`webViewDidFinishLoad:(UIWebView *)webView `方法中

- 删除增加结点
- 添加点击事件

### js调用OC


`webView:(UIWebview )webView shouldStartLoadWithRequest:(NSURLRequest )request `方法中

调用js页面点击按钮后保存图片到相册,必须调用系统提供的方法，不能自己自定义方法