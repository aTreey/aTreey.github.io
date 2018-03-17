---
title: git 使用
date: 2016-11-20 11:10:31
tags: git
---



## 使用 git 提交代码

- 初始化工作目录
	
	```
	git init
	```

- 配置用户名和邮箱（当前仓库）

	```
	git config user.name "aTreey"
	git config user.email "480814177@qq.com"
	```
	
- 配置全局的用户名和密码

	```
	git config --global user.name "aTreey"
	git config --global user.email "480814177@qq.com"
	```
- 创建文件

	```
	touch main.m
	```

- 添加文件到暂缓区

	```
	git add main.m
	```
- 查看文件状态
	
	```
	git status
	```
- 新添加的文件或者新修改的文件在工作区中，没有添加到暂缓区中

	
	```
	Untracked files: （红色）
	```
- 需要添加到暂缓区中

	
	```
	git add main.m
	```
- 出现一下提示方可提交


	```
	Changes to be committed: （绿色）
	```

- 给git起别名

	```
	git config alias.st "status"
	git config alias.ci "commit -m"
	```

- 查看所有版本库日志（只能查看当前版本的以前日志）

	```
	git log
	```
- 查看指定文件的版本库日志

	```
	git log 文件名
	```


- 查看所有的操作的日志
	
	```
	git reflog
	```
 
## 版本会退
- 强制回退到当前版本

	```
	git reset --hard HEAD
	```

- 强制回退到当前版本的上一个版本

	```
	git reset --hard HEAD^
	```


- 强制回退到当前版本的上上个版本

	```
	git reset --hard HEAD^^
	```

- 强制回退到当前版本的前100个版本
	
	```
	git reset --hard HEAD～100
	```

- 强制回退指定版本

	```
	git reset --hard HEAD 版本号前七位
	```