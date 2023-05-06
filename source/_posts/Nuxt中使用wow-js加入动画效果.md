---
title: Nuxt中使用wow.js加入动画效果
date: 2022-12-18 21:30:17
categories: SSR
tags: Nuxt
comments: false
---

### 下载依赖

```
yarn add animate.css
yarn add wow --save
```



### 在nuxt.config.js中引入animate.css

```
export default {
  ...
  // Global CSS: https://go.nuxtjs.dev/config-css
  css: [..., 'animate.css/animate.css'],
  ...
}
```





### 在需要执行动画的vue页面添加以下代码

```
mounted () {
    this.$nextTick(() => {
      // eslint-disable-next-line nuxt/no-env-in-hooks // 这个注释是因为项目中如果添加了eslint后报错代码不执行，用这行注释屏蔽掉eslint
      if (process.browser) { // 在页面mounted生命周期里面 根据环境实例化WOW
      	// 判断是否是客户端，然后注册wow
        const { WOW } = require('wowjs')
        new WOW({ animateClass: 'animate__animated' }).init()
      }
    })
  },
```



### wowjs的默认配置

```
WOW.prototype.defaults = {
  boxClass: 'wow',
  animateClass: 'animated',
  offset: 0,
  mobile: true,
  live: true,
  callback: null,
  scrollContainer: null
};
```

| 属性/方法    | 类型   | 默认值     | 说明                         |
| ------------ | ------ | ---------- | ---------------------------- |
| boxClass     | 字符串 | 'wow'      | 需要执行动画的元素的 class   |
| animateClass | 字符串 | 'animated' | animation.css 动画的 class   |
| offset       | 整数   | 0          | 距离可视区域多少开始执行动画 |
| mobile       | 布尔值 | true       | 是否在移动设备上执行动画     |
| live         | 布尔值 | true       | 异步加载的内容是否有效       |

> 默认 animateClass是animated，但是最新版本的animate.css中的class都是以 animate__ 开头,所以我们需要把默认值 animateClass: 'animated' 改为 animateClass: 'animate__animated'，这样动画才会生效。


### 使用

在需要动画的标签class中添加wow，然后再添加一个animate.css的动画，就可以了。

> 其他配置项：
>
> - data-wow-duration：更改动画持续时间
> - data-wow-delay：动画开始前的延迟
> - data-wow-offset：开始动画的距离（与浏览器底部相关）
> - data-wow-iteration：动画的次数重复（无限次：infinite）
>
> 类名控制：
>
> <h1 class="animate__animated animate__infinite animate__bounce animate__delay-2s">Example</h1>
>
> - animate__slow：用2s执行完
> - animate__slower：用3s执行
> - animate__fast：用800ms执行完
> - animate__faster：用500ms执行完
> - animate__infinite：无限循环
