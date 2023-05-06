---
title: create-react-app构建项目慢的问题
date: 2022-12-21 20:52:34
tags: node
categories: 前端开发
comments: false
---

npm默认的源是`https://registry.npmjs.org/`，地址在墙外所以会比较慢，可以采用换源的方式，换成国内淘宝的资源。
```bash
npm config set registry https://registry.npm.taobao.org
```

检查是否切换成功
```bash
npm config get registry
```