---
title: Swift3.0学习(二)
date: 2017-03-05 23:36:21
tags: swift 
---


字符串

字符串遍历
字符串字节长度
字符串长度

let string = "我是switf,语言"
   
   // 字符串遍历
   for s in string.characters {
       print(s)
   }
   
   // 字符串的长度
   let length = string.characters.count;
   print(length)
   
   // 字符串字节长度 汉字对应3个字节，英文是1个字节
   let characterLength = string.lengthOfBytesUsingEncoding(NSUTF8StringEncoding)
   print(characterLength)

字符串拼接

	let str1 = "你好"
	   let str2 = "不好"
	   let a = 20
	   
	   // 第一种拼接
	   print("\(str1)\(str2)\(a)")
	   
	   // 第二种拼接
	   let str = str1 + str2
	   let strA = str1 + String(a)
	   print(str)
	   print(strA)
	   
	   
截取子串 as 的使用

	let subString = str1.substringToIndex("小程生生世世".endIndex);
	   print(subString)
	   
	   let subStr = str1.substringFromIndex("听说".startIndex)
	   print(subStr)
	   
	   let range = str1.rangeOfString("小程序")
	   let subStr1 = str1.substringWithRange(range!)
	   
	   print(subStr1)
	   
	   // 截取到某一个位置
	   let subStr2 = str1.substringFromIndex(str1.endIndex.advancedBy(-2))
	   print(subStr2)
	       
	   // 限制截取几个字符
	   let subStr3 = str1.substringToIndex(str1.startIndex.advancedBy(5, limit: str1.characters.endIndex))
	   print(subStr3)
	String 和 NSString 之间的 使用 as 无缝转换
	let helloString = "我们一起飞"
	(helloString as NSString).substringWithRange(NSMakeRange(2, 3))
	数组

可以放不同类型的元素
let不可变数组,var可变数组

	// 初始化一个空数组 [String]声明是一个装有字符串的数组类型
	   var emptyArray: [String] = [String]()
	   
	// 可变数组
	   var array = ["哈哈","呵呵","嘿嘿"]
	   array.append("咯咯")
	   print("array = \(array)")
	   
	   // 不可变数组
	   let array2 = ["可可","噗噗"]
	   
	   // 数组拼接
	   let arr = array + array2
	   print("arr = \(arr)")
	   
	   array += array2
	   print("array = \(array)")
	   
数组遍历

	// 1. 
	   for i in 0 ..< tempArray.count {
	       print(tempArray[i])
	   }
	   
	   
	   // 2. 
	   for obj in tempArray {
	       print("第二种方法:\(obj)")
	   }
	   
	   
	   // 3. 快速便利
	   for (index,value) in tempArray.enumerate() {
	       print("index = \(index), value = \(value)")
	   }
	   
字典和OC类似

函数调用

必须在函数声明的下面调用内部函数

可以定义函数的外部参数和内部参数

可以在参数前面加上 ‘# ’ 表明函数的外部参数和内部参数名称一样 swift3.0 以后废弃

无参无返回值
三种写法

	func demo9() -> Void {
	       print("无参无返回值第一种写法")
	   }
	   
	   
	   func demo_9() -> () {
	       print("无参无返回值第二种写法")
	   }
	   
	   func demo_99() {
	       print("无参无返回值第三种写法")
	   }
有参有返回值

	func demo10(width a: Int, height b: Int) -> Int {
       return a * b
   }
   
闭包

和OC中block类似，
和在函数中调用一个内部函数原理相同
可当作参数传递
在需要时执行闭包实现回调

	// 定义闭包
	let closure = {() -> () in
	       print("闭包实现")
	   }
	   
	   // 执行闭包
	   closure()

注意循环引用

只有两个对象相互强引用或者三个对象相互强引用形成闭环时才会形成循环引用

weak 和 unsafe_unretained的区别

__weak iOS5.0 推出，当对象被系统回收时，对象的地址会自动指向nil

____unsafe_unretained iOS4.0 推出，当对象被系统回收时，对象的地址不会自动指向nil,会造成野指针访问
解决办法：(官方推荐)weak - strong -dance 解决循环引用，AFNetworking中

尾随闭包

函数的最后一个参数时闭包的时候，函数的参数 ‘()’可以提前关闭，闭包写在 ‘（）’后面，当作尾随闭包来使用



swift 和 OC 区别

swift 和 OC语法的快速的对比

XXX.init(xxx) ==> XXX.(xxx)
selector 类型 ==> ‘函数名’
对象方法的调用 是 ‘.’ self是可以省略的
枚举 枚举名 + 枚举值的名 => .枚举值的名
常量 和变量

let 声明常量 var 声明变量
变量/常量的类型是自动推到的
不同类型之间不能够直接运算 需要手动转换数据类型
可选项
‘?’ 表示表示可选项 可能有值 可能为 nil
可选项会自动带上 Optional 字样
可选项不能够直接参与运算 需要强制解包
‘!’ 表示强制解包 获取可选项中具体的值
‘??’ 快速判断可选项是否为nil 如果为 nil 就去取 ‘??’后面的默认值
控制流
if 没有非零即真的概念 必须制定明确的条件
if let 快速赋值 并且判断赋值对象是否为 nil 如果不为nil 就进入分之执行相关逻辑代码
guard let 作用和 if let 相反 好处: 可以减少一层分支嵌套
where 多重判断 注意 和 && || 不一样的
循环

for in
0..<10
0…10
字符串

更加轻量级 更加高效 是 结构体
支持快速遍历
长度
拼接
截取 as
集合 let 声明不可变的集合 var 声明可变的集合

声明 []
声明一个 空的集合
增删改查
合并
遍历
函数

func 函数名称(外部参数1 内部参数1: 类型, 外部参数2 内部参数2: 类型) -> Int {执行的代码}
函数没有返回值的三种方式
函数内部函数
闭包 () -> () 没有参数没有返回值的闭包类型

提前准备好的一段可以执行代码块
可以当做参数传递
在需要的时候执行闭包产生回调的效果
在闭包中使用self 需要考虑循环引用
函数是一种特殊的闭包



block 的循环引用的解除
__weak 和 weak关键字作用类似 当属性对象被回收是 会自动指向nil
___unsafeunretained 和 assgin关键字作用类似 当属性对象被回收是 不会自动指向nil, 会造成野指针访问 weak -strong - dance