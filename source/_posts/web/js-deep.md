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