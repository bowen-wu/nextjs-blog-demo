---
title: 'Interview'
date: '2023-04-14'
---

## 概述

1. 概念题 => 是什么 + 怎么做 + 解决了什么问题 + 优点 + 缺点 + 怎么解决缺点

## HTML

### 如何理解 HTML 中的语义化标签

1. 语义化标签是一种写 HTML 标签的方法论
2. 实现方法是遇到标题就用 h1 到 h6，遇到段落用 p，遇到文档用 article，主要内容用 main，边栏用 aside，导航用 nav
3. 它主要是明确了 HTML 的书写规范
4. 优点在于 1. 适合搜索引擎检索 2. 适合人类阅读，利于团队维护

### HTML5 有哪些新标签

文章相关：header、main、footer、nav、section、article
多媒体相关：video、audio、svg、canvas

### Canvas 和 SVG 的区别是什么？

1. Canvas 主要是用笔刷来绘制 2D 图形的
2. SVG 主要是用标签来绘制不规则矢量图的
3. 相同点：都是主要用来画 2D 图形的
4. 不同点
    1. SVG 画的是矢量图，Canvas 画的是位图
    2. SVG 节点多时渲染慢，Canvas 性能更好一点，但写起来更复杂
    3. SVG 支持分层和事件，Canvas 不支持，但是可以用库实现

## CSS

### BFC 是什么

BFC 是 Block Formatting Context，是块级格式化上下文。以下可以触发 BFC

1. 浮动元素（float 值不为 none）
2. 绝对定位元素（position 值为 absolute 或 fixed）
3. inline-block 行内块元素
4. overflow 值不为 visible、clip 的块元素
5. 弹性元素（display 值为 flex 或 inline-flex 元素的直接子元素）

BFC 可以解决 1. 清除浮动 2. 防止 margin 合并 的问题
但是它有相应的副作用，可以使用最新的 display: flow-root 来触发 BFC，该属性专门用来触发 BFC

### 如何实现垂直居中

1. flex
2. position + transform

### CSS 选择器优先级如何确定

1. 选择器越具体，其优先级越高
2. 相同优先级，出现在后面的，覆盖前面的
3. 属性后面加 !important 的优先级最高，但是要少用

### 如何清除浮动

```css
.clearfix:after {
    content: '';
    display: block;
    clear: both;
}
```

### 两种盒模型区别

1. content-box => width 和 height 只包含内容的宽和高，不包括边框和内边距。case：{width: 350px, border: 10px solid red;} 实际宽度为 370
2. border-box => width 和 height 包含内容、内边距和边框。case：{width: 350px, border: 10px solid red;} 实际宽度为 350

## JS

### JS 的数据类型

基本数据类型：number/boolean/string/null/undefined/Symbol/BigInt(任意精度的整数)
引用数据类型：Object

### 判断数据类型

1. typeof => 返回一个字符串，表示操作数的类型
    1. typeof null === 'object'
    2. typeof <function> === 'function'
2. instanceof => 在原型链中查找是否是其实例 => object instanceof constructor
3. 判断是否是数组
    1. arr instanceof Array
    2. arr.constructor === Array
    3. Array.isArray(arr)
    4. Object.prototype.toString.call(arr) === '[object Array]'

### 原型链是什么？

1. case：const a = {}，此时 a.__proto__ == Object.prototype，即 a 的原型是 Object.prototype
2. case：我们有一个数组对象，const a = []，此时 a.__proto__ == Array.prototype，此时 a 的原型是 Array.prototype，此时 Array.prototype.__proto__ == Object.prototype，此时：
    1. a 的原型是 Array.prototype
    2. a 的原型的原型是 Object.prototype
    3. 于是形成了一条原型链
3. 可以通过 const x = Object.create(原型) 或者 const x = new 构造函数() 的方式改变 x 的原型
    1. const x = Object.create(原型) => x.__proto__ == 原型
    2. const x = new 构造函数() => x.__proto__ == 构造函数.prototype
4. 原型链可以实现继承，以上面的数组为例：a ===> Array.prototype ===> Object.prototype
    1. a 是 Array 的实例，a 拥有 Array.prototype 里的属性
    2. Array 继承了 Object
    3. a 是 Object 的间接实例，a 也就拥有 Object.prototype 里的属性
    4. a 即拥有了 Array.prototype 的属性，也拥有了 Object.prototype 的属性
5. 原型链的优点在于：简单优雅
6. 但是不支持私有属性，ES6新增加的 class 可以支持私有属性

### 代码中的 this 是什么？

1. 将所有的函数调用转化为 call => this 就是 call 的第一个参数
2. func(p1, p2) => func.call(undefined, p1, p2) => 如果 context 是 null 或 undefined，window 是默认的 context（严格模式下默认是 undefined）
3. obj.child.method(p1, p2) => obj.child.method.call(obj.child, p1, p2)

### JS 的 new 做的什么？

```javascript
function Person(name) {
  this.name = name;
}

const ming = new Person("ming");

const ming = (function (name) {
  // 1. var temp = {}; => 创建临时对象
  // 2. this = temp; => 指定 this = 临时对象
  this.name = name;
  // 3. Person.prototype = {...Person.prototype, constructor: Person} => 执行构造函数
  // 4. this.__proto__ == Person.prototype => 绑定原型
  // return this; => 返回临时对象
})("ming");
```

### JS 的立即执行函数是什么？

1. 声明一个匿名函数，然后立即执行它，这种做法就是立即执行函数
2. 例如: 每一行代码都是一个立即执行函数
    1. (function() {} ())
    2. (function() {})()
    3. !function() {}()
    4. +function() {}()
    5. -function() {}()
    6. ~function() {}()
3. 在 ES6 之前只能通过立即执行函数来创建局部作用域
4. 其优点在于兼容性好
5. 目前可以使用 ES6 的 block + let 代替
   ```javascript
   {
      let a = '局部变量';
      console.log(a); // 局部变量
   }
   console.log(a); // Uncaught ReferenceError: a is not defined
   ```

### JS 的闭包是什么？

1. 闭包是 JS 的一种语法特性，闭包 = 函数 + 自由变量。对于一个函数来说，变量分为：全局变量、本地变量、自由变量
2. case：闭包就是 count + add 组成的整体
   ```javascript
   const add2 = (function() {
      var count = 0;
      return function add() {
        count++;
      }
   })()
   
   // 此时 add2 就是 add
   add2(); 
   
   // 相当于
   add();
   
   // 相当于
   count++;
   ```
3. 以上就是一个完整的**闭包的应用**
4. 闭包解决了
    1. 避免污染全局环境 => 因为使用了局部变量
    2. 提供对局部变量的间接访问 => 只能 count++，不能 count--
    3. 维持变量，使其不被垃圾回收
5. 其优点是：简单好用
6. 但是闭包**使用不当**可能造成内存泄漏。case：
   ```javascript
   function test() {
      var x = {name: 'x'};
      var y = {name: 'y', content: '这里很长很长，占用了很多很多字节'}
      return function fn() {
        return x;
      }
   }
   
   const myFn = test(); // myFn 就是 fn 了
   const myX = myFn(); // myX 就是 x 了
   ```
7. 对于正常的浏览器来说，y会在一段时间内自动消失，被垃圾回收器回收，但是旧版本的 IE 浏览器不会回收，这是 IE 浏览器的问题

