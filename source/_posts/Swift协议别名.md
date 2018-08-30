Swift协议和别名

### 协议

某些属性的集合，可以有多个协议组成，具有的特点

 1. 可以声明函数，但是不能实现，由实现这协议的具体结构或者类来实现
 2. 声明的函数如果有参数也不能设置参数的默认值， 默认参数被认为也是函的一种实现
 3. 可以声明属性，必须是计算型属性 只能使用var 修饰，不能是存储型属，必须在 {} 中明确指出属性的读取类型 例如 {get set}
 4. 遵守协议的类可选择存储型属性或者计算型属性来实现协议中定义的属性
 5. protocol Person: class { } 表示此协议只能被类来遵守，结构体能遵守
 6. 协议可以看着是一种类型，如果协议中的属性是只读的，如果是协议类型只能访问get 方法， 但是遵守协议的具体类或者结构可以设置属性的读和写

 7. 协议中的构造函数
 
 		- 如果父类中遵守了协议中的构造函数，子类中必须重写父类中的构造函数并且使用require修饰构造函数，表示继承此类的子类也必须实现该构造函数
 		- 如果class 使用了final 修饰，表示不能被继承，此时不需要 require 关键词修饰

### 类型别名

- 定义别名 `typealias `, 供扩展使用

	```
	typealias Length = Double
	extension Double {
	    var km: Length {return self * 1_000.0}
	    var m: Length {return self * 100.0}
	    var cm: Length {return self / 10.0}
	    /// 英尺
	    var ft: Length { return self / 3.28084}
	}
	
	```
	
	使用
	
	```
	let distance: Length = 10.5.km
	```

 - 定义别名防止硬编码

 	```
 	// 音频采样率
	typealias AudioSample = UInt64
 	
 	```
 	

### 协议中 `typealias`

**协议中定义一个属性，该属性的类型根据不同的情况返回不同的类型（我理解为类似泛型），具体的类型由遵守此协议的类或者结构指定**

- 定义协议中别名

```
protocol WeightCalculable {
    // 属性重量根据不同体积返回的可能是Int，可以可能是double类型，所以使用别名, 协议中使用associatedtype关键词，
    associatedtype WeightType
    var weight: WeightType {  get  }
}

```

扩展 Int 中的吨单位

```
extension Int {
    typealias Weight = Int
    var t: Weight {return self * 1_000}
}

```

- 使用协议中别名


 - 一部手机的时使用该协议`WeightCalculable`

```
class iPhone: WeightCalculable {
    typealias WeightType = Double
    var weight: WeightType {
        return 0.5
    }
}
```

 - 一辆大卡时使用该协议`WeightCalculable`
 
```
class Car: WeightCalculable {
    typealias WeightType = Int
    let weight: WeightType
    
    init(weight: Int) {
        self.weight = weight
    }
}
```

```
let bigCar = Car(weight: 8_000.t) // 8 吨
```
  
  
### 面向协议编程

- 定义比赛协议，具有 赢/输/总场次/胜率 等特性

```
protocol Recordable: CustomStringConvertible {
    var wins: Int { get }
    var losses: Int { get }
    
    func winningPerent() -> Double
}
```

- 协议中方法或者属性的默认实现

```
extension Recordable {
	// CustomStringConvertible 协议默认实现
    var description: String {
        return String(format: "WINS: %d---LOSSES: %d", [wins, losses])
    }
    
    // 默认实现总场数计算型属性
    var totalGames: Int {
        return wins + losses
    }
    
    // 默认实现的方法
    func shoutWins() {
        print("come on", wins, "times!")
    }
}
```


#### 篮球比赛：

- Equatable 协议 - 相等比较 需要重载 `==`
- Comparable 协议 - 是否可比较 ，重载 `<` 可直接调用sort()函数
- CustomStringConvertible 协议 - 打印信息，需重写 `description`
- Recordable协议 - 比赛协议，没有平局 

```
struct BasketballRecord:Equatable, Comparable, CustomStringConvertible, Recordable {
    var wins: Int
    var losses: Int
    
    
    /// 在具体定义的类型中实现 协议中的属性或者方法就会覆盖协议中的
    var totalGames: Int = 200
    
    var description: String {
        return "本次比赛胜: " + String(wins) + "负" + "\(losses)"
    }
    
    // 胜率
    func winningPerent() -> Double {
        return (Double(wins) / Double(totalGames))
    }
    
    // swift4.1 后，不需要再重写此方法，swift 会自动合成，如果不想让某一个属性参与比较，就需要重写该方法
    static func == (lhs: BasketballRecord, rhs: BasketballRecord) -> Bool {
        return lhs.wins == rhs.wins && lhs.losses == rhs.losses
    }
    
    // Comparable
    static func < (lhs: BasketballRecord, rhs: BasketballRecord) -> Bool {
        if lhs.wins != rhs.wins {
            return lhs.wins < rhs.wins
        }
        return lhs.losses > rhs.losses
    }
}
``` 

#### 足球比赛：

有平局的情况，原来的协议不能满足，需要增加一个平局的协议

- 定义平局协议

```
protocol Tieable {
    /// 平局，可读写
    var ties: Int { get set }
    
}
```

- 足球比赛需要遵守的协议 `Recordable` 和 `Tieable`

	- 总场数需要加上平局的场数，需要实现totalGames的get方法
	

```
struct FootableRecord: Recordable, Tieable {
    var wins: Int
    var losses: Int
    // 平局数
    var ties: Int
    
    // 总场数
    var totalGames: Int {
        return wins + losses + ties
    }
    
    var description: String {
        return "本次比赛胜: " + String(wins) + "负" + "\(losses)"
    }
        
    func winningPerent() -> Double {
        return (Double(wins) / Double(totalGames))
    }
}
```

