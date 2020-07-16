---
title: charles抓包https显示unknown问题解决
date: 2020-07-08 10:30:55
categories: 开发工具
tags: 开发工具
desc: 记录抓包日常
---

抓包前的工作，就不做详细介绍了， 网上有一大堆。这里主要记录下，我抓包的时候，一直显示unknown的原因。

问题描述：

按照网上的教程，比如 [Charles抓包https](https://www.jianshu.com/p/ec0a38d9a8cf)完整的配置完之后，charles一直显示unknown，无法显示。

解决：

在安装手机证书的时候，charles有提示，如下图：

![install certificate](/images/charles-1.png)

就是说，安装的时候，需要链接到charles的代理上，也就手机端访问`chls.pro/ssl`之前，需要先链接到charles的代理才可以，手机上的证书需要和电脑端匹配才行的。

如此简单的操作，如此粗浅的逻辑，实在是。。。不应该
