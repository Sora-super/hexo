---
title: node请求csv文件中文乱码
date: 2021-01-07 17:25:25
comments: true
tags:
	- 常见问题
---


实践可行！直接上代码

``` javascript
const http = require('http')
var iconv = require('iconv-lite');

const responseUrl = "http://quotes.money.163.com/service/chddata.html?code=0601398&start=20201201&end=20210106"


http.get(responseUrl,function(res){
let result = []
  res.on('data',function(chunk){
    result.push(chunk)
  })
  res.on('end',function(){
    var html = iconv.decode(Buffer.concat(result),'gb2312')
    console.log(html)
  })
})


http.createServer((req,res)=>{
    console.log(11)
    res.end()
}).listen(8999)




console.log('server is running in 8999')
```
<!-- more -->

关键点就在于 iconv 插件了，有兴趣的可以去[npm官网看看](https://www.npmjs.com/package/iconv-lite)





## 用了框架，不会举一反三？

再来个koa的
```javascript
const router = require('koa-router')()
const axios = require('axios')
var iconv = require('iconv-lite');

router.get('/', async (ctx, next) => {
  // 获取股票历史成交数据【csv格式】
  const response = await axios({

    methods: 'get',
    url: "http://quotes.money.163.com/service/chddata.html?code=0601398&start=20201201&end=20210106",
    responseType:'arraybuffer'
  })
  var dv = new Array(response.data)

  let data = iconv.decode(Buffer.concat(dv),'gb2312')
  ctx.body = data
})

router.get('/string', async (ctx, next) => {
  ctx.body = 'koa2 string'
})

router.get('/json', async (ctx, next) => {
  ctx.body = {
    title: 'koa2 json'
  }
})

module.exports = router

```


[使用cheeerio操作html，服务器端的jquery](https://github.com/cheeriojs/cheerio/wiki/Chinese-README)