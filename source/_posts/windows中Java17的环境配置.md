---
title: pip安装django失败的解决方案
date: 2023-01-09 14:49:19
tags:
  - Java
categories: Java
---

### JDK下载
#### 压缩包（推荐）
官网下载地址：https://www.oracle.com/cn/java/technologies/downloads/#java17

http://chaohangweb.cn下载地址：http://www.chaohangweb.cn:82/jdk-17_windows-x64_bin.zip

#### 安装包
官网下载地址：https://www.oracle.com/cn/java/technologies/downloads/#java17

http://chaohangweb.cn下载地址：http://www.chaohangweb.cn:82/jdk-17_windows-x64_bin.exe

### 1、解压软件到软件目录
### 2、打开环境变量，新建一个系统变量，变量名为`JAVA_HOME`，变量值为刚才解压的文件路径
### 3、在新建一个系统变量，变量名为`CLASSPATH`，变量值为` .;%JAVA_HOME%\lib;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar`
### 4、双击打开path，添加以下代码
```bash
%JAVA_HOME%\bin
%JAVA_HOME%\jre\bin
```

完成！