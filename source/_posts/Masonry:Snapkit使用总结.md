## Masonry使用

基本使用官网Demo非常详细，以下总结的是常见问题和技巧

### 安装遇到的问题
- pod 安装后出现⚠️，项目运行报错
	```
	[UIView mas_makeConstraints:]: unrecognized selector sent to instance
	```
	[github](https://github.com/SnapKit/Masonry/issues/188)
	
	[解决方法](https://www.jianshu.com/p/3c3fc3cf1218)
	
- 使用`lastBaseline`属性`crash`

	[解决办法](https://github.com/SnapKit/Masonry/issues/353)

### 使用时注意点

- `edgesForExtendedLayout` 方法
	
	重写此方法的目的是为了确定布局位置从什么位置开始什么位置结束，
	```
	- (UIRectEdge)edgesForExtendedLayout {
    return UIRectEdgeNone;
	}
	```
	
	**重写后**
	![](https://ws1.sinaimg.cn/large/006tNc79ly1fsgcn3fmo0j30s61iyalk.jpg)
	
	**重写前**
	![](https://ws4.sinaimg.cn/large/006tNc79ly1fsgcokvpdwj30s61iy7eq.jpg)
	
- `translucent` 设置`UINavigationBar`是否半透明, 如果设置了导航栏的`style` 为`UIBarStyleBlackTranslucent` 就为true
	
	**设置为false不透明效果**
	![](https://ws4.sinaimg.cn/large/006tNc79ly1fsgg7ugz8mj30ow0nqq4h.jpg)
	
	
- `extendedLayoutIncludesOpaqueBars` 导航栏不透明条件下是否可以扩展，默认是NO不可以扩展
	
	```
   self.navigationController?.navigationBar.isTranslucent = false
    self.extendedLayoutIncludesOpaqueBars = true
	```
	
	**设置不透明，设置可以扩展此时从坐标的（0，0）点开始布局**
	![](https://ws2.sinaimg.cn/large/006tNc79ly1fsggem13mjj30ow0j83zy.jpg)	

- `inset` 的使用，不用再考虑正负值的问题
	
	```
	[yellowdView makeConstraints:^(MASConstraintMaker *make) {
        make.height.equalTo(@150);
        make.top.equalTo(self).inset(220);
        make.leading.equalTo(self).inset(10);
        make.trailing.equalTo(greenView.leading).inset(10);
        make.width.equalTo(greenView);
    }];
    ```
- `edges`的使用，约束边距更方便
 
	```
	[edgeView remakeConstraints:^(MASConstraintMaker *make) {
            make.edges.equalTo(lasView).insets(UIEdgeInsetsMake(10, 5, 5, 5));
        }];
	```
	
- `Priority` 设置约束的优先级

 	- `priorityHigh`
 	- `priorityMedium`
 	- `priorityLow`

- `makeConstraints`、`updateConstraints`、`remakeConstraints`三者区别
	
	- `makeConstraints` 添加约束
	- `updateConstraints` 更新约束，更新之前会查找一边控件已经存在的约束，没有就添加，有就更新到最新的约束
	- `remakeConstraints`删除控件以前的所有约束重新添加约束
	
- 设置控件或者某一约束的'key' 方便冲突时⚠️查找

	```
	greenView.mas_key = @"greenView";
    [greenView makeConstraints:^(MASConstraintMaker *make) {
        make.height.equalTo(yellowdView).key(@"greenView-height");
        make.top.equalTo(yellowdView).key(@"greenView-top");
        make.width.equalTo(yellowdView).key(@"greenView-width");
        make.trailing.equalTo(self).inset(10).key(@"greenView-trailing");
        // 冲突约束
//        make.leading.equalTo(greenView.trailing).inset(10).key(@"greenView-leading");
    }];
	```	

- 使用 `MASAttachKeys`批量给`view`设置key

	```
	MASAttachKeys(greenView, yellowdView);
	``` 
	
	![](https://ws3.sinaimg.cn/large/006tNc79ly1fsgalvd4l6j31a20modnp.jpg)
	
- `dividedBy`和 `multipliedBy` 约束宽高比

	```
	[topInnerView makeConstraints:^(MASConstraintMaker *make) {
        // 设置自己的宽高比是3:1
        make.width.equalTo( topInnerView.height).multipliedBy(3); // 乘因数
       
        // 设置宽高并且设置他们的优先级最低
        make.width.height.lessThanOrEqualTo(topView);
        make.width.height.equalTo(topView).priorityLow();
        
        // 设置位置
        make.top.equalTo(topView);
    }];
	```
- 多个控件批量约束
	
	```
	NSValue *sizeValue = [NSValue valueWithCGSize:CGSizeMake(100, 100)];
    [@[blueView, redView, yellowView] makeConstraints:^(MASConstraintMaker *make) {
        make.size.equalTo(sizeValue);
    }];
    
    [@[blueView, redView, yellowView] mas_makeConstraints:^(MASConstraintMaker *make) {
        make.top.equalTo(self).inset(20);
    }];
    
    [blueView makeConstraints:^(MASConstraintMaker *make) {
        make.left.equalTo(self).inset(20);
    }];
    
    [redView makeConstraints:^(MASConstraintMaker *make) {
        make.left.equalTo(blueView.right).inset(10);
    }];
    
    [yellowView makeConstraints:^(MASConstraintMaker *make) {
        make.left.equalTo(redView.right).inset(10);
    }];
	```
	
	![批量约束](https://ws1.sinaimg.cn/large/006tNc79ly1fsgaz670hvj30s61iyn2l.jpg)
	
	**以上代码也可以简化为**

	```
	[redView makeConstraints:^(MASConstraintMaker *make) {
        make.left.equalTo(blueView.right).inset(10);
    }];
    
    [yellowView makeConstraints:^(MASConstraintMaker *make) {
        make.left.equalTo(redView.right).inset(10);
    }];
    
    
    [blueView makeConstraints:^(MASConstraintMaker *make) {
        make.top.left.equalTo(self);
        make.size.equalTo(@[redView, yellowView, sizeValue]);
    }];
	
	```
	![](https://ws2.sinaimg.cn/large/006tNc79ly1fsgbhz8lw5j30s61iyte5.jpg)

## Snapkit 使用

- Swift 中`edgesForExtendedLayout:UIRectEdge` 扩展布局的边缘是一个属性，默认是`All`

	```
	self.edgesForExtendedLayout = []
	```

	在Swift中闭包参数可以使用$0、$1、$2 一次代表第一个，第二个，第三个参数

- 使用$0 来代替闭包参数
	
	```
	nullDataImageView.snp.makeConstraints {
        $0.top.equalTo(view).offset(44)
        $0.centerX.equalTo(view)
        $0.width.height.equalTo(200)
    }
	```
	
- `deactivate` 和 `activate` 使用
	
	- `activate` 激活指定的约束
	- `deactivate` 移除指定的约束

	添加约束
	
	```
	nullDataImageView.snp.makeConstraints {
        nullDataViewTopConstraint = $0.top.equalTo(view).offset(80).constraint
        nullDataViewLeftConstraint = $0.left.equalTo(view).inset(10).constraint
        $0.width.height.equalTo(200)
    }
	```
	
	移除对应约束，设置约束再激活约束
	```
	nullDataViewLeftConstraint?.deactivate()
    nullDataViewLeftConstraint?.update(inset: 100)
    nullDataViewLeftConstraint?.activate()
    UIView.animate(withDuration: 1.5) {
        self.view.layoutIfNeeded()
    }
	```

- 更新约束

	- 添加约束
	
	```
    nullDataImageView.snp.makeConstraints {
	    nullDataViewTopConstraint = $0.top.equalTo(view).offset(64).constraint
	    $0.centerX.equalTo(view)
	    $0.width.height.equalTo(200)
	}
	```
	
	- 更新约束
	
	```
	nullDataViewTopConstraint?.update(inset: 84)
    UIView.animate(withDuration: 0.5, delay: 0, options: .curveEaseOut, animations: {
        self.view.layoutIfNeeded()
    }) { (finished) in
        print("动画执行完毕")
    }
	```
	
- 添加`debug`模式中的	`key`使用`labeled()`方法
	
	```
	nullDataImageView.snp.makeConstraints {
        nullDataViewTopConstraint = $0.top.equalTo(view).offset(80).constraint
        nullDataViewLeftConstraint = $0.left.equalTo(view).inset(10).constraint
        $0.width.height.equalTo(200).labeled("width和height约束")
    }
	```
	
- 其他注意点都和`Masnory`相同
	
	