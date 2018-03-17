---
title: Quartz2D绘制曲线
date: 2016-12-04 23:53:04
tags: Quartz2D
---

使用函数绘制曲线

Quartz 2D提供了CGContextAddCurveToPoint() 函数和CGContextAddQuadCurveToPoint()两个函数来向当前上下文添加曲线，前者用于添加贝塞尔曲线，后者用于添加二次曲线。
确定一条贝塞尔曲线需要4个点：开始点、第一个控制点、第二个控制点和结束点。

确定一条二次曲线需要三个点：开始点、控制点和结束点。

添加贝塞尔曲线

	CGContextAddCurveToPoint()
	
	CGContextRef ctx = UIGraphicsGetCurrentContext();
	
	// 添加曲线路径
	// 设置起点
	CGContextMoveToPoint(ctx, 0, 0);



 * 参数1: 上下文对象
 * 参数2: 控制点1X坐标
 * 参数3: 控制点1Y坐标
 * 参数4: 控制点2X坐标
 * 参数5: 控制点2Y坐标
 * 参数6: 终点x坐标
 * 参数7: 终点Y坐标
 
code:
 
	CGContextAddCurveToPoint(ctx, 30, 200, 300, 20, 300, 300);
	
	// 设置颜色
	[[UIColor magentaColor] setStroke];
	
	// 渲染
	CGContextStrokePath(ctx);
	
	// 关闭图形上下文
	CGContextClosePath(ctx);
	添加二次曲线
	CGContextAddQuadCurveToPoint()
	
	// 获取图形上下文
	CGContextRef ctx = UIGraphicsGetCurrentContext();
	
	// 起始点
	CGContextMoveToPoint(ctx, 20, 10);


 * 参数: 图形上下文
 * 参数: 控制点坐标
 * 参数: 结束点坐标
 
添加二次曲线

	CGContextAddQuadCurveToPoint(ctx, 30, 200, 200, 40);
	
	[[UIColor cyanColor] setStroke];
	CGContextStrokePath(ctx);
	// 关闭图形上下文
	CGContextClosePath(ctx);


绘制形状

使用绘制二次曲线的函数绘制花瓣

程序中CGContextAddFlower代码分别添加5瓣花朵路径、6瓣花朵路径、7瓣花朵路径，然后使用不同的颜色来填充这些路径。注意到上面的程序并未在每次添加花朵路径后立即关闭，这也是允许的，而且每次填充路径时并不会再次填充前一次已经填充过的路径。这是因为只用程序绘制了CGContextRef当前所包含的路径，系统会自动清除已经绘制的路径。

**注意：每次绘制完成后，CGContextRef会自动清除已经绘制完成的路径**


 该方法负责绘制花朵。
 n：该参数控制花朵的花瓣数；dx、dy：控制花朵的位置；size：控制花朵的大小；
 length：控制花瓣的长度
 
 
	void CGContextAddFlower(CGContextRef c , NSInteger n
	                        , CGFloat dx , CGFloat dy , CGFloat size , CGFloat length)
	{
	    CGContextMoveToPoint(c , dx , dy + size);  // 移动到指定点
	    CGFloat dig = 2 * M_PI / n;
	    // 采用循环添加n段二次曲线路径
	    for(int i = 1; i < n + 1 ; i++)
	    {
	        // 计算控制点坐标
	        CGFloat ctrlX = sin((i - 0.5) * dig) * length + dx;
	        CGFloat ctrlY= cos((i - 0.5 ) * dig) * length + dy;
	        // 计算结束点的坐标
	        CGFloat x = sin(i * dig) * size + dx;
	        CGFloat y =cos(i * dig) * size + dy;
	        // 添加二次曲线路径
	        CGContextAddQuadCurveToPoint(c, ctrlX , ctrlY , x , y);
	    }
	}


绘制花瓣

	- (void)drawStar
	{
	    CGContextRef ctx = UIGraphicsGetCurrentContext();  // 获取绘图的CGContextRef
	    CGContextBeginPath(ctx);  // 开始添加路径
	    CGContextAddFlower(ctx , 5 , 50 , 100 , 30 , 80);  // 添加5瓣花朵的路径
	    CGContextSetRGBFillColor(ctx, 1, 0, 0, 1);  // 设置填充颜色
	    CGContextFillPath(ctx);
	    CGContextAddFlower(ctx , 6 , 160 , 100 , 30 , 80);  // 添加6瓣花朵的路径
	    CGContextSetRGBFillColor(ctx, 1, 1, 0, 1);  // 设置填充颜色
	    CGContextFillPath(ctx);
	    CGContextAddFlower(ctx , 7 , 270 , 100 , 30 , 80);  // 添加7瓣花朵的路径
	    CGContextSetRGBFillColor(ctx, 1, 0, 1, 1);  // 设置填充颜色
	    CGContextFillPath(ctx);
	    CGContextClosePath(ctx);  // 关闭路径
	}
