---
title: Swift 3.0 学习
date: 2017-03-01 22:39:10
tags: Swift
---

## 和OC区别

- 没有了.h 和 .m 文件，没有main.m 文件
- 程序入口发生变化 在appdelegate中的 @UIApplicationMain
- 每行代码可以不写 ‘;’
- self可以不写，建议不写self，因为闭包中必须要写
- 真假表示只有 true／ false，没有非0即真的概念
- 类型自动推到


## 类型自动推到

## 提前指定类型
```
let float :Double = 9.6 
print(float)
```

## 常量和变量

- let 声明常量，有且只有一次赋值机会
- var 声明变量
- 尽量选择使用let如需修改值再使用var 系统会报警告


## 可选项

- Optional: 表示一个常量或者是变量可能有值可能为nil，在打印的时候会带有Optional字样
- ? 表示是一个可选项
- 可选项不能参与运算
- ! 表示可选项中一定要有值，可以强制解包,解包时如果没有值会boom

- ?? 合并空选项 快速判断可选项是否为nil, 如果为nil取后面的默认值，在基本数据类型和字符串使用较多

## 条件分支

- if let

快速赋值，判断赋值对象是否为nil，如果不为nil，就赋值进入分支
生成request是url是一个可选项，需要强制解包或者合并空选项

```
let urlString = "http://www.baidu.com"
let url = NSURL(string: urlString)
let request = NSURLRequest(URL: url!)
print(request)
```

- 以上代码使用if let实现

```
if let u = NSURL(string: urlString) {
    let request = NSURLRequest(URL: u)
    print(request)
}

```
- where条件语句配合多重判断

```
if url != nil  {
    if url?.host == "www.baidu.com" {
        let request = NSURLRequest(URL: url!)
        print(request)
    }
}

```

- 使用if let

```
if let u = url where u.host == "www.baidu.com" {
    let request = NSURLRequest(URL: u)
    print(request)
}
```


- 多重判断

```
if let u = url, s = string {
    let request = NSURLRequest(URL: u)
    print(request)
}

```

- guard let 和 if let 相反如果为nil，可以直接返回

```
let urlString  = "http://www.baidi.com"
let url = NSURL(string: urlString)
guard let u = url else { return }
    
//程序走到这个地方就表示 u 一定有值
let request = NSURLRequest(URL: u)
print(request)
```

- swith语句

	- swift语句中可以case任意类型，
	- 在OC中只能case整数
	- 不需要写break,会自动跳出，
	- 如果需要实现case穿透，需要加入fallthrough语句
	- case语句中临时变量不需要写‘{ }’
	- 可同时case多个值	
	- 每句case语句中至少要有一行可执行的代码

```
let string = "string1"
       switch string {
       case "string", "string1":
           let str = "stringOne"
           print(str)
           
       case "string2":
           let str = "stringTWO"
           print(str)
           
       case "string3":
           print(string)
           
       case "string4":
           print(string)
       default:
           print("empty")
       }

```

- switch 中同样能够赋值和使用 where 子句

```
let point = CGPoint(x: 10, y: 10)
switch point {
case let p where p.x == 0 && p.y == 0:
    print("中心点")
case let p where p.x == 0:
    print("Y轴")
case let p where p.y == 0:
    print("X轴")
case let p where abs(p.x) == abs(p.y):
    print("对角线")
default:
    print("其他")
}
```

- 循环语句

```
for i in 0 ..< 10 { // 不包含10  循环范围中两边的值格式必须一直（空格问题）
       print(i)
   }
       
for _ in 0...5 {// 包含5  _ 表示忽略，不关心，只占位
    print("循环语句")
}

```