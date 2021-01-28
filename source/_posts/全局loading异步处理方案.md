---
title: 全局loading异步处理方案
date: 2021-01-18 19:06:56
comments: true
tags:
	- 常见问题
---


开发过程中，经常会用到loading状态，可以在单个接口请求中触发loading的开启和关闭，也可以在全局统一处理

单个接口中处理这些状态必然是相对于全局灵活一些的，但是会造成代码量增多


#### 全局处理

以下引入的是关于axios + elementui 的全局loading处理方案

<!-- more -->

先基于elementui封装loading

``` javascript
function startLoading () {
  loading = Vue.prototype.$loading({
    lock: true,
    text: '',
    background: 'rgba(255, 255, 255, 0.8)',
    fullscreen: true,
    spinner:"el-icon-loading",
    customClass: 'loading_color',
    textColor:"#096",
    color:'#096'
  })
}

function endLoading () {
  if(loading){
    loading.close()
  }
}
```


在请求拦截器中触发laoding开启

``` javascript
// request请求拦截器
service.interceptors.request.use(
  config => {
    startLoading()
    return config
  },
  error => {
    endLoading()
    Promise.reject(error)
  }
)
```

在响应拦截器中触发loading关闭

``` javascript
// respone响应拦截器
service.interceptors.response.use(
  response => {
    const res = response.data
    startLoading()
    return response.data
  },
  error => {
    endLoading()
    return Promise.reject(error)
  }
)

```

**然后问题就出现了，我有多个请求怎么办？**


很简单，计算机是个标记语言，我们只需要设计一个标记就好

``` javascript
// 定义loading状态
let loading
// 定义接口的总数，当数目为0时，可关闭loading
let needLoadingRequestCount = 0

```

标记好了以后，当请求进来的时候，数目加一处理
此函数在请求拦截器中替换startLoading()
``` javascript
function showFullScreenLoading () {
  if (needLoadingRequestCount === 0) {
    startLoading()
  }
  needLoadingRequestCount++
}

```

当请求结束，数值减一
此函数在响应拦截器中替换endLoading

``` javascript
async function tryHideFullScreenLoading () {
  if (needLoadingRequestCount <= 0) return
  needLoadingRequestCount--
  if (needLoadingRequestCount === 0) {
    endLoading()
  }
}

```


**上面这些代码，在同步请求中是完美的解决方案，在异步请求就会出现laoding闪动**

上面已经说过，全局的灵活性必然是没有单独写loading强大的

所以我们只能做一个兼容处理，参考防抖

在即将关闭loading的时候，设置一个0.3S的延迟，几乎可以解决大部分的闪动问题

``` javascript
async function tryHideFullScreenLoading () {
  if (needLoadingRequestCount <= 0) return
  needLoadingRequestCount--
  // 防抖处理
  await Promise.delay(300)
  if (needLoadingRequestCount === 0) {
    endLoading()
  }
}

```


#### 下面是所有的代码

``` javascript
import axios from 'axios'
import { Promise } from "bluebird";

// 创建axios实例
const service = axios.create({
  baseURL: "", // api的base_url
  timeout: 30000 // 请求超时时间
})


// request请求拦截器
service.interceptors.request.use(
  config => {
    showFullScreenLoading()
    return config
  },
  error => {
    tryHideFullScreenLoading()
    Promise.reject(error)
  }
)
// respone响应拦截器
service.interceptors.response.use(
  response => {
    const res = response.data
    tryHideFullScreenLoading()
    return response.data
  },
  error => {
    tryHideFullScreenLoading()
    return Promise.reject(error)
  }
)


let loading
let needLoadingRequestCount = 0

async function tryHideFullScreenLoading () {
  if (needLoadingRequestCount <= 0) return
  needLoadingRequestCount--
  console.warn(needLoadingRequestCount)
  // 防抖处理
  await Promise.delay(300)
  if (needLoadingRequestCount === 0) {
    endLoading()
  }
}

function startLoading () {
  loading = Vue.prototype.$loading({
    lock: true,
    text: '',
    background: 'rgba(255, 255, 255, 0.8)',
    fullscreen: true,
    spinner:"el-icon-loading",
    customClass: 'loading_color',
    textColor:"#096",
    color:'#096'
  })
}

function endLoading () {
  if(loading){
    loading.close()
  }
}
export default service

```