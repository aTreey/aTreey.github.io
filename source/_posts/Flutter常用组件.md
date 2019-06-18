## flutter 常用组件

## 组件

- StatefulWidget
    - 具有可变状态的窗口部件，使用应用时可随时变化，例如进度条
- StatelessWidget 
    - 不可变窗口部件
    
## Text 组件

```
//  Text 组件
class MyText extends StatelessWidget{
  @override
  Widget build(BuildContext context){
    return Text('reacctNative、 flutter 学习之路是非常坚信不疑的，作为一个程序员，因该对于技术有着追求，不断的提高自己才是王道，可是道理都懂依然过不好这一生',
      maxLines: 2,
      overflow: TextOverflow.fade,
      textAlign: TextAlign.center,
      style: TextStyle(
        fontSize: 20.0,
        color: Colors.deepOrange,
        decoration: TextDecoration.underline,
        decorationColor: Colors.redAccent,
        decorationStyle: TextDecorationStyle.dotted
      ),
    );
  }
}
```

## COntainer 组件

```

// Container容器组件
class MyContainer extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text('Container 容器中有文字',
        style: TextStyle(
          backgroundColor: Colors.cyan,
        ),
      ),
      alignment: Alignment.center, // 容器中内容对其方式
      width: 300.0,
      height: 200.0,
      padding: const EdgeInsets.all(10), // 容器内边距
      margin: const EdgeInsets.fromLTRB(10, 5, 10, 5), // container 和外部控件的边距
      decoration: new BoxDecoration(
        gradient: const LinearGradient(
          colors: [Colors.lightBlue, Colors.greenAccent, Colors.yellow]
        ),

        // 设置 border 的宽度
        border: Border.all(width: 5, color: Colors.black) 

      ),
    );
  }
}

```

## Image 组件

### fit属性
- BoxFit.fill:全图显示，图片会被拉伸，并充满父容器。
- BoxFit.contain:全图显示，显示原比例，可能会有空隙。
- BoxFit.cover：显示可能拉伸，可能裁切，充满（图片要充满整个容器，还不变形）。
- BoxFit.fitWidth：宽度充满（横向充满），显示可能拉伸，可能裁切。
- BoxFit.fitHeight ：高度充满（竖向充满）,显示可能拉伸，可能裁切。
- BoxFit.scaleDown：效果和contain差不多，但是此属性不允许显示超过源图片大小，可小不可大。


### 图片的混合模式
- color：是要混合的颜色，如果你只设置color是没有意义的。
- colorBlendMode: 是混合模式。

### repeat 图片填充模式
- ImageRepeat.repeat : 横向和纵向都进行重复，直到铺满整个画布。
- ImageRepeat.repeatX: 横向重复，纵向不重复。
- ImageRepeat.repeatY：纵向重复，横向不重复

```
class MyImage extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return Container(
      child: new Image.network(
        // 'https://images.xiaozhuanlan.com/photo/2019/28ed76123023e46483fcf0ae5c2ba11b.png',
        'https://p.ampmake.com/mall/product/9666f00e-f02a-46ce-9b7c-090bf6aba9aa.png',

        // 1. 设置填充模式
        // fit: BoxFit.scaleDown,
        // fit: BoxFit.none,
        // fit: BoxFit.fill,
        // fit: BoxFit.fitHeight,
        // fit: BoxFit.fitWidth,

        // 保持原图比例
        // fit: BoxFit.contain,

        // 充满整个容器，图片被裁切
        // fit: BoxFit.cover,
        // scale: 1.0,

        // 2. 图片混合
        color: Colors.red,
        colorBlendMode: BlendMode.overlay,
        
        // 3. reap
        repeat: ImageRepeat.repeat,
      ),
      width: 300.0,
      height: 200.0,
      color: Colors.lightBlue,
    );
  }
}
```

## ListView 列表组件


```
class MyListView extends StatelessWidget{
  @override
  Widget build(BuildContext contex){
    
    return ListView(
      children: <Widget>[
        new ListTile(
              leading: new Icon(Icons.camera),
              title: Text('相册'),
            ),
            
            new ListTile(
              leading: new Icon(Icons.camera_alt),
              title: Text('相机'),
            ),

            new ListTile(
              leading: new Icon(Icons.camera_enhance),
              title: Text('相机1'),
            ),

            new ListTile(
              leading: new Icon(Icons.camera_front),
              title: Text('相机2'),
            ),

            new ListTile(
              leading: new Icon(Icons.camera_rear),
              title: Text('相机3'),
            ),
      ],
    );
  }
}
```

### ListView.builder 适合列表项比较多（或者无限）的情况

```
class MyListViewBuilder extends StatelessWidget {
  
  @override
  Widget build(BuildContext context){
    return ListView.builder(
      itemCount: 100, // 数量
      itemExtent: 40, // Tile 高度
      itemBuilder: (BuildContext context, int index){
        return ListTile(title: Text("$index"));
      }
    ); 
  }
}
```

### 横向列表

```
class MyHorizontalListView extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      height: 100,
      child: ListView(
        // 设置滚动方式
        scrollDirection: Axis.horizontal,
        children: <Widget>[
          new Container(
            width: 180.0,
            color: Colors.green,
          ),
          new Container(
            width: 180.0,
            color: Colors.yellow,
          ),
          new Container(
            width: 180.0,
            color: Colors.blue,
          ),
          new Container(
            width: 180.0,
            color: Colors.cyan,
          ),
          new Container(
            width: 180.0,
            color: Colors.purple,
          ),
        ],
      ),
    );
  }
}
```

### 动态列表和构造方法的创建

```
class MyDynamicListView extends StatelessWidget {
  // 声明参数
  final List<String> items;
  // 构造方法
  MyDynamicListView({Key key, @required this.items}):super(key:key);
  @override
  Widget build(BuildContext context) {
    return Container(
      child: new ListView.builder(
          itemCount: items.length,
          itemBuilder: (context, index){
            return ListTile(
              title: new Text('通过构造方法创建的动态列表${items[index]}'),
            );
          },
        ),
    );
  }
}
```
## GridView 网格列表的使用