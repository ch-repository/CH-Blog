---
title: JavaScript常见面试题
date: 2023-05-06 20:40:54
categories:
  - 前端开发
  - JavaScript
tags: JavaScript
comments: false
---

## JS的数据类型

**string、number、boolean、undefined、null、symbol、bigint、object、array**

## 值类型和引用类型的区别？

**两种类型的区别是存储位置不同：**

* **值类型存储在栈内存中，占据空间小，大小固定，属于被频繁使用数据，所以放入栈中存储。**
* **引用类型存储在堆内存中，占据空间大，大小不固定。如果存在栈中，影响程序运行性能。引用类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆内存中获得实体。**

## JS的类型检测

* **typeof：判断一个变量是什么类型，undefined、object、function、boolean、string、unmber、symbol。**
* **instanceof：判断当前对象是不是某个类型**

```
const a = b instanceof Array;
```

* **Object.prototype.toString.call()，检测一个对象的类型**

```
const a = Object.prototype.toString.call("hello"); // [object String]
```

## ==和===的区别？

**==在允许强制类型转换的条件下检查值的等价性，而===是在不允许强制类型转换的条件下检查值的等价性。**

## null和undefined的区别？

* **null表示一个对象是个空值**
* **undefined表示一个变量声明了没有初始化赋值**
* **undefined的类型用typeof检查是undefined**
* **null的类型用typeof检查是object**
* **JavaScript将未赋值的变量默认值设为undefined**
* **JavaScript从来不会将变量设为null**

## DOM操作，怎样添加、移除、移动、复制、创建和查找节点？

```
// 创建新节点
createDocumentFragmend() // 创建一个DOM片段
createElement() // 创建一个具体的元素
createTextNode() // 创建一个文本节点

// 添加、移除、替换、插入
appendChild() // 添加
removechild() // 移除
replaceChild() // 替换
inserBefore() // 插入

// 查找
querySelector()
querySelectorAll()
getElementByTagName()
getElementByClassName()
getElementById()
```

## offsetWidth/offsetHeight，clientWidth/clientHeight与scrollWidth/scrollHeight的区别

* **offsetWidth/offsetHeight 返回值包含content-padding-border，效果与e.getBoundingClientRect()相同。**
* **clientWidth/clientHeight 返回值只包含content-padding，如果有滚动条，也不包含滚动条。**
* **scrollWidth/scrollHeight 返回值包含content-padding-溢出内容的尺寸。**

## 对象的原生方法

#### Object.assign()

**copy对象的可枚举属性**

```
const obj = {a: 1};
const copyObj = Object.assign({}, obj);
```

#### Object.create()

**创建新对象**

**语法：Object.create(proto, [propertiesObject])**

**参数：新创建对象的原型对象，用于指定创建对象的一些属性，eg：是否可读、是否可写，是否可以枚举etc**

#### Object.is()

**用来判断两个值是否是同一个值**

```
Object.is('hello', 'hello') // true
Object.is([], []) // false
```

#### Object.keys/Object.values

**返回指定对象的自身可枚举属性/值的数组，数组中属性名的排列顺序和使用for...in循环遍历该对象时返回的顺序一致**

```
const arr = ['a', 'b', 'c'];
Object.keys(arr) // ['a', 'b', 'c']

const obj = {0: 'a', 1: 'b', 2: 'c'};
Object.keys(obj) // ['0', '1', '2']

const obj = {0: 'a', 1: 'b', 2: 'c'};
Object.values(obj) // ['a', 'b', 'c']
```

## 数组的遍历方法

* **标准for循环**
* **forEach()**
* **for in**
* **for of**
* **map()**
* **reduce()**
* **filter()**
* **every()**
* **some()**
* **find()**
* **findIndex()**
* **keys()**
* **values()**

## 数组有哪些方法

* **splice() 插入、删除、更新**
* **sort() 数组排序**
* **pop() 删除数组最后一个元素**
* **shift() 删除数组第一个元素**
* **push() 向数组末尾添加元素**
* **unshift() 向数组开头添加元素**
* **reverse() 颠倒数组中的顺序**
* **slice()**
* **join() 数组转字符串**
* **toLocaleString() 数组转字符串**
* **concat() 合并数组，返回一个新数组**
* **indexOf() 查找数组是否存在某个元素，返回下标**
* **lastIndexOf() 从数组末尾开始查找是否存在某个元素，返回下标**
* **includes() 查找数组是否包含某个元素，返回布尔值**

