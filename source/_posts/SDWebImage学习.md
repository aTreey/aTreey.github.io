---
title: SDWebImage的用法及原理
date: 2017-02-08 23:17:50
tags: SDWebImage
---

### SDWebImage 介绍

SDWebImage 是用于网络中下载且缓存图片，并设置图片到对应的控件或上

Demo: <https://github.com/aTreey/DownImage.git>

- 提供了UIImageView的category用来加载网络图片并且下载的图片的缓存进行管理
- 采用异步方式来下载，确保不会阻塞主线程，memory＋disk来缓存网络图片，自动管理缓存
- 支持GIF动画，[self.imageView setImageWithURL:[[NSBundle mainBundle] URLForResource:@”xx.gif” withExtension:nil];
- 支持WebP格式
- 同一个URL的网络图片不会被重复下载
- 失效的URL不会被无限重试

### SDWebImage 的使用

克隆

git clone https://github.com/rs/SDWebImage.git
使用以上命令克隆会报错，框架中Vendors文件夹中的文件未能全部下载导致报错



解决办法：https://github.com/rs/SDWebImage/blob/master/Docs/ManualInstallation.md

git clone --recursive https://github.com/rs/SDWebImage.git


文件全部下载


自定义operation 加入NSOperationQueue中

- 自定义NSOperation,
重写main方法

```
- (void)main {
@autoreleasepool {
    NSURL *url = [NSURL URLWithString:_urlStr];
    NSData *data = [NSData dataWithContentsOfURL:url];
    UIImage *image = [UIImage imageWithData:data];

    if (_finishBlock) {
        _finishBlock(image);
    }
}
}

```

- 一个NSOperation对象可以通过调用start方法来执行任务，默认是同步执行的。也可以将NSOperation添加到一个NSOperationQueue(操作队列)中去执行，而且是异步执行的
创建队列

```
- (NSOperationQueue *)downLoadQueue {
    if (!_downLoadQueue) _downLoadQueue = [[NSOperationQueue alloc] init];
    return _downLoadQueue;
}

```


- 添加一个任务到队列

```
[_downLoadQueue addOperation:operation];
添加一组operation, 是否阻塞当前线程

[_downLoadQueue addOperations:@[operation] waitUntilFinished:NO];
添加一个block 形式的operation

[_downLoadQueue addOperationWithBlock:^{
    NSLog(@"执行一个新的线程");
}];
```

**注意：**

1. NSOperation 添加到 queue之后，通常短时间内就会执行，但是如果存在依赖，或者整个queue被暂停等原因，也可能会需要等待

2. NSOperation添加到queue之后，绝不要修改NSOperation对象的状态，因为NSOperation对象可能会在任何时候运行，因此改变NSOperation对象的依赖或者数据会产生不利的影响，只能查看NSOperation 对象的状态，比如是否正在运行、等待运行、已经完成等

- NSOperation 添加依赖，
依赖关系不局限于相同的queue 中的NSOperation对象，可以夸队列进行依赖，但是不能循环依赖，

```
// NSOperation 添加依赖
{
    NSOperationQueue *testQueue = [[NSOperationQueue alloc] init];
    NSBlockOperation *operation1 = [NSBlockOperation blockOperationWithBlock:^{
        NSLog(@"operation1 NSThread = %@", [NSThread currentThread]);
    }];

    NSBlockOperation *operation2 = [NSBlockOperation blockOperationWithBlock:^{
        NSLog(@"operation2 NSThead = %@", [NSThread currentThread]);
    }];

    // 添加依赖，1 依赖于 2，只有2 执行完之后才执行1
    [operation1 addDependency:operation2];
    // 加入到队列
    [testQueue addOperation:operation1];
    [testQueue addOperation:operation2];
}

```

- 修改Operation的执行顺序
对于添加到queue中的operation执行顺序有一下2点决定
operation 是否已经准备好，是否添加了依赖
根据多有operations的相对优先性来决定，优先等级是operation对象本身的一个属性，默认都是“普通”优先级，可以通过setQueuePriority方法提高和降低优先级，优先级只能用于相同 queue 中的 operation，多个queue中operation的优先级相互独立，因此不同queue中的低优先级的operation可能比高优先级的operation更早执行
注意优先级和依赖不能相互替代，优先级只是对已经准备好的operation确定执行顺序，先满足依赖，然后再看已经准备好的operation中的优先级

- 设置 queue 的最大并发数，队列中最多同时运行几条线程
虽然NSOperationQueue类设计用于并发执行Operations,你也可以强制单个queue一次只能执行一个Operation。setMaxConcurrentOperationCount:方法可以配置queue的最大并发操作数量。设为1就表示queue每次只能执行一个操作。不过operation执行的顺序仍然依赖于其它因素,比如operation是否准备好和operation的优先级等。因此串行化的operation queue并不等同于GCD中的串行dispatch queue

```
// 每次只能执行一个操作  
queue.maxConcurrentOperationCount = 1;  
// 或者这样写  
[queue setMaxConcurrentOperationCount:1];  
```
   
- 取消 operations
一旦添加到operation queue,queue就拥有了这个Operation对象并且不能被删除,只能取消。调用Operation对象的cancel方法取消单个操作,也可以调用operation queue的cancelAllOperations方法取消当前queue中的所有操作, 使用cancel属性来判断是否已经取消了

[operation cacel] 只是打了一个死亡标记, 并没有正真意义上的取消,称为 “自杀“,需要在被取消的任务中时时判断是否取消(在程序的关键处), 如果取消,结束任务, 具体是指在自定义的 operation 的main方法中结束

```
[queue cancelAllOperations] 由队列来取消，称为 “他杀”

// 取消单个操作  
[operation cancel];
// 取消queue中所有的操作  
[queue cancelAllOperations];
```

- 等待operation 完成

为了最佳的性能,你应该设计你的应用尽可能地异步操作,让应用在Operation正在执行时可以去处理其它事情。如果需要在当前线程中处理operation完成后的结果,可以使用NSOperation的waitUntilFinished方法阻塞当前线程，等待operation完成。通常我们应该避免编写这样的代码,阻塞当前线程可能是一种简便的解决方案,但是它引入了更多的串行代码,限制了整个应用的并发性,同时也降低了用户体验。绝对不要在应用主线程中等待一个Operation,只能在第二或次要线程中等待。阻塞主线程将导致应用无法响应用户事件,应用也将表现为无响应。
```
// 会阻塞当前线程，等到某个operation执行完毕  
[operation waitUntilFinished];
```
除了等待单个Operation完成,你也可以同时等待一个queue中的所有操作,使用NSOperationQueue的waitUntilAllOperationsAreFinished方法。注意：在等待一个 queue时,应用的其它线程仍然可以往queue中添加Operation,因此可能会加长线程的等待时间。

// 阻塞当前线程，等待queue的所有操作执行完毕  
[queue waitUntilAllOperationsAreFinished]; 
暂停和继续operation
如果你想临时暂停Operations的执行,可以使用queue的setSuspended:方法暂停queue。不过暂停一个queue不会导致正在执行的operation在任务中途暂停,只是简单地阻止调度新Operation执行。你可以在响应用户请求时,暂停一个queue来暂停等待中的任务。稍后根据用户的请求,可以再次调用setSuspended:方法继续queue中operation的执行
```
// 暂停queue  
[queue setSuspended:YES];  

// 继续queue  
[queue setSuspended:NO]; 
```


### 增加Manager管理类，

作用：

- 下载队列
- 图像缓存
- 操作缓存（内存缓存）
- 沙盒缓存
- 防止错乱,取消老的未开始下载的操作
- 图片显示逻辑：内存缓存 – 沙盒缓存 – 网络下载
- 图片缓存逻辑：下载完成 – 缓存沙盒 – 加载到内存

注意：缓存到本地时只能保存property列表里的对象（NSData, NSDate, NSNumber, NSString, NSArray, NSDictory）, 图片需要转化为二进制数据

### SDWebImage

- SDWebImageDownloader 下完图片后都需要手动设置给UIImageView
- SDWebImageManager 下完图片后都需要手动设置给UIImageView
- 下载后要显示可以使用 sd_setImageWithURL 方法