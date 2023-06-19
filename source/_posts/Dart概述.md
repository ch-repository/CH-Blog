---
title: Dart快速入门
date: 2023-06-19 10:50:57
tags:
- Dart
categories:
- Dart
---

# Dart快速入门

## Dart数据类型

### 数字

##### num

num类型是数字类型的父类，即接受浮点类型也接受整数类型
```dart
num num1 = -1.0;
num num2 = 2;
```

##### int

int类型是num的子类，只能接受整数
```dart
int num3 = 3;
```

##### double

double类型是num的子类，只能接受浮点数，为双精度类型
```dart
double d1 = 1.68;
```

**数字类型转换的方法示例：**
```dart
num1.abs() /// 绝对值
num1.toInt() /// 转成int类型
num1.toDouble() /// 转成double类型
...依次类推
```

### 字符串

##### String

在Dart中字符串既可以用单引号也可以用双引号
```dart
String str1 = '字符串1';
String str2 = "字符串2";
```

##### 字符串拼接
```dart
/// 单引号双引号都可以，通过$的方式拼接
String str3 = 'str: $str1; str: $str2';
/// 加号的拼接
String str4 = 'str:' + str1 + 'str:' + str2
```

**字符串类型常用方法示例：**
```dart
str1.substring(0, 2); /// 字符串的截取
str1.indexOf('串'); /// 获取指定字符的位置
...  startsWith,replaceAll,contains.split等
```

### 布尔

##### bool

Dart是强bool类型检查，只有bool类型的值是true，才会被认为是true或者是false
```dart
bool success = true;
bool fail = false;
```

**布尔类型表达式示例**
```dart
success || fail; /// true
success && fail; /// false
```

### 集合

##### List

集合初始化
```dart
List list = [1,2,3,'集合'];
```

集合泛型
```dart
List<int> list2 = [1,2,3]; /// 这里的集合只能放int类型
```

**集合常见方法示例**
```dart
List list3 = [];
list3.add('添加元素'); /// 添加元素
list3.addAll(list); /// 拼接list

List list4 = List.generate(3, index => index * 2); /// 集合的生成器（generate）
```

**集合常用的遍历方式**

for循环
```dart
for(int i = 0; i < list4.length; i++) {
    print(list4[i]);
}
```

for in
```dart
for (var e in list4) {
    print(e);
}
```

forEach
```dart
list4.forEach((e) => {
    print(e);
})
```

> 集合常用方法：... removeXx,insert,sublist,indexOf等.

### map

##### Map

map是将key和value相关联的对象，key和value都可以是任何类型的对象，并且key是唯一的，如果key重复的话，后添加的key会替换掉前面的key里面的内容
```dart
Map map1 = { 'name': '张三', 'age': '18' };

Map map2 = {};
map2['name'] = '张三'
map2['age'] = '18'
```

**Map常用的遍历方式**

forEach
```dart
map2.forEach((k /* key */, v /* value */) => {
    print(k, v);
})
```

map
```dart
M2p map3 = map2.map((k, v) => {
    return MapEntry(v, k);
})
```

for in
```dart
for (var key in map3.keys) {
    print(map3["key"]);    
}
```

> map常用方法：... keys,values,remove,containsKey等.

## Dart面向对象⭐

创建的类默认都会继承Object

### 标准构造方法

```dart
class Person {
    String name;
    int age;
    Person(this.name, this.age); /// 标准构造方法 - 最普通的构造方法
    
    @override
    String toString() {
        /// 重写父类的toString方法，多态的一个体现    
    }
}
```

继承
```dart
class Student extends Person {
    String _school; /// 通过下划线来标识私有字段
    String city;
    String country;
    String name;

    /// 在初始化子类的构造方法的时候要调用父类的构造方法
    /// 通过this._school初始化自有参数
    /// name, age交给父类进行初始化
    /// city为可选参数
    /// 默认参数：首先它需要是可选参数，然后通过等号来给它赋予默认值，country为默认参数
    Student(this._school, String name, int age, { this.city, this.country = 'china' }) : super(name, age);
    
    /// 构造方法的方法体示例
    Student(this._school, String name, int age, { this.city, this.country = 'china' })
    : super(name, age) {
        print('构造方法体不是必须的');
    }
}
```

### 初始化列表

- 初始化列表跟在构造函数冒号后面
- 除了调用父类构造器之外，在子类构造器方法体之前，也可以初始化实例变量，不同的表达式之间用逗号分割
- 如果父类没有默认构造方法的话（无参的构造方法），则需要在初始化列表中调用父类的构造方法来进行初始化