## 字符串有哪些方法

* **carAt() 返回字符串字符个数**
* **substring() 返回字符串子集**
* **replace() 替换字符串**
* **toLowerCase()/toUpperCase 转换字母大小写**
* **repeat() 获取字符串重复n次**
* **startsWith() 返回布尔值，表示参数字符串是否在源字符串的头部**
* **endsWith() 返回布尔值，表示参数字符串是否在源字符串的尾部**
* **padStart()/padEnd() 填充字符串左右字符**

## 什么是JavaScript作用域链？

**全局函数无法查看局部函数的内部细节，但局部函数可以查看其上层的函数细节，直至全局细节。**

**当需要从局部函数查找某一个属性或方法时，如果当前作用域没有找到，就会向该作用域的上层去查找，直至全局函数，这种组织形式就是作用域链。**

## 讲一讲JavaScript中的闭包

**函数中有另一个函数或者对象，里面的函数或者对象都可以使用外面函数中定义的变量或者参数，这种形式叫做闭包。**

**闭包的用途：缓存数据，延长作用域链。形成私有作用域，保护里面私有变量不受外界干扰，避免全局污染。**

**缺点：耗内存，耗性能，函数中的变量不能及时释放。**

## 对this的理解

* **this总是指向函数的直接调用者**
* **如果有new关键字，this指向new出来的那个对象**
* **在事件中，this指向触发这个事件的对象，特殊的是，IE中的attachEvent中的this总是指向全局对象windiw。**
* **普通函数的this指向是在函数的执行期间绑定的**
* **箭头函数的this指向是在函数创建期间就绑定好了，指向的是创建该箭头函数所在的作用域对象**

## apply、call、bind的区别

* **apply(对象，[参数1，参数2，参数3])**
* **call(对象，参数1，参数2，参数3)**
* **bind(对象，参数1，参数2，参数3)     返回值是复制之后的这个函数**

**区别：**

* **apply、call是调用的时候改变this指向，然后返回函数执行的结果，传递参数的方式不同**
* **bind是复制一份函数并返回，并且这个函数的this执行变成了传入的第一个参数**

## 原型

#### 什么是原型

**构造函数中有一个属性prototype，是个对象，叫原型，可以叫原型对象。**

**每个对象都会在其内部初始化一个属性，就是prototype（原型）。**

**作用：**

* **共享数据，目的是节省内存空间**
* **实现继承，目的是节省内存空间**

## 原型链

#### 什么是原型链

**当对象查找一个属性的时候，如果没有在自身找到，那么就会查找自身的原型，如果原型还没有找到，那么就会继续查找原型的原型，直到找到Object.prototype的原型是，此时它的__proto__为null，查找停止。通过原型链接逐级向上的查找被称为原型链。**

## new操作符具体干了什么

1. **创建一个新对象**
2. **新对象的__proto__属性指向构造函数的原型对象**
3. **将构造函数的this指向新对象**
4. **执行构造函数内部的代码，将属性添加给该对象中的this对象**
5. **返回新对象**

## 什么是DOM事件模型

**DOM事件模型主要有三种：**

1. **事件冒泡：元素先监听到这个事件，往外传播**
2. **事件捕获：document最先监听到这个事件**
3. **DOM事件流：先捕获，再冒泡。允许在某些地方监听事件。**

## 事件委托

**绑定在父级元素，利用事件冒泡去触发父级事件处理函数的一种技巧。**

## 什么是节流和防抖

**介绍：**

* **节流：作用是在用户动作每隔一定时间执行一次回调**
* **防抖：作用是让在用户动作停止之后延迟n秒在执行回调**

## Promise

**promise是异步编程的一种解决方案，比传统的异步解决方案回调函数和事件更合理、更强大。**

**具有三种状态：**

* **pending：初始状态**
* **fulfilled：成功**
* **rejected：失败**

**优点：**

* **一旦状态改变，就不会再变，任何时候都可以得到这个结果**
* **可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数**

**缺点：**

* **无法取消promise，错误需要通过回调函数来捕获**
* **当处于pending状态时，无法得知目前进展到哪一个阶段**

## async/await

**介绍：**

* **async用于声明一个异步的function**
* **await用于等待一个异步方法执行完成**

**async/await是一种处理JS异步操作的语法糖，可以帮助我们摆脱回调地狱，编写更加优雅的代码。**

#### async

