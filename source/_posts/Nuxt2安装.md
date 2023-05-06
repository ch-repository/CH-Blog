---
title: Nuxt2安装
date: 2022-12-18 21:27:53
categories: SSR
tags: Nuxt
comments: false
---

npx

```
npx create-nuxt-app 项目名
```

yarn

```
yarn create nuxt-app 项目名
```



## 目录结构

- assets用于组织未编译的静态资源，比如less、sass、javascript。
- components用于组织应用的vue.js组件。这些组件不会像页面组件那样有asyncData方法的特性。
- layouts用于组织应用的布局组件。
- middleware目录用于存放应用的中间件。
- page用于组织应用的路由及视图。Nuxt.js会读取该目录下所有的.vue文件并自动生成对应的路由配置。
- plugins用于组织那些需要在根vue.js应用实例化之前需要运行的javascrips插件。
- static用于存放应用的静态文件，此类文件不会被Nuxt.js调用webpack进行构建编译处理。服务器启动的时候，该目录下的文件会映射至应用的根路径/下
- store用于组织应用的vuex状态树文件。Nuxt.js 框架集成了 Vuex状态树 的相关功能配置，在 `store` 目录下创建一个 index.js 文件可激活这些配置。
- nuxt.config.js文件用于组织Nuxt.js应用的个性化配置，以便覆盖默认配置。
- package.json文件用于描述应用的依赖关系和对外暴露的脚本接口。
