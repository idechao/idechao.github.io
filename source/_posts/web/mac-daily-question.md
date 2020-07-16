---
title: mac日常问题记录
date: 2020-07-11 10:39:44
categories: [mac问题]
tags:	[mac问题]
---

# Alfred无法搜到到应用程序

新安装的程序，在Alfred中无法搜索到。按照网上的提示，重建索引，提示成功，但是并没有成功。

```
// 重建索引
sudo mdutil -E /

// 结果，然而并未生效
Indexing enabled. 
```

使用如下命令可以完美解决：

```
sudo mdutil -a -i on
```