### JS 如何实现类

1. 使用原型

```javascript
function Dog(name) {
  this.name = name;
  this.legsNum = 4;
}

Dog.prototype.kind = 'dog';
Dog.prototype.run = function () {
  console.log("I am running with " + this.legsNum + " legs.")
}
Dog.prototype.say = function () {
  console.log("Wang Wang, I am " + this.name);
}

const dog = new Dog("ming");
dog.say();
```

2. 使用类

```javascript
class Dog {
  kind = 'dog';

  constructor(name) {
    this.name = name;
    this.legsNum = 4;
  }

  run() {
    console.log("I am running with " + this.legsNum + " legs.")
  }

  say() {
    console.log("Wang Wang, I am " + this.name);
  }
}

const dog = new Dog('ming');
dog.say();
```

### JS 实现继承

1. 使用原型链

```javascript
// dog => Dog => Animal
function Animal(legsNum) {
  this.legsNum = legsNum;
}

Animal.prototype.kind = 'animal';
Animal.prototype.run = function () {
  console.log("I am running with " + this.legsNum + " legs.");
}

function Dog(name) {
  this.name = name;
  Animal.call(this, 4); // 继承属性
}

// Dog.prototype.__proto__ == Animal.prototype
const temp = function () {}
temp.prototype = Animal.prototype;
Dog.prototype = new temp();

Dog.prototype.kind = 'dog';
Dog.prototype.say = function () {
  console.log("Wang Wang, I am " + this.name);
}

const dog = new Dog("ming"); // Dog 函数就是一个类
console.log(dog);
```

2. 使用类

```javascript
class Animal {
  kind = 'animal';

  constructor(legsNum) {
    this.legsNum = legsNum;
  }

  run() {
    console.log("I am running with " + this.legsNum + " legs.");
  }
}

class Dog extends Animal {
  kind = 'dog';

  constructor(name) {
    super(4);
    this.name = name;
  }

  say() {
    console.log("Wang Wang, I am " + this.name);
  }
}

const dog = new Dog('ming');
console.log(dog);
```

### JS 手写节流 & 防抖

1. 节流 throttle => 技能冷却中 => 场景
    1. Select 去服务端动态搜索
    2. 按钮用户点击过快，发送多次请求

```javascript
function throttle(time, callback) {
  let flag = true;
  return (...args) => {
    if (flag) {
      flag = false;
      callback(args);
      setTimeout(() => {
        flag = true;
      }, time);
    }
  }
}

const fn = throttle(2000, () => {console.log("Hello!")});
fn();
fn();
setTimeout(fn, 3000);
```

2. 防抖 debounce => 回城被打断 => 场景
    1. 滚动事件

```javascript
function debounce(time, callback) {
  let timer;
  return (...args) => {
    timer && clearTimeout(timer);
    timer = setTimeout(() => {
      callback(args);
    }, time);
  }
}

const fn = debounce(2000, () => console.log("Hello!"));
fn();
setTimeout(fn, 1000);
```

### JS 手写发布订阅

```javascript
const eventBus = {
  bus: {},
  on(eventName, callback) {
    if (!this.bus[eventName]) {
      this.bus[eventName] = [callback];
    } else {
      this.bus[eventName].push(callback);
    }
  },
  emit(eventName, data) {
    if (!this.bus[eventName]) {
      throw new Error("Please check eventName " + eventName);
    }
    this.bus[eventName].forEach(callback => callback.call(null, data));
  },
  off(eventName, callback) {
    if (!this.bus[eventName]) {
      throw new Error("Please check eventName " + eventName);
    }
    const index = this.bus[eventName].indexOf(callback);
    if (index < 0) {
      return;
    }
    this.bus[eventName].splice(index, 1);
  }
}

eventBus.on('click', console.log)
eventBus.on('click', console.error)
setTimeout(() => {
  eventBus.emit('click', 'Hello!');
}, 3000)
```

### JS 手写 AJAX

```javascript
const ajax = (method, url, data, success, fail) => {
  const request = new XMLHttpRequest();
  request.open(method, url);
  request.onreadystatechange = function () {
    if (request.readyState === 4) {
      if (request.status >= 200 && request.status < 300 || request.status === 304) {
        success(request);
      } else {
        fail(request);
      }
    }
  }
  if (method === "post") {
    request.send(data);
  } else {
    request.send();
  }
}
```

### JS 手写简化版 Promise

```javascript
class Promise2 {
  #status = 'pending';

  constructor(fn) {
    this.queue = [];

    const resolve = (data) => {
      this.#status = 'fulfilled';
      const f1f2 = this.queue.shift();
      if (!f1f2 || !f1f2[0]) return;
      const x = f1f2[0].call(undefined, data);
      if (x instanceof Promise2) {
        x.then(data => resolve(data), reason => reject(reason));
      } else {
        resolve(x);
      }
    }

    const reject = (reason) => {
      this.#status = 'rejected';
      const f1f2 = this.queue.shift();
      if (!f1f2 || !f1f2[1]) return;
      const x = f1f2[1].call(undefined, reason);
      if (x instanceof Promise2) {
        x.then(data => resolve(data), reason => reject(reason));
      } else {
        resolve(x);
      }
    }

    fn.call(undefined, resolve, reject);
  }

  then(f1, f2) {
    this.queue.push([f1, f2]);
  }
}

const p = new Promise2((resolve, reject) => {
  setTimeout(() => {
    reject('Error!');
  }, 3000);
});

p.then((data) => console.log(data), error => console.error(error));
```

### JS 手写 Promise.all

1. 要在 Promise 上写而不是在原型上写
2. Promise.all 参数(Promise 数组)和返回值(新 Promise 对象)
3. 用数组记录结果
4. 只要有一个 reject 就整体 reject

```javascript
Promise.myAll = function (list) {
  const results = [];
  let count = 0;
  return new Promise((resolve, reject) => {
    list.map((promise, index) => {
      promise.then((result) => {
        results[index] = result;
        count++;
        if (count >= list.length) {
          resolve(results);
        }
      }, reason => reject(reason));
    });
  });
}
```

### JS 手写深拷贝

JSON

```javascript
const copy = JSON.parse(JSON.stringify(a));
```

缺点：

1. 不支持 Date、正则、undefined、函数等数据
2. 不支持引用，即环状结构

递归。要点：

1. 判断类型
2. 检查环
3. 不拷贝原型上的属性

```javascript
const deepCopy = (a, cache) => {
  if (!cache) {
    cache = new Map();
  }

  // 不考虑跨 iframe
  if (a instanceof Object) {
    if (cache.get(a)) {
      return cache.get(a);
    }
    let result = null;
    if (a instanceof Function) {
      // 有 prototype 就是普通函数
      if (a.prototype) {
        result = function () {
          return a.apply(this, arguments);
        }
      } else {
        result = (...args) => {
          return a.call(undefined, ...args);
        }
      }
    } else if (a instanceof Array) {
      result = [];
    } else if (a instanceof Date) {
      result = new Date(a - 0);
    } else if (a instanceof RegExp) {
      result = new RegExp(a.source, a.flags);
    } else {
      result = {};
    }
    cache.set(a, result);
    for (let key in a) {
      if (a.hasOwnProperty(key)) {
        result[key] = deepCopy(a[key], cache);
      }
    }
    return result;
  }
  return a;
}

const a = {
  number: 1, bool: false, str: 'hi', empty1: undefined, empty2: null,
  array: [
    {name: 'frank', age: 18},
    {name: 'jacky', age: 19}
  ],
  date: new Date(2000, 0, 1, 20, 30, 0),
  regex: /\.(j|t)sx/i,
  obj: {name: 'frank', age: 18},
  f1: (a, b) => a + b,
  f2: function (a, b) { return a + b }
}
a.self = a;

const b = deepCopy(a);
console.log(b.self === b); // true
b.self = 'hi'
console.log(a.self !== 'hi'); //true
```

