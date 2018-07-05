Swift 学习

## 枚举

#### 1. 定义 rawValue 为 Int 类型，初始值为1，

```
enum Month: Int {
    case January = 1
    case February
    case March
    case April
    case May
    case June
    case July
    case August
    case September
    case October
    case November
    case December
}

```

#### 2. rawValue 的整型值可以不连续

```
enum Season: Int {
    case Spring = 1
    case Summer = 5
    case Autumn = 10
    case Winter = 40
}
```


#### 3. 枚举可以是字符串,字符串是`变量名`和 `rawValue` 的值相等
```
enum ProgrammerLanguae: String {
    case Swift
    case OC = "Objective-C"
    case C = "语言"
    case RN = "React-Native"
    case Java
}

```

使用`rawValue`
 
```
func residueNewYear(month: Month) -> Int {
    return 12 - month.rawValue
}

```

调用枚举的Moth的构造函数生成一个Month

```
let month = Month(rawValue: 5)
let swift = ProgrammerLanguae.Swift.rawValue
let oc = ProgrammerLanguae.OC
let RN = ProgrammerLanguae.RN.rawValue
```

#### 4. 枚举关联值 associate value
```
enum Status {
    case success(Int)
    case fail(String)
    case null // 未关联值
}

func associateValueTest(isSuccess: Bool) -> Status {
    if isSuccess {
        return .success(200)
    }
    return .fail("失败")
}
```

#### 5. 枚举associateValue多个值

* 本质是关联了一个元祖(value0, value1, value2...)

```
enum Shape {
    case Square(width: Double)
    case Reactangle(width: Double, height: Double)
    case Circle(x: Double, y: Double, radius: Double)
    case Point
}

let square = Shape.Square(width: 20)
let reactangle = Shape.Reactangle(width: 10, height: 20)
let circle = Shape.Circle(x: 10, y: 10, radius: 20)
let point = Shape.Point

private func area(shape: Shape) -> Double {
    switch shape {
    case let .Square(width):
        return width * width
    case let .Reactangle(width, height):
        return width * height
    case let .Circle(_, _, radius):
        return radius * radius * Double.pi
    case .Point:
        return 0
    }
}
```

#### 6. 递归枚举

- 定义一个递归算术表达式
- 使用 indirect 来修饰

```
indirect enum ArithmeticalExpression {
    case Number(Int)
    case Addition(ArithmeticalExpression, ArithmeticalExpression) // + 时两边也是一个表达式
    case Multiplication(ArithmeticalExpression, ArithmeticalExpression) // * 时两边也是一个表达式
    
//    indirect case Addition(ArithmeticalExpression, ArithmeticalExpression) // + 时两边也是一个表达式
//    indirect case Multiplication(ArithmeticalExpression, ArithmeticalExpression) // * 时两边也是一个表达式
}
```

- 递归表达式使用 (2+3) * 4

```
let two = ArithmeticalExpression.Number(2)
let one = ArithmeticalExpression.Number(3)
let sum = ArithmeticalExpression.Addition(two, one)
let indirectEnumResult = ArithmeticalExpression.Multiplication(sum, ArithmeticalExpression.Number(4))
```

- 计算表达式值的函数

```
private func calculate(expression: ArithmeticalExpression) -> Int {
    switch expression {
    case let .Number(value):
        return value
    case let .Addition(left, right):
        return calculate(expression: left) + calculate(expression: right)
    case let .Multiplication(left, right):
        return calculate(expression: left) * calculate(expression: right)
    }
}
```

## 结构体

- 结构体中的属性值没有初始化时必须使用构造函数来初始化，否则报错
- 

```
struct Location {
    var latitude: Double
    var longitude: Double
    var placeName: String?
}

let location1 = Location(latitude: 37.3230, longitude: -122.0322, placeName: "测试")
```

- 结构体中属性如果都赋了初始值就可以直接初始化

```
struct Location2 {
    var latitude: Double = 0
    var longitude: Double = 0
}

let location2 = Location2()

```

- 如果未指定初始值，swift 就不会默认初始化为（不初始化）
- 当属性值为可选值时此时允许值为nil，那么就允许不通过构造函数来初始化赋值
- let 只有一次赋值机会
- 不管是类还是结构体都应该提供一个全参数的构造函数 `init(latitude: Double, longitude: Double, placeName: String?)`

```
struct Location3 {
    let latitude: Double
    let longtitude: Double
    var placeName: String?
    
    init(coordinateString: String) {
        let index = coordinateString.index(of: ",")
        let index1 = coordinateString.index(after: index!)
        let test_1 = coordinateString.prefix(upTo: index!)
        let test_2 = coordinateString.suffix(from: index1)
        
        latitude = Double(test_1)!
        longtitude = Double(test_2)!
    }
    
    init(latitude: Double, longitude: Double, placeName: String?) {
        self.latitude = latitude
        self.longtitude = longitude
        self.placeName = placeName
    }
}

let test3 = Location3(coordinateString: "12,45")
```

## 类


## 属性和方法

#### 计算型属性

#### 类型属性

#### 类型方法

#### 属性管擦器

#### 延迟属性

#### 访问控制

#### 单利模式



## 继承和构造函数

#### swift 中继承

#### 多态性

#### 函数重载

#### 两段式构造

#### 便利构造函数和指定构造函数

#### 构造函数继承

#### required 构造函数