## 学习笔记

>无志者长立志，有志者立常志

### 1.fill()

将数组中指定区域的所有元素的值，替换成某个固定的值

>array.fill(value [,start [,end]])
>
>value 必选，start、end 可选

```javascript
[1,2,3,4,5].fill(0,1,2); //[1,0,3,4,5]

Array(3).fill(1); //[1,1,1]
```

### 2.鼠标event

1. mousemove 
   - 鼠标移动事件
2. mousedown 
   - 鼠标按下事件
   - e.which //0:无 1:左键 2:中间滑轮 3:右键
3. mouseup 
   - 鼠标抬起事件
4. contextmenu
   - 右键弹出菜单事件
   - e.preventDefault() //阻止

### 3.localStorage

>html5 新增特性，localStorage 和 sessionStorage
>
>用来作为会话存储和本地存储来使用
>
>解决了cookie存储空间不足的问题（每条cookie的存储空间4k）

![localStorage和sessionStorage](localStorage.png)

### 4.队列 和 栈

>队列：先进先出，用 `push` 和 `shift`
>
>栈：先进后出，用 `push` 和 `pop`

### 5.搜索算法

>广度优先搜索：用`队列`数据结构
>
>深度优先搜索：用`栈`数据结构
>
>A*搜索：用`排序`数据结构，能找到最优路径的启发式寻路叫 `A*`，不一定能找到最终的启发式寻路叫 `A`

### 6.null 和 object

>javascript 中数据类型有 基本类型 和 引用类型 两大类
>
>基本类型：String、Number、Boolean、Null、Undefined、Symbol、BigInt
>
>引用类型：Object、Array、Function

判断类型方式

1. typeof
   - 返回一个字符串，表示未经计算的操作数的类型 
   - 可以判断基本类型，除了Null
   - `typeof null` 结果是 "object"
   - 不能判断 Object、Array、Function 结果是 "object"

2. instanceof
   - 用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型上
   - function C() {}; var o = new C(); console.log(o instanceof C); //true  

### 7.乘方运算法 `**`
```javascript
2**3 //8
```

### 8.class声明

>class声明 创建一个基于原型继承的具有给定名称的新类
>
>类声明不允许再次声明已经存在的类
>
>类声明不可以提升，与函数声明不同

```javascript
//类声明，构造函数是可选的
class polygon {
   //constructor 是一种用于创建和初始化class创建的对象的特殊方法
   constructor(height, width) {
      this.name = 'polygon';
      this.area = height * width;
   }
}

//继承
//super 用于访问和调用一个对象的父对象上的函数
//super()只能在构造函数中使用，必须用this关键字调用
class Square extends polygon {
   constructor(length) {
      super(length, length);//调用父对象的构造函数
      this.name = 'square';
   }
}
console.log(new polygon(4,3).area) //12
console.log(new polygon(4,3).name) //polygon
console.log(new Square(2).name)//square
```
  
  
## 疑难笔记

### 1.行内元素空白间距问题

原因：inline 或 inline-block 下元素间的空格或换行符引起
解决：父级元素设置 `font-size:0;`
扩展：图片间的空白间距也可以用此办法解决
  
  
## 学习总结

本周主要两个编程小项目的训练，`寻路`

涉及 广度优先搜索、异步编程可视化UI、启发式搜索 等

选学：二叉堆?（没学）

