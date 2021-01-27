---
title: vue3/node项目基础搭建
date: 2021-01-07 15:11:13
comments: true
tags:
	- 基础搭建
---


## vue3

[官网链接](https://v3.cn.vuejs.org/guide/migration/introduction.html#%E6%A6%82%E8%A7%88)

使用Vite

>  yarn create vite-app hello-vue3 # OR npm init vite-app hello-vue3


使用vue-cli

> 1. yarn global add @vue/cli  # OR npm install -g @vue/cli

> 2. vue create hello-vue3



## node项目

[我们常说，要站在巨人的肩膀上](https://koa.bootcss.com/),没错，就是koa

这里使用了koa脚手架【koa-generator】

1. 全局安装

> yarn global add koa-generator  # OR npm install -g koa-generator

2. 创建项目

> koa -e project  # e表示使用ejs模板

3. 修改依赖

进入项目package.json，我们分析一下里面的模块

- co

> 可以看到脚手架引入的koa版本号是1.X，所以用到了co模块，co模块是用于Generator函数的自动执行的一个小工具

- debug

> debug工具，众所周知

- ejs

> ejs模板，koa+ejs标准MVC

- jade

> 也是个模板

- koa

> 关键

- koa-bodyparser

> 中间件，可以将post请求的参数转为json格式返回,比较有用

``` javascript
const Koa = require('koa');
const app = new Koa();
// 引入koa-bodyparser中间件，这个中间件可以将post请求的参数转为json格式返回
const bodyParser = require('koa-bodyparser');
// 使用中间件后，可以用ctx.request.body进行获取POST请求参数，中间件自动给我们解析为json
app.use(bodyParser());
// request.method可以获取请求方法。get，post或者其他类型(request对象被封在ctx内，所以也可以ctx.method获取)
app.use(async(ctx)=>{
    if (ctx.url === '/' &&ctx.method === 'POST'){
        let postdata = ctx.request.body
        ctx.body = postdata
    }else {
        // 其他请求显示404
        ctx.body = '<h1>404!</h1>'
    }
})


app.listen(3000,()=>{
    console.log('server starting at localhost:3000')
})
```
- koa-json

> 中间件,比较有用

``` javascript
var json = require('koa-json');
var Koa = require('koa');
var app = new Koa();

app.use(json());

app.use((ctx) => {
  ctx.body = { foo: 'bar' };
});

```
- koa-logger

- 日志中间件

```javascript
const logger = require('koa-logger')
const Koa = require('koa')

const app = new Koa()
app.use(logger())
```
- koa-onerror

> 基于koa的错误处理程序

```javascript
const fs = require('fs');
const koa = require('koa');
const onerror = require('koa-onerror');

const app = new koa();

onerror(app);

app.use(ctx => {
  // foo();
  ctx.body = fs.createReadStream('not exist');
});
```

- koa-router

> koa的路由

- koa-static

> koa静态服务中间件

```javascript
const serve = require('koa-static');
const Koa = require('koa');
const app = new Koa();

// $ GET /package.json
app.use(serve('.'));

// $ GET /hello.txt
app.use(serve('test/fixtures'));

// or use absolute paths
app.use(serve(__dirname + '/test/fixtures'));

app.listen(3000);

console.log('listening on port 3000');
```

- koa-views

> 模块渲染中间件


### 当然，以上是koa1.x的内容，我们其实一般用的都是koa2.x


1. 安装

> cpm install -g koa-generator

2. 创建项目

> koa2 HelloKoa2

这时候就可以看到，co模块没有了，因为koa2使用的是async/await终极异步解决方案

