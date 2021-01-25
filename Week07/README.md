# 第7周：重学Javascript(二)

>边看边忘:dog::dog::dog:

<!-- TOC -->

- [第7周：重学Javascript(二)](#第7周重学javascript二)
  - [:lollipop:知识点点](#lollipop知识点点)
    - [1.Expression（表达式）](#1expression表达式)
      - [1.1.Operator](#11operator)
      - [1.2.Reference](#12reference)
      - [1.3.Type Convertion（类型转换）](#13type-convertion类型转换)
    - [2.Statement（语句）](#2statement语句)
      - [2.1.Completion Record](#21completion-record)
      - [2.2.简单语句](#22简单语句)
      - [2.3.复合语句](#23复合语句)
      - [2.4.声明](#24声明)
      - [2.5.预处理（pre-process）](#25预处理pre-process)
      - [2.6.作用域](#26作用域)
    - [3.结构化](#3结构化)
      - [3.1.JS执行粒度（运行时）](#31js执行粒度运行时)
      - [3.2.事件循环（Event Loop）](#32事件循环event-loop)
      - [3.3.函数调用](#33函数调用)
      - [3.4.Function - Closure（闭包）](#34function---closure闭包)
      - [3.5.Realm](#35realm)
  - [:hourglass:疑难笔记](#hourglass疑难笔记)
      - [1.StringToNumber](#1stringtonumber)
      - [2.NumberToString](#2numbertostring)
      - [3.可视化直观感受一下Realm](#3可视化直观感受一下realm)
  - [:trophy:学习总结](#trophy学习总结)
  - [:sunflower:资料参考](#sunflower资料参考)
  - [:gift_heart:学习交流](#gift_heart学习交流)

<!-- /TOC -->

## :lollipop:知识点点

### 1.Expression（表达式）

- Grammar
  - Grammar Tree vs Priority 语法树和运算符优先级
  - Left hand side & Right hand side 运算符左值和右值
- Runtime
  - Type Convertion 类型转换
  - Reference 引用类型

#### 1.1.Operator

>运算符优先级以此降低  

1. Member运算（点运算符）
  - a.b
  - a[b]
  - foo\`string\`
  - super.b
  - super['b']
  - new.target
  - new Foo()

2. New
  - new Foo 

```javascript
Example:
new a()()
new new a()
```

3. Call（函数调用）
  - foo()
  - super()
  - foo()['b']
  - foo().b
  - foo()\`abc\`

```javascript
Example:
new a()['b']
``` 

4. Left Handside & Right Handside

>左手运算：能不能放到等号左边

```javascript
Example:
a.b = c;
a+b = c;（不可以）
```

5. Update（自增自减运算符）
  - a ++
  - a --
  - -- a
  - ++ a

```javascript
//两个语法不合法
++ a ++ 
++ (a ++)
```

6. Unary（单目运算符）
  - delete a.b
  - void foo()
  - typeof a
  - \+ a
  - \- a
  - ~ a
  - ! a
  - await a

7. Exponental（指数运算符）
  -  **（乘方，唯一右结合的运算符）

```javascript
Example：
//两式等同
3 ** 2 ** 3
3 ** (2 ** 3)
```

8. Multiplicative（乘法）
  - \* / %

9. Additive（加法）
  - \+ -
  
10. Shift（移位运算）
  - <<（左移） 
  - \>>（带符号右移） 
  - \>>>（无符号右移）

11. Relationship（关系比较表达式）
  - <
  - \>
  - <=
  - \>=
  - instanceof
  - in

12. Equality（相等）
  - ==
  - !=
  - ===
  - !==

13. Bitwise（位运算）
  - &
  - ^
  - |

14. Logical（逻辑）
  - &&
  - ||

>有 `短路原则`，有时候用来代替 `if else`    
>&& 运算符 前面为false，后面的不执行  
>|| 运算符 前面为true，后面的不执行

15. Conditional（三目运算符）
  - ? : 

>有 `短路原则`  

#### 1.2.Reference

>引用类型：标准中的类型，不是语言中的类型

- Object
- Key

- delete
- assign 赋值
  

#### 1.3.Type Convertion（类型转换）

| \ | Number | String | Boolean | Undefined | Null | Object | Symbol |
| -- | -- | -- | -- | -- | -- | -- | -- |
| Number | - | 复杂 | 0 false | X | X | Boxing | X |
| String | 复杂 | - | "" false | X | X | Boxing | X |
| Boolean | true 1 <br> false 0 | 'true' <br> 'false' | - | X | X | Boxing | X |
| Undefined | NaN | 'undefined' | false | - | X | X | X |
| Null | 0 | 'null' | false | X | - | X | X |
| Object | valueOf | valueOf <br> toString | true | X | X | - | X |
| Symbol | X | X | X | X | X | Boxing | - |

1. 类型发生转换
  - 相加
  - ==
  - 取属性 a[b]

2. Unboxing（拆箱转换）
   
>把 Object 转换成 基本类型

  - ToPremitive
    - toString
    - valueOf
    - Symbol.toPrimitive //定义该方法，会忽略上面两个方法
```javascript
var o = {
  toString() { return "2"},
  valueOf() { return 1},
  // [Symbol.toPrimitive]() { return 3}
}

//加法：优先 Symbol.toPrimitive，接下来 valueOf，前两个方法没定义的话，会调用 toString
conosle.log("x" + o) //x1

//作为属性名：Symbol.toPrimitive -> toString -> valueOf
var x = {}
x[o] = 1
```

3. Boxing（装箱转换）

>把 基本类型 转换成 Object

| 类型 | 对象 | 值 |
| -- | -- | -- |
| Number | new Number(1) | 1 |
| String | new String("a") | "a" |
| Boolean | new Boolean(true) | true |
| Symbol | new Object(Symbol("a")) | Symbol("a") |

***

### 2.Statement（语句）

- Grammar
  - 简单语句
  - 组合语句
  - 声明
- Runtime
  - Completion Record（运行结果记录）
  - Lexical Environment（作用域）

#### 2.1.Completion Record

- [[type]]: normal, break, continue, return, throw
- [[value]]: 基本类型
- [[target]]: label
  - 语句前面加一个冒号+标识符，配合 break, continue 使用

#### 2.2.简单语句

- ExpressionStatement（表达式语句）
- EmptyStatement（空语句）
  - 单独的分号
- DebuggerStatement
  - 调试使用
- ThrowStatement
  - 抛出异常
- ContinueStatement
  - 跳出当次循环
- BreakStatement
  - 结束整个循环
- ReturnStatement
  - 函数中使用

#### 2.3.复合语句

- BlockStatement
  - 一对花括号中间一个语句的列表
- IfStatement
- SwitchStatement
  - 不太建议使用，容易写错
- IterationStatement（循环）
  - while
  - do while
  - for
  - for in
  - for of
  - for await
- WithStatement
- LabelledStatement
  - 带label的break可以跳出多层循环
- TryStatement
  - try{ }catch(){ }Finally{ }

#### 2.4.声明

- FunctionDeclaration
  - function
- GeneratorDeclaration
  - function *
- AsyncFunctionDeclaration
  - async function
- AsyncGeneratorDeclaration
  - async function *
- VariableStatement
  - var
- ClassDeclaration
  - class
- LexicalDeclaration
  - const
  - let

#### 2.5.预处理（pre-process）
```javascript
var a = 2;
void function (){
  a = 1;
  return;
  var a;
}();
console.log(a); //2

var a = 2;
void function (){
  a = 1;
  return;
  const a;
}();
console.log(a); //报错 
```

#### 2.6.作用域
```javascript
var a = 2;
void function (){
  a = 1;
  {
    var a;
  }
}();
console.log(a);

var a = 2;
void function (){
  a = 1;
  {
    const a;
  }
}();
console.log(a);
```

***

### 3.结构化

#### 3.1.JS执行粒度（运行时）

- 宏任务（MacroTask）
  - 传给Javascript引擎的任务
  - script(整体代码)
  - setTimeout
  - setInterval
  - I/O
  - UI交互事件
  - postMessage
  - MessageChannel
  - setImmediate(Node.js 环境)
- 微任务（MicroTask）
  - 在JavaScript引擎内部的任务，一般都由Promise产生
  - Promise.then
  - Object.observe（基本已废弃）
  - MutaionObserver
  - process.nextTick(Node.js 环境)
- 函数调用（Execution Context）
- 语句/声明（Completion Record）
- 表达式（Reference）
- 直接量/变量/this...

#### 3.2.事件循环（Event Loop）

>get code -> execute -> wait -> get code

#### 3.3.函数调用

- Execution Context Stack（执行上下文栈）
  - Running Execution Context（栈顶正在跑的上下文）
  - Execution Context
    - code evaluation state
      - 用于async和generator函数的，代码执行到哪儿的保存信息
    - Function
    - Script or Module
    - Generator
    - Realm
      - 内置对象
    - LexicalEnvironment
      - 保存变量的环境
    - VariableEnvironment

- ECMAScript Code Execution Context
  - code evaluation state
  - Function
  - Script or Module
  - Realm
  - LexicalEnvironment
  - VariableEnvironment
- Generator Execution Contexts
  - code evaluation state
  - Function
  - Script or Module
  - Realm
  - LexicalEnvironment
  - VariableEnvironment
  - Generator
- LexicalEnvironment
  - this
  - new.target
  - super
  - 变量
- VariableEnvironment
  - 是个历史遗留的包袱，仅仅用于处理var声明
- Environment Record
  - Declarative Environment Records
    - Function Environment Records
    - Module Environment Records
  - Global Environment Records
  - Object Environment Records

#### 3.4.Function - Closure（闭包）

>JavaScript里面每一个函数它都会生成一个闭包

闭包分成两个部分，代码部分和环境部分，环境由一个object和一个变量的序列来组成的

在JavaScript里面，每一个函数都会带一个定义时的Environment Record

#### 3.5.Realm

在JS中，函数表达式和对象直接量均会创建对象  

使用.做隐式转换也会创建对象  

这些对象也是有原型的，如果我们没有Realm，就不知道他们的原型是什么  



## :hourglass:疑难笔记

#### 1.StringToNumber

#### 2.NumberToString

#### 3.可视化直观感受一下Realm


## :trophy:学习总结

本周主要学习内容 ：`JS表达式`、`JS语句`、`JS结构化`  

涉及 JS表达式、JS语句、JS结构化 等  

- [x] JS表达式
- [x] JS语句
- [x] JS结构化


## :sunflower:资料参考

- <a href="https://blog.csdn.net/wu_xianqiang/article/details/105215104">事件循环 - CSDN</a>
  
  
## :gift_heart:学习交流

![Hao](https://haoer.oss-cn-hangzhou.aliyuncs.com/hao.jpg)

欢迎大家加微信交流 Thanksヾ(✿ﾟ▽ﾟ)ノ
  
  