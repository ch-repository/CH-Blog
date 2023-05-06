---
title: JavaScript防抖&节流
date: 2022-12-18 21:31:39
categories:
  - 前端开发
  - JavaScript
tags: JavaScript
comments: false
---

> 两者的作用都是为了防止函数被多次调用
### 防抖

说明：在事件被触发n秒后在执行相应的代码，如果在这个期间内又被触发了，则重新开始计时。

代码示例：

```javascript
function debounce(fun, wait) {
	let timeout;
	return function () {
		const _that = this
		const args = arguments

		clearTimeout(timeout)
		timeout = setTImeout(() => {
			fun.apply(_that, args)
		}, wait)
	}
}
```

### 节流

在特定时间内只能触发一次函数，如果在这个时间内又触发了多次则不执行。

代码示例：

```javascript
function throttle(fun, wait) {
	let timeout
	return function () {
		const _that = this
		const args = arguments
		if (!timeout) {
			timeout = setTImeout(() => {
				timeout = null
				fun.apply(_that, args)
			}, wait)
		}
	}
}
```
