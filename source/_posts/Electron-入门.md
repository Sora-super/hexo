---
title: Electron 入门
date: 2021-01-08 11:30:38
comments: true
tags:
	- 基础搭建
---


[官网文档](http://www.electronjs.org/docs)


1. 克隆示例项目

> git clone https://github.com/electron/electron-quick-start

2. 进入

> cd electron-quick-start

3. 运行

> npm i && npm start


---

直接运行几次就会发现，每次修改代码都要重新执行一遍npm start,这很麻烦，热更新就可以解决这个问题

1. 安装依赖

> npm i electron-reloader

2. main.js插入以下代码

```
try {
  require('electron-reloader')(module);
} catch (error) {
}
```


---

使用electron发送请求的时候，由于5.x默认关闭了nodeIntegration,直接require文件会报requre is not define，需要在mian.js修改以下代码

```javascript
function createWindow () {
  // Create the browser window.
  const mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js'),
      nodeIntegration: true,
      enableRemoteModule: true
    }
  })

  // and load the index.html of the app.
  mainWindow.loadFile('index.html')

  // Open the DevTools.
  mainWindow.webContents.openDevTools()
}

```

renderer.js
```javascript
var button = document.getElementById('onclick');
button.onclick = getData;

function getData()
{
  const {net} = require('electron').remote;
  const request = net.request('http://10.73.35.148:3000');
  request.on('response', (response) => {
      response.on("data", (chunk)=>{
          console.log("接收到数据：", JSON.parse(chunk.toString()));
      })
      response.on('end', () => {
          console.log("数据接收完成");
      })
  });

  request.end();
}
```



---

#### 进阶！

既然掌握了最基本的搭建方案，下面介绍一种vue开发者福音-electron-vue

[vue-cli创建项目文档](https://electron.org.cn/vue/index.html)

[electron-vue中文文档]

[npm](https://www.npmjs.com/package/vue-electron)

这是一个vue-cli的模块

1. 全局安装vue-cli

> npm install -g vue-cli

2. 创建项目

> vue init simulatedgreg/electron-vue my-project

3. 安装依赖&&开发

> yarn && yarn run dev


有的人可能会遇到安装完依赖dev就发现报错，可能和node版本太高有关，切换下node版本就好了。我是10.15.0，没啥问题