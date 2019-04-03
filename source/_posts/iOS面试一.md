
# 数据结构



## UITableView 相关

- 重用机制
- 并发访问，数据拷贝问题：主线程删除了一条数据，子线程数据由删除之前的数据源copy而来，子线程网络请求，数据解析，预排版等操作后，回到主线程刷新时已经删除的数据又会重现
	- 主线程数据源删除时对数据进行记录，子线程的数据回来后再进行一次同步删除操作，然后再刷新数据
	
	- 使用GCD中的串行队列，将数据删除和数据解析在此队列中进行
	
	
## UIView 和CALayer 的关系

- UIView 为 CALayer 提供内容，负责处理触摸事件，参与响应链
- CALayer 负责显示内容，分工明确，UIView 的Layer 实际上就是CALayer， backgroundColor 等属性本质是对CALayer 做了一层包装

## 事件传递

![](https://ws3.sinaimg.cn/large/006tKfTcly1g1omq41fi4j31hn0u0n3a.jpg)

- hitTest 方法是返回响应时间的试图
- pointInside withEvent 方法判断点击位置是否在当前视图范围内
- 倒序遍历最终响应事件的试图，最后添加的试图会最优先被遍历到

**需求案例**

只让正方形的view中圆形区域内可以响应事件，四个角的位置不响应事件
![](https://ws1.sinaimg.cn/large/006tKfTcly1g1onklz0k8j309q0a674h.jpg)

```
class CircularAreaResponseButton: UIButton {
    
    override func hitTest(_ point: CGPoint, with event: UIEvent?) -> UIView? {
        if self.isHidden || !self.isUserInteractionEnabled ||  self.alpha <= 0.01 {
            return nil
        }
        
        var hitView: UIView?
        if self.point(inside: point, with: event) {
            // 遍历当前视图的子试图
            for subView in subviews {
                // 坐标转换
                let convertPoint = self.convert(point, to: subView)
                if let hitTestView = subView.hitTest(convertPoint, with: event) {
                    // 找到了最终响应事件的试图
                    hitView = hitTestView
                    break;
                }
            }
            
            if hitView != nil {
                return hitView
            }
            return self
        }
        return nil
    }
    
    
    override func point(inside point: CGPoint, with event: UIEvent?) -> Bool {
        let x1 = point.x
        let y1 = point.y
        
        let x2 = self.frame.width / 2.0
        let y2 = self.frame.height / 2.0
        // 使用平面内两点见距离公式计算距离, 并且判断是否 <= 圆的半径
        return sqrt((x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2)) <= x2
    }
}
```

## 视图响应

UIView 都继承于 UIResponder，都有以下方法

```
func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?)
func touchesMoved(_ touches: Set<UITouch>, with event: UIEvent?) 
func touchesEnded(_ touches: Set<UITouch>, with event: UIEvent?)
func touchesCancelled(_ touches: Set<UITouch>, with event: UIEvent?)
```
如果事件沿着视图响应链一直传递到 UIApplicationDelegate 仍然没有任何一个视图处理事件，程序将会发生什么？

- 忽略掉了这个事件当作什么都没有发生


## 图像显示原理

CALayer 的 contents 是位图

![](https://ws1.sinaimg.cn/large/006tKfTcly1g1oqxcw9dvj32260n041w.jpg)

- CPU的工作

	Layout   | Display   | Prepare    | Commite |
	---------|-----------|------------|---------|
	UI布局	  | 绘制	     | 图片编解码  | 提交位图 | 
	文本计算  | 

- GPU渲染管线

	定点着色 | 图元转配 | 光栅化 | 片段着色 | 片段处理 |
	--------|--------|-------|---------|---------|
	
	
			提交到帧缓冲区（frameBuffer）
	

## UI卡顿掉帧的原因

1/60 ms 时间内， 由cpu 和GPU 协同产生最终的数据，当 CPU UI布局，文本计算所用的时间较长时，留给GPU渲染，提交到帧缓冲区的时间就会变短，这样以来就会发生掉帧情况


### UIView 的绘制原理




**CPU优化方案**

- 对象的创建、调整、销毁放在子线程，节省cpu的一部分时间
- 预排版（布局计算，文本计算）放在子线程中
- 预渲染（文本等异步绘制，图片编解码）等

**GPU优化方案**


## 在屏渲染
指GPU的渲染操作是在当前用于显示的屏幕缓冲区域中进行

## 离屏渲染
指GPU在当前屏幕缓冲区域外新开劈了一个缓冲区域进行渲染操作
指定了UI视图图层的某些属性，标记为视图在未预合成之前不能用于在当前屏幕上直接显示的时候就会发生离屏渲染，例如图层的圆角设置和蒙层设置

- 离屏渲染何时触发

	- 设置layer 的圆角并且和 maskToBounds 一起使用时
	- 图层蒙版
	- 阴影
	- 光栅化

- 为何要避免离屏渲染
	-  离屏渲染，会增加 GPU 的工作量，GPU工作量的增加就很有可能导致CPU和GPU的总耗时超过1/60s，导致UI卡顿和掉帧 

- 避免离屏渲染

###异步绘制

![](https://ws3.sinaimg.cn/large/006tKfTcly1g1osowi1dtj31l70u0ag5.jpg)

## 内存管理

![](https://ws2.sinaimg.cn/large/006tKfTcly1g1mabhcs20j31cb0u0n2p.jpg)

- stack 栈是从高地址向低地址扩展，所以栈是向下增长的，对象和block copy 后都会放在堆上
- heap 堆区向上增长


- 散列表方式
	
	- 自旋锁
		- 自旋锁是“忙等” 的锁
	- 引用计数表
	- 弱应用表