- 足球比赛中以上写法存在的问题
	
	- 所有有平局的比赛计算总场次的逻辑都是一样，每次都要写一次实现
	- 单一修改 Recordable 协议不能解决问题，
	- 存在平局和没有平局两种情况 totalGames 的计算逻辑不相同 

- 解决办法：
	- 扩展遵守了`Recordable`协议的类型，前提条件是：这个类型遵守了 Tieable 同时也遵守了 `Recordable ` 协议，可以理解为如果该类型遵守了 `Tieable `协议之后，`Recordable` 协议中的 totalGames 属性实现是另一种方式了，不在是以前的 `totalGames` 中直接返回 `wins + losses` 的值
	

定义协议

```
protocol Recordable: CustomStringConvertible {
    var wins: Int { get }
    var losses: Int { get }
    
    func winningPerent() -> Double
}
```

协议默认实现

```
extension Recordable {
    var description: String {
        return String(format: "WINS: %d---LOSSES: %d", [wins, losses])
    }
    
    // 默认实现总场数计算型属性
    var totalGames: Int {
        return wins + losses
    }
    
    // 默认实现胜率
    func winningPerent() -> Double {
        return (Double(wins) / Double(totalGames))
    }
    
    // 默认实现的方法
    func shoutWins() {
        print("come on", wins, "times!")
    }
}

```

扩展遵守了Tieable 的 类型的 Recordable 协议

```
extension Recordable where Self: Tieable {
    var totalGames: Int {
        return wins + losses + ties
    }
}

```

### 协议聚合

如果对某一类型需要限制他必须遵守某些协议，可以使用协议聚合来定义

比如： 有个奖赏协议

```
protocol Prizable {
    func isPrizable() -> Bool
}

```

篮球比赛遵守此协议

```
// 篮球比赛奖赏
extension BasketballRecord: Prizable {
    func isPrizable() -> Bool {
        return totalGames > 10 && winningPerent() > 0.5
    }
}

```

足球比赛奖赏遵守此协议

```
extension FootableRecord: Prizable {
    func isPrizable() -> Bool {
        return wins > 1
    }
}
```

现在有某一个学生也遵守了此协议

```
struct Student: Prizable {
    var name: String
    var score: Int
    
    func isPrizable() -> Bool {
        return score >= 60
    }
}

```

定义奖赏的方法，参数类型必须是遵守了此协议的结构或者类型

```
private func award(_ one: Prizable) {
    if one.isPrizable() {
        print(one)
        print("恭喜获得奖励")
    } else {
        print(one)
        print("很遗憾")
    }
}
```

如果说 BasketballRecord 这个类还遵守了其他的协议，例如遵守了 `Recordable` 协议， 并且这个协议也遵守了 `CustomStringConvertible` 并且默认实现了`description` 的`get` 方法

```
// MARK: - 比赛协议，具有 赢。输。总场次。胜率 特性
protocol Recordable: CustomStringConvertible {
    var wins: Int { get }
    var losses: Int { get }
    
    func winningPerent() -> Double
}

```

`Recordable` 默认实现

```

extension Recordable {
    var description: String {
        return String(format: "WINS: %d---LOSSES: %d", [wins, losses])
    }
    
    // 默认实现总场数计算型属性
    var totalGames: Int {
        return wins + losses
    }
    
    // 默认实现胜率
    func winningPerent() -> Double {
        return (Double(wins) / Double(totalGames))
    }
    
    // 默认实现的方法
    func shoutWins() {
        print("come on", wins, "times!")
    }
}
```

此时如果 `Student` 类还是调用 `award` 方法的话，print(one) 打印的信息将是`Recordable `中默认实现的内容，因此需要约束`award`函数的参数必须遵守两个协议让`Student` 也重写自己的`description `属性的`get`方法，不能再让 `Prizable` 扩展 默认实现

- swift 3 写法： protocol<A, B>
- swift 4 写法： A & B

```
private func award2(_ one: Prizable & CustomStringConvertible) {
    if one.isPrizable() {
        print(one)
        print("恭喜获得奖励")
    } else {
        print(one)
        print("很遗憾")
    }
}
```

### 泛型约束


-  定义一个函数，找出一个学生数组中分数最大的
	- 参数：一个学生数组，都遵守了 Comparable 的类型
	- 返回值：某个遵守了 Comparable 的类型实例
   	- 此时函数报错 `Protocol 'Comparable' can only be used as a generic constraint because it has Self or associated type requirements`, 
   	
   	因为 Comparable 协议中定义的方法  public static func < (lhs: Self, rhs: Self) -> Bool 的参数类型是Self，是具体的某个类型

```
func maxScore(seq: [Comparable]) -> Comparable { }
```


- 如果需要定义一个函数实现在一个数组中找出需要奖励的人的名字该如何实现呢
	- 参数：遵守两个协议 Comparable 和 Prizable 协议, 并且使用泛型
	- 返回值：返回值是可选值，有可能没有任何奖励的对象

```
func topPrizable<T: Comparable & Prizable>(seq: [T]) -> T? {
    return seq.reduce(nil) { (tempTop: T?, condender: T) in
        guard condender.isPrizable() else { return tempTop }
        // 解包 condender 失败, 上一层验证了他必须是奖励的那个
        guard let tempTop = tempTop else { return condender }
        return max(tempTop, condender)
    }
}

```

