RN学习中遇到的错误总结

### 执行 npm install 时报错

#### 错误1. `Unexpected end of JSON input while parsing near '...er":"0.4.0"},"bin":{"'`

本地有`package.json`文件，执行 npm install 报错，或者clone 的项目执行npm install 也报同样的错误, 可能是因为缓存导致，

解决办法：

1. 设置镜像源，以前可能大部分都是用的淘宝

	```
	npm set registry https://registry.npmjs.org/
	```

2. 清除缓存

	```
	npm cache clean –force
	```

3. 如果提示失败，使用`sudo`
	
	```
	sudo npm cache clean --force
	```
	
	
	
#### 错误2. `xcrun: error: unable to find utility "simctl", not a developer tool or in PATH`

可能是因为 Xcode 版本问题引起，XCode 偏好设置中 Command line Tools 中为选择版本问题导致（我的情况是安装了Xcode 10 beta版导致的）

解决办法：

![](https://ws3.sinaimg.cn/large/006tNbRwly1fut616x7ivj31840uc77y.jpg)


```
The following build commands failed:
        CompileC /Users/liepin/LiePinWorkspace/RNDemo/GitHubPopular/ios/build/Build/Intermediates.noindex/RCTWebSocket.build/Debug-iphonesimulator/RCTWebSocket.build/Objects-normal/x86_64/RCTSRWebSocket.o RCTSRWebSocket.m normal x86_64 objective-c com.apple.compilers.llvm.clang.1_0.compiler
(1 failure)
Installing build/Build/Products/Debug-iphonesimulator/GitHubPopular.app
An error was encountered processing the command (domain=NSPOSIXErrorDomain, code=2):
Failed to install the requested application
An application bundle was not found at the provided path.
Provide a valid path to the desired application bundle.
Print: Entry, ":CFBundleIdentifier", Does Not Exist

Command failed: /usr/libexec/PlistBuddy -c Print:CFBundleIdentifier build/Build/Products/Debug-iphonesimulator/GitHubPopular.app/Info.plist
Print: Entry, ":CFBundleIdentifier", Does Not Exist

```

#### 错误3. `Error: Cannot find module '../lib/utils/unsupported.js'`

安装的node 版本不是稳定的版本，需要删除后重新安装

```
sudo rm -rf /usr/local/lib/node_modules/npm
brew reinstall node
```

### 已有项目集成RN


报错1. 文件路径错误，查看你的`node_modules` 文件夹是在当前目录还是上级目录

	[!] No podspec found for `React` in `../node_modules/react-native

- `../` 是指父级目录
	
- `./` 是指当前目录
	
	
报错 2. yoga Y 大小写为问题

	[!] The name of the given podspec `yoga` doesn't match the expected one `Yoga`_modules/react-native/ReactCommon/yoga"


### pod install 之后

#### 报错 1. 

![](https://ws2.sinaimg.cn/large/006tNbRwly1fuwfgkx6z1j31kw10nu0x.jpg)

解决办法： pod 'React', :subspecs 中加入 'CxxBridge', 'DevSupport'
	
	pod 'React', :path => './node_modules/react-native', :subspecs => [
    'Core',
    'RCTText',
    'RCTNetwork',
    'RCTWebSocket', # 这个模块是用于调试功能的
    'CxxBridge', # Include this for RN >= 0.47
    'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43
]


#### 报错 2. 

`RCTAnimation/RCTValueAnimatedNode.h' file not found`

解决方法：

[修改头文件导入方式](https://github.com/facebook/react-native/issues/13198)