### JS 手写数组去重

```javascript
const unique = (nums) => {
  const map = new Map();
  for (let i = 0; i < nums.length; i++) {
    if (nums[i] === undefined || map.has(nums[i])) {
      continue;
    }
    map.set(nums[i], true);
  }
  return [...map.keys()];
}
```

## DOM

### DOM 事件模型

1. 先经历从上到下的捕获阶段，再经历从下到上的冒泡阶段
2. addEventListener("click", fn, options, useCapture)
    1. options 中有一个 capture 参数，true 表示捕获阶段，false 表示冒泡阶段
    2. useCapture true 表示捕获阶段，false 表示冒泡阶段
3. 可以使用 event.stopPropagation() 来阻止捕获或冒泡

### 手写事件委托

```javascript
ul.addEventListener('click', (e) => {
  if (e.target.tagName.toLowerCase() === 'li') {
    // do something
  }
});
```

1. 如果点击 li 里面的 span，就没有办法触发事件
2. 点击元素之后，递归遍历点击元素的祖先元素直至遇到 li 或者 ul

```javascript
const delegate = (element, eventType, selector, fn) => {
  element.addEventListener(eventType, e => {
    let ele = el.target;
    while (!ele.matches(selector)) {
      if (ele === element) {
        return;
      }
      ele = ele.parentNode;
    }
    fn.call(ele, e);
  });
  return element;
}

delegate(ul, 'click', 'li', (e) => console.log(e));
```

1. 事件委托优点
    1. 节省监听器
    2. 实现动态监听
2. 事件委托缺点 => 调试比较复杂，不容易确定监听者

### 手写可拖曳 div

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Drag</title>
    <style>
        .drag {
            border: 1px solid red;
            position: absolute;
            top: 0;
            left: 0;
            width: 100px;
            height: 100px;
        }
    </style>
</head>
<body>
<div class="drag"></div>
<script>
    let dragging = false;
    let position = null;
    const dragEle = document.querySelector('.drag');
    dragEle.addEventListener("mousedown", e => {
        dragging = true;
        position = [e.clientX, e.clientY];
    });

    document.addEventListener("mousemove", e => {
        if (!dragging) return;
        const x = e.clientX;
        const y = e.clientY;
        const moveX = x - position[0];
        const moveY = y - position[1];
        const left = parseInt(dragEle.style.left || 0);
        const top = parseInt(dragEle.style.top || 0);
        dragEle.style.left = left + moveX + 'px';
        dragEle.style.top = top + moveY + 'px';
        position = [x, y];
    });

    document.addEventListener('mouseup', e => {
        dragging = false;
    })
