Swift中内存管理

### 强引用循环和 weak 关键字

房东

```
class Landlord {
    var name: String
    var house: House?
    
    init(name: String) {
        self.name = name
    }
    
    deinit {
        print("Landlord内存释放")
    }
}
```

房子

```
class House {
    var name: String
    var landlord: Landlord?
    
    init(person: String) {
        self.name = person
    }
    
    deinit {
        print("House内存释放")
    }
}

```

循环引用

```
func strongRetain() {
    var landlord: Landlord? = Landlord(name: "老李")
    var house: House? = House(person: "望京府邸")
    // 房东有个房子
    landlord?.house = house
    // 房子有个主人
    house?.landlord = landlord
    
    landlord = nil
    house = nil
}
```

- 虽然 landlord = nil 和 house = nil，但是 Landlord 和 House 内存空间并没有释放 （并未执行 deinit 函数）

- 原因：因为两个内存空间相互引用，引用计数不为0，形成了强循环引用

- 解决办法：使用weak 关键字来修饰两者中其中一个变量

	- 使用weak修饰的变量必须是可选类型，可以赋值为nil
	- 使用weak修饰必须是var 类型变量

```
class House {
    var name: String
    weak var landlord: Landlord?
    
    init(person: String) {
        self.name = person
    }
    
    deinit {
        print("House内存释放")
    }
}
```


### unowned 关键字的使用

#### 相互循环引用的两个变量有一个是可选的

有个身份证的类，每个身份证肯定对应一个人

```
class ID {
    var number: String
    let person: Landlord
    
    init(person: Landlord, number: String) {
        self.person = person
        self.number = number
    }
    
    deinit {
        print("ID内存释放")
    }
}

``` 

房东

```
class Landlord {
    var name: String
    var house: House?
    /// 身份证可以为nil，
    var id: ID?
    
    init(name: String) {
        self.name = name
    }
    
    deinit {
        print("Landlord内存释放")
    }
}

```

循环引用

```
func unownedTest() {
    var person: Landlord? = Landlord(name: "老王")
    var id: ID? = ID(person: person!, number: "10010")
    
    person?.id = id
    person = nil
    id = nil
}

```

-  person 和 id 之间有强引用关系
-  每个id必须有一个对应的人并且是固定的只能使用let 修饰，不能为nil, 而id可以为空 
- 	unowned和 weak 作用相同都是弱引用，只是unowned 修改的变量不能是nil （可选型的）
-  unowned只能使用在 class 类型上，不能修饰函数类型，例如闭包存在循环引用时不能使用unowned修饰
-  解决方法：
	-  此时可以通过使用 weak 修饰 id 的方式，因为 id 可以为nil
	-  let 修饰并且不能为nil 这种情况可以使用 unowned 修饰解决

	```
	class ID {
	    var number: String
	    unowned let person: Landlord
	    
	    init(person: Landlord, number: String) {
	        self.person = person
	        self.number = number
	    }
	    
	    deinit {
	        print("ID内存释放")
	    }
	}
	```
	
**unowned有一定的危险性** 

因为unowned 修饰的对象不能赋值为nil，所以执行下面的代码会crash，

```
func unownedTest() {
        var person: Landlord? = Landlord(name: "老王")
        var id: ID? = ID(person: person!, number: "10010")
        
        person?.id = id
        
        // 提前释放 person 内存
        person = nil
        
        // 获取身份证对应的人
        print(id?.person) // 此处奔溃，
        let owner = id?.person
        id = nil
 }
```

正确的写法因该是先将🆔设置为nil，再将 Person 设置为nil，再调用 print(id?.person) 就不会再报错

```
func unownedTest() {
        var person: Landlord? = Landlord(name: "老王")
        var id: ID? = ID(person: person!, number: "10010")
        
        person?.id = id
        
        id = nil

        // 提前释放 person 内存
        person = nil
        
        // 获取身份证对应的人
        print(id?.person) // 此处奔溃，
        let owner = id?.person
 }
```

#### 相互循环引用的两个变量都不是可选的

使用隐式可选类型

### 闭包中的强引用循环

- 并不是所有的闭包都会产生循环引用，只有相互引用的两个对象会产生循环引用

- 使用swift闭包捕获列表，在闭包的声明参数之前使用 [unowned self], 不需要解包

- 使用 unowned表示这个对象不能为空，可能存在风险，如果对象为空就会crash


使用 unowned 对象不能为空，可能存在风险

```
class Closure {
    var closure: ((Int) -> Void)?
    
    init() {
        closure = { [unowned self] index in
            print(self)
            print(index)
        }
    }
}
```

使用 weak, 此时需要解包

```
closure = { [weak self] index in
            // 解包
            if let `self` = self {
                print(self)
                print(index)
            }
        }
```

强制解包
```
closure = { [weak self] index in
            // 强制解包
            print(self!)
            print(index)
        }
```

