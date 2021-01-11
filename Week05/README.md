# 第5周：编程与算法练习

>`Reactive实现`没太听明白:dizzy_face::dizzy_face::dizzy_face:

<!-- TOC -->

- [第5周：编程与算法练习](#第5周编程与算法练习)
  - [:lollipop:知识点点](#lollipop知识点点)
    - [1.Proxy](#1proxy)
    - [2.Object.defineProperty()](#2objectdefineproperty)
    - [3.Vue响应式实现](#3vue响应式实现)
    - [4.DOM操作](#4dom操作)
    - [5.Range](#5range)
      - [5.1.创建对象](#51创建对象)
      - [5.2.方法](#52方法)
    - [6.Infinity](#6infinity)
    - [7.Event](#7event)
      - [7.1.类型](#71类型)
      - [7.2.常用方法](#72常用方法)
      - [7.3.监听/移除](#73监听移除)
    - [8.单元测试](#8单元测试)
  - [:hourglass:疑难笔记](#hourglass疑难笔记)
    - [1.监听 mousemove、mouseup](#1监听-mousemovemouseup)
  - [:trophy:学习总结](#trophy学习总结)
  - [:sunflower:资料参考](#sunflower资料参考)
  - [:gift_heart:学习交流](#gift_heart学习交流)

<!-- /TOC -->

## :lollipop:知识点点

### 1.Proxy

用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）  

```javascript
//语法
const p = new Proxy(target, handler)
```

- target：代理对象，可以是任何类型的对象（数组、函数、另一个代理）
- handler：通常是函数，定义执行各种操作时代理 `p` 的行为

**handle 对象的方法**

- `set()` 属性设置操作
- `get()` 属性读取操作
- `deleteProperty()` delete操作符
- `apply()` 函数调用操作
- `construct()` new操作符

```javascript
let target = {}
let p = new Proxy(target, {
  set(target, prop, val) {
    return target[prop] = val;
  },
  get(target, prop) {
    return target[prop]
  }
});

p.a = 37;

console.log(target); //{a: 37}
console.log(target.a); //37
```

***

### 2.Object.defineProperty()

该方法会直接在对象上定义新属性，或者修改对象的现有属性，并返回此对象

```javascript
//语法
Object.defineProperty(obj, prop, descriptor)
```

- obj：要定义属性的对象
- prop：要定义或修改的属性的名称或`Symbol`
- descriptor：要定义或描述的属性描述符（也就是特性），定义时只能是 数据/存储 其一，出现两种描述符会报错
  - 数据描述符：`configurable`（是否能修改删除）、`enumerable`（是否可枚举，也就是使用for...in或Object.keys()）、`value`、`writable`（是否可写）、
  - 存储描述符：`configurable`（是否能修改删除）、`enumerable`（是否可枚举）、`get`、`set`

**数据描述符**

```javascript
let obj = {};

Object.defineProperty(obj, "name", {
  configurable: true, //默认 false
  enumerable: true, //默认 false
  value: "hao", //默认 undefined
  writable: false //默认 false
})

console.log(obj.name); //"hao"

obj.name = "time";
console.log(obj.name); //"hao" - 因为 writable 为 false

for(let o in obj) {
  console.log(o) // "name" - 如果 enumerable 设置为false，这里没数据
}

delete obj.name; //true - 如果 configurable 设置为false，删除不成功
console.log(obj.name); //undefined - 如果 configurable 设置为false，"hao"
```

**存储描述符**

```javascript
let obj = {};

let initValue = "hao";

Object.defineProperty(obj, "name", {
  configurable: true, //默认 false
  enumerable: true, //默认 false
  get:function() {
    return initValue;
  },
  set: function(value) {
    return initValue = value;
  }
})
//get
console.log(obj.name); //"hao"

//set
obj.name = "time";
console.log(obj.name); //"hao"

```

***

### 3.Vue响应式实现

- `vue2.x`：ES5 - Object.defindProperty
- `vue3.x`：ES6 - Proxy

***

### 4.DOM操作

- `transform` 旋转、缩放、倾斜、平移元素  
  - `translate()` 平移
  - `rootate()` 旋转
  - `scale()` 缩放
  - `skew()` 倾斜
- `childNodes` 获取子节点列表
- `textContent` 获得节点文本内容

***   

### 5.Range

表示一个包含节点与文本节点的一部分文档片段  

通过Range对象，可以获取用户选中的区域，或者指定选中区域，得到Range的起点和终点、修改或者复制里边的文本，甚至是HTML。  

如用户用鼠标拖动选中的区域、富文本编辑，等等，使用的都是Range对象。

#### 5.1.创建对象
```javascript
let range = doucment.createRange();
```

#### 5.2.方法

- `setStart(startNode, startOffset)` 设置 Range 的起点
- `setEnd(endNode, endOffset)` 设置 Range 的终点
- `insertNode(newNode)` 在 Range 的起始位置插入节点
- `toString()` 把 Range 的内容作为字符串返回
- `getBoundingClientRect()`
  - CSSOM 方法
  - 返回一个 DOMRect 对象，元素的大小及其相对视口的位置；
  - 该对象将范围中的内容包围起来；
  - 即该对象是一个将范围内所有元素的边界矩形包围起来的矩形
  - 此方法可用于确定文本区域中选中的部分或光标的视窗坐标

```javascript
let range = document.createRange();
range.setStart(div, 0);
range.setEnd(div, 0);
range.getBoundingClientRect(); //

let newNode = document.createElement("p").appendChild(document.createTextNode("Hao"));
range.insertNode(newNode);
```

***

### 6.Infinity

全局属性 `Infinity` 是一个数值，表示无穷大

`Number.POSITIVE_INFINITY` 的值同全局对象 `Infinity` 属性的值相同  

***

### 7.Event

`Event` 接口表示在 DOM 中出现的事件

#### 7.1.类型
- 由 用户 触发 的事件，例如鼠标或键盘事件
  - `selectstart` 鼠标选中
- 由 API 生成 的事件，例如指示动画已经完成运行的事件，视频已被暂停等等
- 由 脚本代码 触发 的事件，例如对元素调用 `HTMLElement.click()` 方法
- 定义一些自定义事件

#### 7.2.常用方法

- `preventDefault()` 取消事件（如果该事件可取消）
- `stopPropagation()` 停止冒泡

#### 7.3.监听/移除

- `Element.addEventListener(event, function)` 监听
- `Element.removeEventListener(event, function)` 移除

***

### 8.单元测试

- Mocha
- Jest


## :hourglass:疑难笔记

### 1.监听 mousemove、mouseup

一般选择在 `document` 上监听  

防止鼠标拖的过快，移开了可拖动区域，会出现拖断的情况

  
## :trophy:学习总结

本周主要学习内容：`reactive原理实现`、`调色盘`、`正常流中拖拽`

涉及 Proxy、Range、CSSOM 等  

- [ ] Proxy
- [ ] Range
- [ ] CSSOM
- [x] 编程与算法学习总结
  - [第一周：三子棋 和 红绿灯](https://github.com/HaoTime/Frontend-07-Template/tree/main/Week01)
  - [第二周：寻路](https://github.com/HaoTime/Frontend-07-Template/tree/main/Week02)
  - [第三周：LL算法构建AST](https://github.com/HaoTime/Frontend-07-Template/tree/main/Week03)
  - [第四周：字符串分析算法 - 字典树、KMP、Wildcard](https://github.com/HaoTime/Frontend-07-Template/tree/main/Week03)
  - [第五周：Reactive 原理实现、调色盘、正常流中拖拽](https://github.com/HaoTime/Frontend-07-Template/tree/main/Week03)
  
  
## :sunflower:资料参考

- <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy">Proxy - MDN</a>
- <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty">definProperty - MDN</a>
- <a href="https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform">transform - MDN</a>
- <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/Range">Range - MDN</a>
- <a href="https://segmentfault.com/a/1190000009875696">JS Range对象的使用 - Segmentfault</a>
    
  
## :gift_heart:学习交流

![Hao](https://haoer.oss-cn-hangzhou.aliyuncs.com/hao.jpg)

欢迎大家加微信交流 Thanksヾ(✿ﾟ▽ﾟ)ノ
  
  