</script>
</body>
</html>
```

## HTTP

### HTTP status code

- 200 OK
- 201 Created
- 204 No Content
- 301 Move Permanently
- 302 Found
- 304 Not Modify
- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 405 Method Not Allowed
- 409 Conflict
- 410 Gone
- 414 URI Too Long
- 415 Unsupported Media Type
- 500 Internal Server Error
- 502 Bad Gateway
- 504 Gateway Timeout

### GET 和 POST 的区别

1. 根据技术文档规格，GET 和 POST 最大区别就是语义，一个读一个写
2. 实践上会有很多区别，如:
3. 由于 GET 是读，POST 是写。所以 GET 是幂等的，POST 是不幂等的(一个 HTTP 方法是幂等的，指的是同样的请求被执行一次与连续执行多次的效果是一样的，服务器的状态也是一样的)
4. 由于 GET 是读，POST 是写。所以 GET 结果会被缓存，POST 结果不会被缓存
5. 由于 GET 是读，POST 是写。所以 GET 打开的页面刷新是无害的，POST 打开的页面刷新需要确认
6. 通常情况下，GET 请求参数放置在 URL 里，POST 请求参数放在 body 里
7. GET 比 POST 更不安全，因为参数直接暴露在 URL 上，所以不能用来传递敏感信息
8. GET 请求参数放在 URL 里是有长度限制的(浏览器限制的，414 URI to long)，而 POST 放在 body 里没有长度限制(长度其实可是配置)
9. GET 产生一个 TCP 数据包，POST 产生两个或以上 TCP 数据包

### 简单请求 vs 复杂请求

1. 简单请求不会触发 CORS 预检请求
2. 以下条件是简单请求：
    1. method => GET | POST
    2. header => 需要关注 Content-Type => text/plain | multipart/form-data | application/x-www-form-urlencoded
3. 复杂请求会触发 CORS 预检请求
4. 预检请求 => 首先使用 OPTION 方法发起一个预检请求到服务器，已获知服务器是否允许该实际请求

### Cookie

1. Cookie 是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上
2. 通过 Set-Cookie 设置，是一个 key=value 结构
3. Expires | Max-Age => 指明过期时间，只与客户端有关
4. HttpOnly => 保证 Cookie 不会被脚本访问 => JS document.cookie API 无法访问带有 HttpOnly 属性的 Cookie
5. Domain | Path => 允许 Cookie 应该发送给哪些 URL

### HTTP 缓存有哪些方案？

1. HTTP 缓存分为**强缓存(缓存)**和**弱缓存(内容协商)**
2. HTTP 1.1 时代
    1. 强缓存(缓存)
        - 在 response header 中添加 Cache-Control: max-age = 3600，浏览器会自动缓存一个小时，如果在此时间内，再次访问相同的 url(path + query)，直接不发送这个请求
        - 在 response header 中添加 Etag：ABC，代表该文件的特征值
    2. 弱缓存(内容协商)
        - 强缓存过期之后，该缓存是否可以继续使用 => request header 中添加 if-None-Match: ABC => 浏览器向服务器发送的一个询问
        - 服务器返回相应的状态码：304(Not Modified，继续使用缓存的内容) 或 200(使用新的文件，其中 response header 中包含了 Cache-Control 和 Etag)
3. HTTP 1.0 时代
    1. 强缓存(缓存)
        - Expires => 以电脑本地时间为准
        - Last-Modified => 一个文件1s内更改多次，无法区分是否是最新的
    2. 弱缓存(内容协商)
        - if-Modified-Since
        - 状态码：304 或 200

### HTTP 和 HTTPS 的区别

1. HTTPS = HTTP + SSL/TLS(安全层)
2. HTTP 是明文传输的，不安全。HTTPS 是加密传输的，非常安全
3. HTTP 使用 80 端口。HTTPS 使用 443 端口
4. HTTP 较快。HTTPS 较慢
5. HTTP 不需要证书。HTTPS 需要证书

### HTTP/1.1 和 HTTP/2 的区别有哪些?

1. HTTP/2 使用**二进制**传输，并且将 head 和 body 分成**帧**来传输。HTTP/1.1 是字符串传输
2. HTTP/2 支持**多路复用**，一个 TCP 连接可以发送多个请求。HTTP/1.1 不支持，一个请求建立一个 TCP 连接。多路复用就是一个 TCP 连接从单车道变成了几百个双向通行的车道
3. HTTP/2 可以**压缩 head**。HTTP/1.1 不可以
4. HTTP/2 支持**服务器推送**。HTTP/1.1 不支持

### TCP 三次握手和四次挥手

![TCP 三次握手和四次挥手](https://upload-images.jianshu.io/upload_images/9617841-6e75be3b52703c2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ACK => acknowledge => 接受
- RCVD => received => 收到
- SYN => synchronize => 同步
- seq => sequence => 顺序
- EATABLISHED => established => 已建立

1. 三次握手
    1. 浏览器向服务器发送 TCP 数据 => SYN(seq = x)
    2. 服务器向浏览器发送 TCP 数据 => SYN(seq = y), ACK = x + 1
    3. 浏览器向服务器发送 TCP 数据 => ACK = y + 1
2. 四次挥手
    1. 浏览器向服务器发送 TCP 数据 => FIN(seq = x + 2), ACK = y + 1
    2. 服务器向浏览器发送 TCP 数据 => ACK = x + 3
    3. 服务器向浏览器发送 TCP 数据 => FIN(seq = y + 1)
    4. 浏览器向服务器发送 TCP 数据 => ACK = y + 2

### 同源策略和跨域

1. 同源指的是 protocol + host + port 相同便是同源的
2. 同源策略用于控制不同源之间的交互。**跨源写**和**跨源资源嵌入**一般是允许的，但是**跨源读**操作一般是不允许的。
3. 只要在浏览器里打开页面，默认遵守同源策略
4. 保证了用户的隐私安全和数据安全
5. 很多时候前端需要访问另一个域名的后端接口，此时浏览器会将响应屏蔽，并报错 CORS
6. 解决跨域 => 通常需要在 response header 中添加以下即可，此时浏览器将不会屏蔽响应
   ```
   Access-Control-Allow-Origin: <前端访问域名>
   Access-Control-Allow-Method: POST, OPTIONS, GET, PUT
   Access-Control-Allow-header: Content-Type
   ```
7. 使用 Node.js 或者 NGINX 代理 => 前端 -> NGINX/Node.js -> 另一个域名的服务端

### Session、Cookie、LocalStorage、SessionStorage 的区别

1. Session => 会话，用户信息 => 存储在服务器的文件中，如 MySQL 或者 Redis
2. Cookie => 保存了用户凭证 => 存储在浏览器文件中，在请求的时候会发送到服务端，大小 4k 左右
3. LocalStorage vs SessionStorage => 存储
    1. LocalStorage 如果不手动清除，会一直存在。SessionStorage 会话关闭就清除

## TypeScript

### TS 和 JS 的区别是什么？有什么优势？

1. 语法层面 => TS = JS + Type。TS 就是 JS 的超集
2. 执行环境层面 => 浏览器、NodeJS 可以直接执行 JS，但不能直接执行 TS
3. 编译层面 => TS 有编译阶段。JS 没有编译阶段
4. TS 类型更安全，IDE 可以进行提示

### any、unknown、never 的区别是什么？

1. any 和 unknown 都是**顶级类型（top type）**，任何类型的值都可以赋值给顶级类型变量
   ```
   let foo: any = 123; // 不报错
   let bar: unknown = 123; // 不报错
   ```
2. 但是 unknown 比 any 类型检查更严格，any 什么检查都不做，unknown 要求先收窄类型
   ```ts
   const value: unknown = "hello world";
   const str: string = value; // 报错：Type 'unknown' is not assignable to type 'string'.(2322)
   const str1: string = value as string; // 不报错
   ```
3. 如果改成 any，基本在哪都不会报错。所以能用 unknown 就优先使用 unknown，类型更安全一点
4. never 是**底类型**，表示不应该出现的类型
   ```ts
   interface Foo {
      type: 'foo'
   }
   interface Bar {
      type: 'bar'
   }
   
   type All = Foo | Bar;
   
   function handleValue(val: All) {
        switch (val.type) {
            case 'foo':
                // 这里的 val 被收窄为 Foo
                break;
            case 'bar':
                // 这里的 val 被收窄为 Bar 
                break;
            default:
                // val 在这里是 never
                const check: never = val;
                break;
        }
   }
   ```
5. 在 default 里把被收窄为 never 的 val 赋值给了一个显示声明为 never 的变量，如果一切逻辑正确，那么这里可以编译通过
6. 某一天更改了 All 的类型 => ` type All = Foo | Bar | Baz `
7. 此时如果没有修改 handleValue，此时 default 会被收窄为 Baz，无法赋值给 never，此时会产生一个编译错误
8. 通过这个方法，可以确保 handleValue 总是穷尽（exhaust）了所有 All 的可能类型

### type 和 interface 的区别是什么？

1. 组合方式 => interface 使用 extends 来实现继承。type 使用 & 来实现联合类型
2. 扩展方式 => interface 可以重复声明用来扩展(merge)。type 一个类型只能声明一次
   ```ts
   interface Foo {
      title: string
   }
   interface Foo {
      content: string
   }

   type Bar = {
      title: string
   }

   // Error: Duplicate identifier 'Bar'
   type Bar = {
      content: string
   }
   ```
3. 范围不同 => type 适用于基本类型。interface 被用于描述对象(declare the shapes of objects)
4. 命名方式 => interface 会创建新的类型名。type 只是创建类型别名，并没有新创建类型

## 浏览器

### 单页面应用中实现前端路由有哪些方式

1. hash 模式 和 history 模式
2. hash 模式 =>
    1. 通过监听 URL 中 hash 部分的变化(hashchange)，从而做出对应的渲染逻辑
    2. URL 中带有 #
    3. 前端即可完成
    4. 请求的时候 # 后面的内容不会包含在 HTTP 请求中，所以改变 hash 不会重新加载页面
3. history 模式 => HTML5 history 全局对象 => go/forward/back/pushState/replaceState
    1. 使用 pushState 实现
    2. 需要后端配合，将所有路径都指向首页
4. 调用 ` pushState() ` 和 ` window.location = '#foo' ` 基本上一样，都会创建一个新的历史记录

|     | pushState(state, title, url)                             | window.location = '#foo' |
|-----|----------------------------------------------------------|--------------------------|
| URL | 新的 URL 必须是同源的                                            | 只有在设置锚的时候才使用当前 URL       |
| URL | 可能会改变页面的 URL，<br/> 输入相同的 URL 时不会改变 URL，<br/> 但是会创建新的历史记录 | 设置相同的锚不会创建新的历史记录         |
| 数据  | 数据可以放在 state 中                                           | 只能将数据写在锚的字符串中            |

### 微任务和宏任务

1. 浏览器中并不存在宏任务，宏任务（Macrotask）是 Node.js 发明的术语
2. 浏览器中只有任务和微任务（Microtask）
    1. 使用 script 标签、setTimeout 可以创建任务
    2. 使用 Promise#then、window.queueMicrotask 可以创建微任务
3. 微任务会在任务间隙执行 => 微任务只能插任务的队
4. 多个 then 里面的回调并不会一次性插入到等待队列中，而是执行完一个再插入下一个
5. 一个 return Promise.resolve(...) 等于两个 then

```ts
// queue => [0, 1] => 0 -> [1, 4x] => 1 -> [4x, 2] => [2, 4x(剩余一个then)] => 2 -> [4x, 3] => [3, 4x(下一次就将打印)] => 3 -> [4x, 5] => 4x -> [5] => 5 -> [6] => 6
Promise.resolve().then(() => {
    console.log(0);
    return Promise.resolve('4x');
}).then((res) => {
    console.log(res)
});
Promise.resolve().then(() => {
    console.log(1);
}).then(() => {
    console.log(2);
}, () => {
    console.log(2.1)
}).then(() => {
    console.log(3);
}).then(() => {
    console.log(5);
}).then(() => {
    console.log(6);
})
```

### Web 性能优化

1. HTTP/1.1 => 连接复用 + 并行连接
2. HTTP/2 => 多路复用 => Frame + Stream => 1个 TCP 连接中可以同时进行多个请求和响应
3. 缓存 + 内容协商
    - HTTP/1.1 => Cache-Control + ETag + If-None-Match + 304 | 200
    - HTTP/1.0 => Expires + Last-Modified + If-Modified-Since + 304 | 200
4. cookie-free => cookie 最大有4k，每一个同源请求都会带着 cookie，某些文件可以启用新的域名，从而做到 cookie-free 和 并行连接
5. 使用 CDN => cookie-free + 并行连接 + 下载速度快
6. 资源合并 => 典型的方案有 icon font 和 SVG symbol
7. 代码层面
    - 分层 => 将一个 JS 拆分成多个 JS，从而达到分层的目的
    - 懒加载 | 预加载 => 多屏图片先加载第一屏，滚动到第二屏在加载第二屏的图片
    - js 动态导入 => ` import("lodash").then(_ => _.deepClone()) `


## 工程化

### babel 原理

1. babel 主要是将 A 类型的文件转化为 B 类型
2. webpack 只支持 JS 文件，所以需要将其他文件类型都转化为 JS 文件
3. parse => 主要将代码转化为 AST
4. traversal => 遍历 AST 进行修改
5. generator => 将修改后的 AST 转化为 code

### webpack 流程


### webpack 常见 loader 和 plugin 有哪些？二者区别是什么？

#### [loader](https://webpack.js.org/loaders/)

1. Transpiling
    1. babel-loader => 将 ES2015+ 转化为 ES5
    2. ts-loader => 加载 TS，并提示类型错误
    3. thread-loader => 多线程打包
2. Templating
    1. html-loader => 将 HTML 导出为字符串
3. Styling
    1. sass-loader => 加载并编译 sass 文件
    2. less-loader => 加载并编译 less 文件
    3. style-loader => 将 css 转化为 style
    4. css-loader => 把 CSS 变成 JS 字符串
4. Frameworks
    1. vue-loader => 加载并编译 Vue 组件

#### [plugin](https://webpack.js.org/plugins/)

1. HtmlWebpackPlugin => 创建 HTML 文件并自动引入 JS 和 css
2. CleanWebpackPlugin => 用于清理之前打包的残余文件
3. SplitChunksPlugin => 用于代码分包
4. DLLPlugin + DLLReferencePlugin => 用于避免大依赖被频繁重新打包，大幅降低打包时间
5. EslintWebpackPlugin => 用于检查代码中的错误
6. DefinePlugin => 用于在 webpack config 中添加全局变量
7. CopyWebpackPlugin => 用于拷贝静态文件到 dist

#### 区别

1. loader 是文件加载器，主要使用在 make 阶段，将 A 类型的文件转化为 B 类型。它能够对文件进行编译、优化、压缩等
2. plugin 是 webpack 插件，plugin 可以挂载在整个 webpack 打包过程中的钩子中，可以实现更多功能，如定义全局变量、加速编译、Code Split

### webpack 如何解决开发时的跨域问题

1. 在配置中添加代理即可
    ```
    devService: {
       proxy: {
          '/api': {
             target: 'http://host', 
             changeOrigin: true
          }
       }
    }
    ```

### 如何实现 tree-shaking?

1. tree-shaking 就是让没有用到的 JS 代码不打包，以减少包的体积
2. 利用 tree-shaking 部分 JS 代码
    1. 使用 ES2015 模块语法，即 export 和 import。
    2. 不要使用 CommonJS，CommonJS 无法 tree-shaking => babel-loader 添加 ` modules: false ` 选项
    3. 引入的时候只引用需要的模块
        - ` import {cloneDeep} from 'lodash-es' ` => 仅仅打包 cloneDeep
        - ` import _ from 'lodash' ` => lodash 全部打包，无法 tree-shaking 没有用到的模块
3. 不 tree-shaking JS 代码
    1. 在项目的 package.json 中添加 "sideEffects" 属性，防止某些文件被 tree-shaking
        - case: ` import x.js ` x.js 添加了 window.x 属性，那么 x.js 就要放到 sideEffects 中
        - 所有被 import 的 CSS 都要放在 sideEffects 中
4. 如何开启 tree-shaking => 在 webpack config 中将 mode 设置为 production => ` mode: production ` 给 webpack 加了非常多的优化

### 如何提高 webpack 构建速度

1. 使用 DLLPlugin 将不常变化的代码提前打包并复用，如 Vue、React
2. 使用 thread-loader 进行多线程打包
3. 处于开发环境时，在 webpack config 中将 cache 设为 true
4. 处于生产环境时，关闭不必要的环节，如 source map

### Webpack 和 Vite 的区别

#### 开发环境区别

1. Vite 自己实现 server，不对代码打包，充分利用浏览器对 ` <script type=module> ` 的支持
    - 假设 main.js 引入了 vue
    - 该 server 会把 ` import {createApp} from 'vue' ` 改为 ` import {createApp} from '/node_modules/.vite/vue.js' `
      这样浏览器就知道去哪里找 vue.js 了
2. webpack-dev-server 常使用 babel-loader 基于内存打包，比 Vite 慢很多很多
    - 该 server 会把 vue.js 的代码（递归地）打包进 main.js

#### 生产环境区别

1. Vite 使用 rollup + esbuild 来打包 JS 代码
2. Webpack 使用 babel 来打包 JS 代码，比 esbuild 慢很多很多

#### 文件处理时机

1. Vite 只会在你请求某个文件的时候处理该文件
2. Webpack 会提前打包好 main.js，等你请求的时候直接输出打包好的 JS 给你

#### 目前已知 Vite 缺点

1. 热更新常常失败
2. 有些功能 rollup 不支持，需要自己写 rollup 插件
3. 不支持非现代浏览器

### Webpack 怎么配置多页应用

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    app: './src/app.js',
    admin: './src/admin.js'
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: 'index.html',
      chunks: ['app']
    }),
    new HtmlWebpackPlugin({
      filename: 'admin.html',
      chunks: ['admin']
    })
  ]
}
```

