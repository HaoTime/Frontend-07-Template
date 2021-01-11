# 第1周：编程与算法练习

>基础薄弱，每每一个知识点都值得深研下去

<!-- TOC -->

- [第1周：编程与算法练习](#第1周编程与算法练习)
  - [:lollipop:知识点点](#lollipop知识点点)
    - [1.DOM方法](#1dom方法)
    - [2.循环](#2循环)
    - [3.深拷贝](#3深拷贝)
    - [4.异步编程](#4异步编程)
  - [:trophy:学习总结](#trophy学习总结)
  - [:gift_heart:学习交流](#gift_heart学习交流)

<!-- /TOC -->

## :lollipop:知识点点

### 1.DOM方法
```javascript
//获取元素
document.getElementById("id");

//创建元素
document.createElement("div");

//元素添加/删除类
element.classList.add/remove("class");

//添加子元素
element.appendChild(childElement);

//监听事件
element.addEventListener("click", function(){}, {once: true});

//class类名对象集合
document.getELementsByClassName("class");

```

***

### 2.循环
```javascript
//无限循环-最佳实践
while(true){ }
//无限循环-
for(;;){ }

//跳出此次循环，继续循环
continue

//跳出当前循环，终止循环
break
```

***

### 3.深拷贝
```javascript
//内置JSON函数
JSON.parse(JSON.stringify(obj))

//一维数组
Object.create(obj)
```

***

### 4.异步编程
```javascript
//callback

//promise
new Promise((resolve, reject)=>{}).then();

//async/await

//generator/yield
```

  
## :trophy:学习总结

本周主要两个编程小项目的训练：`三子棋`和`红绿灯`

- [x] 三子棋
- [x] 红绿灯
  

## :gift_heart:学习交流

![Hao](https://haoer.oss-cn-hangzhou.aliyuncs.com/hao.jpg)

欢迎大家加微信交流 Thanksヾ(✿ﾟ▽ﾟ)ノ

