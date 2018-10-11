## ReactNative 中使用 TypeScript


>从RN0.57版本开始已经可以直接支持typescript，无需任何配置。在保留根入口文件index.js的前提下，其他文件都可以直接使用.ts或.tsx后缀。但这一由babel进行的转码过程并不进行实际的类型检查，因此仍然需要编辑器和插件来共同进行类型检查


### TypeScript和JavaScript对比

[两者深度对比](https://juejin.im/entry/5a52ed336fb9a01cbd586f9f)

最大的区别：

- TypeScript是强类型语⾔和静态类型语⾔（需要指定类型）
- JavaScript是弱类型语⾔和动态类型语⾔（不需要指定类型）


### TypeScript

由微软开发的自由和开源的编程语言，是JavaScript的一个严格超集，并添加了可选的静态类型和基于类的面向对象编程


[TypeScript定义](https://zh.wikipedia.org/wiki/TypeScript)

### 安装及语法

[教程](https://www.tslang.cn/docs/home.html)

##### 安装

```
npm install -g typescript
```

##### 安装 typescript 依赖管理器 typings

```
npm install -g typings
```


##### 安装 typings 依赖

```
typings install npm~react --save
typings install dt~react-native --globals --save
```

##### 使用 yarn 安装

如果已经安装了 yarn 安装如下：

```
yarn add tslib @types/react @types/react-native
yarn add --dev react-native-typescript-transformer typescript

```

##### 创建 tsconfig.json 配置文件

- cd 到 ReactNative 项目根文件夹下

	```
	tsc --init
	```
- 修改 tsconfig.json 文件 

	如果一个目录下存在一个tsconfig.json文件，那么它意味着这个目录是TypeScript项目的根目录。 tsconfig.json文件中指定了用来编译这个项目的根文件和编译选项

	[tsconfig.json文件配置详解](https://www.tslang.cn/docs/handbook/tsconfig-json.html)
	
	[TypeScript配置文件tsconfig简析](https://github.com/hstarorg/HstarDoc/blob/master/%E5%89%8D%E7%AB%AF%E7%9B%B8%E5%85%B3/TypeScript%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6tsconfig%E7%AE%80%E6%9E%90.md)
	
	**务必设置"jsx":"react"
注意多余的注释可能会不兼容，需要移除**
	
	```
	{
	    "compilerOptions": {
	        "target": "es6",
	        "allowJs": true,
	        "jsx": "react",
	        "outDir": "./dist",
	        "sourceMap": true,
	        "noImplicitAny": false
	    },
	    
	    // "files"属性 ：指定一个包含相对或绝对文件路径的列表。 "include"和"exclude"属性指定一个文件glob匹配模式列表
	    
	    "include": [
	        "typings/**/*.d.ts",
	        "src/**/*.ts",
	        "src/**/*.tsx"
	    ],
	    "exclude": [
	        "node_modules"
	    ]
	}
	```


- cd 到项目根目录中新建存放 typescripe 源代码的文件夹


##### 建立 rn-cli.config.js 文件，使用支持typescript的transfomer 

```
module.exports = {
  getTransformModulePath() {
    return require.resolve('react-native-typescript-transformer');
  },
  getSourceExts() {
    return ['ts', 'tsx'];
  }
}
```

##### 修改 .babelrc 文件

```
{
 "presets": ["react-native"],
 "plugins": ["transform-decorators-legacy"]
}
```

##### 修改项目中文件导入

- 入口`index.js`和`App.js`的文件名请不要修改，项目根目录下新建`src`文件夹
- 在`src`文件夹下新建 `.tsx` 后缀的文件
- `.tsx` 后缀的文件引用react的写法有所区别

	```
	import * as React from 'react'
	import { Text, View } from 'react-native';
	import { Component } from 'react';
	
	export default class HelloWorld extends Component{
	    render() {
	        return (
	            <View>
	                <Text>Hello!</Text>
	                <Text>Typescript</Text>
	            </View>
	        );
	    }
	}
	```

- 在`App.js`中使用
	- `App.js` 导入方式修改为如下方式
		
		```
		import './src';
		``` 
		
	- 导入 .tsx 类型的组建
	
		```
		import HelloWorld from "./src/HelloWorld";
		```	

### 配置jsconfig

`jsconfig.json`目录中存在文件表明该目录是JavaScript项目的根目录。该jsconfig.json文件指定根文件和JavaScript语言服务提供的功能选项

[jsconfig详解](https://code.visualstudio.com/docs/languages/jsconfig)

[jsconfig.json](https://jeasonstudio.gitbooks.io/vscode-cn-doc/content/md/%E8%AF%AD%E8%A8%80/javascript.html?q=)


##### 新建 `jsconfig.json` 文件

```
touch jsconfig.json
```

##### 编辑 `jsconfig.json` 文件

```
{
    "compilerOptions": {
    "allowJs": true,
    "allowSyntheticDefaultImports": true,
    "emitDecoratorMetadata": true
    },
    "exclude": [
    "node_modules",
    "rnApp/Tools" // 项目根目录下新建的存放RN中所用的一些工具类
    ]
}

```


### package.json 文件配置

```
{
  "name": "RNProject",
  "version": "0.0.1",
  "private": true,
  "scripts": { // 通过设置这个可以使npm调用一些命令脚本，封装一些功能
    "start": "node node_modules/react-native/local-cli/cli.js start",
    "test": "jest"
  },
  "dependencies": { // 发布后依赖的包
    "@types/react": "^16.4.16",
    "@types/react-native": "^0.57.4",
    "react": "16.5.0",
    "react-native": "0.57.2",
    "react-transform-hmr": "^1.0.4",
    "tslib": "^1.9.3"
  },

  "devDependencies": { // 开发中依赖的其它包
    "babel-jest": "23.6.0",
    "jest": "23.6.0",
    "metro-react-native-babel-preset": "0.48.0",
    "react-native-typescript-transformer": "^1.2.10",
    "react-test-renderer": "16.5.0",
    "typescript": "^3.1.2"
  },


  "jest": { // JavaScript 的单元测试框架
    "preset": "react-native"
  }
}
```

##### `–save` 和 `-–save-dev` 区别

- --save参数表示将该模块写入dependencies属性，
- --save-dev表示将该模块写入devDependencies属性。

##### 添加开发依赖库

- 类型检查

	```
	npm install @types/jest --save-dev
	npm install @types/react --save-dev
	npm install @types/react-native --save-dev 

	```
- 装饰器转化核心插件

	```
	npm install babel-plugin-transform-decorators-legacy --save-dev
	```	
	
- 测试框架
	
	```
	npm install react-addons-test-utils --save-dev
	npm install react-native-mock --save-dev
	```	

##### 删除依赖库

```
npm uninstall @types/jest --save
```

```
npm uninstall @types/jest --save-dev
```

##### 彻底删除，-d 表示 devDependencies， -O 表示 optionalDependencies 中

```
npm uninstall -s -D -O @types/jest

```

### 报错汇总

##### 错误1 `Cannot find module 'react-transform-hmr/lib/index.js'`

 [解决1](https://stackoverflow.com/questions/50142561/cannot-find-module-react-transform-hmr-lib-index-js?noredirect=1&lq=1)
	
 [解决2](https://github.com/facebook/react-native/issues/21530)

- 安装 `react-transform-hmr`

	```
	npm install react-transform-hmr --save
	```

- 清除缓存重新启动服务

	```
	npm start --reset-cache
	```


