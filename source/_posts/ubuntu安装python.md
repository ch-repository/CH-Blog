---
title: ubuntu安装python
date: 2023-05-06 20:45:12
tags:
 - 服务器
 - Nuxt3
---

**查看python指向**

```
ls -l /usr/bin | grep python
```

![](https://cdn.nlark.com/yuque/0/2023/png/28871256/1683181385855-0c3fd845-e42a-4610-90e3-467bb93cff99.png)

## 下载python

**下载地址：**[https://www.python.org/ftp/python/](https://www.python.org/ftp/python/)

![](https://cdn.nlark.com/yuque/0/2023/png/28871256/1683181416741-9ef3c986-3165-40a8-8c30-0ad7859f11ac.png)

**下载安装包**

```
sudo wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz
```

**开始安装，输入命令解压**

```
sudo tar -zxvf Python-3.8.5.tgz -C ~
cd Python-3.8.5
```

**进行初始化**

```
sudo ./configure
```

**如果出错了，如下图所示，需要安装一下编辑环境**

![](https://cdn.nlark.com/yuque/0/2023/png/28871256/1683180610832-2f23f6c7-c492-48c4-ac62-bb69d3c32a49.png)

**安装编译环境**

```
sudo apt-get install zlib1g-dev libbz2-dev libssl-dev libncurses5-dev  libsqlite3-dev libreadline-dev tk-dev libgdbm-dev libdb-dev libpcap-dev xz-utils libexpat1-dev   liblzma-dev libffi-dev  libc6-dev
```

**参考文章：**

* [https://blog.csdn.net/weixin_44105042/article/details/127087760](https://blog.csdn.net/weixin_44105042/article/details/127087760)
