# OC 语言特性

## 分类

### 原理

由运行时来决议的，不同分类当中含有同名方法 谁最终生效取决于谁最终参与编译，最后参编译的同名分类方法会最终生效，假如分类方法中添加的方法和宿主类中的某一个方法名相同，分类中的方法会覆盖宿主类中的同名方法，覆盖是指在消息传递过程中优先查找数组靠前的元素，如果查找到了同名方法就直接调用，实际上宿主类的同名方法实现仍然是存在的，我们可可以通过一些手段调用到原有类的同名方法的实现 

### 使用场景

- 申明私有方法，分类的 .m 文件中申明私有方法啊，对外不暴露
- 对类中的代码进行抽取分解
- 把Framework的私有方法公开化

### 特点
- 运行时决议，运行时通过runtime 才把分类中的方法添加到了宿主类上
- 可以为系统类添加方法
- 有多个分类中存在同名方法时，最后编译的方法会最先生效
- 分类中有和宿主类同名方法时，底层中通过内存copy将分类方法“覆盖”宿主类方法
- 名字相同的分类编译会报错

### 分类中可以添加的内容

- 实例方法
- 类方法
- 协议
- 属性，实际上只是声明了 getter / setter 方法，并没有添加成员变量

### 关联对象技术

关联对象由 AssocicationsManager 管理并在 AssociationsHashMap 上存储
所有对象的关联内容都在同一个全局容器中

- 给分类添加成员变量，通过关联对象的方式


## 扩展

### 使用场景
- 声明私有属性
- 声明私有方法
- 声明私有成员变量

### 特点
- 编译时决议
- 只以声明的形式存在，多数情况下寄生在宿主类的.m中
- 不能为系统添加扩展

## 代理
- 代理设计模式，传递方式是一对一
- 使用weak 避免循环引用

## 通知

使用观察者模式来实现的用于跨层传递消息的机制


## KVO 

KVO 是系统对观察者模式的又一实现，使用isa 混写技术（isa - swizzling）来动态运行时为某一个类添加一个子类重写了它的setter 方法，同时将原有类的isa指针指向了新创建的类上


- KVO 是OC对观察者模式的又一实现
- apple 使用isa 混写技术（isa - swizzling）来实现KVO
	- 给我A类注册一个观察者时本质上是调用系统的 Observer for keyPath 方法，系统会创建一个NSKVONotifiying_A 的类
- 使用 setter 方法设置值KVO才能生效
- 使用setValue:forKey: 改变值KVO才能生效
- 使用下划线的成员变量直接修改值需要手动添加KVO才能生效（增加 willChangeValueForKey 和 didChangeValueForKey 两个方法）



## KVC
apple提供的键值编码技术

- (nullable id)valueForKey:(NSString *)key;
- (void)setValue:(nullable id)value forKey:(NSString *)key;

	以上两个方法中的key是没有任何限制的，只要知道对应的成员变量名称，就可以对私有的成员变量设置和操作，这违背了面向对象的编程思想

**(void)setValue:(nullable id)value forKey:(NSString *)key; 方法调用流程**

- 先判断是否有跟key 相关的setter 方法，如果有就直接调用，结束调用
- 如果没有再判断是否存在实例变量，如果存在直接给赋值，结束调用
- 如果说实例变量不存在，会去调用 `- (void)setValue:(nullable id)value forUndefinedKey:(NSString *)key;` 抛出异常


## 属性关键字

### 读写权限

- readonly

- readwrite 系统默认

### 原子性
- atomic （系统默认）保证赋值和获取（对成员属性的直接获取和赋值）时线程安全，并不代表操作和访问，如果atomic修饰的一个数组，只能保证独具改数组的读取和赋值时线程安全，并不能保证对数据操作（增加/删除数组元素）时是安全的
- nonatomic

### 引用技术

- assign 
	- 修饰基本数据类型，
	- 修饰对象时不改变其引用计数，
	- 会产生悬垂指针
- weak  
	- 不改变被修饰对象的引用计数，
	- 所指对象被释放之后指针自动置为nil

**两者区别：**
- weak可以修饰对象，而assign 既可以修饰对象也可以修饰基本数据类型
- assign 修饰的对象，对象释放后指针仍然指向原对象的内存地址，而weak 修饰的对象，释放后会自动置为nil

**问题1** 

weak 修饰的对象为什么释放后会自动置为nil

**问题2**

`@property (copy) NSMutableArray *array;` 有什么问题

- 被copy修饰后，如果赋值过来的是NSMutableArray，copy之后就是NSArray，如果赋值过来的是NSAarray，copy 之后仍然是NSArray，有可能会调用array的添加元素方法会导致crash

### copy 关键词

 ![](https://ws2.sinaimg.cn/large/006tNc79ly1g1uh6lwwe2j320s0lm7bm.jpg)
 
 - 浅拷贝：内存地址的复制，让目标对象指针和源对象指向同一块内存空间，
 	- 会增加对象的引用计数
 	- 并没有一个新的内存分配

 
 - 深拷贝：目标对象指针和源对象指针分别指向两块内容相同的内存空间
 	- 不会增加源对象的引用计数
 	- 有新对象的内存分配

## MRC 重写 retain修饰的变量的setter 方法

```

- (void)setObj:(id)obj {
    if (_obj != obj) {
        [_obj release];
    }
    _obj = [obj retain];
    
}
```

判断是为了防止异常处理，如果不做 if 判断，当传入的 obj 对象正好是原来的_obj 对象，对原对象尽行releas 操作，实际上也会对传入的对象进行releas 操作进行释放，此时如果再通过obj指针访问废弃的对象时就会导致carsh



