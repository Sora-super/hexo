---
title: 'createPersistedState[vuex]持久化缓存'
date: 2021-01-04 11:53:11
comments: true
tags:
	- 常见问题
---


在vue中使用vuex时，经常出现刷新页面数据丢失的问题，这时候,createPersistedState插件就派上了用场

[npmjs官网地址](https://www.npmjs.com/package/vuex-persistedstate)


~开始使用它!

- 安装

``` javascript
npm install vuex-persistedstate
```

- 使用

``` javascript
/*  vuex/index.js*/
import Vue from 'vue'
import Vuex from 'vuex'
import getters from './getters'
import createPersistedState from 'vuex-persistedstate'
import modules from './modules'
import Config from '../service/config' // 用来配置当store数据发生改变的时候，修改浏览器本地缓存
Vue.use(Vuex)

const store = new Vuex.Store({
  modules,
  getters,
  plugins: [
    createPersistedState({
      key: `vuex${Config.echartModify}`
    })
  ]
})

export default store


// 当然 这样会造成多次更新版本的时候本地缓存产生垃圾，可以通过下面这种方法处理 

/* main.js */
if (window.localStorage.getItem('version') != Config.echartModify){
  window.localStorage.removeItem('vuex' + window.localStorage.getItem('version'))
  window.localStorage.setItem('version',Config.echartModify)
}

```

## 拓展


- paths <Array> : 默认全路径[]

``` javascript
/* module.js */
export const dataStore = {
  state: {
    data: []
  }
}

/* store.js */
import { dataStore } from './module'

const dataState = createPersistedState({
  paths: ['data']
})

export new Vuex.Store({
  modules: {
    dataStore
  },
  plugins: [dataState]
})
```

- 自定义

默认储存在localstorage，可能在某些情况下不是想象中的这么理想，可以通过提供的自定义功能,设置自己想要的存储方式，一下为cookie的配置


```javascript
import { Store } from "vuex";
import createPersistedState from "vuex-persistedstate";
import * as Cookies from "js-cookie";

const store = new Store({
  // ...
  plugins: [
    createPersistedState({
      storage: {
        getItem: (key) => Cookies.get(key),
        setItem: (key, value) => Cookies.set(key, value, { expires: 3, secure: true }),
        removeItem: (key) => Cookies.remove(key),
      },
    }),
  ],
})
```
