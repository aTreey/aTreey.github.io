# Flutter中布局

## 线性布局

### Row 水平方向布局组件

- 非自适应布局：根据元素本身大小来布局
- 自适应布局：使用 Expanded 布局

非自适应布局
```
class MyRowNoChangeLayout extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Container(
         child: Row(
           children: <Widget>[
             new RaisedButton(
               onPressed: (){
                 print('点击了---- 红色');
               },

               color: Colors.red,
               child: new Text('按钮一红色'),
               
             ),

             new RaisedButton(
               onPressed: (){
                 print('点击了---- 黄色');
               },

               color: Colors.yellow,
               child: new Text('按钮一黄色'),
               
             ),
             new RaisedButton(
               onPressed: (){
                 print('点击了---- 绿色');
               },

               color: Colors.green,
               child: new Text('按钮一绿色'),
               
             )
           ],
      ),
    );
  }
}
```


自适应布局

```
class MyRowAutoLayout extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Container(
         child: Row(
           children: <Widget>[
             Expanded(
               child: new RaisedButton(
               onPressed: (){
                 print('点击了---- 红色');
               },

               color: Colors.red,
               child: new Text('按钮一红色'),
              ),
             ),

             Expanded(
               child: new RaisedButton(
               onPressed: (){
                 print('点击了---- 黄色');
               },

               color: Colors.yellow,
               child: new Text('按钮一黄色'),
               
             )
             ),
             
             Expanded(
               child: new RaisedButton(
               onPressed: (){
                 print('点击了---- 绿色');
               },

               color: Colors.green,
               child: new Text('按钮一绿色'),
               
              )
             )
           ],
      ),
    );
  }
}
```

非自动布局和自动布局混合使用

```
class MyRowLayout extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Container(
         child: Row(
           children: <Widget>[
             new RaisedButton(
               onPressed: (){
                 print('点击了---- 红色');
               },

               color: Colors.red,
               child: new Text('按钮一红色'),
              ),

             Expanded(
               child: new RaisedButton(
               onPressed: (){
                 print('点击了---- 黄色');
               },

               color: Colors.yellow,
               child: new Text('按钮一黄色'),
               
             )
             ),
             
             new RaisedButton(
               onPressed: (){
                 print('点击了---- 绿色');
               },

               color: Colors.green,
               child: new Text('按钮一绿色'),
               
              )
           ],
      ),
    );
  }
}
```

### Column 垂直方向布局组件

```
class MyColunmLayout extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Container(
        color: Colors.cyan,
         child: Column(
           crossAxisAlignment: CrossAxisAlignment.stretch,
           mainAxisSize: MainAxisSize.max,
           // 设置主轴的对齐方式
           mainAxisAlignment: MainAxisAlignment.center,
           // 设置横轴的对齐方式
           crossAxisAlignment: CrossAxisAlignment.start,
           children: <Widget>[
             new RaisedButton (
               onPressed: (){
                 print('点击了---- 红色');
               },

               color: Colors.red,
               child: new Text('按钮一红色'),
              ),
             
             new RaisedButton(
               onPressed: (){
                 print('点击了---- 绿色');
               },
               color: Colors.green,
               child: new Text('按钮一绿色'),
              ),

              new Text('文字内容',
                textAlign: TextAlign.center,
              ),
              new Text('测试文字',
                textAlign: TextAlign.right,
              )
           ],
      ),
    );
  }
}
```