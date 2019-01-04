Charles 抓包


### 原理

简单理解如下：

- 正常请求

	客户端 ——> 服务器


- Charles 抓包
						
	客户端 ——>  代理 （Charles）——> 服务器

### 安装

[Charles官网](https://www.charlesproxy.com/)

破解版本请自行百度

**重要说明安装之前请暂时关闭电脑的VPN**

**重要说明安装之前请暂时关闭电脑的VPN**

**重要说明安装之前请暂时关闭电脑的VPN**

- 启动Charles 

- 安装Charles证书 

	![](https://ws3.sinaimg.cn/large/006tNc79ly1fyuumd2nkoj320y0j6jtz.jpg)
	
	如果安装失败，请检查以前是否已经安装过，如果已经安装过并且已经失效，请删除失效证书后再安装

- 打开钥匙串--> 选择Charles CA -> 双击证书 --> 信任证书

	![](https://ws3.sinaimg.cn/large/006tNc79ly1fyuuq2z76mj31as0ri76z.jpg)


- 设置 https 

	- Proxy -> SSL Proxying Settings -> SSL Proxying -> Add
	- Host: * 为需要过滤的域名地址,
	- *: 表示不过滤Port, 固定为443 `*`表示任意端口
	
	![](https://ws3.sinaimg.cn/large/006tNc79ly1fyuv3ii2bkj30wm0ocmye.jpg)

### Mac 抓包

- Proxy -> macOS Proxy （☑️）

	![](https://ws2.sinaimg.cn/large/006tNc79ly1fyuv7zqgaij31v00l0wik.jpg)

### 真机抓包

此时可以将 macOS Proxy 选项中的 ✅ 去掉，

- Mac与iPhone连接必须同一网络
- 设置代理端口号
	![](https://ws1.sinaimg.cn/large/006tNc79ly1fyuve5e6mbj31240u0wnu.jpg)
	
- 手机安装证书

	- 安装手机证书
		
		![](https://ws3.sinaimg.cn/large/006tNc79ly1fyuvn5p8ijj31zi0iuq5h.jpg)
		
	- 安装完成后弹窗
	
		![](https://ws4.sinaimg.cn/large/006tNc79ly1fyuvln2bycj31b20cq0tz.jpg)

	
- 查看mac 地址，为手机设置代理

	1. 根据安装手机证书后的弹窗显示IP配置
	
	2. Charles --> Help --> Loca IP Address
	
		![](https://ws4.sinaimg.cn/large/006tNc79ly1fyuvgh515wj30uo0k0dhh.jpg)
		
	3. 打开电脑网络设置查看
	
		![](https://ws2.sinaimg.cn/large/006tNc79ly1fyuvic8h7vj30yo0u0dl3.jpg)


	- 设置代理

		第一步
		![](https://ws1.sinaimg.cn/large/006tNc79ly1fyuvzrnksjj30oo0g8t96.jpg)
		
		第二步
		![](https://ws3.sinaimg.cn/large/006tNc79ly1fyuvztfs46j30pu16uq4f.jpg)
		
		第三步
		![](https://ws4.sinaimg.cn/large/006tNc79ly1fyuvzv0wdzj30q216wq46.jpg)
	
 	
- 手机信任证书 
 - 设置 --> 通用 --> 关于本机 --> 证书信任设置

 	![](https://ws3.sinaimg.cn/large/006tNc79ly1fyuvzqy9shj31310u0doe.jpg)

### 模拟器抓包

- 安装模拟器证书

	![](https://ws4.sinaimg.cn/large/006tNc79ly1fyux3vn7cbj31d80imaij.jpg)
	
- 勾选 `macOS Proxy` 选项

	![](https://ws4.sinaimg.cn/large/006tNc79ly1fyux3cejcij30e40j4gmi.jpg)


如果失败，请先关闭模拟器，重新启动Charles 再打开模拟器
	
### 使用浏览器打开网页提示不是私密链接

[解决办法](https://blog.csdn.net/qq_31411389/article/details/79615688)