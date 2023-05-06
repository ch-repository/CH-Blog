---
title: HTML常见面试题
date: 2022-12-18 21:31:39
categories:
  - 前端开发
  - HTML
tags:
  - HTML
  - 前端开发
comments: false
---

## HTML、XML、XHTML的区别

* **HTML：超文本标记语言，是语法较为松散的，不严格的web语言**
* **XML：可扩展的标记语言，主要用于存储数据和结构，可扩展**
* **XHTML：可扩展的超文本标记语言，基于XML，作用与HTML类似，但语法更严格**

## 什么是HTML5以及和HTML的区别是什么

**概念：**

**HTML5是HTML的新标准，其主要目标是无需任何额外的插件如Flash、Silverlight等，就可以传输所有内容，它包括了动画、视频、丰富的图形用户界面等。**

**HTML5是由万维网联盟（W3C）和web Hypertext Application Techology Working Group 合作创建的HTML新版本。**

**从语义结构上看：**

* **HTML4.0：没有体现结构语义化的标签，通常都是通过 `<div id="header"></div>`来命名的，这样表示网站的头部。**
* **HTML5：在语义上有很大的优势。提供了很多新的语义化的标签。**

### 行内元素有哪些？块级元素有哪些？空（void）元素有哪些？

* **行内元素：a、b、span、img、input、select、strong**
* **块级元素：div、ul、li、dl、dt、dd、h1-5、p等**
* **空元素：br、hr、img、link、meta**

## 页面导入样式时，使用link和@import有什么区别

* **link属于HTML标签，而@import是CSS提供的**
* **页面被加载时，link会同时被加载，而@import引用的CSS会等到页面被加载完成再加载**
* **@import只在IE5以上才能识别，而link是XHTML标签，无兼容问题**
* **link方式的样式的权重高于@import的权重**

## img标签上title属性与alt属性的区别是什么

* **alt是为了在图片未能正常显示时给予文字说明。且长度必须少于100个英文字符或者用户必须保证替换文字尽可能的短。**
* **title属性为设置该属性的元素提供建议性的信息。使用title属性提供非本质的额外信息。**

## 如何理解语义化标签

**概念：**

**语义化是指根据内容的结构化，选择合适的标签，便于开发者阅读和写出更优雅代码的同时，让浏览器的爬虫和机器很好的解析。**

**语义化的好处：**

* **用正确的标签做正确的事情**
* **去掉或者丢失样式的时候能够让页面呈现出清晰的结构**
* **方便其他设备解析**
* **有利于SEO和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息，爬虫依赖与标签来确定上下文和各个关键字的权重。**
* **便于团队开发和维护，语义化更具可读性，遵循W3C标砖的团队都遵循这个标准，可以减少差异化。**

## iframe有哪些优缺点

**优点：**

* **iframe可以实现无刷新文件上传**
* **iframe可以跨域通信**
* **解决了加载缓慢的第三方内容如图表和广告的加载问题**

**缺点：**

* **iframe会阻塞主页面的onload事件**
* **无法被一些搜索引擎找到**
* **页面会增加服务器的http请求**
* **会产生很多页面，不容易管理**

## src和href有什么区别

* **src用于替换当前元素，href用于在当前文档和引用资源之间的联系**
* **src是source的缩写，指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置，而href是Hypertext Reference的缩写，指向网络资源所在位置，建立当前元素或当前文档之间的链接。**

## HTML和DOM有什么关系

**HTML是死的，只是一个字符串，而DOM是由HTML解析而来，是活的，我们可以通过JavaScript维护DOM。**

## property和attribute的区别是什么

* **property是DOM中的属性，是JavaScript里的对象**
* **attribute是HTML标签上的特性，它的值只能够是字符串**

**简单的理解就是：attribute就是DOM节点自带的属性，例如HTML中常见的id、class、title、align等，而property是这个DOM元素作为对象，其附加的内容，例如childNodes、firstChild等。**

## HTML5有哪些新特性、移除了哪些元素

**新特性：**

**HTML5现在已经不是SGML的子集，主要是关于图像、位置、存储、多任务等功能的增加**

