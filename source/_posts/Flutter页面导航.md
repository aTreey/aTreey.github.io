# Flutter页面导航

程序主入口

```
class MyApp extends StatelessWidget {

  // 声明参数
  final List<String> items;

  // 构造函数
  MyApp({Key key, @required this.items}):super(key: key); // 调用父类的方法


  // 重写
  @override

  Widget build(BuildContext context) {

    // 返回一窗口
    return MaterialApp(
      title:'Flutter --',
      // 基础组件学习
      // home: MyLearningFlutterHome(),

      home: FirstScreen(),
    );
  }
}
```

第一级页面
```
class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: AppBar(
        title: new Text('第一个页面'),
      ),

      body: Center(
        child: RaisedButton(
          child: new Text('点击跳转详情页'),
          onPressed: (){
            Navigator.push(context, new MaterialPageRoute(
              builder: (context) => (new SecondScreen())
              // builder: (context){
              //   new SecondScreen();
              // }
            ));
          },
        ),
      ),
    );
  }
}
```

详情页面
```
class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('详情页面')
      ),

      body: Center(
        child: RaisedButton(
          child: Text('返回上一级页面'),

          onPressed: () => (Navigator.pop(context)),
          
          // onPressed: (){
          //   Navigator.pop(context);
          // },
        ), 
      ),
    );
  }
}