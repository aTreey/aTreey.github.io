---
title: 本地已有项目添加Pod
date: 2017-12-02 00:38:18
tags:
---

上一篇中主要记录了如何使用 `pod` 工具自动创建 `pod` 库并且生成一个测试Demo，这边主要记录给已有的项目添加 `pod`

## 创建 .podsepc 文件

- `cd` 到已有项目的根目录执行命令： `pod spec create [ProjectName].podspec`
	
	```
	pod spec create EmotionKeyboard.podspec
	```
	
## 修改编辑 `.podsepc` 文件
	
	```
	s.name         = "EmotionKeyboard"
  	s.version      = "0.0.1"
  	s.summary      = "A  Emotional Keyboard."
  	
  	# 此处的格式不能修改
  	s.description  = <<-DESC

	Swift Emotional Keyboard
                   DESC

	s.homepage     = "https://github.com/aTreey/EmotionKeyboard"
	
	# 两种写法，使用下面第二种  
	# s.license      = "MIT (example)"
	s.license      = { :type => "MIT", :file => "LICENSE" }
	
	s.author             = { "aTree" => "480814177@qq.com" }
	
	s.platform     = :ios, "8.0"
	
	s.source       = { :git => "https://github.com/aTreey/EmotionKeyboard.git", :tag => "#{s.version}" }
	
	
	# 从 .podspec 同级别的目录下起匹配 
	s.source_files  = "EmotionKeyboard", "EmotionKeyboard/Classes/**/*.{h,m}"
	
	# 设置自己项目中依赖的系统库
	s.framework  = "UIKit"
  	
	``` 
	
## 调整文件的目录结构
	
- 在项目根目录下新建一个放组件代码的文件夹
- 如果项目中包含 `bundle` 文件, 需要在组件文件夹下新建一个 `Assets` 文件夹，用来存放组件中的资源

	最终结果如下:
	
	![](https://ws1.sinaimg.cn/large/a1641e1bly1fm2jl7ykslj21gc0o80xt.jpg))

- 创建并添加必要文件
	
	- `LICENSE` 文件: 开源许可文件, 如果创建项目的时候选择了开源许可就可以自动生成忽略此步骤,如果没有直接拷贝一份其他的工程中的即可
	- `README.md` 文件: 主要用来说明你开源库的信息及使用方法

	
## 验证本地 `.podspec` 文件

- 错误1: `.podspec `中的文件路径设置错误
	
	![](https://ws1.sinaimg.cn/large/a1641e1bly1fm2jlboq82j20v807ejug.jpg)
	
- 错误2: bundle 文件资源路径错误

	![](https://ws1.sinaimg.cn/large/a1641e1bly1fm2jl4shnnj20zq0cwgqf.jpg)
	

- 本地验证结果

 - **EmotionKeyboard passed validation.** 本地验证验证通过

## 网络验证 `.podspec` 文件 
	
- 给 `master` 分支打上 `.podspec` 文件中对应的 `tag` 标签, 提交推送, 
- 执行命令

	```
	pod spec lint
	```	
- 网络验证结果

	```
	EmotionKeyboard.podspec passed validation.
	```
	
	
**本文参考**

[给 Pod 添加资源文件](http://blog.xianqu.org/2015/08/pod-resources/)

[基于 Swift 创建 CocoaPods 完全指南](http://swift.gg/2016/12/15/cocoapods-making-guide/)