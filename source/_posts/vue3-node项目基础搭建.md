---
title: vue3/node项目基础搭建
date: 2021-01-07 15:11:13
tags:
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

> 可以看到脚手架引入的koa版本号是1.X，所以用到了co模块，co模块是用于Generator函数的自动执行的一个小工具，我们接下来要对koa进行一个升级，使用async/await替换掉Generator，所以将这个文件删掉

- debug

> debug工具，众所周知

- ejs

> ejs模板，koa+ejs标准MVC,如果只是想提供接口，删掉它

- jade

- 也是个模板，直接删

- koa

> 重头戏，直接修改为^2.13.0

- koa-bodyparser

> 中间件，可以将post请求的参数转为json格式返回,比较有用，版本号升级为4.3

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

> 中间件,比较有用，升级为2.x

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

- 日志中间件,升级为3.2.1

```javascript
const logger = require('koa-logger')
const Koa = require('koa')

const app = new Koa()
app.use(logger())
```
- koa-onerror

> 基于koa的错误处理程序,升级为4.x

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

> koa的路由，升级10.x

- koa-static

> koa静态服务中间件，升级为5.0.0

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

> 模块渲染中间件，直接删掉


全部删完以后，package.json就成了以下这个样子
``` json
{
  "name": "gugod",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "start": "node bin/www",
    "dev": "./node_modules/.bin/nodemon bin/www",
    "prd": "pm2 start bin/www",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "axios": "^0.21.1",
    "debug": "^2.6.3",
    "koa": "^2.13.0",
    "koa-bodyparser": "^4.3.0",
    "koa-json": "^2.0.2",
    "koa-logger": "^1.3.1",
    "koa-onerror": "^1.1.0",
    "koa-router": "^10.0.0",
    "koa-static": "^5.0.0"
  },
  "devDependencies": {
    "nodemon": "^1.8.1"
  }
}
```


这时候，直接执行yarn安装依赖就可以了
