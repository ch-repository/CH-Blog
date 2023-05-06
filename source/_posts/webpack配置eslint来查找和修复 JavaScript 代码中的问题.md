---
title: webpack配置eslint来查找和修复 JavaScript 代码中的问题
date: 2022-12-21 14:42:12
tags:
  - webpack
  - 前端开发
categories: webpack
comments: false
---

> 需要用到`EslintWebpackPlugin`插件，该插件使用 [`eslint`](https://eslint.org/) 来查找和修复 JavaScript 代码中的问题。

## 安装插件

首先，需要安装 `eslint-webpack-plugin`

```bash
npm install eslint-webpack-plugin --save-dev
```

**注意**: 如果未安装 `eslint >= 7` ，你还需先通过` npm `安装

```
npm install eslint --save-dev
```

## 在`webpack`配置文件中添加插件

```javascript
const ESLintPlugin = require('eslint-webpack-plugin');

module.exports = {
  // ...
  plugins: [new ESLintPlugin(options)],
  // ...
};
```

> `options`是插件中的一些配置选项，具体参数内容查看[`EslintWebpackPlugin`配置参数](https://webpack.docschina.org/plugins/eslint-webpack-plugin/#options)

## `.eslintrc.js`文件

> 写好`webpack`配置文件之后进行打包操作此时是报错状态，还需要在项目根目录创建`.eslintrc.js`文件来对`eslint`进行配置

```javascript
// .eslintrc.js

module.exports = {
	root: true,
	env: {
		node: true,
		browser: true,
		es6: true,
	},
        rules: {
                'no-console': 'error'
        }
};
```

#### 配置项参数

- root - 限定配置文件的使用范围
- parser - 指定`eslint`的解析器
- parserOptions - 设置解析器选项
- extends - 指定eslint规范
- plugins - 引用第三方的插件
- env - 指定代码运行的宿主环境
- rules - 启用额外的规则或覆盖默认的规则
- globals - 声明在代码中的自定义全局变量
