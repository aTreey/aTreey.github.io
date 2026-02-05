---
title: CocoaPods制作私有pod
date: 2017-11-29 20:57:56
tags:
---

## CocoaPods原理

**本文主要记录制作自己的pod库过程**

CocoaPods 是自动化工具，主要用来管理一些开源的第三方框架，或者是制作自己的私有库,开源库,也可以使用它讲自己的项目组件化。

具体可以参考：

1.  [细聊 Cocoapods 与 Xcode 工程配置](https://juejin.im/post/58730a25a22b9d0058971144)
2. [你真的会用 CocoaPods 吗?](https://juejin.im/post/59f2c7eaf265da432c2318e5)

## CocoaPods 安装

[安装教程](http://www.pluto-y.com/cocoapods-getting-stared/)


## 创建私有索引仓库
每次使用第三方开源库的时候只需要在 `Podfile` 文件中指定所用的库然后 `Pod install` ，这是因为安装pod的时候 `pod setup` 从[远程仓库](https://github.com/CocoaPods/Specs)（索引库） clone到了本地,此仓库中存放了所有支持pod框架的描述信息(包括每个库的各个版本)

文件目录：

	~/.cocoapods/repos/master
	
- 在 [码云](https://gitee.com/) 或者 [coding]() 上创建一个私有仓库

- 添加私有库到CocoaPods
	
	- 格式 `pod repo add [repoName] (URL)`

	```
	pod repo add PrivateSpec https://gitee.com/aapenga/PrivateSpec.git 
	```	


## 创建本地私有库

 - `cd` 到任意一个文件夹下, 执行 `pod lib create [项目名]`
	
	``` 
 	pod lib create PPExcelView
 	```
 	
 - 将文件放入到生成的 Demo 文件夹 的 `Classes` 文件目录下
 - `cd` 到 `Example` 下(含有 `Podfile` 文件) 执行

 	```
 	pod install
 	```
 或者
 
 	```
 	pod update
 	```
 	
 此时添加的文件出现在 `Dome` Pod 的 `Development Pods` 目录中
 
 - 从本地验证 pod 是否通过
 
 	```
 	pod lib lint
 	```
 	如果有警告，可以使用 `--private` 或者 `--allow-warnings` 忽略
	
	```
	pod lib lint --allow-warnings
	pod lib lint --private
	```
	**出现 	`PPExcelView passed validation.` 为通过**
 	
 - 从远程仓库验证 pod 

 	```
 	pod spec lint
 	```
 	**出现 	`PPExcelView.podspec passed validation` 为通过**
 	
## 推送 `.podspec` 文件到私有仓库

`cd` 到 `.podspec` 所在的文件下

	pod repo push MyGitSpec PPExcelView.podspec
	
	
 	
 	