1. 但是这样配置后会有一个**重复打包**的问题：假设 app.js 和 admin.js 都引入了 vue.js，那么 vue.js 的代码会打包进 app.js，也会打包进 admin.js
2. 我们需要使用 ` optimization.splitChunks ` 将共同依赖单独打包成 common.js，HtmlWebpackPlugin 会自动引入 common.js

### swc、esbuild 是什么？

#### swc

- 实现语言 => Rust
- 功能 => 编译 JS/TS，打包 JS/TS => TS: 类型擦除
- 优势 => 比 babel 快很多很多（20倍以上）
- 能否集成进 webpack => 可以
- 缺点 => 1. 对 TS 代码进行类型检查（用 tsc 先检查再打包）+ 2. 无法打包 CSS/SVG 等非 JS 文件

#### esbuild

- 实现语言 => GO
- 功能 => 编译 JS/TS，打包 JS/TS => TS: 类型擦除
- 优势 => 比 babel 快很多很多（10 - 100倍）
- 能否集成进 webpack => 可以
- 缺点 => 1. 对 TS 代码进行类型检查（用 tsc 先检查在打包）+ 2. 无法打包 CSS/SVG 等非 JS 文件

### Docker

1. Docker 是一种技术
2. 在系统上安装 Docker 软件即可使用
3. 核心概念
    1. 容器(Container) => 一台虚拟的计算机，拥有独立的网络、文件系统、进程。默认和宿主机不发生任何交互
    2. 镜像(Image) => 一个预先定义好的模板文件，Docker 引擎按照这个模板文件启动无数个一模一样，互不干扰的容器。默认是分层的 => 复用、节省空间
