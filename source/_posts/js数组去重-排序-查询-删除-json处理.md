---
title: js数组去重-排序-查询-删除-json处理
date: 2022-12-19 17:55:49
tags:
  - 前端开发
  - JavaScript
categories: JavaScript
comments: false
---

## 数组去重

### 1、数组对象去重-ES5

```javascript
/**
   * 对象数组去重
   * @param {any} array:数组
   * @param {any} field:去重字段
   */
function filterFun(array, field) {
    let obj = {};
    array = array.reduce((cur, next) => {
        obj[next.id] ? "" : obj[next.id] = array.push(next);
        return cur;
    }, []);//设置cur默认类型为数组,并且初始值为空的数组
    return array;
}

```

### 2、数组对象去重-ES6

```javascript
/**
* 对象数组去重,type表示对象里面的一个属性
*/
function filterFun(array, type) {
    const map = new Map()
    return array.filter(item => !map.has(item[type]) && map.set(item[type], 1))
}
```

## 排序

### 1、日期排序

```javascript
this.tableData.sort(function(a, b) {
    return b['datatime'] < a['datatime'] ? 1 : -1
})
```

### 2、数据排序

```javascript
this.tableData.sort(function(a, b) { return a-b })
```

## 查询

### 1、查询id=999的下标

```javascript
this.tableData.findIndex(item => item.id === 999)
```

### 2、查询数组对象是否存在相对于key的值

```javascript
// if 可以省略
var result = arr.some(item=>item.name==='张三')
console.log(result) // 如果arr数组对象中含有name:'张三',就会返回true，否则返回false
```

### 3、数组对象转换为只存在的id数组

```javascript
//数据案例
let result = [{name:'张三',id:1},{name:'李四',id:2},{name:'王五',id:3}] 
// 我们需要获取Id  [1,2,3]
const newResult =  result.map(item => {
    return item.id
})
console.log(newResult) // [1,2,3]
```

### 4、数组opt返回中文

```javascript
// 数据案例
typeOpt = [ { label: '见面',   value: '1'  }, {  label: '电话',   value: '2'   }, { label: '微信',   value: '3'  },  { label: 'QQ',  value: '4' }]
// 我们需要的结果是 type = '1'  or 我们显示的name 见面
// 方法封装
const typeValue = '1'

/*
* array  当前数组对象
* label  当前显示名称的key
* value  当前想匹配值的key
* val    当前value值
*/
function formatterName(array, label, value, val) {
    let name = ''
    array.forEach(item => {
        if (`${item[label]}` === `${val}`) {
            name = item[value]
        }
    })
    return name
},

// 调用
formatterName(typeOpt,'value','label',typeValue)  // 结果: 见面
```

### 5、数据过滤，返回一个新数组

```javascript
// 案例数据  
const list = [{name:'张三',age:23},{name:'李四',age:21},{name:'王五',age:18},]

const age = 18

// 数据处理
const newList = list.filter(item=>{ return item.age > age })
console.log(newList) // [{name:'张三',age:23},{name:'李四',age:21}]
```

### 6、数组对象过滤并且返回id

```javascript
// 数据
const list = [{ name: '张三', id: 23 }, { name: '李四', id: 21 }, { name: '王五', id: 18 }]

// 处理
const newList = list.filter(item => { return item.id > 18 }).map(item => { return item.id })
console.log(newList) // [23,21]
```

### 7、前端分页

```javascript
const List = [1,2,3,4,5,7]
List.slice((currentPage-1)*pagesize,currentPage*pagesize)


//翻页设置
pagesizes: [10, 20, 30,],
pagesize: 15,
currentPage: 1,
total: 0,
```

## 删除

### 1、根据下标删除数据

```javascript
this.nodeChecked.splice(index, 1)
```

## 数据处理

### 1、JSON数组集合生成树行解构

```javascript
// 案例 数据
const list = [
    { 'id': '1', 'name': '湖北省', 'parentId': '0', 'createUserId': null, 'createTime': null, 'updateUserId': null, 'updateTime': null, 'order': null, 'remark': null, 'code': null, 'whetherThereAreSubDepartments': true, 'superiorId': null, 'children': null }, { 'id': '421000000000', 'name': '荆州市', 'parentId': '1', 'createUserId': null, 'createTime': null, 'updateUserId': null, 'updateTime': null, 'order': null, 'remark': null, 'code': null, 'whetherThereAreSubDepartments': true, 'superiorId': null, 'children': null }
]

/**
* 获取xmSelect下拉菜单树 数据
* @param trrData
* @param id
*/
getTreeData(trrData, id) {
    const _this = this
    const array = []
    trrData.forEach(function(item, index) {
        if ((item.parentId || 0) == id) {
            item['children'] = _this.getTreeData(trrData, item['id'])
            array.push(item)
        }
    })
    return array
},
```

