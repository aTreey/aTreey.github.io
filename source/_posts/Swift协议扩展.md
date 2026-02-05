Swift协议扩展

### 定义结构体

```
struct Point {
    var x = 0.0
    var y = 0.0
    
}

struct Size {
    var height = 0.0
    var width = 0.0
}

struct Rectangle {
    var origin = Point()
    var size = Size()
    
    init(origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }
}

```

### 扩展

- 扩展方法

```
extension Rectangle {
    mutating func translate(x: Double, y: Double) {
        self.origin.x += x
        self.origin.y += y
    }
}

```

- 只能扩展计算型的属性，不能扩展存储型属性, 存储型属性需要在定义类或者结构体时声明

```
extension Rectangle {
    var center: Point {
        get {
            let center_x = origin.x + size.width / 2.0
            let center_y = origin.y + size.height / 2.0
            return Point(x: center_x, y: center_y)
        }
        
        set {
            origin.x = newValue.x - size.width / 2.0
            origin.y = newValue.y - size.height / 2.0
        }
    }
}

```

- 扩展构造方法

	- 类中不能扩展指定构造方法，只能在结构体中扩展
	- 结构体中不能扩展便利构造方法m，只能在类中扩展

```

extension Rectangle {
    init(center: Point, size: Size) {
        let origin_x = center.x - size.width / 2.0
        let origin_y = center.y - size.height / 2.0
        self.origin = Point(x: origin_x, y: origin_y)
        self.size = size
    }
    
    // 结构体中不能扩展便利构造函数 Delegating initializers in structs are not marked with 'convenience'
//    convenience init(center: Point, size: Size) {
//        let origin_x = center.x - size.width / 2.0
//        let origin_y = center.y - size.height / 2.0
//        self.origin = Point(x: origin_x, y: origin_y)
//        self.size = size
//    }
}

```

- 扩展嵌套类型

```
extension Rectangle {
    enum Vertex: Int {
        case left_top
        case left_bottom
        case right_bottom
        case right_top
    }
    
    // 获取某一个顶点坐标
    func point(of vertex: Vertex) -> Point {
        switch vertex {
        case .left_top:
            return origin
        case .left_bottom:
            return Point(x: origin.x, y: origin.y + size.height)
        case .right_bottom:
            return Point(x: origin.x + size.width, y: origin.y + size.height)
        case .right_top:
            return Point(x: origin.x + size.width, y: origin.y)
        }
    }
}

```

- 扩展下标,根据传入的索引获取对应顶点坐标

```
extension Rectangle {
    subscript(index: Int) -> Point? {
        assert(0 <= index && index < 4, "传入值非法")
        return point(of: Vertex(rawValue: index)!)
    }
}
```

### 扩展系统方法

```
extension Int {
    /// 平方
    var square: Int {
        return self * self
    }
    
    /// 立方
    var cube: Int {
        return self * self * self
    }
    
    /// 判断数组是否在某一个范围内
    func inRange(clousedLeft left: Int, openRight right: Int) -> Bool {
        return self >= left && self > right
    }
    
    /// 重复执行操作
    func repeatitions(task: () -> ()) {
        for _ in 0..<self {
            task()
        }
    }
    
    /// 变长函数
    func stride(from: Int, to: Int, by: Int, task: () -> ()) {
        // 系统变长函数
        for i in Swift.stride(from: 0, to: 21, by: 3) {
            print(i)
        }
        
        for _ in Swift.stride(from: from, to: to, by: by) {
            task()
        }
    }
}

```
扩展后使用

```
	print(2.square)
	print(3.cube)
	print(4.inRange(clousedLeft: 0, openRight: 4))
	4.repeatitions {
	    print("extension")
	}
	    
	    
	// 使用变长函数
	4.stride(from: 0, to: 8, by: 4) {
	    print("stride")
	}

```
