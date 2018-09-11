swift类型检查和类型转换

### is 关键字类型检查

判断父类和子类之间的关系

```
func typeCheck() {
    let programmer = Programmer(name: "老张-php")
    if dev is iOSDeveloper {
        print("iOS 开发者")
    } else {
        print("其他开发者")
    }
    
    if programmer is Coder {
        print("老张-php是一农个码农")
    } else {
        print("")
    }
}
```

### as 关键字类型转换

```
let ios = iosDev as? Programmer
let ios2 = iosDev as! Coder
```

### 判断是否遵守了协议

swift 中协议可以当作是一个类型使用

```
et ios = iosDev as? Programmer
    let ios2 = iosDev as Coder
    
    // 是否遵守了 Person 协议
    if iosDev is Person {
        print("遵守了Person协议")
    }
    
    let devs = [dev, iosDev, programmer] as [Any]
    
    for dev in devs {
        if let dev = dev as? Person {
            print(dev)
            print("遵守Person协议")
        } else {
            print("未遵守Person协议")
        }
    }
```

### AnyObject Any


```
var anyObjectArray: [AnyObject] = [CGFloat(0.5) as AnyObject,
                             			1 as AnyObject,
                             			"string" as AnyObject,
                         				iOSODev]

```

swift是面向函数编程， 函数是一等公民，数组中可以存入一个函数，函数表达的是一个过程，不是一个名词或者物体，所以函数不是一个对象,需要使用 any 关键字

```
var anyArray: [Any] = [CGFloat(0.5),
							1,
							"string", 
							iOSODev]
```

放入函数

```
anyArray.append({ (a: Int) -> (Int) in return a * a })
```


