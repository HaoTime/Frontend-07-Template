# 第6周：重学Javascript(一)

>开启下一个阶段，冲冲冲:tada::tada::tada:

<!-- TOC -->

- [第6周：重学Javascript(一)](#第6周重学javascript一)
  - [:lollipop:知识点点](#lollipop知识点点)
    - [1.语言按语法分类](#1语言按语法分类)
    - [2.产生式](#2产生式)
      - [2.1.产生式](#21产生式)
      - [2.2.BNF](#22bnf)
      - [2.3.通过产生式理解乔姆斯基谱系](#23通过产生式理解乔姆斯基谱系)
    - [3.现代语言分类](#3现代语言分类)
    - [4.编程语言性质](#4编程语言性质)
      - [4.1.图灵完备性](#41图灵完备性)
      - [4.2.动态与静态](#42动态与静态)
      - [4.3.类型系统](#43类型系统)
    - [5.一般命令式编程语言](#5一般命令式编程语言)
      - [5.1.结构层级](#51结构层级)
    - [6.重学JavaScript](#6重学javascript)
      - [6.1.Number](#61number)
        - [6.1.1.运行时](#611运行时)
        - [6.1.2.语法](#612语法)
      - [6.2.String](#62string)
      - [6.3.Boolean](#63boolean)
      - [6.4.Null & Undefined](#64null--undefined)
      - [6.5.Object](#65object)
        - [6.5.1.对象三要素](#651对象三要素)
        - [6.5.2.Class](#652class)
        - [6.5.3.Prototype](#653prototype)
        - [6.5.4.Object in JavaScript](#654object-in-javascript)
      - [6.6.Function Object](#66function-object)
      - [6.7.Host Object](#67host-object)
  - [:hourglass:疑难笔记](#hourglass疑难笔记)
    - [1.用 UTF8 对 string 进行遍码](#1用-utf8-对-string-进行遍码)
    - [2.用 JavaScript 去设计狗咬人的代码](#2用-javascript-去设计狗咬人的代码)
    - [3.JavaScript 标准里面所有具有特殊行为的对象](#3javascript-标准里面所有具有特殊行为的对象)
  - [:trophy:学习总结](#trophy学习总结)
  - [:sunflower:资料参考](#sunflower资料参考)
  - [:gift_heart:学习交流](#gift_heart学习交流)

<!-- /TOC -->

## :lollipop:知识点点

### 1.语言按语法分类

- 非形式语言
  - 中文、英文
- 形式语言（乔姆斯基谱系：计算机科学中刻画形式文法表达能力的一个分类谱系）
  - 0型 无限制文法
  - 1型 上下文相关文法
  - 2型 上下文无关文法
  - 3型 正则文法

> 形式语言：上下包含，2型肯定是1型也是0型，但1型肯定不是2型

***

### 2.产生式

#### 2.1.产生式  

在计算机中指 Tiger 编译器将源程序经过词法分析（Lexical Analysis）和语法分析（Syntax Analysis）后得到的一系列符合文法规则（BNF）的语句

通俗来说，`产生式` 就是符合 某个规则 的 一系列语句

#### 2.2.BNF

巴科斯诺尔范式（Backus Normal Form）  

一种用于表示上下文无关文法的语言   
一种用递归的思想来表述计算机语言符号集的定义规范   

通俗来说，BNF 就是 推导规则（产生式）的集合  
举个例子：用加减乘除的描述出四则运算，就是BNF
~~我自己理解的，不确定对~~  

**结构：**

<符号> ::= <使用符号的表达式>

`符号` 是非终结符  
`表达式` 由一个符号序列，或用指示选择的竖线 '|' 分隔的多个符号序列构成  

`表达式` 是左端的 `符号` 的一种可能的替代  
通俗来说，`表达式` 进一步解释、定义 `符号`    

**语法：**
  - 语法结构
    - 基础结构（终结符），引号和中间的字符表示
    - 复合结构（非终结符），其他语法结构定义
  - `::=` 表示 定义
  - `""` 双引号里内容表示 字符，双引号外内容表示 语法部分
  - `<>` 尖括号里的内容表示 必选项
  - `[]` 方括号里的内容表示 可选项
  - `{}` 大括号里的内容表示 可重复0至无数次的项
  - `|` 竖线两边的是可选内容，相当于or
  - `*` 表示重复多次
  - `+` 表示至少一次

**举例：**
```javascript
<Hao> ::= <程序员> | <阳光宅男> | <修仙选手>
<Number> ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "9"
```

#### 2.3.通过产生式理解乔姆斯基谱系

- 0型 无限制文法
  - `?::=?`
  - 左右可以随便写
- 1型 上下文相关文法
  - `?<A>?::=?<B>?`
  - A前后的?必须是有关，有意义
- 2型 上下文无关文法
  - `<A>::=?`
  - 左边只能写一个非终结符，右边随便写
- 3型 正则文法
  - `<A>::=<A>?`
  - 递归定义的<A>不能出现在末尾，只能在最开头

> Javascript 总体是上下文无关文法，表达式大部分是正则文法  
> 但是有两个特例：  
> 乘方运算符\*\*  是右结合，右边先算，2\*\*1\*\*2 = 2  
> {get a{ reutn 1},  get: 1}  
>所以严格算是上下文相关文法  

***

### 3.现代语言分类

- 形式语言 - 用途
  - 数据描述语言
    - JSON、HTML、XAML、SQL、CSS
  - 编程语言 
    - C、C++、JAVA、C#、Python、Ruby、Perl、Lisp、T-SQL、Clojure、Haskell、JavaScript
- 形式语言 - 表达方式
  - 声明式语言：结果导向
    - JSON、HTML、XAML、SQL、CSS、Lisp、Clojure、Haskell
  - 命令式语言：过程导向（达成结果，每个步骤是怎样的）
    - C、C++、JAVA、C#、Python、Ruby、Perl、JavaScript

***

### 4.编程语言性质

#### 4.1.图灵完备性

所有的可计算的问题都可用来描述的语言 就具有 图灵完备性  

图灵完全性：具有无限存储能力的通用物理机器或编程语言  

- 命令式 —— 图灵机（等价于任何有限逻辑数学过程的终极强大逻辑机器）
  - goto
  - if 和 while
- 声明式 —— lambda 演算（邱奇）
  - 递归

#### 4.2.动态与静态

- 动态
  - 在用户的设备/在线服务器上
  - 产品实际运行时
  - Runtime
- 静态
  - 在程序员的设备上
  - 产品开发时
  - Compiletime

#### 4.3.类型系统

- 动态类型系统与静态类型系统
- 强类型与弱类型
  - 强类型：无隐式转换
    - JavaScript
      - String + Number -> String
      - String == Boolean
  - 弱类型：有隐式转换
- 复合类型
  - 结构体
  - 函数签名
- 子类型
- 泛型
  - 逆变/协变
  - 协变：凡是能用Array<Parent>的地方，都能用Array<Child>
  - 逆变：凡是能用Function<Child>的地方，都能用Function<Parent>

***

### 5.一般命令式编程语言

#### 5.1.结构层级  

- Atom （原子）
  - Identifier（标识符）
  - Literal（字面量）
  - 变量名
- Expression（表达式）
  - Atom
  - Operator（运算符）
  - Punctuator（标点符号）
- Statement（语句）
  - Expression
  - Keyword
  - Punctuator
- Structure（结构）
  - Function
  - Class
  - Process（过程 - JS没有，PASCAL有）
  - Namespace（命名空间 - C++有）
- Program
  - Program
  - Module
  - Package
  - Library

***  

### 6.重学JavaScript

语法 ——语义——> 运行时

通过一定的语法，表达一定语义，改变了运行时状态  

Atom
 - Grammar
   - Literal
   - Variable
   - Keywords
   - Whitespace
   - Line Terminator
 - Runtime
   - Types
     - Number
     - String
     - Boolean
     - Object
     - Null
     - Undefined
     - Symbol
   - Execution Context
   - 
#### 6.1.Number

##### 6.1.1.运行时
- Double Float - 64位双精度浮点型：IEEE 754 定义
  - Sign（1位） - 符号：0表示正数，1表示负数
  - Exponent（11位） - 阶码（指数位）：表示范围
    - 指数位有基准值（1后面加10个0），大于这个值是正的范围，小于是负的范围
    - 表示时要减掉这个基准值
  - Fraction（52位） - 尾数（精度位）：表示精度，也就是小数
    - 第一位之前有一个隐藏位，是1，精度一定是以1开头的，所以隐藏起来，不算在52位之中
  - 表示：符号 + 精度位 乘以 2 的 指数位减1 次方（很像二进制的科学计数法）
  - 每一位是一个bit，0或者1
  - 精度损失
    - 十进制转二进制时
    - 加法时
    - 比较等于时
    - 最大有可能损失1个e

##### 6.1.2.语法
  - DecimalLiteral（十进制）
    - 0
    - 0.
      - `0.toString()` 会报错，因为 `0.` 是一个合法的写法
      - 可以写作 `0 .toString()`
    - .2
      - 小数点一面有数字就是合法的
    - 1e3
  - BinaryIntegerLiteral（二进制）
    - 0b111
  - OctallIntegerLiteral（八进制）
    - 0o10
  - HexIntegerLiteral（十六进制）
    - 0xFF

#### 6.2.String

- Character（字符）：用 Code Point 表示
  - ASCII：127个字符，26个大写字母。26个小写字符，数字0到9，各种制表符，特殊字符，换行符等常用的字符，
    - 美国发明，其他编码格式都会兼容，因为它是计算机的基础
  - Unicode：全世界联合的庞大的编码集
  - UCS：0000 ~ FFFF
  - GB
    - GB2312
    - GBK（GB13000）
    - GB18030
    - 跟Unicode不兼容
    - 国标范围小，省空间
  - ISO-8859
    - 一堆东欧国家
    - 8859系列，在 ASCII 基础上扩展，不是一个统一的标准，互不兼容
    - 没中文
  - BIG5
    - 台湾，俗称大五码
    - 不用Unicode时，台湾游戏玩不了，乱码，不兼容
    - 不兼容就是同一个码点表示的意义不一样
    - 不能混合使用，操作系统要一下语言才能不出乱码
- Code Point（码点）：就是数字，比如用97表示a
- Encoding（编码方式）：
  - UTF
    - UTF8
      - 默认用一个字节表示一个字符
    - UTF16
      - 默认用二个字节表示一个字符

#### 6.3.Boolean

- true
- false

#### 6.4.Null & Undefined

- null
  - 关键字，不能做变量
- undefined
  - 不是关键字
  - `void 0` 获取 Undefined 值

#### 6.5.Object

##### 6.5.1.对象三要素

- identifier
- state
- behavior

在设计对象的状态和行为时，我们总是遵循 “行为改变状态” 的原则  

##### 6.5.2.Class

类是一种常见的描述对象的方式  

而 “归类” 和 “分类” 则是两个主要的流派  

对于 “归类” 方法而言，多继承是非常自然的事情。如C++  

而采用 “分类” 思想的计算机语言，则是单继承结构。并且会有一个基类Object。如C#、Java  

JavaScript比较接近 “分类” ，但也不完全时，是一个多范式的面向对象的语言  

##### 6.5.3.Prototype

原型是一种更接近人类原始认知的描述对象的方法  

我们并不试图做严谨的分类，而是采用 “相似” 这样的方式去描述对象  

任何对象仅仅需要描述它自己与原型的区别即可  

一般最顶层的原型 Object Prototype，有的最上次还有个 null

##### 6.5.4.Object in JavaScript

在JavaScript运行时，原生对象的描述方式非常简单，我们只需要关系 `原型 Prototype` 和 `属性 property` 两个部分  

JavaScript 的属性 就可以描述状态，也可以描述行为，因为其函数可以放进属性里  

唯一标识性用内存地址表示  

原型链：获取一个属性，会一直沿着原型往上找，原型的原型，一直到null

JavaScript用属性来统一抽象对象状态和行为  

一般来说，数据属性用于描述状态，访问器属性则用于描述行为  

数据属性中如果存储函数，也可以用于描述行为  

- 属性：kv对
  - k
    - Symbol
    - String
  - v
    - Data Property（数据属性）
      - [[value]]
      - writable
      - enumerable
      - configurable
    - Accessor Property（访问器属性）
      - get
      - set
      - enumerable
      - configurable
- 语法
  - 基本的
    - `{}` 创建对象
    - `.` 访问属性
    - `[]` 定义新属性
    - `Object.defineProperty` 改变属性的特征值
  - 基于原型的
    - `Object.create` 创建对象
    - `Object.setPrototypeOf` 修改对象的原型
    - `Object.getPrototypeOf` 获取对象的原型
  - 基于分类的
    - new
    - class
    - extends
  - 历史包袱（不建议用）
    - new
    - function
    - prototype

#### 6.6.Function Object

JavaScript 中 特殊的对象，比如函数对象  

除了一般对象的属性和原型，函数对象还有一个行为[[call]]  

用JavaScript中的function关键字、箭头运算符或者Function构造器创建的对象，会有[[call]]这个行为  

用类似f()这样的语法把对象当做函数调用时，会访问到[[call]]这个行为  

如果对应的对象没有[[call]]行为，则会报错

>使用双方括号定义的行为 属于对象的内置行为，在 JavaScript 无论通过什么方式都无法访问到

#### 6.7.Host Object

宿主环境定义的对象  


## :hourglass:疑难笔记

### 1.用 UTF8 对 string 进行遍码

### 2.用 JavaScript 去设计狗咬人的代码

### 3.JavaScript 标准里面所有具有特殊行为的对象


## :trophy:学习总结

本周主要学习内容 ：`JS语言通识`、`JS类型`、`JS对象`  

涉及 编程语言分类、常用性质、产生式、BNF、Number、String、Object 等  

- [x] 编程语言分类、常用性质
- [x] 产生式
- [x] BNF
- [x] Number
- [x] String
- [x] Object


## :sunflower:资料参考

- <a href="https://www.zhihu.com/question/21833944">应该如何理解「上下文无关文法」？ - 知乎</a>
- <a href="https://bk.tw.lvfukeji.com/baike-%E5%B7%B4%E7%A7%91%E6%96%AF%E8%8C%83%E5%BC%8F">巴科斯范式 - 中文维基百科</a>
- <a href="https://juejin.cn/post/6877043337262694408">BNF范式（巴科斯范式）到底是什么？- 掘金</a>
  
  
## :gift_heart:学习交流

![Hao](https://haoer.oss-cn-hangzhou.aliyuncs.com/hao.jpg)

欢迎大家加微信交流 Thanksヾ(✿ﾟ▽ﾟ)ノ
  
  