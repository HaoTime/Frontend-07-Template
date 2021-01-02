## :pushpin:学习笔记

>今天不学习，明天变垃圾:ghost::ghost::ghost:

### 1.Proxy

用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）  

#### 1.1.语法

```javascript
const p = new Proxy(target, handler)
```

- target：代理对象，可以是任何类型的对象（数组、函数、另一个代理）
- handler：通常是函数，定义执行各种操作时代理 `p` 的行为

#### 1.2.handle 对象的方法

- `set()` 属性设置操作
- `get()` 属性读取操作
- `deleteProperty()` delete操作符
- `apply()` 函数调用操作
- `construct()` new操作符

```javascript
let target = {}
let p = new Proxy(target, {});

p.a = 37;

console.log(target.a); //37
```

***

### 2.CSS

#### 2.1.transform

旋转、缩放、倾斜、平移元素  

- `translate()` 平移
- `rootate()` 旋转
- `scale()` 缩放
- `skew()` 倾斜

#### 2.2.childNodes

获取子节点列表

#### 2.3.textContent

获得节点文本内容

***   

### 3.Range

表示一个包含节点与文本节点的一部分文档片段

**创建**
```javascript
doucment.createRange()
```


## :hourglass:疑难笔记

### 1.监听 mousemove、mouseup

一般选择在 `document` 上监听  

防止鼠标拖的过快，移开了可拖动区域，会出现拖断的情况


  
## :trophy:学习总结

本周主要学习内容：`reactive原理实现`、`拖拽`  

涉及 Proxy、Range、CSSOM 等  

- [ ] Proxy
- [ ] Range
- [ ] CSSOM
- [x] 编程与算法学习总结
  - [第一周：三子棋 和 红绿灯](https://github.com/HaoTime/Frontend-07-Template/tree/main/Week01)
  - [第二周：寻路](https://github.com/HaoTime/Frontend-07-Template/tree/main/Week02)
  - [第三周：LL算法构建AST](https://github.com/HaoTime/Frontend-07-Template/tree/main/Week03)
  - [第四周：字符串分析算法 - 字典树、KMP、Wildcard](https://github.com/HaoTime/Frontend-07-Template/tree/main/Week03)
  - [第五周：Reactive 原理实现 和 拖拽](https://github.com/HaoTime/Frontend-07-Template/tree/main/Week03)
  
  
## :sunflower:资料参考

- <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy">Proxy - MDN</a>
- <a href="https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform">transform - MDN</a>
    
  
## :gift_heart:学习交流

![Hao](https://haoer.oss-cn-hangzhou.aliyuncs.com/hao.jpg)

欢迎大家加微信交流 Thanksヾ(✿ﾟ▽ﾟ)ノ
  
  