4. 命令 =>
    1. docker run --name -v -p -e
    2. docker images
    3. docker ps
5. 解决了什么问题 =>
    1. 保证开发、测试、交付、部署的环境完全一致
    2. 保证资源的隔离
    3. 启动临时的、用完即弃的环境，如测试
    4. 秒级的超大规模的部署和扩容
6. 实现隔离技术 => namespace + Control Groups

## React

### React setState 异步同步

1. 在 setTimeout、Promise 等原生事件 API 调用中
    - setState 和 useState 是**同步执行**的，立即执行 render
        - Class Component 能获取到最新值 => this.state => **引用类型**
        - Function Component 不能获取到最新值 => 只能得到之前的值 => **闭包**
    - 多次执行 setState 和 useState，每一次执行都会调用一次 render
2. 在 React 合成事件中
    - setState 和 useState 是**异步执行**的，不会立即执行 render，都不能立即获取到更新后的值
    - 多次执行 setState 和 useState，只会调用一次 render
    - setState 可能会进行 state 合并
        - 传入一个对象，会进行 state 合并 => +1
        - 传入一个函数，则不会进行 state 合并 => +2
    - useState 不会进行 state 合并

### React Fiber

#### 在 Fiber 出现之前 React 的问题

1. 在 Fiber 出现之前对比更新虚拟 DOM 使用递归加循环，这种对比方式有一个问题，就是任务一旦开始则无法中断
2. 如果应用组件过多，层级较深，那么主线程将会被一直占用
3. 这会导致一些用户操作或者动画无法立即得到执行，页面会产生卡顿
4. 总结 => 递归无法中断，执行任务耗时长，JavaScript 是单线程的，比较虚拟 DOM 过程中无法执行其他任务，导致任务延迟页面卡顿

#### Fiber 架构思路

1. Fiber 架构使用**循环**模拟递归，循环可以随时被中断
2. Fiber 将大的任务拆分成一个个小任务
3. 使用 requestIdleCallback 利用浏览器空余时间执行小任务，执行完一个任务单元后，查看是否有其他优先级更高的任务，如果有，放弃占用线程，先执行优先级更高的任务
4. requestIdleCallback 方法插入一个函数，这个函数将在浏览器空闲时期被调用

#### Fiber 节点

1. tag
2. key
3. stateNode => DOM 节点
4. child => 子节点
5. sibling => 下一个兄弟节点
6. return => 父级节点
7. firstEffect
8. nextEffect
9. lastEffect

#### Fiber 执行

1. 两棵树 => currentFiber + workInProgress
2. render 阶段 => 利用 DFS 和 diff 算法构建 workInProgress Fiber 树，可中断，收集 Effect List 链表
3. commit 阶段 => 根据 Fiber 节点的 tag 进行 DOM 操作 => 不可中断

#### render 阶段

1. DFS 遍历 Fiber 树，遍历的时候会进行 current 和 workInProgress 的 diff，决定哪些旧节点需要复用、删除、移动，哪些新节点需要创建
2. 边构建、边遍历、边对比、可中断
3. performUnitOfWork -> beginWork -> completeUnitOfWork -> completeWork
4. performUnitOfWork => 开始函数
5. beginWork => 返回 workInProgress.child 节点
6. completeUnitOfWork => check sibling -> parent
7. completeWork => 如果有 stateNode 则更新，如果没有 stateNode 则创建 => stateNode 对应 DOM 节点，有 stateNode 说明这个节点有对应的 DOM 节点
8. completeWork 执行完之后收集 effect
9. completeUnitOfWork 的内部循环会自底向上收集 effect，不断把有 effectTag 的子节点和自身向上合并到父节点的 effectList 中，直至根节点，Effect List 就是一个链表
10. diff
    1. 如果根节点类型改变了，比如 div 变为了 p，那么直接认为整颗树都变了，不再对比子节点。此时直接删除对应的真实 DOM 树，创建新的真实 DOM 树
    2. 如果根节点类型没有改变，就看看属性变了没有
        1. 属性没变，保留对应的真实节点
        2. 属性变了，就只更新该节点的属性，不重新创建节点 => case: 更新 style 时，如果多个 css 属性只有一个改变了，那么 React 只更新改变的
    3. 遍历子节点
        1. 子节点是单节点 => 遍历旧的节点，如果有可以复用的，复用，没有则将旧节点的 effect 标记为删除，新建子节点
        2. 子节点是多节点 =>
            - 第一轮：
                1. 如果新节点遍历完，则删除所有旧节点，对比结束
                2. 如果旧节点遍历完，则新增所有新节点，对比结束
            - 第二轮：建立 map 对比
                1. 有节点位置发生改变
                    ```
                    A -> B -> C
                    A -> C -> B
                    // 将 B 节点 effect 标记为 Placement
                    ```
                2. 中途出现增删的节点

#### commit 阶段

1. Before Mutation 前
    1. flushPassiveEffects => 触发 useEffect 回调与其他同步任务，由于这些任务可能触发新的渲染，所以要一直遍历到没有任务
    2. commitRoot 是**同步**完成的，清除 callbackNode 以允许安排新的回调
    3. reset workInProgress
    4. 获取 Effect List
2. Before Mutation
    1. 循环遍历 Effect List
    2. 处理 DOM 节点渲染或删除后的 autoFocus/blur 逻辑
    3. call getSnapshotBeforeUpdate
3. Mutation => 执行 DOM 操作
    1. 循环遍历 Effect List
    2. reset text
    3. 更新 ref
    4. 根据 flags 分别处理
        - Deletion => commitDeletion
            1. 递归的删除节点
            2. 清除 ref
            3. 调用 componentWillUnmount => ClassComponent
            4. 调度 destroy 函数 => FunctionComponent
        - Update => commitWork
            1. 递归调度 effect destroy 函数 => FunctionComponent
        - Placement => commitPlacement
            1. 获取父级 DOM 节点
            2. insertOrAppendPlacementNodeIntoContainer | insertOrAppendPlacementNode
