---
title: css常见问题小记
date: 2019-12-09 14:31:07
categories: CSS
tags: css日常开发
---

css日常开始时，遇到的问题，通过搜索找到的答案，做下总结。

# less参考

[less](https://less.bootcss.com/)

## &符号解释

举例：

```
.head{
  height: 100px;
  width: 100px;
  border: 1px solid gainsboro;
  background-color: #000000;

  .content{
    background-color: #fff;
  }

  &.body{
    background-color: #72cc26;
  }

}
```

编译之后：

```
.head {
  height: 100px;
  width: 100px;
  border: 1px solid gainsboro;
  background-color: #000000;
}
.head .content {
  background-color: #fff;
}
.head.body {
  background-color: #72cc26;
}
```

这里我们可以看到在类前面添加了`&`之后，编译之后的`css`变为且的关系，而没有使用`&`的`css`是父子的关系。

> 这里需要注意.a.b和.a .b之间的区别，.a.b 是且的关系意思就是2者必须都具备，而.a .b是上下级，父子关系:

```
<!--.a.b-->
<div class="a b"></div>
<!--.a .b-->
<div class="a">
    <div class="b"></div>
</div>
```


# 类选择器的方式

1. 后代选择器, E1 E2, 选择所有被E1包含的E2。中间用空格分隔。匹配那些由第一个元素作为祖先元素的所有第二个元素(后代元素) ,不需要相匹配元素之间要有严格的父子关系
2. p,h2,h1, 将同样的定义应用于多个选择符
3. 子元素选择器, X>Y, ,只会匹配那些作为第一个元素的直接后代(子元素)的第二元素
4. .a.b，就是2者必须都具备，`<div class="a b"></div>`

# flex布局汇总

参考链接：
知乎： https://zhuanlan.zhihu.com/p/25303493

阮一峰总结：https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

## 容器上的属性


* flex-direction：决定主轴的方向(即项目的排列方向)，默认值：row，主轴为水平方向，起点在左端。
* flex-wrap：决定容器内项目是否可换行
* flex-flow：flex-direction 和 flex-wrap 的简写形式
* justify-content：定义了项目在主轴的对齐方式。
* align-items：定义了项目在交叉轴上的对齐方式
* align-content：定义了多根轴线的对齐方式，如果项目只有一根轴线，那么该属性将不起作用

## item上的属性，子元素

* order：定义项目在容器中的排列顺序，数值越小，排列越靠前，默认值为 0
* flex-basis：定义了在分配多余空间之前，项目占据的主轴空间，浏览器根据这个属性，计算主轴是否有多余空间；当主轴为水平方向的时候，当设置了 flex-basis，项目的宽度设置值会失效，flex-basis 需要跟 flex-grow 和 flex-shrink 配合使用才能发挥效果。
* flex-grow：定义项目的放大比例
* flex-shrink：定义了项目的缩小比例
* flex：flex-grow, flex-shrink 和 flex-basis的简写。快捷键如下：
	* flex: auto (1 1 auto) 
	* flex: none (0 0 auto)
	* flex: 1； flex 取值为一个非负数字，则该数字为 flex-grow 值，flex-shrink 取 1，flex-basis 取 0%，意思就是剩余元素撑满空间
	* flex: 0；对应的三个值分别为 0 1 0%
	* flex: 0%; flex: 24px; 当 flex 取值为一个长度或百分比，则视为 flex-basis 值，flex-grow 取 1，flex-shrink 取 1，有如下等同情况（注意 0% 是一个百分比而不是一个非负数字）

# 文本一行，超出显示...

效果如下图：

<div style='display:flex;border:1px solid red;'>
        <div style='text-overflow: ellipsis;
            overflow: hidden;
            white-space: nowrap;
            display:block;'        
        >
        做 Web 开发的同学应该比较熟悉 Postman ，一个 HTTP API 测试工具。它是一个基于 Electron 开发的客户端软件，支持 OSX，Window 和 Linux。Postman 功能非常强大，支持 REST，SOAP 和 GraphQL 请求，可以实现自动化接口测试、接口监控、模拟接口数据、生成接口文档、多人协作等。总之，对开发 Web API 来说，Postman 是一个非常好的工具。
        </div>     
</div>

实例代码如下：

```
<!-- div元素 -->
<div class="container">
  <div class="content">
    做 Web 开发的同学应该比较熟悉 Postman ，一个 HTTP API 测试工具。它是一个基于 Electron 开发的客户端软件，支持 OSX，Window 和 Linux。Postman 功能非常强大，支持 REST，SOAP 和 GraphQL 请求，可以实现自动化接口测试、接口监控、模拟接口数据、生成接口文档、多人协作等。总之，对开发 Web API 来说，Postman 是一个非常好的工具。
  </div>
</div>


<!-- less语法如下 -->
.container {
    display: flex;

    .content {
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
        
        display: block;
    }
}
```

其中，`content`标签里的前三个是必须的。

虽然有时候测试的时候，hidden不写也生效了，但是规范一些，还是需要增加上。

`display: block;`这个，最好也是增加上，在不生效的情况，应该增加这个属性。

# list文本在一行，超出不展示

- [ ] 设置高度固定
- [ ] 设置flex-wrap: wrap; 貌似无需设置overflow: hidden;

# 设置两个视图顶部对齐

比如a需要设置顶部和b对齐，但是b的高度是自动撑开的，不知道本身的高度。

这时候就可以在外面再包一层，然后a和b都设置为absolute，这样顶部就可以对齐了。

# 文本垂直居中

这个方法只能将单行文本置中。只需要简单地把 `line-height` 设置为那个对象的 height 值就可以使文本居中了。

```
<div id="content"> Content here</div>
```

```
#content {
	height: 100px;
	line-height: 100px;
}
```

优点：

* 适用于所有浏览器
* 无足够空间时不会被截断

缺点：

* 只对文本有效(块级元素无效)
* 多行时，断词比较糟糕

这个方法在小元素上非常有用，例如使按钮文本或者单行文本居中。