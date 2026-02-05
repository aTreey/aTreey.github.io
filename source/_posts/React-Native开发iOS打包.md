# React-Native开发iOS打包


## 打jsbudnle包

#### 创建打包后存放文件目录

- cd 到 React-Native 项目根目录下 并且 进入 ios 文件目录下
- 创建 bundle 文件夹
	
	```
	mkdir bundle 
	```
	
#### 配置 React-Native 打包命令

- 打包命令

	```
	react-native bundle
	```

- 在React Native项目的根目录下执行命令

	```
	react-native bundle --entry-file index.js --platform ios --dev false --bundle-output ./ios/bundle/index.jsbundle --assets-dest ./ios/bundle
	```
	
	- `--entry-file`： ios或者android入口的js名称，比如 `index.js`

	- `--platform`： 平台名称(ios或者android)
	
	- `--dev`： 设置为`false`的时候将会对`JavaScript`代码进行优化处理。
	
	- `--bundle-output`： 生成的jsbundle文件的名称和路径，比如 `./ios/bundle/index.jsbundle`
	
	- `--assets-dest`： 图片以及其他资源存放的目录


#### 使用打包命令

- 将打包命令添加到 `packger.json` 中

	```
	"scripts": {
	    "start": "node node_modules/react-native/local-cli/cli.js start",
	    "test": "jest",
	    "bundle-ios": "node node_modules/react-native/local-cli/cli.js bundle --entry-file index.js --platform ios --dev false --bundle-output ./ios/bundle/index.jsbundle --assets-dest ./ios/bundle"
  },
	```
	
- 再次打包只需要 `React Native`项目的根目录下执行命令，不用再次输入类似上面的命令

	```
	npm run bundle-ios
	```

## 集成到Xcode（iOS原生项目中）	

- 添加已经打好的`.jsbundle` 离线包

	![](https://ws1.sinaimg.cn/large/006tNbRwly1fw4adqh37aj318e130gpd.jpg)

	
- iOS 项目代码配置

	```
	private func loadReactNativeTest() {
			// debug 测试加载本地文件
        let url = URL(string: "http://localhost:8081/index.bundle?platform=ios")!
        
        // 加载已经加入的离线包
        let url2 = Bundle.main.url(forResource: "index", withExtension: "jsbundle")
        
        let rootView = RCTRootView(
            bundleURL: url,
            moduleName: "MyApp", //这里的名字要和index.js中相同
            initialProperties: nil,
            launchOptions: nil
        )
        view = rootView
    }
	
	```
	