---
title: iOS即时通讯实现二
date: 2018-01-12 21:54:26
tags:
---

## 实现IM通信所用协议

几个协议理解

xmpp 是基于xml协议主要是易于扩展，早起用于PC时代的产品使用，流量大，耗电大交互复杂其实并不适合移动互联网时代的IM产品

MQTT 协议 适配多平台，需自己扩展好友，群组等功能

protobut 协议

## 封装socket，自己实现

所要考虑的问题

- 传输协议选择 
	- TCP ？ 一般公司都选用（技术不是很成熟）
	- UDP ？QQ 腾讯增加自己的私有协议，保证数据传递的可靠性，解决了丢包，乱序的问题
	
- 聊天协议选择及代表框架
	- Socket ？ CocoaAsyncSocket 
	- WebSockt ？是传输通讯协议，基于socket封装的一个协议 SocketRocket
	- MQTT ？MQTTKit 聊天协议为上层协议
	- protobut 协议 ？
	- XMPP ？XMPPFramework  聊天协议为上层协议
	- 自定义
- 传输数据格式
	- Json？
	- XML？
	- ProtocolBuffer？

- 细节相关
	- TCP 长连接如何保持，
	- 心跳机制
	- 重连机制
	- 数据安全机制

### CocoaAsyncSocket 
	
- CocoaAsyncSocket 的 delegate 用来设置代理处理各个回调
- delegateQueue 
- socketQueue 串行队列，贯穿全类并没有任何加锁，确保了socket 操作中每一步都是线程安全的
- 创建了两个读写队列（本质是数组）
	- `NSMutableArray *readQueue;` 
	- `NSMutableArray *writeQueue;`

- 全局数据缓冲: 当前socket未获取完的数据大小
	- `GCDAsyncSocketPreBuffer *preBuffer;`
	 
- 交替延时变量: 用于进行另一服务端地址请求的延时 
	- `alternateAddressDelay`

- connect

- disconnect

## 数据粘包,断包

- 粘包: 如果客户端同一时间发送几条数据，而服务器只收到一大条数据
	
	- 原因：
	- 由于传输的是数据流，经过TCP传输后，TCP使用了优化算法将多次间隔较小且数据量小的数据合并成一个大的数据块，因此三条数据合并成了一条
	
		**TCP,UDP都可能造成粘包的原因**
	
	- 发送端需要等缓冲区满了才发送出去，做成粘包
	- 接收方不及时接收缓冲区的包，造成多个包接收
	
- 断包: 因为一次发送很大的数据包，缓冲区有限，会分段发送或读取数据


## CocoaAsyncSocket的封包，拆包处理

- 读取数据方法：

		// 有数据时调用，读取当前消息队列中的未读消息
		- (void)readDataWithTimeout:(NSTimeInterval)timeout tag:(long)tag


	**每次读取数据，每次都必须手动调用上述 readData 方法超时设置为不超时才能触发消息回调的代理**
	
	**因此在第一连接时调用，再在接到消息时调用，上述方法就可以达到每次收到消息都会触发读取消息的代理**
	
	以上做法存在的问题就是没有考虑数据的拆包会有粘包情况，这时候需要用下面两个`read`方法，1. 读取指定长度、2.读取指定边界
	
		// 读取特定长度数据时调用
		- (void)readDataToLength:(NSUInteger)length withTimeout:(NSTimeInterval)timeout tag:(long)tag
		
		// 读到特定的 data 边界时调用
		- (void)readDataToData:(NSData *)data withTimeout:(NSTimeInterval)timeout tag:(long)tag


	**解决办法及具体思路**
	
	- 封包：给每个数据包增加一个长度或者加一个开始结束标记，表明数据的长度和类型（根据自己的项目：文本、图片、语音、红包、视频、话题、提问等等）
		- 可以在数据包之后加一个结束标识符，解决了传输过程中丢包，丢失头部信息的错误包读取，读到这样的就丢弃直接读下一个数据包
	
	- 拆包：获取每个包的标记，根据标记的数据长度获取数据，最后根据类型处理数据,最后读取数据包头部的边界

- 利用缓冲区对数据进行读取
	- 创建读取数据包
	- 添加到读取的队列中
	- 从队列中取出读取任务包 
	- 使用偏移量 `maxLength ` 读取数据 
	
	```
		- (void)readDataWithTimeout:(NSTimeInterval)timeout
                     buffer:(NSMutableData *)buffer
               bufferOffset:(NSUInteger)offset
                  maxLength:(NSUInteger)length
                        tag:(long)tag
{
	if (offset > [buffer length]) {
		LogWarn(@"Cannot read: offset > [buffer length]");
		return;
	}
	
	// 1. 创建读取数据包
	GCDAsyncReadPacket *packet = [[GCDAsyncReadPacket alloc] initWithData:buffer
	                                                          startOffset:offset
	                                                            maxLength:length
	                                                              timeout:timeout
	                                                           readLength:0
	                                                           terminator:nil
	                                                                  tag:tag];
	
	dispatch_async(socketQueue, ^{ @autoreleasepool {
		
		LogTrace();
		
		if ((flags & kSocketStarted) && !(flags & kForbidReadsWrites))
		{
			// 2. 向读的队列添加任务包
			[readQueue addObject:packet];
			// 3. 从队列中取出读取任务包 
			[self maybeDequeueRead];
		}
	}});
	
	// Do not rely on the block being run in order to release the packet,
	// as the queue might get released without the block completing.
}

	```

doReadData 方法时读取数据的核心方法

- 如果读取对列中没有数据，就去判是否是上次已经读取了数据，但是因为没有目前还没有读取到数据的标记，这是正好设置了 read 方法中的超时时间，所以要断开socket 连接


				
![](https://www.jianshu.com/p/fdd3d429bdb3)				


- 接收到数据后在 socket系统进程的数据缓冲区中
	- （基于TLS的分为两种）他们在各自管道中流动，完成数据解密后又流向了全局数据缓冲区
		-  CFStreem 
		-  SSLPrebuffer

处理数据逻辑： 如果当前读取包长度给明了，则直接流向currentRead，如果数据长度不清楚，那么则去判断这一次读取的长度，和currentRead可用空间长度去对比，如果长度比currentRead可用空间小，则流向currentRead，否则先用prebuffer来缓冲。


### 心跳机制
- TCP 自带的keep-alive 使用空闲时间来发并且默认超时时间太长是2 小时，
- 如果应用使用了socks ，socks proxy会让tcp keep-alive失效，因为socks 协议只管转发TCP层具体的数据包，并不会转发TCP协议内的实现具体细节数据包
- TCP 的长连接理论上是一直保持连接的，可以设置 TCP keep-alive的时间 但是实际情况下中，可能出现故障或者是因为防火墙的原因会自动把一定时间段内没有数据交互的连接给断开，心跳机制就可以维持长连接，保持活跃状态

- iOS 中使用 NSTimer  `scheduledTimerWithTimeInterval` 需要在调用之前的关键位置设置 fireDate 为未来的某个时间, 然后再需要的时候开启
- 或者直接使用 `timerWithTimeInterval` 创建，然后添加到`runloop`中














