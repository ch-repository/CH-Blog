---
title: ubuntu部署Nuxt3
date: 2023-05-06 15:59:33
tags:
 - 服务器
 - Nuxt3
---

### 安装pm2

```
npm i -g pm2
```

**可能会出现npm低的问题，按照以下命令升级npm**

```
npm install -g npm@9.6.5 to update
```

**检查npm是否升级成功**

```
npm -v
```

**升级成功之后重新安装pm2**

**检查是否安装成功**

```
pm2 --version
```

**设置自启动**

```
pm2 startup

pm2 save
```

### 安装nginx

**更新源**

```
sudo apt-get update
```

**安装nginx**

```
sudo apt-get install -y nginx
```

**如果忘记了nginx的程序目录，可以运行此目录查看**

```
whereis nginx
```

**Nginx如果指定默认加载**/etc/nginx/nginx.conf**的配置文件。如果要查看加载的是哪个配置文件，可以用这个命令**sudo nginx -t**或者**ps -ef | grep nginx

**nginx的常用命令**

```
sudo service nginx {start | stop | restart | reload | force-reload | status | configtest | rotate | upgrade }
```

**参考文章：**[https://juejin.cn/post/6844903954132779016](https://juejin.cn/post/6844903954132779016)

### 安装git

```
sudo apt-get install -y git
```

**检查是否安装成功**

```
git --version
```

**全局配置自己个人信息**

```
git config --global user.name '用户名'
git config --global user.email '邮箱'
```

**查看配置结果**

```
git config --global -l
```

**配置ssh**

**提示：如果 C:\Users\Administrator\.s**[sh文件](https://so.csdn.net/so/search?q=sh%E6%96%87%E4%BB%B6&spm=1001.2101.3001.7020)夹下面之前创建过公钥私钥，把里面的文件清空

```
ssh-keygen -t rsa -C "邮箱"
```

**输入完之后需要按几次回车**

**然后在github中添加公钥，就可以随意的拉取推送代码啦！**

**参考文章：**[https://blog.csdn.net/weixin_60353902/article/details/125962025](https://blog.csdn.net/weixin_60353902/article/details/125962025)

### Nuxt3部署

**拉去完之后安装依赖**

```
npm install
```

**在项目根目录打包**

```
npm run build
```

**打包完成之后通过pm2运行项目**

```
pm2 start node --name "项目名称" -- .output/server/index.mjs
```

**参考文章：**

* [https://nuxt.com/docs/getting-started/deployment](https://nuxt.com/docs/getting-started/deployment)
* [https://juejin.cn/post/6844903954132779016](https://juejin.cn/post/6844903954132779016)

### 设置nginx反向代理

**因为nuxt3默认启动端口是3000，在不改变默认端口的情况下，如果只想用户访问域名不加端口号的情况下就可以访问到该网站，可以设置反向代理，用户默认访问80端口的时候可以代理成3000端口。**

**在nginx的默认80配置文件中的 `localtion /`下面添加以下代码**

```
proxy_pass http://127.0.0.1:3001/;
```

**参考文章：**[http://www.phonegap100.com/athreadinfo_7335.html](http://www.phonegap100.com/athreadinfo_7335.html)

**反向代理完成之后出现的问题就是css和图片找不到，需要在默认配置文件中添加一些配置**

```
location ~ .*\.(js|css)?$ {
    expires 12h;
    proxy_pass http://172.16.90.232:86;
}
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)?$ {
    expires 12h;
    proxy_pass http://172.16.90.232:86;
}
```

**参考文章：**[https://www.cnblogs.com/minseo/p/15508433.html](https://www.cnblogs.com/minseo/p/15508433.html)

**不出意外的话就成功啦！**
