# 第3周：编程与算法练习

>编译原理:dizzy_face::dizzy_face::dizzy_face:

<!-- TOC -->

- [第3周：编程与算法练习](#第3周编程与算法练习)
  - [:lollipop:知识点点](#lollipop知识点点)
    - [1.AST](#1ast)
      - [定义](#定义)
      - [特点](#特点)
      - [应用](#应用)
    - [2.上下文无关文法](#2上下文无关文法)
    - [3.BNF范式](#3bnf范式)
      - [基本结构：](#基本结构)
      - [语法：](#语法)
      - [产生式：](#产生式)
      - [举例：](#举例)
    - [4.语法分析算法](#4语法分析算法)
      - [两种核心思想](#两种核心思想)
      - [四则运算 - 词法定义](#四则运算---词法定义)
      - [四则运算 - 语法定义](#四则运算---语法定义)
      - [终结符：](#终结符)
      - [非终结符：](#非终结符)
    - [5.正则表达式](#5正则表达式)
      - [创建对象](#创建对象)
      - [对象方法](#对象方法)
      - [修饰词](#修饰词)
      - [方括号](#方括号)
      - [元字符](#元字符)
      - [限定符](#限定符)
      - [对象属性](#对象属性)
      - [支持正则表达式的 String对象 的方法](#支持正则表达式的-string对象-的方法)
      - [分组语法](#分组语法)
    - [6.生成器 Generator](#6生成器-generator)
      - [语法](#语法-1)
      - [方法](#方法)
    - [7.try/catch/finally](#7trycatchfinally)
  - [:hourglass:疑难笔记](#hourglass疑难笔记)
    - [1.四则运算 - 语法定义](#1四则运算---语法定义)
  - [:trophy:学习总结](#trophy学习总结)
  - [:sunflower:资料参考](#sunflower资料参考)
  - [:gift_heart:学习交流](#gift_heart学习交流)

<!-- /TOC -->

## :lollipop:知识点点

### 1.AST

#### 定义

AST 叫做抽象语法树，是用编程语言编写的源代码的抽象语法结构的树状表现形式，树的每个节点表示在源代码中出现的构造；  

这里的语法是 `抽象` 的，并不会表示出真实语法中出现的每个细节，只是结构、内容的相关细节；  

语法树通常由 `解析器 Parser` 在源代码转换和编译过程中构建，一旦构建，通过后续处理（如：上下文分析）将附加信息添加到AST。  
  
javaScript Parser 是把js源码转化为抽象语法树的解析器，一般分为词法分析、语法分析以及代码生成或执行  

![parser](https://haotime.oss-cn-hangzhou.aliyuncs.com/Frontend-07-Template/parser.png)

- 词法分析，也叫扫描scanner  
  词法解析代码的时候，是一个个字母读取，遇到空格、操作符或者特殊字符的时候，会认为一个话已经完成了读取代码，并且移除空白符、注释等，将整个代码分割进一个tokens列表（一维数组）。
- 语法分析，构建AST的过程  
  将词法解析生成出来的tokens列表生成树形的表达形式，同时验证语法错误，错误的话就抛出错误；在生成树的过程中，解析器会删除一些没必要的tokens（比如不完整的括号）

#### 特点

简单说，就是编程语言太多，需要一套统一的结构让计算机识别  

抽象语法树的特点：不依赖具体的文法、不依赖语言的细节  

>当源程序语法分析工作时，是在相应程序设计语言的语法规则指导下进行的；  
>语法规则描述了该语言的各种语法成分的组成结构；  
>通常可以用所谓的 `上下文无关文法` 或与之等价的 `Backus-Naur范式（BNF）` 将一个程序设计语言的语法规则确切的描述出来；  
>上下无关文法有 LL(1)、LR(0)、LR(1)、LR(k)、LALR(1) 等几类，每种的文法都有不同的要求；  
>  
>开发语言时，一开始可能选择LL(1)文法描述语言的语法规则，编译器前端生成LL(1)语法树，编译器后端对LL(1)语法树进行处理，生成字节码或者汇编语言；  
>但随着工程的开发，在语言中加入了更多的特性，这时用LL(1)文法描述时，感觉限制很大，并且编写文法时很吃力；  
>决定换用LR(1)文法，如果这时候把编译器前端改成LR(1)语法树，编译器后端不得不也要修改；
>  
>这时就是体现出 抽象语法树 的优点，不论是LL(1)，还是LR(1)，都要求在语法分析时，构造出相同的语法树

#### 应用

抽象语法树，可用于语法检查、编译、代码高亮、代码转换、优化、压缩等等场景  

Babel、Webpack、Lint等工具的核心都是通过抽象语法树实现的，实现对代码的检查、分析

***

### 2.上下文无关文法

上下文无关文法描述了一列形式语言，用来描述计算机语言语法的符号集

上下文无关文法中 **所有的产生式左边只有一个非终结符**  

比如：  
```javascript
S -> aSb
S -> ab
```
这就是上下文无关文法，这个文法有两个产生式，每个产生式左边都有一个非终结符S，因为只要找到符合产生式右边的串，就可以把它规约为对应的非终结符

比如：
```javascript
aSb -> aaSbb
S -> ab
```
这就是上下文相关文法，因为他的第一产生式左边有不止一个符号，所以你在匹配这个产生式中的S的时候必须确保这个S有正确的`上下文`，也就是确保左边的a和右边的b.

***

### 3.BNF范式

巴科斯诺尔范式（Backus Normal Form）  
一种用于表示上下文无关文法的语言  
一种用递归的思想来表述计算机语言符号集的定义规范  
  
BNF范式是推导规则（产生式）的集合  

#### 基本结构：

<符号> ::= <使用符号的表达式>
 
`符号` 是非终结符，而 `表达式` 由一个符号序列，或用指示选择的竖线 '|' 分隔的多个符号序列构成  

每个符号序列整体都是左端的符号的一种可能的替代  

在上下文无关文法中，从未在左端出现的符号叫做 `终结符`  

通俗来说，`表达式` 进一步解释、定义 `符号`  

#### 语法：
  - ::= 表示 定义
  - "" 双引号里内容表示 字符，双引号外内容表示 语法部分
  - <> 尖括号里的内容表示 必选项
  - [] 方括号里的内容表示 可选项
  - {} 大括号里的内容表示 可重复0至无数次的项
  - | 竖线两边的是可选内容，相当于or

#### 产生式：

在计算机中指编译器将源程序经过词法分析和语法分析后得到的一系列符合文法规则（BNF）的语句，即推导规则  

#### 举例：
```javascript
<Hao> ::= <程序员> | <阳光宅男> | <修仙选手>
<Number> ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "9"
```

***

### 4.语法分析算法

#### 两种核心思想
- LL算法：从左到右扫描，从左到右规约
- LR算法

#### 四则运算 - 词法定义  
- TokenNmber：
  - 1 2 3 4 5 6 7 8 9 0 的组合
- Operator：+、-、*、/ 之一
- Whitespace：<SP> //空格
- LineTerminator：<LF> <CR> //行终止符
  
#### 四则运算 - 语法定义

```javascript
//加法和乘法有优先级的关系，用产生式定义加法和乘法
//
//加法由左右两个乘法组成的
//单独的Number看成一个特殊的乘法，就是零次的一个乘法
//单独的乘法看成一个特殊的加法，就是零次的加法
//
//加法可以进行连加，是一个重复自身的序列，一个递归式的产生式的结构
//
//
//EOF 终结符 End of file
//

<Expression>::=
    <AdditiveExpression><EOF>

<AdditiveExpression>::=
    <MultiplicativeExpression>
    |<AdditiveExpression><+><MultiplicativeExpression>
    |<AdditiveExpression><-><MultiplicativeExpression>

<MultiplicativeExpression>::=
    <Number>
    |<MultiplicativeExpression><*><Number>
    |<MultiplicativeExpression></><Number>
```

#### 终结符：
  - Number
  - + - * /

#### 非终结符：
  - MultiplicativeExpression
  - AdditiveExpression 

***

### 5.正则表达式

#### 创建对象

```javascript
var regexp = new RegExp(pattern, modifiers);
或
var regexp = /pattern/modifiers;

var regexp = new RegExp("\\w+", "g");
var regexp = /\w+/g;
```

#### 对象方法

- `exec` 检索字符串中指定的值，有返回找到的值和位置，没有返回null
- `test` 检索字符串中指定的值，返回true 或 false

```javascript
var str1 = "hao,hello,world!";
var str2 = "ni,hao";
var regexp = /hello/g;

console.log(regexp.exec(str1)) //["hello", index: 4, input: "hao,hello,world!"]
console.log(regexp.exec(str2)) //null

console.log(regexp.test(str1)) //true
console.log(regexp.test(str2)) //false
```

#### 修饰词

- `g` 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）
- `i` 执行对大小写不敏感的的匹配

#### 方括号

用于查找某个范围内的字符

- `[0-9]` 匹配任何从 0 至 9 的数字
- `[abc]` 匹配方括号之间的任何字符（也就是a,b,c）
- `[^abc]` 匹配任何不在方括号之间的字符（除了a,b,c以外）
- `[a-z]` 
- `[A-Z]`
- `[A-z]`
- `(red|bule|green)` 匹配任何指定的选项（red 或 bule 或 green）

#### 元字符

- `-` 匹配单个字符，除了换行符和行结束符
- `\w` 匹配数字、字母及下划线
- `\W` 匹配非单词字符
- `\d` 匹配数字
- `\D` 匹配费数字字符
- `\s` 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 `[ \f\n\r\t\v]`
- `\S` 匹配任何非空白字符。等价于 `[^ \f\n\r\t\v]`

#### 限定符

注：`n` 可以是一个字符，也可以是一个表达式

- `n*` 匹配任何包含 0个 或 多个的字符串，等价于 `{0,}`
- `n+` 匹配任何包含 至少1个 n 的字符串，等价于 `{1,}`
- `n?` 匹配任何包含 0个 或 1个 n 的字符串，等价于 `{0,1}`
- `n{x}` 匹配包含 x个n 的字符串
- `n{x,}` 匹配包含 至少x个n 的字符串
- `n{x,y}` 匹配包含 至少x个n，至多y个n 的字符串
- `n$` 匹配任何 结尾为n 的字符串
- `^n` 匹配任何 开头为n 的字符串
- `?=n` 匹配任何 其后紧接 指定字符串 n 的字符串

#### 对象属性

- `lastIndex` 用于规定下次匹配的起始位置
  
```javascript
var str = "hao 123 hao 456 hao";
var regexp = /[0-9]/g;
regexp.test(str);
console.log(regexp.lastIndex); //5

```

#### 支持正则表达式的 String对象 的方法

- `search` 检索与正则表达式相匹配的值
- `match` 检索一个或多个正则表达式的匹配
- `replace` 替换与正则表达式匹配的字符串
- `split` 分割与正则表达式匹配的字符串为数组

```javascript
"Hello666Hao".search(/\d/)  //5
"HelloHao666Hao".match(/\d/)  //["6", index: 8, input: "HelloHao666Hao"]
"HelloHao666Hao".replace(/\d/, 88) //"Hello8866Hao"
"HelloHao666Hao".split(/\d/) //["HelloHao", "", "", "Hao"];
```

#### 分组语法

- `(exp)` 匹配exp，并捕获文本到自动命名的组里

```javascript
"123456hao123hao456".match(/([1-9]+)([a-z]+)/g); // ["123456hao", "123hao"]
"123456hao123hao456".match(/([1-9]+)|([a-z]+)/g); // ["123456", "hao", "123", "hao", "456"]
```

***

### 6.生成器 Generator

#### 语法

```javascript
function* gen() {
    yield 1;
    yield 2;
    yield 3;
}

let g = gen(); //"Generator {}"
```

#### 方法

- `next` 返回一个由 yield 表达式生成的值
- `return` 返回给定的值并结束生成器

```javascript
function* gen() {
    yield 1;
    yield 2;
    yield 3;
}

let g = gen(); //"Generator {}"

g.next(); //{ value: 1, done: false }
g.return(4); //{ value: 4, done: true }

//done：迭代器已经返回了迭代序列的末尾，则值为 true
//这时候生成器已经结束
//再调 g.next(), 返回 {value: undefined, done: true}

```

***

### 7.try/catch/finally

```javascript
let i = 1;
try {
    if (i!=2) {
        throw "出错了"
    }
}
catch (error){
    //捕获错误的代码块
    conosle.log(error) //出错了
}
finally {
    //无论 try / catch 结果如何都会执行的代码块
    console.log("过")
}
```


## :hourglass:疑难笔记

### 1.四则运算 - 语法定义

加法由左右两个乘法组成的? 

老师说原因是：单独的Number可以看成一个特殊的乘法，就是零次的一个乘法

***但为啥这么认为呢？？？***

***还有 为什么单独的乘法可以看成一个特殊的加法，就是零次的加法？？？***


## :trophy:学习总结

本周主要学习内容 编译原理 中字符串处理：`使用LL算法构建AST`  

涉及 编译原理、词法分析、语法分析、正则表达式、Generator 等  

- [ ] 编译原理
- [x] 词法分析
- [x] 语法分析
- [x] 正则表达式 - 了解
- [x] Generator
  
  
## :sunflower:资料参考

- <a href="https://blog.csdn.net/weixin_39408343/article/details/95984062">AST(抽象语法树)超详细 - CSDN</a>
- <a href="https://www.zhihu.com/question/21833944">应该如何理解「上下文无关文法」？ - 知乎</a>
- <a href="https://bk.tw.lvfukeji.com/baike-%E5%B7%B4%E7%A7%91%E6%96%AF%E8%8C%83%E5%BC%8F">巴科斯范式 - 中文维基百科</a>
- <a href="https://juejin.cn/post/6877043337262694408">BNF范式（巴科斯范式）到底是什么？- 掘金</a>
- <a href="https://time.geekbang.org/column/article/868235">（小实验）理解编译原理：一个四则运算的解释器 - 极客时间</a>
- <a href="https://astexplorer.net/">实时看AST网站 - AST Explorer</a>
- <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp">RegExp - MDN</a>
- <a href="https://www.runoob.com/regexp/regexp-tutorial.html">正则表达式 - 菜鸟教程</a>
- <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator">Generator - MDN</a>
  
  
## :gift_heart:学习交流

![Hao](https://haoer.oss-cn-hangzhou.aliyuncs.com/hao.jpg)

欢迎大家加微信交流 Thanksヾ(✿ﾟ▽ﾟ)ノ
  
  