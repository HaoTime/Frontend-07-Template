## :pushpin:学习笔记

>一遍看不懂看两遍，两遍看不懂看三遍，总会懂得:rainbow::rainbow::rainbow:  

 字符串分析算法

- 字典树
  - 大量高重复字符串的存储与分析
  - 精确匹配
- KMP
  - 在长字符串里找短字符串模式
  - 部分匹配
  - 时间复杂度O(m+n)
- Wildcard
  - 带通配符的字符串模式
  - `?`、`*`
  - 时间复杂度O(m+n)
- 正则
  - 字符串通用模式匹配
- 状态机
  - 通用的字符串分析
- LL LR
  - 字符串多层级结构分析

### 1.Symbol

基本数据类型，Symbol表示独一无二的值。
```javascript
// 没有参数
var s1 = Symbol();
var s2 = Symbol();
s1 === s2 // false

//这里的 hao 只是对 Symbol 的描述，没实质意义，主要为了区分
var s1 = Symbol('hao');
var s2 = Symbol('hao');
s1 === s2 // false
```
#### 1.1.特性

1. 不支持 `new Symbol()` 语法
```javascript
let sym = new Symbol(); // TypeError
```

2. 使用 `typeof` 结果为 "symbol"，使用 `instanceof` 为 false
```javascript
let sym = Symbol("hao");

console.log(typeof sym); // "symbol"

console.log(sym instanceof Symbol); // false
```

3. 如果 Symbol 的参数是一个对象，就会调用该对象的 toString 方法，将其转为字符串，然后才生成 Symbol 值
```javascript
const obj = {
  toString() {
    return "abc";
  }
}

let sym = Symbol(obj); // Symbol(abc); 
```

4. 不能和与其他类型的值进行运算

#### 1.2.方法

1. `for`
使用给定的key搜索现有的symbol，如果找到则返回该symbol。否则将使用给定的key在全局symbol注册表中创建一个新的symbol。

2. `keyFor`
返回一个已登记的 Symbol 类型值的 key。

```javascript
console.log(Symbol.for("hao")); // Symbol(hao)

console.log(Symbol.keyFor(sym2)); // "hao"
```

### 2.in

>语法：(x in object)  
>当 object 为数组，x 为数组的 "索引"  
>当 object 为对象，x 为对象的 "属性"  

```javascript
let arrs = ["a","b","c"]
console.log("b" in arrs) // false
console.log(2 in arrs) // true

let obj = { a: 1, b: 2, c: 3}
console.log(2 in obj) // false
console.log(b in obj) // true
```

### 3.String 方法

#### 3.1.fromCharCode

>返回由指定的 UTF-16 代码单元序列创建的字符串  

语法：`String.fromCharCode(num1[, ...[, numN]])`

`num1, ..., numN` 一系列 UTF-16 代码单元的数字。  
范围介于0到65535（0xFFFF）之间。  
大于 0xFFFF 的数字将被截断。

```javascript
String.fromCharCode(65, 66, 67) // "ABC"
String.fromCharCode(0x2014) // "-"
```

#### 3.2.charCodeAt

>返回 0 到 65535 之间的整数，表示给定索引处的 UTF-16 代码单元  

语法：`str.charCodeAt(index)`  

`index`：字符串的索引
一个大于等于0，小于字符串长度的整数。  
如果不是一个数值，则默认为0。  
如果小于0、等于或大于字符串的长度，则返回NaN。  

```javascript
"AB".charCodeAt(0) // 65
"AB".charCodeAt(1) // 66
"AB".charCodeAt(2)  // NaN
```

### 4.Math

>是一个内置对象，
>不是一个函数对象  
>用于 Number 类型，不支持 BigInt  

#### 4.1.方法

1. `Math.abs(x)` 绝对值
2. `Math.ceil(x)` 向上取整
3. `Math.floor(x)` 向下取整
4. `Math.round(x)` 四舍五入
5. `Math.max([x[, y[, ...]]])` 最大值
6. `Math.min([x[, y[, ...]]])` 最小值
7. `Math.random()` 0到1之间的伪随机数
8. `Math.trunc(x)` 直接取整

```javascript
Math.abs(-2) // 2
Math.ceil(2.2) // 3
Math.floor(2.8) // 2
Math.round(2.5) // 3
Math.max(2,1,3) // 3
Math.min(2,1,3) // 1
Math.random() // 0.5227224445984631
Math.trunc(2.523123) // 2
```
  
  
## :trophy:学习总结

本周主要学习内容 字符串分析算法：`字典树`  

涉及 Symbol 等  

- [x] Symbol
  
  
## :sunflower:资料参考

- <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol">Symbol - MDN</a>
- <a href="https://juejin.cn/post/6909492639842697229">[每日一题]ES6中为什么要是用Symbol? - 掘金</a>
- <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String">String - MDN</a>
- <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math">Math - MDN</a>
    
  
## :gift_heart:学习交流

![Hao](https://haoer.oss-cn-hangzhou.aliyuncs.com/hao.jpg)

欢迎大家加微信交流 Thanksヾ(✿ﾟ▽ﾟ)ノ
  
  