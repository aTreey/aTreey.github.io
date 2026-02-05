---
title: iOS 弹框视图
date: 2016-10-27 00:58:20
tags: AlertView
---

**`UIActionSheet` `UIAlertView` `UIAlertViewController` 的区别**

## UIActionSheet

- iOS 8.3 之后过期
- 模态弹出，显示在试图的底部 类似菜单提供选择

```
UIActionSheet *actionSheet = [[UIActionSheet alloc] initWithTitle:@"title" delegate:self cancelButtonTitle:@"取消" destructiveButtonTitle:nil otherButtonTitles:@"按钮1", @"按钮2", nil];
[actionSheet showInView:self.view];
```


## UIAlertView

- iOS 9.0 之后过期
- 模态弹出，显示在试图的中间位置 类似一个对话框提供选择

```
UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"考勤方式" message:nil delegate:self cancelButtonTitle:@"取消" otherButtonTitles:faceBtn, fingerPrintBtn, nil];
[alertView show];
```

## UIAlertViewController

- iOS 8.0 起可以使用
- `style` : 默认样式, 类似 `UIActionSheet` 列表形式 
- `style` 为：`UIAlertControllerStyleAlert` 对话框形式
- 设置按钮的样式

如果修改 UIAlertAction 中stye 为：

UIAlertActionStyleCancel
取消按钮会和其他按钮分开

如果修改 UIAlertAction 中stye 为：

UIAlertActionStyleDestructive
取消按钮会以红色警告的方式显示， 但是按钮之间不会分开

```
UIAlertController *alert = [UIAlertController alertControllerWithTitle:@"考勤方式" message:nil preferredStyle:UIAlertControllerStyleActionSheet];
UIAlertAction *faceBtn = [UIAlertAction actionWithTitle:@"人脸识别" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {	}];

UIAlertAction *fingerPrintBtn = [UIAlertAction actionWithTitle:@"指纹认证" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {

}];

UIAlertAction *cancelBtn = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {

}];

[alert addAction:faceBtn];
[alert addAction:fingerPrintBtn];
[alert addAction:cancelBtn];
[self presentViewController:alert animated:YES completion:nil];

```