---
title: 使用webpack优化vue项目
date: 2021-01-21 14:38:43
tags:
---


更好的使用webpack完善你的vue项目，优化加载速度


1. 开始gzip压缩


安装插件

```
npm install compression-webpack-plugin --save-dev
```

vue.config.js

```javascript
const CompressionWebpackPlugin = require('compression-webpack-plugin')
module.exports = {
  chainWebpack:config => {
    config.plugin('compression')
      .use(
      new CompressionWebpackPlugin(
        {
          filename: info => {
            return `${info.path}.gz${info.query}`
          },
          algorithm: 'gzip',
          threshold: 10240,
          test: /\.(js|css|json|txt|html|ico|svg)(\?.*)?$/i,
          minRatio: 0.8,
          deleteOriginalAssets: true
        }
      )
    )
  },
}
```


2. 删除未使用的文件

安装插件

```
npm i useless-files-webpack-plugin
```

vue.config.js

简单配置方式

``` javascript
const UselessFile = require('useless-files-webpack-plugin')

module.exports = {
  configureWebpack:{
    plugins: [
      new UselessFile({
        root: './src', // 项目目录
        out: './fileList.json', // 输出文件列表
        clean: false,// 删除文件,
        exclude: path // 排除文件列表, 格式为文件路径数组
      }),
    ]
  },
}
```

函数式

```javascript
const UselessFile = require('useless-files-webpack-plugin')

module.export = {
configureWebpack: (config) =>{
    // 删除未使用文件
    config.optimization.minimizer = [
      new UselessFile({
        root: './src', // 项目目录
        out: './fileList.json', // 输出文件列表
        clean: false,// 删除文件,
        exclude: path // 排除文件列表, 格式为文件路径数组
      })
    ]
  }
}
```

执行build后会生成一个fileList.json文件，列举了未使用的文件路径，可根据需要自行删除

3. 使用HappyPack开启多线程打包

[npmjs](https://www.npmjs.com/package/happypack)

4. 使用DLLPlugin 【动态链接库】



