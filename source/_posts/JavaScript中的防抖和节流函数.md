---
title: JavaScript中的防抖和节流函数
date: 2023-01-31 15:24:38
tags: JavaScript
categories: JavaScript
comments: false
---

### 防抖

函数防抖是指在事件被触发n秒后再执行回调，如果在这n秒内事件又被触发，则重新计时。这可以使用在一些点击请求的事件上，避免因为用户的多次点击向后端发送多次重复请求。

#### 应用场景

- 按钮提交场景：防止多次提交按钮，只执行最后提交的一次
- 服务端验证场景：表单验证需要服务端配合，只窒执行一段连续的输入事件的最后一次，还有搜索联想词功能类似生存环境请用lodash.debounce

#### 防抖函数的实现

```javascript
function debounce(fn, wait) {
    let timer = null;
    return function() {
        let context = this,args = [...arguments];
        // 如果此时存在定时器的话，则取消之前的定时器重新计时
        if (timer) {
            clearTimeout(timer);
            timer = null;
        }
        
        // 设置定时器，使事件间隔指定时间后执行
        timer = setTimeout(() => {
            fn.apply(context, args);
        }, wait)
    }
}
```

### 节流

函数节流是指规定一个单位事件，在这个单位时间内，只能有一次触发事件的回调函数执行，如果同一个单位时间内某事件被触发多次，只能由一次能生效。节流可以使用在scroll函数的事件监听上，通过事件节流来降低事件调用频率。

#### 应用场景

- 拖拽场景：固定时间内只执行一次，防止超高频次触发位置变动
- 缩放场景：监控浏览器resize
- 动画场景：避免短时间内多次触发动画引起性能问题

#### 节流函数的实现

时间戳版

```javascript
function throttle(fn, delay) {
    let preTime = Date.now();
    return function() {
        let context = this,args = [...arguments],nowTime = Date.now();
        // 如果两次时间间隔超过了指定时间，则执行函数。
        if (nowTime - preTime >= delay) {
            preTime = Date.now();
            return fn.apply(context, args);
        }
    }
}
```

定时器版

```javascript
function throttle(fun, wait) {
    let timeout = null;
    return function() {
        let context = this;
        let args = [...arguments];
        if (!timeout) {
            timeout = setTimeout(() => {
                fun.apply(context, args)
                timeout = null
            }, wait)
        }
    }
}
```
