---
title: Quartz2D 初识
date: 2016-12-04 13:34:28
tags: Quartz2D
---


**Quartz2D 二维绘图引擎，支持iOS和Mac系统么，可以实现的功能有**

- 绘制图形: 线条，三角形，矩形，圆，弧等
- 绘制文字
- 绘制图像或者图片
- 读取生成PDF
- 截图/裁剪图片
- 自定义UI控件
- 手势解锁
- 图形上下文

## 保存绘图信息和状态
确定输出目标(PDF,Bitmap或者显示器)

	Bitmap Graphics Context
	PDF Granhics Context
	Window Graphics Context
	Layer Graphics Context


绘制线段
	
	- (void)drawLine {
	    // 获取图形上下文
	    // 所用的是UIGraphics上下文
	    CGContextRef ctx = UIGraphicsGetCurrentContext();
	
	    // 创建描述路径
	    CGMutablePathRef path = CGPathCreateMutable();
	    // 设置起点
	    // path : 表示给那个路径设置起点
	    CGPathMoveToPoint(path, NULL, 10, 10);
	    CGPathAddLineToPoint(path, NULL, 80, 80);
	
	    // 添加路径到上下文
	    CGContextAddPath(ctx, path);
	
	    // 渲染
	    CGContextStrokePath(ctx);
	}



系统底层自动将路径添加到图形上下文

	- (void)drawLine2 {
	
	    CGContextRef ctx = UIGraphicsGetCurrentContext();
	
	    // 描述路径
	    // 此方法底层会自动将路径添加到图形上下文
	    CGContextMoveToPoint(ctx, 50, 50);
	    CGContextAddLineToPoint(ctx, 100, 100);
	
	    CGContextSetLineWidth(ctx, 5);
	
	    [[UIColor redColor] set];
	    // 渲染
	    CGContextStrokePath(ctx);
	}


使用C语言函数的封装

	- (void)drawLine3 {
	    // 使用UIKit 已经封装的功能,面向对象
	    // 贝塞尔路径
	
	    UIBezierPath *path = [UIBezierPath bezierPath];
	    [path moveToPoint:CGPointMake(10, 10)];
	    [path addLineToPoint:CGPointMake(50, 50)];
	
	    // 绘制路径
	    [path stroke];
	}


绘制多条线段

- 绘制多条线,状态不好管理
- 默认下一条线的起点是上一条线的终点
- 绘制多天不连接的线时需要重新设置起点
- 为了方便管理，应该让每一条线对应一条路径
- 使用UIBeizerPath

code:

	- (void)drawMoreLine {
	    CGContextRef ctx = UIGraphicsGetCurrentContext();
	
	    // 描述路径
	    CGContextMoveToPoint(ctx, 20, 20);
	    CGContextAddLineToPoint(ctx, 100, 100);
	
	    // 设置第二条线的起点
	    CGContextMoveToPoint(ctx, 20, 30);
	    CGContextAddLineToPoint(ctx, 150, 100);
	
	    // 设置绘图状态要在渲染之前,都是给上下文设置
	    // 描边颜色
	    [[UIColor greenColor] setStroke];
	    // 线宽
	    CGContextSetLineWidth(ctx, 10);
	    // 连接样式
	    // kCGLineJoinMiter,    斜接
	    // kCGLineJoinRound,    圆角
	    // kCGLineJoinBevel     平切
	    CGContextSetLineJoin(ctx, kCGLineJoinBevel);
	    // 顶角样式,线的顶端效果
	    CGContextSetLineCap(ctx, kCGLineCapRound);
	
	    // 渲染
	    CGContextStrokePath(ctx);
	}


使用UIBezierPath 绘制多条直线并设置不同状态
	
	- (void)drawUIBezierPahtState {
	
	    // 第一条线
	    UIBezierPath *path = [UIBezierPath bezierPath];
	    [path moveToPoint:CGPointMake(10, 10)];
	    [path addLineToPoint:CGPointMake(80, 80)];
	
	    path.lineWidth = 20;
	    path.lineJoinStyle = kCGLineJoinRound;
	    path.lineCapStyle = kCGLineCapRound;
	    [[UIColor redColor] setStroke];
	
	    [path stroke];
	
		// 第二条线
		{
		    UIBezierPath *path = [UIBezierPath bezierPath];
		    [path moveToPoint:CGPointMake(40, 70)];
		    [path addLineToPoint:CGPointMake(150, 40)];
		
		    path.lineWidth = 20;
		    path.lineJoinStyle = kCGLineJoinRound;
		    path.lineCapStyle = kCGLineCapRound;
		    [[UIColor greenColor] setStroke];
		
		    [path stroke];
		}
	}