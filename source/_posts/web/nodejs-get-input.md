---
title: nodejs脚本获取用户输入
date: 2020-10-13 14:52:05
categories: 
	- [JavaScript, Nodejs]
tags: 
	- [Nodejs]
---

使用node脚本的时候，获取用户输入的内容：

```
const readline = require('readline');


const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

rl.question('你如何看待 Node.js 中文网？', (answer) => {
  // TODO：将答案记录在数据库中。
  console.log(`感谢您的宝贵意见：${answer}`);

  rl.close();
});


```

比如一个简单的获取用户输入姓名、年龄的小脚本：

```
const readline = require('readline');

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

function getName() {
    return new Promise(resolve => {
        rl.question('输入name：', (name) => {
            resolve(name)
        });
    })
}

function getAge() {
    return new Promise(resolve => {
        rl.question('输入age：', (age) => {
            // 一个close就可以了
            rl.close();
            resolve(age)
        });
    })
}

async function getInfo() {
    const name = await getName()
    const age = await getAge()

    console.log(name + '的年龄是：' + age)
}

getInfo()

```

运行的话就： node xx.js



参考：[Nodejs中文网](http://nodejs.cn/api/readline.html)