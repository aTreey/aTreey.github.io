---
title: Swift3.0学习(三)
date: 2017-03-08 23:29:43
tags: swift
---

面向对象–必选属性

构造函数时是分段构造，先构造子类，给’必选属性’设置初始值，然后再构造父类，
定义全局变量（属性）时，需要在构造函数中设置初始化变量值
super.init()是一个类构造结束
重写： override 函数重写（覆盖），如果在重写的方法中 没有super, 父类相关的一些操作就不会被执行
重载：函数名相同, 参数的类型 或者参数的个数不同 就形成了函数的重载
注意：重载构造函数,并且父类默认的构造函数 init()不重写， 父类默认的构造函数就不能够被访问,不能够确保必选属性设置初始值，只能调用重载的构造函数
面向对象–可选属性

使用KVC给对象设置值
注意: 在swift中使用KVC 基本数据类型不能声明为可选项,必须设置为必选项给定初始值

KVC实现的流程
遍历字典的键值 给对象发送 setValue: forKey消息
如果key 对象的属性不存在就将消息转发给 setVale: forUndefinedKey:
如果存在就直接设置值
注意:setVale: forUndefinedKey: 默认抛出异常 不能够super，如果super 相当于没有实现此方法

面向对象–便利构造函数

作用：方便快捷的创建对象
场景：
可检查参数是否正确来实例化
实例化控件
特点
必须以self来调用构造函数
指定的构造函数不能被重写，也不能被super调用
便利构造函数可被子类继承
可以构造失败，返回nil
面向对象 – 描述信息

重写 description 方法，返回值是string类型
使用kvc讲对象属性转换为字典
将字典转化为string类型

	override var description: String {
	        let keys = ["name","age","number","sex"]
	        
	        // 对象转为字典
	        let dict = self.dictionaryWithValuesForKeys(keys)
	        
	        // 每个对象都有描述属性
	        return dict.description
	    }
	    
面向对象 – 存储属性

面向对象 – 计算属性(readOnly)

只有getter，没有setter
只能取值不能赋值
每次调用都会被执行，消耗cpu
不占内存空间，依赖其他的属性来计算
面向对象 – didset 属性设置监察器

能获取 oldValue 和 newValue 的值
通常用来重写setter方法，实现视图绑定模型
在一个属性的didset中计算其他属性

缺点：消耗内存，被计算的属性占内存
优点：减少了cpu消耗
懒加载

