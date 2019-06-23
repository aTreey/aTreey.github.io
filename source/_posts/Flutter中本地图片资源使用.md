# Flutter中本地图片资源使用

## 新建图片文件目录

![](http://ww2.sinaimg.cn/large/006tNc79ly1g4bgdobfvvj30k80wy43y.jpg)

## 添加图片到项目
手动投入图片到新建的文件目录
## 声明图片资源

找到项目中 pubspec.yaml 文件，找到 assets: 选项 按照注释代码格式声明图片路径

```
 assets:
   - lib/assets/ideaOne.png
```

## 使用图片资源

```
Image.asset('lib/assets/ideaOne.png')
```