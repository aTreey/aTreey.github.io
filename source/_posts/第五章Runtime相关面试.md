# Runtime 相关问题

## 数据结构

### objc_object 结构体

### objc_class

#### 对象、类对象、元类对象的区别 
- 类对象存储实例方法列表等信息
- 元类对象存锤类方法列表等信息

## 消息传递

`void objc_msgSend(void /* id self, SEL op, ... */)`

经过编译器转变后
`[self class]` <==>  `objc_msgSend(self, @selector(class))`
第一个参数是消息传递的接收者self, 第二个参数是传递的消息名称（选择器）

`void objc_msgSendSuper(void /* struct objc_super *super, SEL op, ... */)`
第一个参数是一个结构体指针

// 
struuch objc_super {
	__unsafe_unretained id receiver; // 就是当前的对象
}

`[superclass]` <==>  `objc_msgSendSuper(super, @selector(class))`


无论调用[super class] 还是 [self class] 最终这条消息的接收者都是当前对象

图1

![](https://ws4.sinaimg.cn/large/006tNc79ly1g1viq97z3cj31k90u0qd3.jpg)

## 消息传递过程

图2

![](https://ws3.sinaimg.cn/large/006tNc79ly1g1vigugmvmj31qu0u07a4.jpg)

问题1

![](https://ws1.sinaimg.cn/large/006tNc79ly1g1vikx3a6wj31hp0u0dmw.jpg)

`打印结果都是 Phone`

原因:

- 无论调用[super class] 还是 [self class] 最终这条消息的接收者都是当前对象
- [self class] 在Phone的类对象中查找class 方法，没有去父类查找，父类也没有，如图1顺次往上查找到根类，调用根类中class 的具体实现
- [super class]只是跨越了当前类，直接从当前类的父类中查找 class 方法，父类中没有class 方法，如图1所示只能继续往上查找到根类对象，在NSObject 中有class 方法实现，所以接受者任然是当前的对象

### 缓存查找

根据给定的SEL（方法选择器）通过一个函数来映射出bucket_t 在数组当中的位置，本质上是哈希查找

![](https://ws3.sinaimg.cn/large/006tNc79ly1g1vj2xha6gj318i0jc76s.jpg)

### 当前类中查找
当前类中存在着对应的方法列表
- 对于已经排序好的列表，采用二分查找算法查找对应的执行函数
- 对于没有排序的列表，采用一般遍历查找方法查找对应的执行函数


### 父类逐级查找

图3
![](https://ws1.sinaimg.cn/large/006tNc79ly1g1vkkjffglj31xm0ssgpn.jpg)

- 最关键是通过当前类的 superclass 成员变量去查找父类，或者访问父类
- curClass 的父类是否为 nil？如果是NSObject 类，他的父类为空，所以会结束查找
- 如果当前类有父类就要去该父类的缓存中查找，如果在缓存中命中，那么就结束查找，如果缓存中没有，就需要遍历当前类的父类的方法列表，如果没有就遍历父类的父类查找，沿着superclass指针逐级向上查找，直到查找到NSObject，再去Superclass。如果还是没有为nil时就结束父类逐级查找


## 消息转发

### resolveInstanceMethod:

- 该方法是类方法不是实例方法，参数是SEL 方法选择器类型
- 返回值是一个BOOL值
- 告诉系统是否要解决当前实例方法的实现
- 如果返回YES，结束消息转发
- 如果返回NO，系统会给第二次机会处理这个消息，会回掉forwardingTargetForSelector:

### forwardingTargetForSelector：
- 参数是 SEL 方法选择器
- 返回值是 id 类型，说明有哪个对象来处理，转发的对象是谁，如果返回了转发目标就会结束当前调用
- 如果返回为nil，系统会调用 methodSignatureForSelector:方法，最后一次机会处理这个消息

### methodSignatureForSelector：

- 参数是 SEL 方法选择器
- 返回值是一个对象，实际上这个对象是对`methodSignatureForSelector` 方法的参数，参数个数，参数类型和返回值的包装
- 如果返回一个方法签名的话，就会调用 forwardInvocation: 方法
- 如果返回为nil，标记为消息无法处理，程序crash

### forwardInvocation：
- 如果此方法不能处理消息，程序crash


代码示例

.h 文件

```
@interface RunTimeObject : NSObject
// 只声明，不实现，验证消息转发机制
- (void)test;
@end
```

.m 文件

```
@implementation RunTimeObject

// 实现消息转发流程 中的方法
/**
 是否要解决当前实例方法的实现
 */
+ (BOOL)resolveInstanceMethod:(SEL)sel {
    if (sel == @selector(test)) {
        NSLog(@"resolveInstanceMethod:");
        // 如果返回YES，消息转发就会结束
//        return YES;
        
        return NO;
    } else {
        
        NSLog(@"super resolveInstanceMethod:");
        // 返回父类的默认调用
        return [super resolveInstanceMethod:sel];
    }
}


/**
 - 参数是 SEL 方法选择器
 - 返回值是 id 类型，说明有哪个对象来处理，转发的对象是谁，如果返回了转发目标就会结束当前调用
 - 如果返回为nil，系统会调用 methodSignatureForSelector:方法，最后一次机会处理这个消息
 */
- (id)forwardingTargetForSelector:(SEL)aSelector {
    NSLog(@" forwardingTargetForSelector: ");
    return nil;
}

/**
 - 参数是 SEL 方法选择器
 - 返回值是一个对象，实际上这个对象是对`methodSignatureForSelector` 方法的参数，参数个数，参数类型和返回值的包装
 - 如果返回一个方法签名的话，就会调用 forwardInvocation: 方法
 - 如果返回为nil，标记为消息无法处理，程序crash
 
 */
- (NSMethodSignature *)methodSignatureForSelector:(SEL)aSelector {
    if (aSelector == @selector(test)) {
        NSLog(@"methodSignatureForSelector:");
        /**
         v: 表示这个方法返回值是 void
         
         固定参数@: 表示参数类型是 id, 即self
         固定参数`:` : 表示参数类型是选择器类型，即 @selector(test)
         */
        return [NSMethodSignature signatureWithObjCTypes:"v@:"];
    } else {
        
        NSLog(@"super methodSignatureForSelector:");
        // 返回父类的默认调用
        return [super methodSignatureForSelector:aSelector];
    }
}


- (void)forwardInvocation:(NSInvocation *)anInvocation {
    NSLog(@"forwardInvocation:");
}

@end

```

调用 

```
- (void)runTiemTest {
    RunTimeObject *obj = [[RunTimeObject alloc] init];
    [obj test];
}

```

结果：

```
resolveInstanceMethod:
forwardingTargetForSelector:
methodSignatureForSelector:
super resolveInstanceMethod:
forwardInvocation:

```

## 为类动态添加方法

methond Swizzling
### 使用场景
- 页面中的进出添加统计信息，使用 methond Swizzling 替换viewillAppear 方法

### 动态添加方法

- perforSelector: 实际上是考察class_addMethod 的方法使用


### 动态方法解析
- @dynamic关键字
- 动态运行时语言将函数决议推迟到运行时
- 编译时不生成setter/getter 方法，而是在运行时才添加具体的执行函数

## 问题
### [obj foo] 和 obj_msgSend()函数之间有什么关系？

- `[obj foo]`经过编译器处理过后会变成 `obj_msgSend() `有两个参数，一个是obj，第二个参数是foo 选择器


### runtime 如何通过Selector找到对应的IMP地址的？

消息传递机制

### 能否向编译后的类增加实例变量
编译之前就完成了实例变量的布局，
runtime_r_t 表示是readonly 的，编译后的类是不能添加实例变量的，可以向动态添加的类中添加实例变量

