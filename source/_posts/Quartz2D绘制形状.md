---
title: Quartz 2D绘制形状(二)
date: 2016-12-10 21:58:31
tags: Quartz 2D
---


使用Quartz 2D绘制圆弧，扇形，下载进度以及绘制的图形的填充和描边属性的设置.
将贝塞尔路径添加到图形上下文中
图形上下文中的状态保存和恢复
对图形上下文进行矩阵操作（平移，缩放，选转），实现一些特殊效果

### 使用贝塞尔曲线绘制矩形

```
{
    UIBezierPath *path = [UIBezierPath bezierPathWithRect:CGRectMake(0, 0, 20, 30)];
    [[UIColor magentaColor] setStroke];
    [[UIColor brownColor] setFill];
    [path fill]; // 填充
    [path stroke]; // 描边
}

```

### 圆角矩形
```
// 绘制圆角矩形
{
    UIBezierPath *path = [UIBezierPath bezierPathWithRoundedRect:CGRectMake(35, 10, 50, 70) cornerRadius:5.0];
    [[UIColor magentaColor] setStroke];
    [[UIColor cyanColor] setFill];
    [path fill];
    [path stroke];
}

```
### 绘制圆
```
// 画圆
{
    UIBezierPath *path = [UIBezierPath bezierPathWithRoundedRect:CGRectMake(90, 5, 40, 40) cornerRadius:20.0];
    [[UIColor purpleColor] setStroke];
    [[UIColor magentaColor] setFill];
    [path fill];
    [path stroke];
}
```

### 绘制扇形
```
CGPoint center = CGPointMake(200, 50);
UIBezierPath *sectorPath = [UIBezierPath bezierPathWithArcCenter:center radius:100 startAngle:0 endAngle:M_PI_4 clockwise:YES];
[sectorPath addLineToPoint:center];
[[UIColor brownColor] setStroke];
[sectorPath fill];
[sectorPath stroke];
```
### 绘制圆弧

```
/**
* 绘制圆弧
* 参数1: 圆心坐标
* 参数2: 半径
* 参数3: 开始角度
* 参数4: 结束角度 使用弧度制表示
* 参数5: clockwis顺时针方法的
*/

UIBezierPath *circuLarArcPath = [UIBezierPath bezierPathWithArcCenter:CGPointMake(100, 60) radius:50.0 startAngle:0 endAngle:M_PI_2 clockwise:YES];

[circuLarArcPath moveToPoint:CGPointMake(100, 60)];

[[UIColor magentaColor] setStroke];
[[UIColor brownColor] setFill];
[circuLarArcPath stroke];
[circuLarArcPath fill];

```
### 绘制文字


```
/**
 * 绘制文字
 * drawAtPoint 不能换行，当文字过多过长时不会有效果
 * 使用 drawInRect 可以实现换行
 */


NSString *text = @"我是绘制文字价格就看过几个框架刚开始大家高考的感觉是快乐的关键时刻大家赶快来时的结果看了风水格局来看就哭了感觉树大根深来对抗肌肤抵抗力";

NSMutableDictionary *attributeDict  = [NSMutableDictionary dictionary];
attributeDict[NSFontAttributeName] = [UIFont systemFontOfSize:18];

// 字体颜色
attributeDict[NSForegroundColorAttributeName] = [UIColor greenColor];
// 描边颜色
attributeDict[NSStrokeColorAttributeName] = [UIColor yellowColor];
// 描边宽度
attributeDict[NSStrokeWidthAttributeName] = @1;
//  阴影
NSShadow *shadow = [[NSShadow alloc] init];
shadow.shadowColor = [UIColor redColor];
shadow.shadowOffset = CGSizeMake(1.0, 1.0);

attributeDict[NSShadowAttributeName] = shadow;

// 可以换行
[text drawInRect:self.bounds withAttributes:attributeDict];

// 不换行
[text drawAtPoint:CGPointZero withAttributes:attributeDict];

```
### 绘制图片

