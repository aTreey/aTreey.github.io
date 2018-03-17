---
title: WKWebiView 使用
date: 2017-02-02 22:15:40
tags: WKWebView
---


### iOS中静态库动态库介绍及编译

- 静态库

iOS中根据源码的公开情况可分为2种

- 开源库
源代码公开，能够看到具体的代码实现

比如 SDWebImage AFNetworking。。。

- 闭源库
不公开源代码， 是经过编译后的二进制文件， 看不到具体实现
主要分为：静态库、动态库

- 存在形式

静态库： .a 和 .framework
动态库：.tbd 和 .framework (iOS 9 之前是 .dylib, iOS 9 之后取消了 .dylib 而使用 .tbd 来代替 .dylib )

.framework 的库可以是静态库也可以是静态库， 凡是系统的 .framework 都是动态库，第三方的基本上全是静态中

- 两者区别

静态库：链接时会被完整的复制到可执行文件中，每次被使用都会被复制
动态库：链接时不复制，程序运行时由系统动态加载到内存中，供程序调用，系统只加载一次，多个程序可以公用，节省内存

### 静态库编译

编译时如果使用的是模拟器，生成的 .a 只能模拟器用，真机不可用，生成的 .a 文件在 Debug-iphonesimulator 文件夹下

编译时如果使用 generic iOS Device，生成的 .a 只能真机而模拟器不可用，生成的 .a 文件在 Debug-iphoneos 文件夹下


编译静态库时，自己手动创建的类的头文件不会被导出，只是默认导出创建工程时类的头文件

导出手动添加类的头文件，点击工程名称 –> build Phases –> Copy Files 添加头文件，然后编译



- 架构种类

模拟器：i386 32位架构 (4s~5) ／ x86_64 64位架构 (5s以后机型)
真机：armv7 32位架构 (4~4s) ／ armv7s 特殊架构 (5~5C) ／ arm64 64位架构 (5s 以后机型)

- 架构查看
lipo -info xxxx.a
默认生成模拟器只生成一种架构 i386 x86_64；真机默认导出两种架构 armv7 ／ arm64

- 合并多个架构

将模拟器的多个架构合并，项目名称–>Build Settings –> Achitectures –> Build Active Architecture Only –> Debug设置为NO
合并真机多个架构 按照以上方法运行真机即可
合并真机和模拟器架构 cd products lipo -create Debug-iphoneos/xxx.a Debug-iphonessimulator/xxx.a -output xx.a
合成 5 个架构 在Build Setting –> Architectures 中手动添加三种架构,再编译合并

- 静态库报错

Undefined symbols for architecture （arm64 ／ armv7 ／ armv7s ／ i386 ／ x86_64），架构包导入出错，静态库有真机与模拟器