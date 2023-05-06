---
title: Nuxt中使用CSS预处理器
date: 2022-12-18 21:29:20
categories: SSR
tags: Nuxt
comments: false
---

## less

安装依赖

npm

```
npm install less less-loader --save
npm install @nuxtjs/style-resources --save
```

yarn

```
yarn add less-loader less
yarn add @nuxtjs/style-resources
```

配置

添加@nuxtjs/style-resources模块和全局样式文件

```
export default { 
    ...
 modules: [
    '@nuxtjs/style-resources'
  ],
  styleResources: {
    less: '@/assets/vars.less'
  }
    ...
}
```

> 全局标量有多个的话，可以改成数组的形式
## scss

安装依赖，这里安装依赖需要指定版本，不然会报错，nuxt版本问题不兼容。

```
npm install -D sass-loader@^10 sass
```