```
/**
 * 绘制图片
 * drawAtPoint 默认绘制的内容尺寸和图片的尺寸一样大
 *
 */

- (void)drawImage {

    UIImage *image = [UIImage imageNamed:@"wc_dk"];
    UIImage *image1 = [UIImage imageNamed:@"wc_hq"];

    [image drawAtPoint:CGPointMake(10, 90)];
    [image1 drawAtPoint:CGPointMake(70, 80)];

    // 绘制在控件内部，和控件大小一致
    [image1 drawInRect:self.bounds];

    // 以平铺的方式绘制在控件的内部
    [image1 drawAsPatternInRect:self.bounds];
}
```

### 圆形下载进度的绘制

drawRect 方法不能手动调用，因为图形上下文不能手动创建
需要调用setNeedsDisplay方法来重绘，系统会创建图形上下文
绘制时用到的定时器，一般不使用NSTimer定时器，因为调度优先级比较低，不会被准时带,界面会出现卡顿的现象
CADisplayLink 每次刷新屏幕的时候就会调用，1s刷新 60 次
setNeedsDisplay调用此方法时并不是立即调用drawRect方法，只是给给当前控件添加刷新标记，直到下一次屏幕刷新的时候才调用drawRect 方法，正好与CADisplayLink 方法一致，所以界面流畅不会卡顿
图形上下文状态栈

UIBezierPath图形上下文和CGContextRef的图形上下文不是同一个图形上下文

可以将贝塞尔路径添加到图形上下文中

也可以保存当前的图形上下文，CGContextSaveGState(ctx);
在下一次使用时可以恢复，CGContextRestoreGState(ctx);
画两种不同状态的线条

```
/**
* 图形上下文栈的理解
* 画两条状态不同的线段
*/

// 获取上下文
CGContextRef ctx = UIGraphicsGetCurrentContext();

// 第一条
UIBezierPath *path = [UIBezierPath bezierPath];
[path moveToPoint:CGPointMake(10, 60)];
[path addLineToPoint:CGPointMake(240, 60)];

// 把路径添加到上下文
CGContextAddPath(ctx, path.CGPath);

// 保存上下的状态
CGContextSaveGState(ctx);

// 设置当前状态
[[UIColor greenColor] setStroke];
//此时设置path 的状态无效，需要设置当前上下文的状态
path.lineWidth = 5.0;
CGContextSetLineWidth(ctx, 5.0);

// 渲染上下文
CGContextStrokePath(ctx);


// 第二根

// 可使用第一根的路径，因为每次绘制完成之后都会清除
path = [UIBezierPath bezierPath];
[path moveToPoint:CGPointMake(60, 10)];
[path addLineToPoint:CGPointMake(60, 240)];

// 添加路径到上下文 需要转换为CGPath
CGContextAddPath(ctx, path.CGPath);

// 还原之前保存的上下文状态
CGContextRestoreGState(ctx);
CGContextStrokePath(ctx);
图形上下文矩阵

// 使用矩阵

    // 获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 使用贝塞尔路径
    UIBezierPath *ovalPath = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(-150, -100, 300, 200)];
    [[UIColor cyanColor] setFill];



    // 以上绘制的图形有一部分看不到，通过对上下文矩阵操作来实现，平移上下文
    // 操作矩阵必须在添加路径之前

    // 平移
    CGContextTranslateCTM(ctx, 150, 100);

    // 缩放
    CGContextScaleCTM(ctx, 0.5, 0.5);

    // 旋转
    // 可以实现点无法计算时的效果
    CGContextRotateCTM(ctx, M_PI_4);



    // 添加路径到上下文
    CGContextAddPath(ctx, ovalPath.CGPath);

    // 操作矩阵放在添加路径之后无效果
	//  CGContextTranslateCTM(ctx, 150, 100);

    CGContextFillPath(ctx);

    // 正常的绘制椭圆
//    UIBezierPath *ovalPath = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(50, 20, 200, 100)];
//    ovalPath.lineWidth = 4.0;
//    [[UIColor magentaColor] setFill];
//    [[UIColor yellowColor] setStroke];
//    [ovalPath stroke];
//    [ovalPath fill];

```