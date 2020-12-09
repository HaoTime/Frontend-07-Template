## 学习笔记

>基础薄弱，每每一个知识点都值得深研下去

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

### 3.深拷贝
```javascript
//内置JSON函数
JSON.parse(JSON.stringify(obj))

//一维数组
Object.create(obj)
```

### 4.异步编程
```javascript
//callback

//promise
new Promise((resolve, reject)=>{}).then();

//async/await

//generator/yield
```


## 学习总结

本周主要两个编程小项目的训练，`三子棋`和`红绿灯`
