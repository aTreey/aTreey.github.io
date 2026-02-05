---
title: GridView组件使用.md
date: 2019-06-18 23:42:43
tags: Flutter GridView
---

## GridView 组件使用

- padding: const EdgeInsets.all(10.0) 设置边距为10
- crossAxisCount：横轴子元素的数量crossAxisCount。
- mainAxisSpacing：主轴方向的间距。
- crossAxisSpacing：横轴元素的间距。
- childAspectRatio：元素的宽高比

简单例子代码

```
class MyGridView2 extends StatelessWidget{
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Container(
      child: new GridView(
        // 设置边距
        padding: const EdgeInsets.all(10.0),
        gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
          
          // 横轴 数量
          crossAxisCount: 3,
          crossAxisSpacing: 10,
          mainAxisSpacing: 10.0,  
          // 宽高比：默认是1.0        
          childAspectRatio: 1.5,
        ),

        children: <Widget>[
          new Image.network('https://github.com/Jinxiansen/SwiftUI/raw/master/images/example/Text.png', fit: BoxFit.cover),
        new Image.network('https://p.ampmake.com/mall/product/9666f00e-f02a-46ce-9b7c-090bf6aba9aa.png', fit: BoxFit.cover),
        new Image.network('https://p.ampmake.com/mall/product/d6fdc891-eec3-47ce-b837-df726c73a9b8.png', fit: BoxFit.cover),
        new Image.network('https://p.ampmake.com/mall/product/0cccf224-57d1-4a3e-bf06-9df5fed0f4e0.png', fit: BoxFit.cover),
        new Image.network('https://p.ampmake.com/mall/product/2bf5c0d7-dc3a-45d2-ab1b-40fe737aa79a.jpg', fit: BoxFit.cover),
        new Image.network('https://p.ampmake.com/mall/product/26153760-b964-45d5-b811-7f2bf5844102.jpg', fit: BoxFit.cover),
        new Image.network('https://p.ampmake.com/mall/product/a20bb53c-b710-4b39-9607-91bef06ee54e.png', fit: BoxFit.cover),
        ],
      )
    );
  }
}
```


以前的写法
```
class MyGridView extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      child: new GridView.count(
        padding: const EdgeInsets.all(15.0),
        // 横轴 item 数量
        crossAxisCount: 3,
        // 横轴 间距
        crossAxisSpacing: 10.0,
        children: <Widget>[
          const Text('我是第一个'),
          const Text('我是第二个'),
          const Text('我是第三个'),
          const Text('我是第四个'),
          const Text('我是第五个'),
          const Text('我是第六个'),
          const Text('我是第七个'),
        ],
      ),
    );
  }
}
```