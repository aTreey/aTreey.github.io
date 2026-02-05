---
title: Hexo个人博客搭建
date: 2016-11-05 22:15:50
tags: Hexo
---

## 安装Homebrew

- 参考官网教程  <http://brew.sh/index_zh-cn.html>

- 在终端中输入,如果失败需要用sudo

```
 /usr/bin/ruby -e " (curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 安装git
```
 sudo brew install git
```
## 安装node.js

- [下载安装] (https://nodejs.org/en/download) 


## 安装hexo

- 需要输入密码： sudo 管理员权限 -g 全局安装

```
 sudo npm install -g hexo
```

## 初始化 执行 hexo init 命令

- 指定安装文件 blog

```
  hexo init blog
```

- cd 到 文件夹 blog 下，安装 npm

```
   npm install
```
- 开启服务 

```
  hexo s
```

- 复制终端中的 http://localhost:4000/ 到浏览器中打开

## 关联 github

- 添加 ssh key 到github

- 设置 Mac 显示隐藏文件

  - 在终端执行一下命令后，重启Finder

	```
	defaults write com.apple.finder AppleShowAllFiles -bool true
	```

- 检查 SSH keys 是否存在 github

 - 打开 Finder 前往（command + shit + G）

	```
	~/.ssh/id_rsa.pub
	```

- 如果有文件说明存在 SSH key， 打开 id_rsa.pub 文件，复制里面的信息
- 进入 Github –> Settings –> SSH keys –> add SSH key
任意添加一个title 粘贴内容到key 里，点击Add key 绿色按钮即可。


## 创建名字为 用户名.github.io 的仓库

自行百度。。。。

## 配置 hexo

- vim 打开 _config.yml修改 deploy 模块

```
  vim _config.yml
```
```
deploy:
type: git
repository: https://github.com/用户名/用户.github.io.git
branch: master
```
**冒号后需要加空格**

## 生成 hexo 的静态页面
```
  hexo generate 后者 hexo g
```

- 报以下错

```
ERROR Local hexo not found in ~/blog
ERROR Try runing: 'npm install hexo --save'
```

- 执行下面代码

```
  npm install hexo --save
```
## 部署 hexo

```
  hexo deploy 或者 hexo d
```

- 报以下错误

```
ERROR Deployer not found: git
```

**解决方法：**

```
npm install hexo-deployer-git --save
hexo d
```

- 配置 Deloyment 前，需要配置自己的身份信息

```
  git config --global user.name "yourname"
  git config --global user.email "youremail"
```

**如果执行完以上命令还是不行，删除blog文件下的 .deploy_git 文件重新 hexo deploy**
