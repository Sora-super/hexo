---
title: vue.config.js中plugin配置方式
date: 2021-01-21 10:47:56
comments: true
tags:
	- 常见问题
---


今天遇到一个关于vue3-cli中配置插件的问题

官网提供了以下二种方法

[官网链接](https://cli.vuejs.org/zh/guide/webpack.html#%E7%AE%80%E5%8D%95%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%B9%E5%BC%8F)



1. 简单的配置方式

```javascript
module.exports = {
  configureWebpack: {
    plugins: [
      new MyAwesomeWebpackPlugin()
    ]
  }
}
```

2. 函数式配置

```javascript
// vue.config.js
module.exports = {
  configureWebpack: config => {
    if (process.env.NODE_ENV === 'production') {
      // 为生产环境修改配置...
    } else {
      // 为开发环境修改配置...
    }
  }
}
```

**没有关于函数式配置中插件的配置方案**



假如我想配置一个清除无用代码的插件 useless-files-webpack-plugin

使用简单配置方式如下

```javascript
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
```

但是我想用函数式配置他，怎么办？

尝试下以下代码

```javascript

  configureWebpack: (config) =>{
    // provide the app's title in webpack's name field, so that
    // it can be accessed in index.html to inject the correct title.
    // name: name,

    if (process.env.NODE_ENV === 'development') {
        config.devtool = 'source-map';
    }
    if (process.env.NODE_ENV === "production") {
        config.optimization.minimizer[0].options.terserOptions.compress.drop_console = true
    }
    config.optimization.minimizer.push(
      new UselessFile({
        root: './src', // 项目目录
        out: './fileList.json', // 输出文件列表
        clean: false,// 删除文件,
        exclude: path // 排除文件列表, 格式为文件路径数组
      })
    )

    return {resolve: {
      alias: {
        '@': resolve('src')
      }
    }}
  }
```
