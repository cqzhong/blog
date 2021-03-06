---
title: async &amp; await
urlname: cx79xc
date: 2020-05-19 19:00:00 +0800
tags: [async,await]
categories: [前端]
---

async/await 是 Javascript 编写异步程序的新方法。以往的异步方法无外乎回调函数和 Promise。但是 async/await 建立于 Promise 之上。

ES2017 引入了一种新的处理异步任务的方式----async 函数，它比使用 Promise API 更加简洁。

<!-- more -->

## 快速预览

- async 关键字在函数声明前使用。
- await 用于处理 Promise 对象。
- await 只能用在 async 函数中。
- async 函数总是返回一个 Promise 对象，不论函数是否 return Promise 对象。
- async/await 和 Primose 对象在本质上是一样的。

## 使用 async 和 await 的好处

- 代码更加简洁、精确。
- 因为少回调，Debug 起来更容易。
- 从 Promise then/catch 书写形式过渡过来非常自然。
- 代码更加“自上而下”，少嵌套。

## async 和 await 的基本使用

```javascript
// 将函数声明为一个 async 函数，这样就能在内部使用 await 了
async function fetchContent() {
	// 使用 await，而非 fetch.then
	let content = await fetch('/')
	let text = await content.text()

	// async 函数最终返回一个 resolved 状态的 Promise 对象，
	// Promise 对象的 then 回调方法接收的参数就是这里的 text
	return text
}

// 调用 async 函数
let promise = fetchContent.then(...)
```

## await 作用

一般情况下，await 命令后面接的是一个 Promise 对象，等待 Promise 对象状态发生变化，得到返回值，但是也可以接任意表达式的返回结果，来看个例子

```javascript
function a() {
  return "a";
}
async function b() {
  return "b";
}
const c = await a();
const d = await b();
console.log(c, d); // 'a' 'b'
```

由例子可以看到 `await` 后面不管接的是什么表达式，都能等待到结果的返回:

1. 当等到不是 Promise 对象时，就将等到的结果返回
1. 当等到的是一个 Promise 对象时，会阻塞后面的代码，等待 Promise 对象状态变化，得到对应的值作为 await 等待的结果，这里的阻塞指的是 async 内部的阻塞，async 函数的调用并不会阻塞

### async 函数总是返回一个 Promise 对象，不论函数是否 return Promise 对象。

```javascript
async function cook() {
  console.log("开始做饭");
  return "开始切菜";
}

cook().then((res) => console.log(res));
print();
console.log("first");

/*
  // 打印结果
  开始做饭
  first
  开始切菜
*/

// 执行cook()
cook();
/*
  开始做饭
  Promise {<resolved>: "开始切菜"}
    __proto__: Promise
      catch: ƒ catch()
      constructor: ƒ Promise()
      finally: ƒ finally()
      then: ƒ then()
      Symbol(Symbol.toStringTag): "Promise"
      __proto__: Object
        [[PromiseStatus]]: "resolved"
        [[PromiseValue]]: "开始切菜"
    */
```

### async/await 配对出现

```javascript
async function cutUp() {
  console.log("开始切菜");
  // return '切好的菜'
  Promise.reject("没刀咋切菜");
}

async function boil() {
  console.log("开始烧水");
  return "烧好的水";
}

async function cook() {
  let c = await cutUp();
  console.log("c:", c);

  let b = await boil();
  console.log("b:", b);
}
cook();
console.log("first");
/*
  开始切菜
  first
  c: undefined
  
  开始烧水
  b: 烧好的水

  Uncaught (in promise) 没刀咋切菜
*/
```

## 循环中的小问题 (可以面试题)

在写 JS 循环时，JS 提供了许多好用数组 api 接口。

forEach 就是其中一个，但是碰上了`async/await`，可能就悲剧了，得到了不是你想要的结果，来看一个例子：

### 异步执行

```javascript
function getUserInfo(id) {
  return new Promise((resolve) => {
    resolve({
      id: id,
      name: "xxx",
      age: "xxx",
    });
  });
}

const users = [{ id: 1 }, { id: 2 }, { id: 3 }];
let userInfos = [];

users.forEach(async (user) => {
  let info = await getUserInfo(user.id);
  userInfos.push(info);
});
console.log(userInfos); // []
console.log(userInfos[1]); // undefined
```

模拟获取多个用户的用户信息，然后得到一个用户信息数组。

但是很遗憾，上面的`userInfos`得到的是一个空数组，上面这段代码加上了`async/await`后，forEach 循环就变成了异步的，因此不会等到所有用户信息都请求完才打印 userInfos

想要等待结果的返回再打印，还是要回到老式的 for 循环，来看代码：

### 继发式