**async函数会返回一个Promise对象，如果再函数中return一个直接量，async会把这个直接量通过Promise.resolve()封装成Promise对象**

#### await

**await是等待一个async函数完成，不过按照语法说明，await等待的是一个表达式，这个表达式的计算结果是Promise对象或者其他值**

**优势：**

**让代码更容易读**

## 模块化

#### 模块化发展

**无模块化->CommonJS规范->AMD规范->CMD规范->ES6模块化**

**ES6在语言标准层面上，实现了模块化功能，成为浏览器和服务器通用的模块化解决方案。主要由两个命令构成：export和import。export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。**

**ES6在导出的时候有一个默认导出，export default，使用它导出后，在import的时候，不需要加上{}，模块名字可以随意起。该名字实际上就是个对象，包含导出模块里面的函数或者变量。**

## Ajax

#### Ajax是什么，如何创建一个Ajax？

**使用JavaScript异步获取数据，而且页面不会发生整页刷新，提高了用户体验。**

1. **创建XMLHttpRequest对象，也就是创建一个异步调用对象**
2. **创建一个新的HTTP请求，并指定该HTTP请求的方法、URL及验证信息**
3. **设置响应HTTP请求状态变化的函数**
4. **发送HTTP请求**
5. **获取异步调用返回的数据**
6. **使用JavaScript和DOM实现局部刷新**

```
// 创建要给XMLHttpRequest类型的对象，相当于打开了一个浏览器
const xht = new XMLHttpRequest()

// 打开与一个网址之间的连接，相当于在地址栏输入访问地址
xhr.open('get', 'news/list?type=gn')

// 通过连接发送一次请求，相当于回车或者点击访问发送请求
xhr.send(null)

// POST请求
// xhr.open('POST', 'login', true)
// xhr.send('username=admin&password=admin')

// 指定xhr状态事件处理函数，相当于处理网页呈现后的操作
xhr.onreadystatechange = function () {
	// 通过xhr的readystate判断此次请求的响应是否接受完成
  // 4代表done
  if (this.readyState === 4) {
    // 通过xhr的responseText获取到响应的响应体
    console.log(this)
  }
}
```

## Ajax原理

**Ajax的原理简单来说就是在用户和服务器之间加了一个中间层（Ajax引擎），通过XMLHttpRequest对象来向服务器发送异步请求，从服务器获得数据，然后用JavaScript来操作DOM而更新页面。使用户操作与服务器响应异步化。这其中最关键的异步就是从服务器获得请求数据。**

**Ajax的过程只涉及JavaScript、XMLHttpRequest和DOM。XMLHttpResquest是Ajax的核心机制。**

## 什么是同源策略？

**同源策略是浏览器的一种安全策略，所谓同源是指域名、协议、端口号完全相同，只有同源的地址才可以互相通过Ajax的方式请求。**

## 简单解释单线程、任务队列的特点？

#### 单线程

**JavaScript是浏览器用来与用户进行交互、进行DOM操作的，这也使得了它必须是单线程这一特性。**

**所谓单线程也就是一条线，一步一步走。**

#### 任务队列

**任务（消息）队列是一个先进先出的队列，它里面存放着各种任务（消息）。**

**在JavaScript中任务有两种，一种是同步任务，一种是异步任务**

* **同步任务：各个任务按照文档定义的顺序推入“执行站”中，当前一个任务执行完毕，才会开始执行下一个任务。**
* **异步任务：各个任务推入“任务队列”中，只有在当前的所有同步任务执行完毕，才会将队列中的任务“出列”执行。**

**当线程中没有执行任何同步代码的前提下才会执行异步代码。**

## JSON的了解

**是一种轻量级的数据交换格式，它是基于JavaScript的一个子集。**

**数据格式简单，易于读写，占用带宽小。**

## 简单理解面向对象

**一种编程开发思想，是过程式代码的一种高度封装，目的在于提高代码的开发效率和可维护性。**

## 什么叫优雅降级和渐进增强？

* **优雅降级：Web站点在所有新式浏览器中都能正常工作，如果用户使用的是老式的浏览器，则代码会针对旧版本进行剪辑处理，使之在旧式浏览器上以某种形式降级体验却不至于完全不能用。**
* **渐进增强：从被所有浏览器支持的基本功能开始，逐步地添加哪些只有新版本浏览器才支持的功能，向页面增加不影响基础浏览器的额外样式和功能的。当浏览器支持时，它们会自动的呈现出来并发挥作用。**
