---
title: nodejs压缩包安装及环境配置
date: 2022-12-21 15:52:03
categories:
  - 前端开发
  - Node
tags: Node
comments: false
---

## 下载

> 在[Node官网](https://nodejs.org/en/)先下载好Node安装包，压缩包版本的，推荐下载稳定版本

## 安装

> 将压缩包文件解压缩到想要安装的目录

##### 新建两个文件夹`node_global`和`node_cache`，用来存放全局依赖包和缓存路径

![](https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/202212211557878.png)



##### 配置环境变量

> 将解压好的目录，根目录的路径添加到系统变量path中，就可以在任意位置使用node命令了。同时将node_global所在的目录也添加到系统变量中，这样就可以在任意位置使用全局依赖包命令了。

![](https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/202212211601223.png)

依次点击确定，这样node环境变量就算是配置好了，打开命令提示符窗口输入命令进行测试

查看node版本

```bash
node -v
```

查看npm版本

```bash
npm -v
```

![](https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/202212211603661.png)

像这样都打印出了版本号，环境变量就算是配置成功了。



##### 现在设置全局依赖包和缓存路径的位置

```bash
npm config set prefix "D:\MyApps\develops\node-v18.12.1-win-x64\node_global"
npm config set cache "D:\MyApps\develops\node-v18.12.1-win-x64\node_cache"
```

安装全局包进行测试，这里下载yarn进行测试，因为以后也会经常用到yarn的命令

```bash
npm install yarn -g
```

![](https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/202212211609406.png)

在node_global下的node_modules这里就会有一个yarn的文件夹，说明下载成功了，位置也没问题

##### 现在将node_modules的目录路径写入用户变量

![](https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/202212211611675.png)

到这里Node压缩包就安装完成了！
