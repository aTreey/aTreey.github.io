ReactNative 中使用 TypeScript


>从RN0.57版本开始已经可以直接支持typescript，无需任何配置。在保留根入口文件index.js的前提下，其他文件都可以直接使用.ts或.tsx后缀。但这一由babel进行的转码过程并不进行实际的类型检查，因此仍然需要编辑器和插件来共同进行类型检查

###TypeScript

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
	npm start -- --reset-cache
	```



