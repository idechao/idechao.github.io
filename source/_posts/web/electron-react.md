---
title: Electron+React应用，完美解决启动问题
date: 2020-10-30 15:55:59
categories: Electron
tags: Electron
---

这里会首先介绍一下 react 和 electron 各自的环境搭建，再详细介绍如何将其完美的合二为一

# react 的开发环境

## 检查环境版本

最好将 node 版本升级到 10.0.0 以上，这里推荐一个 node 的版本管理工具 nvm, 可以根据不同情况切换 node 版本进行开发。

## 脚手架

一般一款优秀的框架都会为其配备一个优秀的脚手架，react 也不例外，它的脚手架就是
[create-react-app](https://www.html.cn/create-react-app/docs/getting-started/)，它的特点就是

- 简单
- 不需要任何配置
- 没有锁死，既可以开箱再添加些配置 npm run eject 弹出 webpack 配置
根据官网执行以下命令即可创建一个 react 项目，并运行起来。

## npx小知识点

* npx 只有 npm 5.2 以上的版本才会有
* 避免安装全局模块，很多时候 npm 会安装全局模块造成不必要的资源浪费
* 调用项目内部安装的模块：
	例如我们检查 node_modules/.bin 下的 nodemon 版本需要在命令行中这样写:
	`node_modules/.bin/nodemon --version`
	但是使用 npx 就可以直接 执行了:
	`npx nodemon —version` 
	在接下来的 react 的脚手架安装中会使用到 npx

```
npx create-react-app my-app
cd my-app
npm start
```

# electron 的开发环境搭建

如上 react 的 node 环境要求，避免出啥问题，node 版本最好在 10.0.0 以上

## 脚手架

electron 也有自己的脚手架 [electron-quick-start](https://github.com/electron/electron-quick-start)

根据官网执行以下命令便可开启electron 

```
git clone https://github.com/electron/electron-quick-start
cd electron-quick-start
npm install
npm start
```

> tip：这个过程会比较久，大概会有 5 分钟左右，耐心等待。

 ![01](/images/electron-react-01.png)

打开package.json文件需要添加 main.js 作为入口文件 
启动过后便可以看到 electron 运行窗口了

# 如何完美的一步一步结合 react + electron 开发应用

# 前言

React 本地启动在 3000端口
Electron 在创建 BrowserWindow 的时候，可以读取本地的文件或者是 url 
开发环境 读取localhost: 3000 
生产环境 需要加载本地成型以后的本地文件，打包的时候再考虑

## 环境配置

```
npx create-react-app react+electron
cd react+electron  //建立 react 环境
npm install electron —-save-dev  //同样安装需要一定的时间
```
那么同样在安装好之后，需要进行如下操作

- 新建 mian.js 文件
- 打开package.json文件需要添加 main.js 作为入口文件 

![02](/images/electron-react-02.png)

- 在package.json文件中添加执行脚本 `"ele": "electron ."`

![03](/images/electron-react-03.png)

* 安装 electron-is-dev 用于判断 electron 的开发环境

```
npm install electron-is-dev --save-dev
```

* 在 main.js 中写入以下代码，需要一点 electron 基础，请前往 [electron文档](https://www.electronjs.org/docs) 学习

```
const {app, BrowserWindow} = require('electron')
const isDev = require('electron-is-dev')
let mainWindow;

app.on('ready', () => {
  mainWindow = new BrowserWindow({
    width: 1024,
    height: 680,
    webPreferences: {
      nodeIntegration: true,
    }
  })
  const urlLocation = isDev ? 'http://localhost:3000' : 'dummyurl'
  mainWindow.loadURL(urlLocation)
  mainWindow.webContents.openDevTools()
})
```

大概就是创建了一个 1024 * 680 的窗口，在开发环境下，将 [http://localhost:3000](http://localhost:3000) 下的内容加载到electron窗口中。

## 开启 react+electron

在 package.json 文件中可以看到，要启动 react 和 electron 得执行以下两个脚本命令

![04](/images/electron-react-04.png)

那我们就一步一步的来执行以下 在当前目录下分别执行这两个命令

```
npm start //可以看到在浏览器新打开一个监听 3000 端口的一个 tab
npm run ele // 弹出 electron 窗口
```

最后的结果就是

![05](/images/electron-react-05.png)

在electron 窗口中加载了 [http://localhost:3000](http://localhost:3000) 的内容，小伙伴们在实际操作中会发现几个问题

1. 需要跑两个命令
2. 关掉 electron 窗口后，端口仍被占有的情况
3. 需要 3000 端口跑起来了再刷新一下 electron 才会有内容
4. 浏览器会打开一个 3000 端口的 tab 页, electron 会弹出加载了3000端口内容的窗口，理想状态下只需要保留 electron 中的窗口就好了

## 优化1

推荐一个 npm 库 [Concurrently](https://www.npmjs.com/package/concurrently)

在文档中可以看到它的特性，其实使用它的目的是为了解决上面提到的第一个和第二个
问题，同时同步，一次可以完美的运行多个命令。
在 package.json 中的 scripts 添加一个 dev

```
"dev": "concurrently \" electron .\" \" npm start\""
```

> 使用之后，会发现有点小问题，electron桌面程序是白屏的

## 优化2
再推荐一个 npm 库 [wait-on](https://www.npmjs.com/package/wait-on) 字面意思: 等待什么执行完之后再执行什么命令，解决我们的第三个问题
再次 改写 dev

```
 "dev": "concurrently \"wait-on http://localhost:3000 && electron .\" \"npm start\""
```
 
那么这次达到的效果就是等待3000端口跑完了再打开 electron 窗口，为了不看的白屏的情况

## 优化3

再推荐一个 npm 库 [cross-env](https://www.npmjs.com/package/cross-env), 这个大家应该经常用到：解决跨平台设置环境变量的问题。但是这次我们使用它是为了利用它的 BROWSER=none 来解决上面提到的第四个问题，不打开浏览器中的 tab 页。
再次改写一下 dev

```
 "dev": "concurrently \"wait-on http://localhost:3000 && electron .\" \"cross-env BROWSER=none npm start\""

```
 
那么这次改写之后就大功告成了，我们仅需要 执行 npm run dev 这个命令，然后就会弹出这样的 electron 窗口。没有白屏，不需要浏览器再打开tab页，是不是很完美，然后就可以愉快的进行 electron + react hooks 的应用开发啦。

# 总结

emmm 一步一步追求完美。

哈哈，总结我都转过来了~~~

转载自[掘金](https://juejin.im/post/6844904150405218317)