**初始化列表示例**
```dart
class Student extends Person {
    String _school;
    String city;
    String country;
    String name;

    Student(this._school, String name, int age, { this.city, this.country = 'china' })
    : name = '$country - $city', super(name, age);
}
```

### 命名构造方法

**命名构造方法示例**
构造方法由`类名.方法名`组成，里面的参数可以自定义
可以实现多个构造方法，命名构造方法也可以有自己的方法体
```dart
class Student extends Person {
    String _school;
    String city;
    String country;
    String name;

    Student.cover(Student stu) : super(stu.name, stu.age); /// 命名构造方法
    
    Student.cover(Student stu) : super(stu.name, stu.age) {
        /// 方法体    
    }
}
```

### 工厂构造方法

不仅仅是构造方法，更是一种模式（单例），有时候为了返回一个之前已经创建的缓存对象，原始的构造方法已经不能满足要求那么可以使用工厂模式来定义构造方法
```dart
class Logger {
    static Logger _cache; /// static关键字用来定义静态变量
    
    /// 工厂构造方法用factory关键字来辨识，用来返回类的唯一实例
    factory Logger() {
        if (_cache == null) {
            _cache = Logger._internal();    
        }
        return _cache;
    }
    
    Logger._internal();
    
    void log(String mag) {
        print(mag);    
    }
}

Logger log1 = Logger();
Logger log2 - Logger();
log1 == log2; /// true
```

### 命名工厂构造方法

- 由factory `类名.方法名`构成
- 它可以有返回值，而且不需要将类的final变量作为参数，是提供一种灵活获取类对象的方式
```dart
class Logger {
    static Logger _cache; /// static关键字用来定义静态变量

    fectory Logger.log(Logger log) {
        return Logger(log._cache);
    }
}
```

### get/set

##### get
可以通过get方法来让外界可以获取到私有字段，也可以访问某个字段的时候做一些访问控制
```dart
class Student {
    String _name;
    
    Student(this._name);
    
    String get name => _name;
}
```

##### set

```dart
class Student {
    String _name;

    Student(this._name);

    set setName(String value) {
        _name = value;    
    }
}
```

### 静态方法

##### static

静态方法用`static`来标识
```dart
class Student {
    String _name;

    Student(this._name);

    static doPrint(String str) {
        print(str);    
    }
}

Student.doPrint(); /// 使用静态方法
```

### 抽象类/抽象方法

抽象类通过`abstract`修饰符来标识，抽象类不能有自己的实例，不能被实例化，也就是说不能创建自己的对象。抽象类在定义接口的时候非常有用
```dart
abstract class Study {
    void study(); /// 没有方法体的方法 - 抽象方法。 类里面不一定非得有抽象方法，但是一旦有抽象方法，那么类一定要标识为抽象类
    
    /// 也可以拥有有方法体的方法
}

class StudyFlutter extends Study {
    /// 必须实现继承抽象类里面的方法，不想实现的话，StudyFlutter必须也定义为抽象类
    @override
    void study() {
        /// 方法体的实现    
    }
}
```

### mixins

- 通过mixins可以为我们的类来添加一些特性
- mixins是在多个类层次结构中重用代码的一种方式
- 要使用mixins，在with关键字后面跟上一个或多个mixin的名字，多个的话用逗号分割
- mixins特征：需要有一个继承Object类的子类（不能继承其它类），没声明任何构造方法，没调用super
```dart
class Study {
    String name;
    
    String prName() {
        return name;    
    }
}

class Test extends Person with Stydy {
    Test(String name, int age) : super(name, age);
    
    @override
    prName() {
     /// 方法重写    
    }
}
```

### 常用的Dart方法类型

- 方法的构成：返回值类型 + 方法名 + 参数
- 返回值的类型可缺省，也可为void或具体的类型
- 匿名方法不需要方法名，除此之外都需要方法名
- 参数包括参数类型和参数名，参数类型可缺省（另外，参数又分为可选参数和默认参数）

定义方法示例
```dart
int sum(int val1, int val2) {
    return val1 + val2;
}
```

##### 私有方法
私有方法通过_开头命名的方法

定义私有方法示例
```dart
_learn() {
    print('私有方法');    
}
```
##### 匿名方法

匿名方法示例
```dart
anonymousFunction() {
    List<String> list = ['私有方法', '匿名方法'];
        
    list.forEach((i) {
        /// 匿名方法的方法体    
    })
}
```

### 了解Dart泛型再Flutter中的应用

##### 泛型
泛型主要是解决类、接口、方法的复用性，以及对不特定数据类型的支持。提高代码的复用性