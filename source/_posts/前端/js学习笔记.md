---
title: js学习笔记
date: 2019-12-05 20:42:05
categories: 
	- [JavaScript]
tags: 学习笔记
---

# 参考链接

js基础可以参考：

* [网道 / WangDoc.com](https://wangdoc.com/javascript/)
* [javascript.info](https://zh.javascript.info/js)
* [MDN-JavaScript](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)

ES6可以参考下面两个，内容相差不大：

* [ECMAScript 6 入门](https://es6.ruanyifeng.com/)
* [ES6入门文档](http://caibaojian.com/es6/)

# promise 重点！

一个简单promise示例：

```
// resolve结果
let promise = new Promise(function (resolve, reject) {
    setTimeout(() => resolve("done!"), 1000);
})


promise.then(
    result => console.log(result), // 在 1 秒后输出“done!”
    error => console.info(error) // 不会运行
);
```

reject的情况：

```
let promise = new Promise(function (resolve, reject) {
    setTimeout(()=>reject(new Error('time out!')), 1000);
})


promise.then(
    result => console.log(result), // 不会运行
    error => console.info(error) // Error: time out!
);
```

# 数组常用方法

## push/pop,shift/unshift

`shift`，返回删除的第一个元素; `unshift`就是在头部增加多个元素。

<span style="color:red; font-weight:bold">都会修改原数组</span>，想要获取第一个，可以直接使用`arr[0]`。

## 数组的拷贝

1、 优先考虑使用ES6的语法`...`

```
let arr1 = [1,2,3]
let arr2 = [...arr1]
arr2[0] = 0
console.log(arr1) // => [ 1, 2, 3 ]
console.log(arr2) // => [ 0, 2, 3 ]
```

2、 slice语法

无需传递参数，或者从0到数组的长度

```
let arr1 = [1,2,3]
let  arr2 = arr1.slice()

arr2[0] = 0

console.log(arr1)	// => [ 1, 2, 3 ]
console.log(arr2)	// => [ 0, 2, 3 ]
```

3、 通过`Object.assign`方法复制:

```
let arr1 = [1,2,3]
let arr2 = []
Object.assign(arr2, arr1)
arr2[0] = 0
console.log(arr1) // => [ 1, 2, 3 ]
console.log(arr2) // => [ 0, 2, 3 ]
```

## splice 添加，删除和插入元素

语法:

```
arr.splice(index[, deleteCount, elem1, ..., elemN])
```

从 `index` 开始：删除 `deleteCount` 元素并在当前位置插入 `elem1, ..., elemN`。最后返回已删除元素的数组。

可以将 `deleteCount` 设置为 0，`splice` 方法就能够插入元素而不用删除。

这里和其他数组方法中，负向索引是允许的。它们从数组末尾计算位置。

## slice 数组的子数组

它从所有元素的开始索引 `"start"` 复制到 `"end"` (不包括 `"end"`) 返回一个新的数组。`start` 和 `end` 都可以是负数，在这种情况下，从末尾计算索引。语法如下:

```
arr.slice(start, end)
```

如果不传参数，则表示复制整个数组，

## concat 数组合并

语法：

```
arr.concat(arg1, arg2...)
```

如下例子：

```
let arr = [1, 2];

// merge arr with [3,4]
alert( arr.concat([3, 4])); // 1,2,3,4

// merge arr with [3,4] and [5,6]
alert( arr.concat([3, 4], [5, 6])); // 1,2,3,4,5,6

// merge arr with [3,4], then add values 5 and 6
alert( arr.concat([3, 4], 5, 6)); // 1,2,3,4,5,6
```

## indexOf/lastIndexOf 和 includes 查询数组

* `arr.indexOf(item, from)` 从索引 `from` 查询 `item`，如果找到返回索引，否则返回 -1。
* `arr.lastIndexOf(item, from)` — 和上面相同，只是从尾部开始查询。
* `arr.includes(item, from)` — 从索引 `from` 查询 `item`，如果找到则返回 `true`。

> 注意对比`string`的`indexof`。如下测试：
> 
> ```
> const str = 'helloworld'
> console.log(str.indexOf('world')) // => 5
> console.log(str.indexOf('test')) // => -1 没找到
> ```


## find 和 findIndex 条件查询

语法：

```
let result = arr.find(function(item, index, array) {
  // 如果查询到返回 true
});
```

如果它返回`true`，则查询停止，返回 `item`。如果没有查询到，则返回 `undefined`。

**如果多个结果满足条件，则只返回第一个，或者说是查询到满足条件的结果之后，就不会继续查询了。**

如下例子：

```
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

let user = users.find(item => item.id == 1);

alert(user.name); // John
```

## filter 条件查询

语法类似`find`，不过返回的是满足条件所有数据，是一个数组。

```
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

// 返回前两个用户的数组
let someUsers = users.filter(item => item.id < 3);

alert(someUsers.length); // 2
```

## map 数组转换

它对数组中每个元素调用函数并返回符合结果的数组。

语法：

```
let result = arr.map(function(item, index, array) {
  // 返回新值而不是当前元素
})
```

如：

```
let lengths = [1, 2, 3].map(item => item >= 2)
console.log(lengths); // => [ false, true, true ]

// 或者
let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length)
console.log(lengths); // => [ 5, 7, 6 ]
```

## sort 数组排序

注意箭头函数的写法

```
let arr = [1,4,8,5,7]
console.log( arr.sort( (a,b)=>a > b) ) // => [ 1, 4, 5, 7, 8 ]
```

## reverse 颠倒元素

```
let arr = [1, 2, 3, 4, 5];
arr.reverse();

console.log( arr ); // => [ 5, 4, 3, 2, 1 ]
```

## split join 数组字符串转换

`split` 方法有一个可选的第二个数字参数 — 对数组长度的限制。如果提供了，那么额外的元素将被忽略

语法：

```
let string = 'my,name,is,Tom'
let arr = string.split(',')
console.log(arr) // => [ 'my', 'name', 'is', 'Tom' ]
```

**调用空的参数 `split(s)` 会将字符串分成一个字母数组。注意空字符与空参数的区别：**


```
// 参数为空字符
let string = 'my,name,is,Tom'
let arr = string.split('')
console.log(arr) // => [ 'm', 'y', ',', 'n', 'a', 'm', 'e', ',', 'i', 's', ',', 'T', 'o', 'm' ]

// 参数为空，整体返回，变为数组
let string = 'my,name,is,Tom'
let arr = string.split()
console.log(arr) // => [ 'my,name,is,Tom' ]
```


`join` 为数组转为字符串，通过某个沾合符链接数组元素，注意元素为空的情况：

```
let arr = ['Bilbo', 'Gandalf', 'Nazgul'];
let str1 = arr.join(";");
console.log( str1 ); // => Bilbo;Gandalf;Nazgul

let str2 = arr.join("");
console.log( str2 ); // => BilboGandalfNazgul

let str3 = arr.join();
console.log( str3 );  // => Bilbo,Gandalf,Nazgul
```

## reduce/reduceRight 单值计算

当我们需要遍历一个数组时 — 我们可以使用 `forEach`。

当我们需要迭代并返回每个元素的数据时 — 我们可以使用 `map。

`arr.reduce` 和 `arr.reduceRight` 和上面差不多，但有点复杂。它们用于根据数组计算单个值。

语法：

```
let value = arr.reduce(function(previousValue, item, index, arr) {
  // ...
}, initial);
```

举例：

```
let arr = [1, 2, 3, 4, 5];

let result = arr.reduce((sum, current, index, arr) => {
    console.log(index)	// => [0 ... 4]
    return sum + current
}, 10);

console.log(result); // => 25

```

意思就是sum初始化为10，每次sum都再加上current的值。返回之后的sum值。

简写形式如下：

```
let arr = [1, 2, 3, 4, 5];

let result = arr.reduce((sum, current, index, arr) => sum + current, 10);

console.log(result); // => 25

```

<span style="color:red;font-weight:bold;">如果数组为空，那么在没有初始值的情况下调用 reduce 会导致错误。所以建议始终指定初始值。</span>

## foreach遍历

语法:

```
arr.forEach(function(item, index, array) {
  // ... do something with item
});
```

<span style="color:red;font-weight:bold;">注意参数顺序是固定的，如果只有两个参数，那就是`item`和`index`。</span>

## 判断数组

数组基于对象。不构成单独的语言类型。

所以 typeof 无法从对象中区分出数组来:

```
console.log(typeof {}); // => object
console.log(typeof []); // => object

console.log(Array.isArray({})) // => false
console.log(Array.isArray([])) // => true
```

## 总结，数组方法备忘录

数组方法备忘录：

### 添加/删除元素：

* push(...items) — 从结尾添加元素，
* pop() — 从结尾提取元素，
* shift() — 从开头提取元素，
* unshift(...items) — 从开头添加元素，
* splice(pos, deleteCount, ...items) — 从 index 开始：删除 deleteCount 元素并在当前位置插入元素。
* slice(start, end) — 它从所有元素的开始索引 "start" 复制到 "end" (不包括 "end") 返回一个新的数组。
* concat(...items) — 返回一个新数组：复制当前数组的所有成员并向其中添加 items。如果有任何items 是一个数组，那么就取其元素。

### 查询元素：

* indexOf/lastIndexOf(item, pos) — 从 pos 找到 item，则返回索引否则返回 -1。
* includes(value) — 如果数组有 value，则返回 true，否则返回 false。
* find/filter(func) — 通过函数过滤元素，返回 true 条件的符合 find 函数的第一个值或符合 filter 函数的全部值。
* findIndex 和 find 类似，但返回索引而不是值。

### 转换数组：

* map(func) — 从每个元素调用 func 的结果创建一个新数组。
* sort(func) — 将数组倒序排列，然后返回。
* reverse() — 在原地颠倒数组，然后返回它。
* split/join — 将字符串转换为数组并返回。
* reduce(func, initial) — 通过为每个元素调用 func 计算数组上的单个值并在调用之间传递中间结果。

### 迭代元素：

* forEach(func) — 为每个元素调用 func，不返回任何东西。

### 其他： 
 
– Array.isArray(arr) 检查 arr 是否是一个数组。

### 注意事项

<span style="color:red;font-weight:bold;">请注意，sort，reverse 和 splice 方法修改数组本身。</span>

这些方法是最常用的方法，它们覆盖 99％ 的用例。但是还有其他几个：

* arr.some(fn)/arr.every(fn) 检查数组。

在类似于 map 的数组的每个元素上调用函数 fn。如果任何/所有结果为 true，则返回 true，否则返回 false。

* arr.fill(value, start, end) — 从 start 到 end 用 value 重复填充数组。

* arr.copyWithin(target, start, end) — copies its elements from position start till position end into itself, at position target (overwrites existing).将其元素从 start 到 end 在 target 位置复制到 本身（覆盖现有）。

有关完整列表，请参阅[手册](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)。


# 对象


## 属性值的简写

在有变量的情况下，对象可以直接简写为变量名，这样`key`就是变量的名字，值为实际变量的值，写法上可以直接省略`value`，如下：

```
let name = 'Tom'

let user = {
    name,
    age: 30,
}

console.log(user.name)
```

正常输出：

```
Tom
```

## 计算属性


计算属性就是先声明一个变量，然后以<span style="color:red;font-weight:bold;">方括号</span>的形式来设置变量。

```
let name = '随便起的name标识key'
let user = {
    [name]: 'Tom',
}
console.log(user[name])
```

正常输出输出如下：

```
Tom
```

设置和取值的时候都是以<span style="color:red;font-weight:bold;">方括号</span>的形式来设置的，如下直接使用点语法是获取不到的：

```
console.log(user.name)

//输出
undefined
```

这里在Symbol属性中有使用，可以参考下。

## 方法参数，如{index}

如下参数，是个对象，调用的情况下，需要传递`key`为`index`才能生效:

``` javasript
function test({index}) {
    console.log(index)
}

test({index:4}) // => 4
```

# 可迭代对象 ITerables

`Iterable` （可迭代对象）是数组的泛化。这个概念是说任何对象都可在 `for..of` 循环中使用。

数组本身就是可迭代的。但不仅仅是数组。字符串也可以迭代，很多其他内建对象也都可以迭代。

这个方法必须返回一个迭代器 —— 一个有 `next` 方法的对象。`next()` 返回结果的格式必须是 `{done: Boolean, value: any}`，当 `done=true` 时，表示迭代结束，否则 `value` 必须是一个未被迭代的新值。

```
let range = {
    from: 1,
    to: 5
}

range[Symbol.iterator] = function () {
    return {
        current: this.from,
        last: this.to,

        next() {
            if (this.current <= this.last) {
                return {done: false, value: this.current++}
            } else {
                return {done: true, value: this.current}
            }
        }
    }
}

for (let num of range) {
    console.log(num)
}

```

## 可迭代对象和类数组对象

* `Iterables` 是应用于 `Symbol.iterator` 方法的对象，像上文所述。
* `Array-likes` 是有索引和 `length` 属性的对象，所以它们很像数组。

可以使用 Array.from方法，将可迭代对象，或者类数组对象，转变为一个新的数组：

``` 
let arrayLike = {
  0: "Hello",
  1: "World",
  length: 2
};

let arr = Array.from(arrayLike); // (*)
alert(arr.pop()); // World（pop 方法生效）
```

# Symbol类型

`Symbol` 值表示唯一的标识符。

可以使用 Symbol() 来创建这种类型的值。全局的使用`Symbol.for(key)`的方式，如果key存在，则使用，不存在，则创建。

举例：隐藏属性。

如原先可能有定义`id`字段，现在想新增一个唯一标识符，那不确定的情况下，不能直接设置`id`的值，会覆盖。这种情况下就使用`Symbol`唯一标识符。

```
let user = {
    name: 'bob',
}
let id = Symbol('id');
user[id] = 'new id'

console.log(user[id])
```

正常输出为：

```
new id
```

# 逻辑运算符

```
Markdown 最后是会被转换成 HTML 故使用 <code> 标签来解决
| 在 HTML 中传输使用的 ACSII 码为 124，故使用 &#124; 替换之
```

js中的特定功能：

|运算符|语法|说明|
|:---:|:---:|:---:|
|逻辑与，AND（&&）|	expr1 && expr2|	若 expr1 可转换为 true，则返回 expr2；否则，返回 expr1。|
|逻辑或，OR（&#124; &#124;）|	expr1 &#124; &#124; expr2 |	若 expr1 可转换为 true，则返回 expr1；否则，返回 expr2。|
|逻辑非，NOT（!）|	!expr	| 若 expr 可转换为 true，则返回 false；否则，返回 true。|

# 函数

## 函数表达式

使用函数表达的时候，在赋值的情况下，右边的函数带括号和不带括号，使用起来是不一样的，如定义函数：

```
function sayHello() {
    console.log('hello~')
}
```

在赋值的时候，第一种直接使用`sayHello`，不带括号，如下：

```
let hello = sayHello
hello() // => hello
hello // => 没有效果
```

表示`sayHello`复制到了变量`hello`。

第二种，带着括号：

```
let hello2 = sayHello()
hello() // => error
hello // => hello~
```
表示将调用结果写进`hello2`，而不是函数本身。

在直接定义的情况，也就是变量后面直接跟着函数的情况下，是以上面第一种情况一样的，使用的时候需要带着括号，如下：

```
let hello3 = function () {
    console.log('hi')
}
hello3()
```

## 命名函数表达式（NFE）

# 方法和函数的区别

* 函数（function） 函数是一段代码，需要通过名字来进行调用。它能将一些数据（函数的参数）传递进去进行处理，然后返回一些数据（函数的返回值），也可以不返回数据。 
* 方法（method）是通过对象调用的javascript函数。也就是说，方法也是函数，只是比较特殊的函数。  
* 当将函数和对象和写在一起时，函数（function）就变成了方法（method）。 


# rest,spread操作符 ...

方法入参可以传递任意参数，使用`...`的形式，放到最后。其中，`...`最终存储的为数组形式。

```
function sumAll(...args) {
    let sum = 0

    for (let arg of args) sum += arg

    return sum
}

console.log(sumAll(1,2,3,4,5))
```

相反的，比如`Math.max()`函数，支持不限个参数，但是不能是数组，这种情况可能就会用到这个spread操作符。

```
let arr = [3, 5, 1];

console.log( Math.max(...arr) ); // 5（Spread 操作符把数组转为参数列表）
```

<span style="color:red;font-weight:bold;">`...`出现的情况下，不是`Rest`参数，就是`Spread`操作符。</span>

> 扩展，函数的上下文会提供一个非常特殊的类数组对象 `arguments`，所有的参数被按序放置。`arguments `是一个类数组对象。

```
function showName() {
    console.log( arguments.length );
    console.log( arguments[0] );
    console.log( arguments[1] );

}

// 依次输出提示：2，Julius，Caesar
showName("Julius", "Caesar");

// 依次输出提示：1，Ilya，undefined（不存在第二个参数）
showName("Ilya");
```

# Q&A 

## 数组遍历，写法问题导致失败

如下例子，遍历数组，实际执行的时候回报错：`model is not defined`

```
const list = [{a:[]}]
const model = list[0]


(model.a || []).forEach(function (item) {
    console.log('====')
})
```

修改如下：

```
const list = [{a:[]}]
const model = list[0]


;(model.a || []).forEach(function (item) {
    console.log('====')
})
```

前面多了一个分号。

原因是括号的情况下，直接编译成和上面连续的代码了.

```
const list = [{a:[]}]
const model = list[0](model.a || []).forEach(function (item) {
    console.log('====')
})

```