```javascript
function getUserInfo(id) {
  return new Promise((resolve) => {
    resolve({
      id: id,
      name: "xxx",
      age: "xxx",
    });
  });
}

const users = [{ id: 1 }, { id: 2 }, { id: 3 }];
let userInfos = [];

async function call() {
  for (user of users) {
    let info = await getUserInfo(user.id);
    userInfos.push(info);
  }
  console.log(userInfos);
  console.log(userInfos[1]); // 2 {id: 2, name: "xxx", age: "xxx"}
}
call();

/*
    0: {id: 1, name: "xxx", age: "xxx"}
    1: {id: 2, name: "xxx", age: "xxx"}
    2: {id: 3, name: "xxx", age: "xxx"}
      length: 3
      __proto__: Array(0)

    {id: 2, name: "xxx", age: "xxx"}
   */
```

上面这种写法是继发式的，也就是会等前面一个任务执行完，再执行下一个。

但是也许你并不关心执行过程，只要拿到想要的结果就行了，这时并发式的效率会更高，来看代码：

### Promise.all 并发执行，串行打印（推荐）

```javascript
function getUserInfo(id) {
  return new Promise((resolve) => {
    resolve({
      id: id,
      name: "xxx",
      age: "xxx",
    });
  });
}

const users = [{ id: 1 }, { id: 2 }, { id: 3 }];
let userInfos = [];

// 返回一组Prmise对象的数组
const promises = users.map((user) => getUserInfo(user.id));
console.log(promises);
/*
  0: Promise {<resolved>: {…}}
  1: Promise {<resolved>: {…}}
  2: Promise {<resolved>: {…}}
  */

// 并发执行 Promise.all[p1(), p2(), p3()]
Promise.all(promises).then((res) => {
  userInfos = res;
  console.log(userInfos);
  console.log(userInfos[1]); // 2 {id: 2, name: "xxx", age: "xxx"}

  /*
      0: {id: 1, name: "xxx", age: "xxx"}
      1: {id: 2, name: "xxx", age: "xxx"}
      2: {id: 3, name: "xxx", age: "xxx"}
        length: 3
        __proto__: Array(0)

    {id: 2, name: "xxx", age: "xxx"}
    */
});
```

## 模拟 async/await 实现

### 概念

Generator 函数是 ES6 提供的一种异步编程解决方案，执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。

### yield 表达式

形式上，Generator 函数是一个普通函数，但是有两个特征。一是，function 关键字与函数名之间有一个星号；二是，函数体内部使用 yield 表达式，定义不同的内部状态。

```javascript
function* foo(x) {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  return 5;
}
```

必须调用遍历器对象的 next 方法，使得指针移向下一个状态。也就是说，每次调用 next 方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个 yield 表达式（或 return 语句）为止。

换言之，Generator 函数是分段执行的，yield 表达式是暂停执行的标记，而 next 方法可以恢复执行。

```javascript
function* foo() {
  yield 1;
  yield 2;
  return 3;
}

var f = foo();
f.next(); // { value: 1, done: false }
f.next().value; // 2
f.next(); // { value: 3, done: true}
f.next(); // { value: undefined, done: true}
```

Generator 函数已经运行完毕，next 方法返回对象的 value 属性为 3，done 属性为 true，之后再执行 next(),done 都为 true,value 未 undefined

### next 方法的参数

yield 表达式本身没有返回值，或者说总是返回 undefined。
next 方法可以带一个参数，该参数就会被当作上一个 yield 表达式的返回值。

```javascript
function* foo(x) {
  var y = 2 * (yield x + 1);
  var z = yield y / 3;
  return x + y + z;
}

var a = foo(5);
a.next(); // Object{value:6, done:false}
a.next(); // Object{value:NaN, done:false}
a.next(); // Object{value:NaN, done:true}

var b = foo(5);
b.next(); // { value:6, done:false }
b.next(12); // { value:8, done:false }
b.next(13); // { value:42, done:true }
```

### Generator + Promise = 强大的异步回调方式

```javascript
function co(gen) {
  if (!gen) return;
  return new Promise((resolve, reject) => {
    var it = gen();
    try {
      function step(next) {
        if (next.done) {
          return resolve(next.value);
        } else {
          Promise.resolve(next.value).then(
            (res) => {
              return step(it.next(res));
            },
            (e) => {
              return step(it.throw(e));
            }
          );
        }
      }
      step(it.next());
    } catch (e) {
      return reject(e);
    }
  });
}

function sayhello() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(123);
      console.log(123);
    }, 3000);
  });
}

co(function* helloworld() {
  const data = yield sayhello();
  console.log(data);
  console.log(456);
});

// 123
// 123
// 456
```

参考链接: [6 个 Async/Await 优于 Promise 的方面](https://zhuanlan.zhihu.com/p/26260061)