4. Layout => DOM 渲染完成，触发生命周期钩子和 hooks
    1. 循环遍历 Effect List
    2. FunctionComponent => useLayout callback + 调度 useEffect destroy + callback
    3. ClassComponent => (componentDidMount | componentDidUpdate) + 执行 this.setState 的 callback
    4. 更新 ref
5. Layout 之后
    1. 处理 passive effect
    2. 检查 root 上是否有其余的工作
    3. ensureRootIsScheduled => 保证任何在 root 上附加任务被 schedule
    4. flushSyncCallbackQueue => 执行同步任务

### React 合成事件

React 将所有的事件都绑定到了根元素上，自动实现了事件委托，此时不需要使用 addEventListener 为已经创建的 DOM 添加事件监听器

#### 合成事件和原生事件区别

|        | React 合成事件             | 原生事件          |
|--------|------------------------|---------------|
| 命名     | 小驼峰(onClick)           | 纯小写(onclick)  |
| 事件处理函数 | 函数                     | 字符串           |
| 阻止默认行为 | event.preventDefault() | return false  |

### 虚拟 DOM 的原理是什么？

1. 虚拟 DOM 就是**虚拟节点**。React 使用 JS 对象来模拟 DOM 节点，之后将其渲染成真实的 DOM 节点
2. 使用 JSX 语法写出来的 div 其实就是一个虚拟节点
   ```jsx
   <div className="container">
      <span className="red">hi</span>
   </div>
   ```
3. 上面的代码其实是调用了 React.createElement 函数，之后生成了一个 JS 对象
   ```
   {
     tag: 'div',
     props: {className: 'container'},
     children: [
       {
         tag: 'span',
         props: {
           className: 'red'
         },
         children: [
           'hi'
         ]
       }
     ]
   }
   ```
4. 之后会根据 JS 对象信息即虚拟节点渲染为真实节点
5. 如果节点发生改变，并不会把新的虚拟节点直接重新渲染为真实节点，而是要先经过 diff 算法得到一个 patch 再更新到真实节点上
6. 虚拟 DOM 大大提升了 DOM 操作性能问题。通过虚拟 DOM 和 diff 算法减少不必要的 DOM 操作，保证性能。
7. 之前 DOM 操作并不是很方便，但是现在只需要 setState 即可
8. 但是 React 为虚拟 DOM 创造了**合成事件**，和原生 DOM 事件不太一样。所有 React 事件都绑定到了根元素，自动实现事件委托，如果混用合成事件和原生 DOM 事件，可能会出现 bug

### React DOM diff 算法

1. DOM diff 就是对比两颗虚拟 DOM 树的算法
2. 当组件变化时，会 render 出一个新的虚拟 DOM，diff 算法对比新旧虚拟 DOM 之后，得到一个 patch，然后 React 用 patch 来更新真实 DOM
3. 首先对比两棵树的**根节点**
    - 如果根节点类型改变了，比如 div 变为了 p，那么直接认为整颗树都变了，不再对比子节点。此时直接删除对应的真实 DOM 树，创建新的真实 DOM 树
    - 如果根节点类型没有改变，就看看属性变了没有
        1. 属性没变，保留对应的真实节点
        2. 属性变了，就只更新该节点的属性，不重新创建节点 => case: 更新 style 时，如果多个 css 属性只有一个改变了，那么 React 只更新改变的
4. 然后同时遍历两棵树的子节点，每个节点的对比过程如上
5. case1：React 依次对比 A-A，B-B，空-C，发现 C 是新增的，最终会创建真实 C 节点插入页面
   ```html
   <ul>
       <li>A</li>
       <li>B</li>
   </ul>

   // updated
   <ul>
       <li>A</li>
       <li>B</li>
       <li>C</li>
   </ul>
   ```
6. case2: React 对比 B-A，删除 B 节点新建 A 节点；对比 C-B，删除 C 节点新建 B 节点（注意：并不是边对比边删除新建，而是把操作汇总到 patch 里在进行 DOM 操作，会进行标记）；对比 空-C，新建 C 节点
   ```html
   <ul>
       <li>B</li>
       <li>C</li>
   </ul>

   // updated
   <ul>
       <li>A</li>
       <li>B</li>
       <li>C</li>
   </ul>
   ```
7. case2 其实只需要创建 A 节点，保留 B C 节点即可，此时 React 需要你添加 key
   ```html
   <ul>
       <li key="b">B</li>
       <li key="c">C</li>
   </ul>

   // updated
   <ul>
       <li key="a">A</li>
       <li key="b">B</li>
       <li key="c">C</li>
   </ul>
   ```
8. 此时 React 先对比 key，发现 key 增加了 a，此时保留 B C，新建 A 节点

#### Vue Dom diff

Vue **双端交叉对比**

1. 头头对比 => 对比两个数组的头部，如果找到，把新节点 patch 到旧节点，头指针后移
2. 尾尾对比 => 对比两个数组的尾部，如果找到，把新节点 patch 到旧节点，尾指针前移
3. 旧尾新头对比 => 交叉对比，旧尾新头，如果找到，把新节点 patch 到旧节点，旧尾指针前移，新头指针后移
4. 旧头新尾对比 => 交叉对比，旧头新尾，如果找到，把新节点 patch 到旧节点，新尾指针前移，旧头指针后移
5. 利用 Key 对比 => 用新指针对应节点的 key 去旧数组中寻找对应的节点
    - 没有对应的 key => 创建新节点
    - 有 key 并且是相同的节点 => 把新节点 patch 到旧节点
    - 有 key 但是不是相同的节点 => 创建新节点

### React 有哪些声明周期钩子函数？数据请求放在哪个钩子里？

