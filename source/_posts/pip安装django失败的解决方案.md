---
title: pip安装django失败的解决方案
date: 2023-01-09 14:49:19
tags:
  - Python
  - Django
categories: Python
comments: false
---

### windows使用pip安装django包的时候安装到一半会报一大堆的错
解决方法：使用一下命令来安装，切换安装地址，速度快而且还会将缺少的包自动安装上
```bash
pip install -i https://pypi.douban.com/simple django 
```
