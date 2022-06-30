---
title: Object.defineProperty使用
urlname: bx4n82
date: 2020-12-27 20:00:00 +0800
tags: [JavaScript]
categories: [前端]
---

##### [Object.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

**语法：**

```javascript
Object.defineProperty(obj, prop, descriptor);
```

<!-- more -->

**参数说明：**

1. obj：必需。目标对象
1. prop：必需。需定义或修改的属性的名字
1. descriptor：必需。目标属性所拥有的特性

**返回值：**
传入函数的对象。即第一个参数 obj；

---

针对属性，我们可以给这个属性设置一些特性，比如是否只读不可以写；是否可以被 for…in 或 Object.keys()遍历。
给对象的属性添加特性描述，目前提供两种形式：`数据描述`和`存取器描述`。
当修改或定义对象的某个属性的时候，给这个属性添加一些特性：

#### 数据描述：

```javascript
var obj = {
  test: "hello",
};
//对象已有的属性添加特性描述
Object.defineProperty(obj, "test", {
  configurable: true | false,
  enumerable: true | false,
  value: 任意类型的值,
  writable: true | false,
});
//对象新添加的属性的特性描述
Object.defineProperty(obj, "newKey", {
  configurable: true | false,
  enumerable: true | false,
  value: 任意类型的值,
  writable: true | false,
});
```

数据描述中的属性都是可选的，来看一下设置每一个属性的作用。
**value**
属性对应的值,可以使任意类型的值，默认为 undefined

```javascript
var obj = {};
//第一种情况：不设置value属性
Object.defineProperty(obj, "newKey", {});
console.log(obj.newKey); //undefined
------------------------------//第二种情况：设置value属性
Object.defineProperty(obj, "newKey", {
  value: "hello",
});
console.log(obj.newKey); //hello
```

**writable**
属性的值是否可以被重写。设置为 true 可以被重写；设置为 false，不能被重写。默认为 false。

```javascript
var obj = {};
//第一种情况：writable设置为false，不能重写。
Object.defineProperty(obj, "newKey", {
  value: "hello",
  writable: false,
});
//更改newKey的值
obj.newKey = "change value";
console.log(obj.newKey); //hello
//第二种情况：writable设置为true，可以重写
Object.defineProperty(obj, "newKey", {
  value: "hello",
  writable: true,
});
//更改newKey的值
obj.newKey = "change value";
console.log(obj.newKey); //ch
```

**enumerable**
此属性是否可以被枚举（使用 for…in 或 Object.keys()）。设置为 true 可以被枚举；设置为 false，不能被枚举。默认为 false。

```javascript
var obj = {};
//第一种情况：enumerable设置为false，不能被枚举。
Object.defineProperty(obj, "newKey", {
  value: "hello",
  writable: false,
  enumerable: false,
});
//枚举对象的属性
for (var attr in obj) {
  console.log(attr); //console不出来
}
//第二种情况：enumerable设置为true，可以被枚举。
Object.defineProperty(obj, "newKey", {
  value: "hello",
  writable: false,
  enumerable: true,
});
//枚举对象的属性
for (var attr in obj) {
  console.log(attr); //newKey
}
```

**configurable**
是否可以删除目标属性或是否可以再次修改属性的特性（writable, configurable, enumerable）。设置为 true 可以被删除或可以重新设置特性；设置为 false，不能被可以被删除或不可以重新设置特性。默认为 false。
这个属性起到两个作用：
目标属性是否可以使用 delete 删除
目标属性是否可以再次设置特性

```javascript
//-----------------测试目标属性是否能被删除------------------------
var obj = {};
//第一种情况：configurable设置为false，不能被删除。
Object.defineProperty(obj, "newKey", {
  value: "hello",
  writable: false,
  enumerable: false,
  configurable: false,
});
//删除属性
delete obj.newKey;
console.log(obj.newKey); //hello
//第二种情况：configurable设置为true，可以被删除。
Object.defineProperty(obj, "newKey", {
  value: "hello",
  writable: false,
  enumerable: false,
  configurable: true,
});
```

除了可以给新定义的属性设置特性，也可以给已有的属性设置特性

```javascript
//定义对象的时候添加的属性，是可删除、可重写、可枚举的。
var obj = {
  test: "hello",
};
//改写值
obj.test = "change value";
console.log(obj.test); //'change value'
Object.defineProperty(obj, "test", {
  writable: false,
});
//再次改写值
obj.test = "change value again";
console.log(obj.test); //依然是：'change value'
```

提示：一旦使用 Object.defineProperty 给对象添加属性，那么如果不设置属性的特性，那么 configurable、enumerable、writable 这些值都为默认的 false

```javascript
var obj = {};
//定义的新属性后，这个属性的特性中configurable，enumerable，writable都为默认的值false
//这就导致了neykey这个是不能重写、不能枚举、不能再次设置特性
//
Object.defineProperty(obj, "newKey", {});
//设置值
obj.newKey = "hello";
console.log(obj.newKey); //undefined
//枚举
for (var attr in obj) {
  console.log(attr);
}
```

### 存取器描述：

注：当使用了 getter 或 setter 方法，不允许使用 writable 和 value 这两个属性

```javascript
var obj = {};
var initValue = "hello";
Object.defineProperty(obj, "newKey", {
  get: function () {
    //当获取值的时候触发的函数
    console.log("触发get方法");
    return initValue;
  },
  set: function (value) {
    //当设置值的时候触发的函数,设置的新值通过参数value拿到
    console.log("触发set方法");
    initValue = value;
  },
});
//获取值
console.log(obj.newKey); //hello
//设置值
obj.newKey = "hhhh";
console.log(obj.newKey);

/*
 * 触发get方法
 * hello
 * 触发set方法
 * 触发get方法
 * hhhh
 */
```

以上主要使包含了 `Object.defineProperty()`的 `set,get` 的简单运用，其实，`Object.defineProperty()`还 `enumerable，value，writable，configurable` 等描述符的使用，可查阅 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