![React Lifecycle](https://upload-images.jianshu.io/upload_images/9617841-c168322ada0872ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. 挂载时调用 constructor，更新时不调用
2. 更新时调用 shouldComponentUpdate 和 getSnapshotBeforeUpdate，挂载时不调用
3. shouldComponentUpdate 在 render 前调用，getSnapshotBeforeUpdate 在 render 后调用
4. 请求放置在 componentDidMount 里

### React 如何实现组件间通信

1. 父子组件通信 => props + 函数
2. 爷孙组件通信 => 两层父子通信 | Context.Provider 和 Context.Consumer
3. 任意组件通信 => 状态管理 => Redux | Mobx

### 如何理解 Redux

1. Redux 就是一个状态管理库，可以实现任意组件之间的通信，其实就是将信息放置在顶部，如有需要其余组件去顶部获取即可
2. Redux 核心概念
    1. store => 信息/状态存放的地方
    2. action => 可以更改 state 的唯一途径，根据 type 和 payload 进行更改 state
    3. dispatch => 用于派发事件
    4. reducer => 就是一个函数，传递给 reducer 一个旧的 state + action，他会返回一个新的 state
    5. Middleware => 中间件
3. Redux 常与 ReactRedux 联合使用，ReactRedux 提供了以下 Api
    1. connect()(Component)
    2. mapStateToProps
    3. mapDispatchToProps
4. Redux 常用中间件：
    1. redux-thunk => 扩展 redux 可以支持异步 action，如果 action 是一个函数，就直接调用它，否则就进入下一个中间件
    2. redux-promise => 如果 payload 是一个 Promise，就执行个这个 Promise，并在 then 里面去 dispatch

### 什么是高阶组件 HOC

1. 参数是组件，返回值也是组件的函数
2. React.forwardRef
   ```jsx
   const FancyButton = React.forwardRef((props, ref) => {
     <button ref={ref} className="fancyButton">
       {props.children}
     </button>
   });

   // You can now get a ref directly to the DOM button
   const ref = React.createRef();
   <FancyButton ref={ref}>Click me!</FancyButton>
   ```
3. ReactRedux 的 connect

### React Hooks 如何模拟组件生命周期

1. 模拟 componentDidMount
2. 模拟 componentDidUpdate
3. 模拟 componentWillUnmount

## 设计模式

设计模式是人们在长久的生产实践中总结出来的一套方法论，它可以提高开发效率，降低维护成本。举例说明：
1. 发布订阅模式 => 定义了对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都将得到通知。case：EventBus
2. 责任链模式 => 将请求沿着所有处理者组成的链发送，每个处理者都可以决定自己是否处理，或者是否继续处理。实现了请求和处理者之间的解耦。case：Redux 中间件
3. 模板方法 => 一个抽象类定义了执行它的方法的模板，子类可以按需要重写方法实现。case：React 的生命周期函数 + Dart

## 其他

**人岗匹配**

### 离职原因

个人原因：
1. 期待更大的平台，更好的机会
2. 目前准备成家，经济压力比较大

### 期望薪资

1. 请您方便介绍一个贵司的薪酬情况和薪酬结构吗？
2. 希望有一个符合市场预期的涨幅

### 规划

我是一个稳定性很强的人，我觉得贵司经过多年的发展，稳步的一个方式和我是非常匹配的，所以我的规划就是在我的本职岗位上做好，1-3年内提高我的专业能力，并且要对于岗位进行一个深入了解，通过积累3-5年的行业经验以及学习学习，达到一个高级或者资深的水平，后续的发展随着公司同步发展即可

### 缺点

1. 性格比较急躁，对于事情会比较较真
2. 后来经过同事和领导的沟通与交流
3. 之后我仔细思考了一下如何解决这个问题
4. 我从两个方面做出了努力
    - 在与同事的沟通中，转化自己的语言，case：你要这么做 => 你可以考虑一下这样做
    - 在与同事的沟通中，说话慢慢来，从而控制我急躁的性格，这个效果是非常明显的
    - 首先考虑这个事情对于整体有没有影响，如果有，那必须去较真，如果对整体没有影响，那就不去计较
5. 通过几个月的调整，大家也能明显感觉到我的进步，我和同事之间的关系更加紧密了

### 加班看法

1. 我不赞同无效的加班
2. 当公司的重要项目有延期风险，那么为了保证项目如期上线是可以加班的
3. 另外，我觉得更重要的是提升自己的工作效率，降低项目延期的风险

### 你遇到最难的 Bug 是什么？

1. 我们交付了一个打开第三方页面的功能，该功能在测试的时候没有出现任何问题，但是在现场发现某些页面空白
2. 因为我们没有相关账号，所以只能进行远程调试，在沟通协调了一段时间之后，我才看到具体的问题
3. 空白的第三方页面显示 md5 is not defined，之后我查看了一下 window 上是否挂载了 md5，发现没有正常挂载
4. 之后便怀疑 md5 这个包没有正常的下载，之后查看了一下 network，发现 md5.min.js 请求的状态码是 200
5. 按理说我获取到 js 文件，之后就会一行一行的执行该文件
6. 之后把 md5.min.js 文件内容拷贝出来，粘贴在控制台，之后查看 window 上是否挂载了 md5，发现也是没有 md5 这个对象
7. 但是 md5.min.js 被广泛使用，应该不会出现问题，所以我将上面 md5.min.js 文件粘贴到浏览器控制台中，发现 window 上是正常挂载了 md5
8. 那么我现在桌面应用和浏览器控制台调用了相同的 js，表现出不同的结果，说明两者环境可能有差异
9. 最终发现，桌面端应用在创建窗口是开启了 Node 功能。之后把 Node 功能置为 false，发现网页便正常了
10. 之后去查看了一下 md5.min.js 文件，看看对于 md5.min.js 两种环境差在哪里
11. md5.min.js 文件最终会查看 define 和 module，之后再两种环境中分别查看最终结果
12. Node 环境 module.export = t；浏览器是 n.md5 = t
13. 之后将该问题总结记录在了我们公司的 wiki 上，为大家提供解决问题思路

### 平时是如何学习的

手段有 看书，看博客，逛论坛基本上就这些，之后通过 输入、转化、输出来进行学习

- 输入 => 是指阅读别人分享的知识，比如看博客、看书
- 转化 => 是指把知识消化之后变成自己的积累，用自己的知识体系重新阐述一遍新学的概念
- 输出 => 是指把自己的理解以博客、代码的形式分享出来

### 自我介绍

面试官您好，我是XXX，XX年毕业，本科学历，我来应聘前端工程师岗位，目前有X年开发经验。我擅长的语言是 JavaScript，并且对于 Java 和 Dart 有一定的了解。主要使用的技术栈是 React ，熟练掌握算法和数据结构。对于产品研发自动化流程有一定的实践。我在上一份工作中从事前端开发工作，最近在公司主要负责公共组件开发项目。在工作之余我也会积极学习其他方面的知识，如 Java，并且将自己的学到的知识总结，目前已经写了接近 200 篇的技术博客。我觉得我的个人经历是和贵司的岗位是非常匹配的，我也非常喜欢贵司的文化，非常期待能够加入贵司和各位一起去做事情

### 回答问题

1. 您问的是xxx问题吗？
2. 我觉得这个问题可以从两个方面考虑
3. 一是
4. 二是
5. 对了，还有一点可以补充
6. 以上三点就是我对xxx问题的理解

### 反问问题

1. 组里目前最大的挑战是什么？
2. 咱组在整个部门或者公司的组织架构是什么样的？
3. 如果我加入贵司，在我未来的职业生涯中，有怎样一个晋升的途径？我更加适合哪个方式？

### 面试官

1. 时间 => 把握面试节奏，根据需求来决定
2. 分成几个部分，每个部分分别考察什么？=> 标准化(1h)
    1. 自我介绍 => 5min =>
    2. 语言基础知识(写代码/系统设计/算法(1)) => 算法 => 暴力 + 优化(准备3道题目)
    3. 框架
    4. 项目 => 考察岗位匹配程度 + 从一个人过去持续稳定的行为推测他能否胜任现在的岗位(可能短期提升吗？)
    5. 行为问题 => 10 - 15 min => 遇到最难的技术问题是什么？
    6. 反问 => 5min => 不能问结果，好的问题：做什么？挑战是什么？发展是什么？该部门在公司的组织架构上是什么样的？
3. 如何评价？ => 标准：技能匹配 + 文化匹配 + 潜力 => 评分：不合格 | 合格 | 优秀
4. 记录优缺点

项目中的最大的困难或挑战? => 举具体例子
回答的时候要匹配公司文化

httpClient 手写和使用库

亚马逊领导力准则

