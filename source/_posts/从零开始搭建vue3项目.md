---
title: 从零开始搭建vue3项目
date: 2021-03-11 10:25:59
tags:
	- 基础搭建
---


1、 创建项目

``` js
yarn create vite-app vue3
```

大部分情况下，我们是需要使用vue-router，使用vite-app创建的项目是没有router的，需要手动安装


2、 安装vue-router

[vue-router@4](https://next.router.vuejs.org/zh/installation.html)
```js
yarn add 
```
<!-- more -->


3、 在src目录下新建文件夹router,router里新建index.js

##### index.js
```js

import { createRouter, createWebHistory } from "vue-router";
import Home from "../components/HelloWorld"
import Test from "../components/Test"

const routes = [
  {
    path: "/",
    name: "Home",
    component: Home,
  },
  {
    path: "/test",
    name: "Test",
    component: Test,
  }
];

const router = createRouter({
  history: createWebHistory(), // 这里可以替换为hash
  routes,
});

export default router;
```


4、 修改main.js

```js
import { createApp } from 'vue'
import router from "./router";
import ElementPlus from 'element-plus';
import 'element-plus/lib/theme-chalk/index.css';
import App from './App.vue';

const app = createApp(App)
app.use(router)
app.use(ElementPlus)
app.mount('#app')
```



接下来 就可以愉快的使用vue3开发了！



另：

推荐一下elementui-plus，vue3合理搭配ui框架

[elementui-plus](https://element-plus.gitee.io/#/zh-CN/component/installation)


css方面按照习惯安装预编译插件

这里使用的是sass

```js
yarn add sass -D
```

配置vite.config.js

> 注意不是vue.config.js

[vite.config.js](https://cn.vitejs.dev/config/)

