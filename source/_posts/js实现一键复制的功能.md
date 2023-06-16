---
title: js实现一键复制的功能
date: 2023-06-16 11:38:53
tags:
    - JavaScript
categories: coding
---

原生JavaScript实现复制功能，实现思路，创建一个`input`元素，给`input`元素设置`value`值，`value`值内容就是想要复制的内容。将`input`元素插入到文档当中。使用`select`方法选中当前元素的内容，然后使用`document.execCommand('copy')`将内容复制到剪贴板。最后删除`input`元素。

代码示例：
```js
function copy(text) {
    // 创建input元素
    const el = document.createElement('input')
    // 给input元素赋值需要复制的文本
    el.setAttribute('value', text)
    // 将input元素插入页面
    document.body.appendChild(el)
    // 选中input元素的文本
    el.select()
    // 复制内容到剪贴板
    document.execCommand('copy')
    // 删除input元素
    document.body.removeChild(el)

    alert('复制成功')
}

copy('内容')
```