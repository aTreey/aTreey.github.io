---
title: OC中运行时
date: 2016-11-22 11:10:31
tags: RunTime
---


RunTime
对于C语言，函数的调用在编译的时候会决定调用哪个函数，而对于面向对象的语言对于[调用某个对象的方法或函数]就叫「消息传递」。所以在 Java，C++ 这些语言里[调用某个对象的方法或函数]和 Objective-C 里[向某个对象发送消息]在概念上就是一样的事情,

### RunTime 简介

- RunTime 简称运行时，OC中通常叫运行时机制，主要的是消息机制
- 消息机制：在OC中，属于动态调用，在编译的时候并不能决定真正调用哪个函数，只有在真正运行的时候才会根据函数的名称找到对应的函数来调用

### RunTime 作用

- 发送消息：让对象发送消息
- 交换方法
- 动态添加方法
- 动态获取属性和方法
- 字典转模型
- 给分类添加属性

原理：给分类添加一个属性,重写了setter、getter方法，但是没有生成 _成员变量，需要通过运行时关联到分类中， 其本质上就是给这个类添加关联,存值和取值
code

```
self.oldurlPath = urlPath;

- (void)setOldurlPath:(NSString *)oldurlPath {

// RunTime 关联
// 第一个参数：给哪个对象添加关联
// 第二个参数：关联的key，通过这个key获取
// 第三个参数：关联的value
// 第四个参数:关联的策略
objc_setAssociatedObject(self, key, oldurlPath, OBJC_ASSOCIATION_COPY_NONATOMIC);
}

- (NSString *)oldurlPath {
    return objc_getAssociatedObject(self, key);
}

```