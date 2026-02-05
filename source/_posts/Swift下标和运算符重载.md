Swift下标和运算符重载

### 字典数组下标
	
	var array = [1,2,3,4,5,6]
	let temp = array[0] 	// 通过下标访问
	let temp2 = array[2]	// 通过下标访问

### 结构体下标 

- 对于结构体不是直接使用下标访问，会直接报错
 直接使用下标获取属性值报错： `Type 'Vector_3' has no subscript members`

- 可以通过实现 `subscript(index: Int) -> Double?`方法让结构体支持下标访问，可以理解为一个特殊的函数，需要传入参数和返回值
- 可以增加第二种下标--根据坐标轴获取属性值
- 重写 subscript 的 set 方法 可以使用下标修改属性值

```
struct Vector_3 {
    var x = 0.0
    var y = 0.0
    var z = 0.0
    
    // 根据索引获取
    subscript(index: Int) -> Double? {
        
        // 重写了get方法 实现通过下标获取属性值
        get {
            switch index {
            case 0: return x
            case 1: return y
            case 2: return z
            default: return nil
            }
        }
        
        // 重写 set 方法，实现g通过下标设置属性值，使用系统默认的newValue
        set {
            guard let newValue = newValue else {return}
            switch index {
            case 0: x = newValue
            case 1: y = newValue
            case 2: z = newValue
            default: return
            }
        }
    } 
    
    // 增加第二种下标--根据坐标轴获取属性值
    subscript(axis: String) -> Double? {
        
        get {
            switch axis {
            case "X", "x": return x
            case "Y", "y": return y
            case "Z", "z": return z
            default: return nil
            }
        }
        
        set {
            guard let newValue = newValue else {return}
            switch axis {
            case "X", "x": x = newValue
            case "Y", "y": y = newValue
            case "Z", "z": z = newValue
            default: return
            }
        }
    }
}

```    

### 下标访问

结构体使用下标获取值

```
var v = Vector_3(x: 1, y: 2, z: 3) 
print(v[0], v[1], v[2], v[3], v[100]) // Optional(1.0) Optional(2.0) Optional(3.0) nil nil
print(v["x"], v["y"], v["z"], v["i"]) // Optional(1.0) Optional(2.0) Optional(3.0) nil

```

结构体使用下标修改值

```
v[0] = 101
v[1] = 202
v[2] = 303
v[3] = 400
v[100] = 51
print(v[0], v[1], v[2], v[3], v[100]) // Optional(101.0) Optional(202.0) Optional(303.0) nil nil
```
    
```
v["x"] = 100
v["y"] = 200
v["z"] = 300
v["i"] = 50
print(v["x"], v["y"], v["z"], v["i"]) // Optional(100.0) Optional(200.0) Optional(300.0) nil

```

### 多维下标

定义一个关于矩阵的结构体

	struct Matrix {
	    var data: [[Double]]
	    let row: Int
	    let column: Int
	    
	    init(row: Int, column: Int) {
	        self.row = row
	        self.column = column
	        data = [[Double]]()
	        for _ in 0..<row {
	            let aRow = Array(repeating: 1.0, count: column)
	            data.append(aRow)
	        }
	    }
	    
	    // 通过下标 [row,column] 方式访问
	    subscript(row: Int, column: Int) -> Double {
	        get {
	            assert(row >= 0 && row < self.row && column >= 0 && column < self.column, "下标不合法")
	            return data[row][column]
	        }
	        
	        set {
	            assert(row >= 0 && row < self.row && column >= 0 && column < self.column, "下标不合法")
	            data[row][column] = newValue
	        }
	    }
	    
	    // 通过下标 [row][column] 方式访问
	    subscript(row: Int) -> [Double] {
	        get {
	            assert(row >= 0 && row < self.row, "下标不合法")
	            // 直接返回数组，数组本身有下标
	            return data[row]
	        }
	        
	        set {
	            assert(newValue.count == column, "下标不合法")
	            data[row] = newValue
	        }
	    }
	}

 
### 运算符重载
 
- 重载 + 运算符

```
func +(one: Vector_3, other: Vector_3) -> Vector_3 {
    return Vector_3(x: one[0]! + other[0]!, y: one[1]! + other[1]!, z: one[2]! + other[2]!)
    
//    return Vector_3(x: one.x + other.x, y: one.y + other.y, z: one.z + other.z)
}

```

- 两个参数时相减

```
func -(one: Vector_3, other: Vector_3) -> Vector_3 {
    return Vector_3(x: one.x - other.x, y: one.y - other.y, z: one.z - other.z)
}

```
- 一个参数时去反, 需要 prefix 修饰

```
prefix func -(a: Vector_3) -> Vector_3 {
    return Vector_3(x: -a.x, y: -a.y, z: -a.z)
}
```

- 向量相乘/向量和常量相乘

```
func *(one: Vector_3, other: Vector_3) -> Double {
    return (one.x * other.x) + (one.y * other.y) + (one.z * other.z)
}

```

- 两个参数不能交换，需要重载两次 `*`

```
func *(one: Vector_3, a: Double) -> Vector_3 {
    return Vector_3(x: a * one.x, y: a * one.y, z: a * one.z)
}

func *(a: Double, one: Vector_3) -> Vector_3 {
    return one * a
    
    // 也可采用下面写法
//    return Vector_3(x: a * one.x, y: a * one.y, z: a * one.z)
}

```

- 修改自身参数，不需要返回值

```
func +=(one: inout Vector_3, other: Vector_3) {
    // 已经重载过 + 运算符，可以直接调用
    one = one + other
}

func ==(one: Vector_3, other: Vector_3) -> Bool {
    return one.x == other.x &&
            one.y == other.y &&
            one.z == other.z
}

func !=(one: Vector_3, other: Vector_3) -> Bool {
    return !(one == other)
    
    // 也可采用下面写法
    return one.x != other.x ||
            one.y != other.y ||
            one.z != other.z
}

func <(one: Vector_3, other: Vector_3) -> Bool {
    if one.x != other.x {return one.x < other.x}
    if one.y != other.y {return one.y < other.y}
    if one.z != other.z {return one.z < other.z}
    return false
}

func <=(one: Vector_3, other: Vector_3) -> Bool {
    return one < other || one == other
    
    // 也可采用下面写法
    return one.x > other.x &&
            one.y > other.x &&
            one.z > other.z
}

func >(one: Vector_3, other: Vector_3) -> Bool {
    return (one <= other)
}

```

### 自定义操作符

`postfix` 声明前后缀关键词， `operator ` 操作符关键词

- a+++

```
声明后置操作符
postfix operator +++
postfix func +++(vector: inout Vector_3) -> Vector_3 {
    vector += Vector_3(x: 1.0, y: 1.0, z: 1.0)
    return vector
}

```

- +++a

```
// 声明前置操作符
prefix operator +++
prefix func +++(vector: inout Vector_3) -> Vector_3 {
    let temp = vector
    vector += Vector_3(x: 1.0, y: 1.0, z: 1.0)
    return temp
}

```
