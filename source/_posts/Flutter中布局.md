# Flutter中布局

## 线性布局

### 主轴、副轴

- main轴：如果用column组件，垂直就是主轴，如果用Row组件，那水平就是主轴

- cross轴：幅轴，是和主轴垂直的方向。用Row组件，那垂直就是幅轴，Column组件，水平方向就是副轴

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
## 层叠布局
```
// stack层叠布局

class MyStackLayout extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // 返回一个 Stack 布局
    return new Stack(
      // 设置位置
      alignment: const FractionalOffset(0.5, .9),
     // 圆角组件
      children: <Widget>[
        new CircleAvatar(
          backgroundImage: new NetworkImage('https://p.ampmake.com/mall/product/26153760-b964-45d5-b811-7f2bf5844102.jpg'),
          // backgroundImage: new Image.network('https://p.ampmake.com/mall/product/26153760-b964-45d5-b811-7f2bf5844102.jpg'),
          radius: 100.0,
        ),
        
        // 容器组件
        new Container(
        // 设置容器掩饰
          decoration: BoxDecoration(
            color: Colors.blue,
            // border 
            border: Border.all(
              color: Colors.red,
              width: 3,
            ),
          ),
          
          padding: const EdgeInsets.all(10.0),
          
          child: new Text('stack 布局',
            textAlign: TextAlign.center,
            maxLines: 1,
            // 文字样式
            style: TextStyle(
              color: Colors.yellow,
              textBaseline: TextBaseline.ideographic,
              // 下划线
              decoration: TextDecoration.underline,
              // 下划线颜色
              decorationColor: Colors.orange,
              // 下划线样式
              decorationStyle: TextDecorationStyle.double,
            ),
          ),
        )
      ],
      
    );
  }
}
```

### Positioned组件 

- bottom: 距离层叠组件下边的距离
- left：距离层叠组件左边的距离
- top：距离层叠组件上边的距离
- right：距离层叠组件右边的距离
- width: 层叠定位组件的宽度
- height: 层叠定位组件的高度

```
class MyMoreStackLayout extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Stack(
      children: <Widget>[
        new CircleAvatar(
          backgroundImage: NetworkImage('https://p.ampmake.com/mall/product/26153760-b964-45d5-b811-7f2bf5844102.jpg'),
          radius: 100.0,
        ),

        new Positioned(
          top: 5,
          left: 50,
          // right: 5,
          width: 100,
          child: new RaisedButton(
            onPressed: (){
              print("多个组件层叠布局");
            },
            
            color: Colors.deepOrange,
            child: Text('测试按钮'),
          ),
        ),
        
        new Positioned(
          bottom: 10,
          right: 10,
          left: 10,
          child: new Text('flutter 多个试图 stack布局',
            maxLines: 1,
            textAlign: TextAlign.center,
            style: TextStyle(
              color: Colors.lightGreen,
              fontSize: 20,
              fontStyle: FontStyle.italic
            ),
          )
        )
      ],
    );
  }
}
```
### Card 布局

```
class MyCardLayout extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Card(
      child:  Column(
        children: <Widget>[
          new ListTile(
            title: new Text('card 布局1'),
            subtitle: new Text('子标题1', 
              textAlign: TextAlign.right,
              style: TextStyle(
                color: Colors.lightGreen
              ),
            ),
            leading: new Icon(Icons.account_balance, color: Colors.purple)
          ),
          
          new Divider(
            color: Colors.cyan
          ),

          new ListTile(
            title: new Text('card 布局2'),
            subtitle: new Text('子标题2', 
              textAlign: TextAlign.right,
              style: TextStyle(
                color: Colors.lightGreen
              ),
            ),
            leading: new Icon(Icons.account_box, color: Colors.yellow)
          ),

          new Divider(
            color: Colors.red,
          ),

          new ListTile(
            title: new Text('card 布局3'),
            subtitle: new Text('子标题3', 
              textAlign: TextAlign.right,
              style: TextStyle(
                color: Colors.lightGreen
              ),
            ),
            leading: new Icon(Icons.account_circle, color: Colors.red)
          ),

          new Divider(
            color: Colors.orangeAccent,
          ),

          new ListTile(
            title: new Text('card 布局4'),
            subtitle: new Text('子标题4', 
              textAlign: TextAlign.right,
              style: TextStyle(
                color: Colors.lightGreen
              ),
            ),
            leading: new Icon(Icons.camera_enhance, color: Colors.brown)
          )
        ],
      ),
    );
  }
}
```