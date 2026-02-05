---
title: GCD实现按钮定时亮起
date: 2016-12-05 23:59:45
tags: GCD
---

**某些时候需要按钮不能连续点击，需要在一定的时间之后才可以允许交互，或者是实现类似发送验证码的按钮效果，具体做法是采用定时器**


## 使用GCD定时器

### 创建定时器对象

	__block NSInteger time = 30; // 需要强应用

### 创建队列

	dispatch_queue_t queue = dispatch_get_global_queue(0, 0);


### 创建dispatch源(定时器)

	(dispatach_source…timer..)

- 01参数:要创建的source 是什么类型的， 

	`(DISPATCH_SOURCE_TYPE_TIMER)定时器`

- 04参数:队列 —-线程 决定block 在哪个线程中调用

代码

	dispatch_source_t _timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0, queue);

### 设置定时器

- 01参数:定时器对象

- 02参数:开始时间 (DISPATCH_TIME_NOW) 什么时候开始执行第一次任务

- 03参数:间隔时间 GCD时间单位:纳秒

- 04参数:leewayInSeconds精准度:允许的误差: 0 表示绝对精准

code:

	dispatch_source_set_timer(_timer, dispatch_walltime(NULL, 0), 1.0 * NSEC_PER_SEC, 0);


### 定时器每隔一段时间就要执行任务(block回调)

	dispatch_source_set_event_handler(_timer, ^{
	    if (time <= 0) {
	        dispatch_source_cancel(_timer);
	        dispatch_async(dispatch_get_main_queue(), ^{
	
	            // 设置按钮的样式
	            [self.button setTitle:@"重新获取验证码" forState:UIControlStateNormal];
	            [self.button setTitleColor:[UIColor redColor] forState:UIControlStateNormal];
	            [self.button setUserInteractionEnabled:YES];
	        });
	    } else {
	
	        NSInteger seconds = time;
	        dispatch_async(dispatch_get_main_queue(), ^{
	            [self.button setTitle:[NSString stringWithFormat:@"重新发送(%.2ld)", seconds] forState:UIControlStateNormal];
	            [self.button setTitleColor:[UIColor lightGrayColor] forState:UIControlStateNormal];
	            [self.button setUserInteractionEnabled:NO];
	        });
	        time--;
	    }
	});


### 启动定时器(默认是停止的)

	dispatch_resume(timer);