* **拖拽释放（Drag and grop）API**
* **语义化更好的内容标签（header、nav、footer、aside、article、section）**
* **音频、视频（audio、video）**
* **画布（Canvas）API**
* **地理（Geolocation）API**
* **本地离线存储localStorage长期存储数据，浏览器关闭后数据不丢失**
* **sessionStorage的数据在浏览器关闭后自动删除**
* **表单控件（calendar、date、time、email、url、search）**
* **新的技术（websorker、websocket、Geolocation）等**

**移出元素：**

* **basefont：默认字体，不设置字体，以此渲染**
* **font：字体标签**
* **center：水平居中**
* **u：下划线**
* **big：字体**
* **strike：中横字**
* **tt：文本等宽**

## 什么是前端的结构、样式和行为分离？以及分离的好处是什么？

**结构、样式和行为分离：**

**若是将前端比作一个人来举例子，结构就是相当于是人体的骨架，样式就相当于人体的装饰，例如衣服、首饰等，行为就是相当于人做出的一系列动作。**

**在结构、样式和行为分离，就是将三者分离开，各自负责各自的内容，各部分可以通过引用来进行使用。**

**在分离的基础上，我们需要做到代码的：精简、重用、有序**

**分离的好处：**

* **代码分离，利于团队的开发和后期的维护**
* **减少维护的成本，提高可读性和更好的兼容性**

## 对HTML5有什么了解？

* **良好的移动性，以移动设备为主**
* **响应式涉及，以适应自动变化的屏幕尺寸**
* **支持离线缓存技术，webStorage本地缓存**
* **新增了canvas、video】audio等标签元素，以及特殊内容元素：article、footer、header、nav、section等，以及表单控件：calendar、date、time、email、url、search等**
* **新增webSocket/webWork技术**
* **还有新增的地理位置等**

## 如何对网站的文件和资源进行优化

* **文件合并（目的是减少http请求）**
* **文件压缩（目的是直接减少文件下载的体积）**
* **使用缓存**
* **使用cdn托管资源**
* **gizp压缩需要的js和css文件**
* **反向链接，网站外链接优化**
* **meta标签优化**

## HTML5中本地存储的概念是什么，有什么优点，与cookie有什么区别？

**HTML5的web storage的存储方式有两种：sessionStorage和localStorage**

* **sessionStorage用于本地存储一个会话中的数据，当会话结束后就会销毁**
* **和sessionStorage不同，localStorage用于持久化的本地存储，除非用户主动删除数据，否则数据永远不会过期**
* **cookie是网站为了标识用户身份而存储在用户本地终端上的数据**

**区别：**

* **从浏览器和服务器的传递来看：cookie数据始终在同源的http请求中携带，即使不需要，即cookie在浏览器和服务器间来回传递，而sessionStorage和localStorage不会自动把数据发给服务器，尽在本地保存**
* **从大小看：存储大小限制不同，cookie数据不能超过4k，只适合保存很小的数据，而sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。**
* **从数据有效期看：sessionStorage在会话关闭会立刻关闭，因此持续性不久，cookie只在设置的cookie过期时间一直有效，即使窗口或浏览器关闭，而localStorage始终有效**
* **从作用域看：sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面，而localStorage和cookie都是可以在所有的同源窗口中共享的**

## 常见的浏览器内核有哪些？

* **Trident内核：IE最先开发和使用的**
* **Webkit内核：Chrome浏览器**
* **Gecko内核：火狐浏览器**
* **Presto内核：Opera浏览器**

## Canvas是什么？怎样写Canvas？

**概念：**

**Canvas是HTML5的一个元素，它使用JavaScript在网页上绘制图形。Canvas是一个矩形区域。它的每一个像素都可以由HTML5语言来控制。使用Canvas绘制路径、框、圆、字符、和添加图像这几种方法。**

**使用方式：**

**如果要在我们的HTML文档中添加Canvas标签，我们需要ID、宽度和高度。下面是如何将基本Canvas标签写入HTML文档的示例。**

```
<canvas id="myCanvas" width="100" height="100"></canvas>
```
