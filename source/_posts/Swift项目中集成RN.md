Swift已有项目中集成ReactNative

本教程针对于已经有的Swift项目集成ReactNative

### 创建新的空目录

为了不影响原有的swift项目目录结构，建议先创建一个空的文件夹，例如`SwiftRN`

![](https://ws3.sinaimg.cn/large/0069RVTdly1fuymbnmjazj30vc0bqgm4.jpg)

### 创建package.json文件

终端进入创建的`SwiftRN`文件目录下，创建package.json文件

```
touch package.json
```

##### 修改package.json文件，指定各个版本

```
{
  "name": "SwiftRN",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node node_modules/react-native/local-cli/cli.js start",
    "test": "jest"
  },
  "dependencies": {
    "react": "16.3.1",
    "react-native": "^0.56.0"
  }
}
```

##### 执行npm install 

在创建package.json所在的文件目录下执行，在此过程中可以遇到的错误可能较多

```
npm install 
```

##### 添加index.js文件

```
touch index.js
```

##### 修改文件内容

```
import {
    AppRegistry,
    Text,
    View,
} from 'react-native';

import React, { Component } from 'react';

// 用于最后在项目中测试
class SwiftRNHomeView extends Component {
    render() {
        return(
            <View style={{backgroundColor:'white'}}>
                <Text style={{fontSize: 20, marginTop: 200}}>Hellow world</Text>
            </View>
        );
    }
}


class MyApp extends Component {
    render() {
        return <SwiftRNHomeView></SwiftRNHomeView>;
    }
}

AppRegistry.registerComponent('SwiftRN', () => MyApp);
```

### 配置swift项目

将新创建的Swift文件目录下的所有文件拷贝到Swift项目所在的根目录下

![](https://ws3.sinaimg.cn/large/0069RVTdly1fuymlkt3lgj30no0giq3s.jpg)

### 使用Pod集成RN 

swift项目采用了pod管理第三方库，在Podfile文件中加入下面代码

```
pod 'React', :path => './node_modules/react-native', :subspecs => [
    'Core',
    'RCTText',
    'RCTNetwork',
    'RCTWebSocket', # 这个模块是用于调试功能的
    'CxxBridge', # Include this for RN >= 0.47，不引入的话会报错误或者警告
    'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43，不引入的话会报错误或者警告
    'RCTAnimation', 不引入的话会报错误或者警告
]


# 如果你的RN版本 >= 0.42.0，请加入下面这行，不引入的话会报错误或者警告
pod "yoga", :path => "./node_modules/react-native/ReactCommon/yoga"
pod 'DoubleConversion', :podspec => './node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
pod 'glog', :podspec => './node_modules/react-native/third-party-podspecs/glog.podspec'
pod 'Folly', :podspec => './node_modules/react-native/third-party-podspecs/Folly.podspec'
```

#### 创建桥接文件
- 如果原项目中有SwiftDemo-Bridging-Header.h类似这个文件可以跳过。
- 如果从来没有创建过桥接文件，创建一个OC的类会自动弹出是否需要创建桥接文件，直接创建即可，最后把无用的OC文件删掉

#### 引用RN头文件
在桥接文件中导入头文件

```
#import <React/RCTRootView.h>
#import <React/RCTBundleURLProvider.h>
```

#### 使用RN页面 

在模拟器中使用RN的页面, 确保在 info.plist 文件中添加了`Allow Arbitrary Loads=YES`项

- moduleName 对应的名字和index.js中 AppRegistry.registerComponent('SwiftRN', () => MyApp)注册的名字必须相同

```
import UIKit

class RNTestController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        let url = URL(string: "http://localhost:8081/index.bundle?platform=ios")!
        let rootView = RCTRootView(
            bundleURL: url,
            moduleName: "SwiftRN", //这里的名字必须和AppRegistry.registerComponent('MyApp', () => MyApp)中注册的名字一样
            initialProperties: nil,
            launchOptions: nil
        )
        view = rootView
    }
}

```
