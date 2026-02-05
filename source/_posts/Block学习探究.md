---
title: Block学习探究
date: 2018-01-19 00:06:55
tags:
---

### 代码及结果

- 普通局部变量

	```
	- (void)block1 {
	    
	    NSInteger num = 100;
	    
	    // 定义一个block
	    dispatch_block_t block = ^ {
	        NSLog(@"block1 -- numer = %zd", num);
	    };
	    
	    num = 200;
	    
	    block();
	}
	```

-  使用__block修饰变量

   ```
	- (void)block2 {
	    
	    __block NSInteger num = 100;
	    
	    // 定义一个block
	    dispatch_block_t block = ^ {
	        NSLog(@"block2 -- numer = %zd", num);
	    };
	    
	    num = 200;
	    
	    block();
	}
	
	```

- 全局变量

	```
	- (void)block3 {
	    
	    
	    // 定义一个block
	    dispatch_block_t block = ^ {
	        NSLog(@"block3 -- numer = %zd", blockNum);
	    };
	    
	    blockNum = 200;
	    
	    block();
	}
	
	```


- 静态常量

	```
	- (void)block4 {
	    
	    static NSInteger num = 100;
	    
	    // 定义一个block
	    dispatch_block_t block = ^ {
	        NSLog(@"block4 -- numer = %zd", num);
	    };
	    
	    num = 200;
	    
	    block();
	}
	
	```
	
### 运行结果

	2018-01-19 00:04:13.759416+0800 CycleRetain[91251:2382380] block1 -- numer = 100
	2018-01-19 00:04:13.760206+0800 CycleRetain[91251:2382380] block2 -- numer = 200
	2018-01-19 00:04:13.760473+0800 CycleRetain[91251:2382380] block3 -- numer = 200
	2018-01-19 00:04:13.760603+0800 CycleRetain[91251:2382380] block4 -- numer = 200

### 原理及本质


block 根据创建位置不同,共有三种:栈block,堆block,全局block

使用__block本质就是为了保证栈上和堆上block内访问和修改的是同一个变量,具体的实现是将__block 修饰的变动自动封装成一个结构体,让他在堆上创建
基于OC 底层的runtime 机制
block 在内部会有一个指向结构体的指针,当调用block的时候其实就是让block找出对应的指针所指的函数地址进行调用。并传入了block自己本身






