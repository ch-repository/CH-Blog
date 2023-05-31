---
title: iframe父子窗口通信
date: 2023-05-31 11:15:52
categories:
  - 前端开发
tags:
  - 前端
comments: false
---
## 子窗口向父窗口发送消息
window.parent.postMessage(参数1为发送的消息数据，参数2为可以接受到消息的源)
```js
 window.parent.postMessage({
	'type': '自定义消息类型',
	'value':JSON.stringify(发送的数据字段，只能是字符串类型)
}, '*')
```
## 父窗口接受消息
window.addEventListener('message', 事件, false);

```js
const iframe = document.getElementById('iframe')
window.addEventListener(
  'message',
  (e) => {
    try {
      if (e.source === iframe.contentWindow) {
        if (e.data?.type === '自定义事件类型') {
          console.log('查看发送过来的数据', e.data)
        }
      }
    } catch {}
  },
  false
)
```

## 父窗口向子窗口发送消息

Iframe.contentWindow.postMessage(消息，源)

> 注意：只有当iframe加载完毕，即onLoad完成后，才能接收到消息，所以当load完成后父窗口在去发送消息，不然发了也是白发！

```vue
<template>
	<iframe class="map" src="xxxx" @load="iframeLoad"/>
</template>

<script>
export default {
　　data(){
　　　　return {
　　　　　　loadFinish:false
　　　　}
　　},
    methods:{
        postMessage() {
　　　　　　　if(!this.loadFinish)return                                                 				this.$el.querySelector('.map').contentWindow.postMessage('主页面消息', '*');
        },
　　　　　iframeLoad(){
　　　　　　this.loadFinish = true
　　　　　}
　 } 
} </script>
```

## 子窗口接收消息

子窗口接收消息和父窗口接收消息一样的    window.addEventListener('message', 事件名, false);

```js
window.addEventListener(
  'message',
  (e) => {
    try {
      if (e.data?.type === '自定义事件类型') {
      	console.log('查看发送过来的数据', e.data)
      }
    } catch {}
  },
  false
)
```



参考文章：https://www.cnblogs.com/grow-up-up/p/16981279.html