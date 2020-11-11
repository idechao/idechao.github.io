---
title: JavaScript那些事
date: 2020-10-22 11:52:30
categories: JavaScript
tags: JavaScript
---


# 给数组增加firstObj方法

```
// if (!Array.prototype.firstObj) {	// 这里开始可以先判断是否有此方法
    Array.prototype.firstObj = function() {
        if (!Array.isArray(this) || this.length === 0) {
            return null
        }

        return this[0];
    }
// }


const myArray = [1,2,3,4,5,6]
console.log(myArray.firstObj());

```

# 闭包，返回带参数的函数

正常`JavaScript`返回一个函数很正常，对于返回的函数，带参数的情况，如下代码：

```
function sayHello (name) {
    return function greet (greeting) {
        console.log( greeting + ' ' + name)
    };
}

// greetMikeWith is now the returned function, which accepts a `greeting` param
// but still has a reference to `name`
var greetMikeWith = sayHello('Mike');

greetMikeWith('Hello!'); // Hello! Mike
greetMikeWith('Happy Holidays'); // Happy Holidays Mike

```

代码很简单，`greetMikeWith`是一个返回函数，此时里面的`name`值，已经确认，是`Mike`，在调用`sayHello`的时候确认的参数值。

所以在调用`greetMikeWith`的时候，相当于直接调用`greet`参数：

```
function greet (greeting) {
    console.log( greeting + ' ' + 'Mike')
};
```

MDN的介绍：[Closures](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)

# 提取字符串中的数字

```
var str = '123.456plus456.789加12后缀'
var numArr = str.match(/-?([1-9]\d*(\.\d*)*|0\.[1-9]\d*)/g)

console.log(numArr)	// [ '123.456', '456.789', '12' ]
```