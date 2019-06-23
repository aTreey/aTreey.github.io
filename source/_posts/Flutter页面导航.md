# Flutter页面导航

## 导航栏的使用
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

### 导航栏页面跳转参数传递

商品类
```
class Product {
  final String title; // 定义商品标题
  final String description;
  // 定义构造方法
  Product(this.title, this.description);
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
```

### 导航栏页面返回反向参数传递

使用异步请求和等待

商品列表页面

```
class ProductListView extends StatelessWidget {
  // 定义变量，里面是一个对象
  final List<Product> products;
  
  // 使用构造方法 函数的方式接受参数
  ProductListView({Key key, @required this.products}):super(key:key); // 设置必参，调用父类方法

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('商品列表'),
      ),

      // 使用 ListView.builder 动态构建
      body: ListView.builder(
        itemCount: products.length,
        itemBuilder: (context, index) {
          return ListTile(
            title: Text(products[index].title),
            onLongPress: (){
              print("长按事件响应");
            },


            // 点击时传入参数
            onTap: (){
              print('点击事件');
              Navigator.push(
                context, 
                MaterialPageRoute(
                  builder: (context) => ProducterDetailView(product:products[index])
                )
              );
            },
          );
        },
      ),
    );
  }
}
```

商品详情页面点击按钮返回上一级页面，并带有参数
```
class ProducterDetailView extends StatelessWidget {
  final Product product;

  ProducterDetailView({Key key, @required this.product}):super(key:key);
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Product ${product.title} 详情页面')
      ),
      
      body: Center(
        // 有多控件
        child: Column(
          children: <Widget>[
            Text('Product ${product.title} desc ${product.description}'),
            RaisedButton(
              child: Text('back Product List'),
              color: Colors.blue,
              onPressed: (){
                Navigator.pop(context, 'selected Id = ${product.title}');
              },
            )
          ],
        ),
      ),
    );
  }
}
```
