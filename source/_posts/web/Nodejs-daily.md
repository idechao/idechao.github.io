---
title: Nodejs那些事儿
date: 2020-10-15 16:18:54
categories: Nodejs
tags: Nodejs
---

本文记录`Nodejs`日常相关记录。

# util.promisify

util.promisify是在node.js 8.x版本中新增的一个工具，用于将老式的Error first callback转换为Promise对象。

首先要解释一下这种工具大致的实现思路，因为在Node中异步回调有一个约定：Error first，也就是说回调函数中的第一个参数一定要是Error对象，其余参数才是正确时的数据。

举例使用如下：

```
const { promisify } = require('util')
const exec = promisify(require('child_process').exec)

async function execExample() {
    const {stdout, stderr } = await exec('ls');
    console.log(stdout)
    console.log(stderr)
}
execExample()

```


