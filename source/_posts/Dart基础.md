---
title: Dart基础
date: 2023-05-15 16:39:01
tags: Git
categories: 开发工具
comments: false
---

## 数据类型

- num 数字类型的父类，既可以是整型也可以是浮点类型
  num a = 1, b = 1.12;
- int 整型
  int a = 1;
- double 双精度类型
  double a = 1.12;
- String 字符串类型
  String a = 'test';
- Boolean 布尔类型
  Boolean a = true, b = false;
- List 类型，称为集合
  List arr = [1, 2, 3];
- Map类型
  Map map = {'key': 'value'};



dart面向对象技巧

封装、继承、多态

善于封装，大到功能模块的封装、类的封装与抽象。小到方法的封装。封装的目的在于复用和易于扩展和代码的维护。

不要在一个方法体里面堆砌太多的代码，建议小与100行

养成点点点的习惯，点点点的技巧

在一个类的时间里面，万物皆对象，1点查看对象有哪些属性和方法，2点看源码，3点探究竟

安全的调用属性和方法：

不确定对象是否为空的情况下采用`?.`的方式来调用

通过`??`的方式来设置默认值`list.length ?? 1`如果list的长度为空的情况下把长度设置为1

通过`[null, '', 0].contains(target)`这种方式来判断目标的类型
