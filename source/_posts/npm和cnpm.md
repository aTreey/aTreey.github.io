# ReactNative配置.npm 和cnpm 相关

### npm
Nodejs的包管理工具

### npm 常用命令

- 更新或卸载

	```
	npm update/uninstall moduleName
	```
- 查看当前目录下已安装的包

 	```
 	npm list
	```
- 查看全局安装的包的路径

	```
	npm root -g
	```
- 全部命令

	```
	npm help
	```

### cnpm

因为npm安装插件是从国外服务器下载，受网络影响大，可能出现异常，cnpm [淘宝镜像](https://npm.taobao.org/)

### 安装

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

### 设置镜像源

```
cnpm set registry http://registry.cnpm.taobao.com
npm set registry http://registry.cnpm.taobao.com
```


### 配置 .npmrc 文件

前端项目开发离不开安装各种npm依赖包，可以选择远程的仓库也可以选择本地的仓库，但更改仓库地址需要在安装时控制台打命令，比较麻烦， 而npmrc可以很方便地解决上面问题

### .npmrc 原理

当安装项目的依赖包时，会查找并读取项目根目录下的.npmrc文件的配置，自动指定仓库地址

- 打开 .npmrc
	
	```
	open .npmrc
	```
	
- 	编辑文件, 指定特定的库的镜像源, 比如以`@test`开头的库镜像为：`http://registry.cnpm.test.com/` 而其他的正常

	```
	@test:registry=http://registry.cnpm.lietou.com/
	registry=https://registry.npm.taobao.org/

	```
