## Swift 泛型

### 泛型定义--`wikipedia`

1.	在程序编码中一些包含类型参数的类型，也就是说泛型的参数只可以代表类，不能代表个别对象。（这是当今较常见的定义）
2. 在程序编码中一些包含参数的类。其参数可以代表类或对象等等。（现在人们大多把这称作模板）

### 泛型的用处

泛型可以实现传入一个指定类型后做一些操作然后返回后还是原来指定的类型，你可以指定特定的类型（特定是指约束）

### 泛型和 `Any` 的区别

区别：`Any`能代表任何类型，但是不能返回某个具体的类型，都需要使用 as 去转化，而泛型正好相反

- Any 可以代表任何类型，除了class之外还可以代表struct，enum，
- `AnyObject` 可以代码任何 `class` 的类型, swift 中基本类型， `Array` 和 `Dictionary` 都是 `struct` 类型，都不能使用 `AnyObject` 来接受

[Swift中Any 和 AnyObject用法](http://swifter.tips/any-anyobject/)

### 泛型函数和泛型参数

`swapTwoValues`是一个泛型函数，可以接收或者返回任何类型

```
func swapTwoValues<T>(inout a: T, inout b: T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

- `swapTwoValues` 函数名后面的 `<T>` 表示这个 `T` 类型是函数所定义的一个类型, `T` 只是一个类型的占位符，不用关心具体类型

- `(inout a: T, inout b: T)` 中a和b两个参数类型`T`是泛型参数

### 类型约束

两个泛型参数的泛型函数

```
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // function body goes here
}
```

- 泛型参数`T`约束条件：必须是`SomeClass`类的子类
- 泛型参数`U`约束条件：必须遵守`SomeProtocol`协议
- 使用where指定约束





