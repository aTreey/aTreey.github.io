# Flutter中底部TabBar

## 主入口



## 自定义底部导航栏组件
```
import 'package:flutter/material.dart';

/**
 * StatefulWidget ： 具有可变状态(state)的窗口组件
 * 要根据变化状态，调整state 的值
 */


// 继承于 StatefulWidget

class BottomNavigationWidget extends StatefulWidget {
  @override
  _BottomNavigationWidgetState createState() => _BottomNavigationWidgetState();
}

// 继承于 State
class _BottomNavigationWidgetState extends State<BottomNavigationWidget> {
  // 定义icon的颜色，因为需要变化
  final _BottomNavigationColor = Colors.blue;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      bottomNavigationBar: BottomNavigationBar(
        items: [
          BottomNavigationBarItem(
            icon: Icon(
              Icons.comment,
              color: _BottomNavigationColor,
            ),
            
            title: Text(
              '社区',
              style: TextStyle(
                color: _BottomNavigationColor
              ),
            )
          ),

          BottomNavigationBarItem(
            icon: Icon(
              Icons.camera,
              color: _BottomNavigationColor,
            ),
            
            title: Text(
              '理想ONE',
              style: TextStyle(
                color: _BottomNavigationColor
              ),
            )
          ),

          BottomNavigationBarItem(
            icon: Icon(
              Icons.computer,
              color: _BottomNavigationColor,
            ),
            
            title: Text(
              '我',
              style: TextStyle(
                color: _BottomNavigationColor
              ),
            )
          )
          
        ],
      ),
      body: Center(

      ),
    );
  }
}

```