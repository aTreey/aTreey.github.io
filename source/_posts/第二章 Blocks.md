# Blocks

## Block 是什么？

Blocks 是 C 语言的扩充功能，将函数及其执行上下文封装起来的对象，带有自动变量（局部变量）的匿名函数

- 因为block 底层C++ 结构体中存在着 isa 指针，所以是一个对象
- 因为block 底层C++ 结构体中存在着一个函数指针 FuncPtr，所以是一个函数

## 什么是Block 调用
Block 的调用就是函数的调用
在终端使用 clang 编译之后查看文件内容可得出以下结论：

- 对其进行一个强制转换，转换之后取出成员变量 FuncPtr

## 截获变量

**以下代码运行结果是 12**

![](https://ws4.sinaimg.cn/large/006tNc79ly1g1xvmfjgo8j30uu0jmgnl.jpg)

		原因：block 对基本数据类型的局部变量会截获其值，在定义 block 时以值的方式传到了block 对应的结构体当中，而调用block 时直接使用的是block对应结构体当中已经传过来的那个值，所以值为12


- 局部变量截获
	- 基本数据类型的局部变量截获其值
	- 对象类型的局部变量连同所有权修饰符一起截获

- 局部静态变量： 已指针形式截获

- 全局变量：不截获

- 静态全局变量： 不截获


**以下代码运行结果是 8**

![](https://ws1.sinaimg.cn/large/006tNc79ly1g1xwb9qt9oj30n60bq75i.jpg)
		
		原因：block 对局部静态变量会以指针形式截获，所以值为8
		
## __block 修饰符

- __block 修饰后的变量最后变成了对象，底层c++ 结构体中存在isa指针
- 一般情况下，对被截获变量进行赋值操作需要添加__block 修饰符，而操作是不需要添加__block 修饰符的
- 不需要__block 修饰符，全局变量和静态全局变量 block 是不截获的，所以不需要修饰，静态局部变量是通过指针截获的，修改外部的变量，也不需要添加
	- 静态局部变量
	- 全局变量
	- 静态全局变量

**不需要添加__Block 修饰符**

```
- (void)test {
    NSMutableArray *array = [NSMutableArray array];
    void(^block)(void) = ^{
        [array addObject:@1234];
    };
    
    block();
}
```

**需要添加__Block 修饰符**

```
- (void)test {
    __block NSMutableArray *tempArray = nil;
    void(^block)(void) = ^{
        tempArray = [NSMutableArray array];
    };
    
    block();
}
```

**以下代码运行结果是 8**

```
- (void)test__Block3 {
    __block int multiplier = 6;
    int(^Block)(int) = ^int(int num){
        return multiplier * num;
    };
    multiplier = 4;
    NSLog(@"__block--test__Block3---result is %d", Block(2));
}

```

原因

![](https://ws4.sinaimg.cn/large/006tNc79ly1g1xxea9gckj31hi0l2ju3.jpg)

![](https://ws1.sinaimg.cn/large/006tNc79ly1g1xxfpnz45j30zm0lstad.jpg)


**栈上Block 的__block 变量中的 __forwarding 指针指向的是自身**


## Block 的内存管理

**Block 的 内存分布**

![](https://ws1.sinaimg.cn/large/006tNc79ly1g1xxnyye6kj31ag0ran0f.jpg)

**Block 的 Copy 操作**
 
![](https://ws1.sinaimg.cn/large/006tNc79ly1g1xxouef30j315a0d640x.jpg)

- 全局block
- 堆block
- 栈block

## Block 循环引用

- 如果当前block对当前对象的某一变量进行截获的话，block 会对对应变量有一个强引用，当前block因为当前的对象对其有个强引用，产生了自循环引用，通过声明为__weak变量来解决
- 如果定义了__block 修饰符的话也会产生循环引用，在ARC 下会产生循环引用，而在MRC 下不会，在ARC 下通过打破环来消除，但是存在弊端时当block 没有机会执行时，



循环引用1
![](https://ws2.sinaimg.cn/large/006tNc79ly1g1xzmtsetqj31g20oewic.jpg)

解决方法：

![](https://ws2.sinaimg.cn/large/006tNc79ly1g1xzmtsetqj31g20oewic.jpg)


大环循环引用2 

![](https://ws2.sinaimg.cn/large/006tNc79ly1g1xzqfwf1uj319c0la0vk.jpg)

以上代码，在MRC 下，不会产生循环引用，在ARC 下，会产生循环应用，引起内存泄露
![](https://ws3.sinaimg.cn/large/006tNc79ly1g1xzvs524zj31h80l20uo.jpg)

解决方法

如果block 没有机会执行，那么循环引用将不会被打破
![](https://ws2.sinaimg.cn/large/006tNc79ly1g1xzxf0iyxj31hp0u0aeq.jpg)



