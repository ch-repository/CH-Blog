---
title: JavaScript宏任务和微任务简单理解
date: 2023-01-28 13:02:47
tags: JavaScript
categories: JavaScript
comments: false
---

宏任务 - macro task，又称为task，包括：

- script代码块
- I/O
- xhr
- setTimeout
- setInterval
- setImmediate
- UI交互事件
- postMessage

浏览器为了能够使得JS引擎跟UI引擎有序配合执行，会在每一个宏任务执行结束后，在下一个宏任务执行前，对页面进行重新渲染，执行流程为

```markdown
macro task(JS引擎) -> render(UI引擎) -> macro task(JS引擎)
```

微任务 - micro task包括：

- Promise.then
- await
- process.nextTick
- MutaionObserver

执行流程为：

```markdown
micro task -> micro task -> ...
```

注意：

- 微任务总是在宏任务执行过程中产生的任务，它不能单独存在（微任务实际上是宏任务的一个步骤），可以理解为宏任务是微任务寄生的环境
- 在同一个层级的任务中，微任务执行优先级要高于宏任务的优先级，如果不考虑同一个层级，那么比较微任务和宏任务的执行级别毫无意义
- 在宏任务执行过程中产生的微任务，会在该宏任务结束前一次执行完毕
- 微任务实际是宏任务的一个步骤，所有微任务会在下一个宏任务之前全部执行完毕
- 在微任务执行过程中产生的微任务会直接添加到微任务队列的末端，并在下一个宏任务执行之前全部执行掉
- 如果在微任务执行过程中产生的宏任务，则会进入到宏任务队列末尾，按照宏任务顺序在后面的事件循环中执行
- process.nextTick会在当前执行站的末尾，下一次Event loop之前出发回调函数，也就是说，它指定的任务总是发生在所有异步任务之前

setTimeout

单线程运行机制，同一时间只能做一件事。不管怎样，都是要等主线线程的流程中兴完毕后才会进行，且按照setTimeout设置的顺序进行排队执行。该函数指定的任务为宏任务，优先级低于process.nextTick()

process.nextTick()

nodejs的一个异步执行函数，效率比setTimeout更高，执行顺序要早于setTimeout,在主逻辑的末尾任务队列调用之前执行。

setInterval()

setInterval定时器函数，按照指定的周期不断调用函数。等主线程执行完毕后调用。和setTimeout执行时间一致时，按照设置的顺序来执行。

process.nextTick和Promise

Node执行完所有同步任务，接下来就会执行process.nextTick的任务队列。如果你希望异步任务尽可能快地执行，那就使用process.nextTick。

根据语言规格，Promise对象的回调函数，会进入异步任务里面的“微任务”(microtask)队列。微任务队列追加在process.nextTick队列的后面，也属于本轮循环。

process.nextTick跟setImmediate区别

可以把process.nextTick理解为往已经存在的事件队列头部插入任务，而setImmediate可以理解为吧任务添加到已经存在事件队列的末端。
