---
title: vue打包优化-cdn加载资源
date: 2021-06-09 14:07:26
tags:
    - vue优化
---


vue项目打包以后，经常会遇到文件特别大的情况，我们建议将elementui等第三方库提取出来单独使用cdn引入，来缩减打包文件大小

1.在vue项目中，执行npm run build -- --report,这时候打包后的文件会有一个report.html，打开文件

![image](http://cimg.xuezsl.com/image/vue1.png)





在图中可以看到，elementui占据了很大一部分内存



2.修改vue.config.js文件

 configureWebpack: {
    externals:{
      "vue": "Vue",
      // "vue-router": "VueRouter",
      // "vuex": "Vuex",
      // "axios": "axios",
      "element-ui": "ELEMENT",
      "echarts": "echarts",
      "bluebird": 'bluebird'
    }
  },


再次执行npm run build -- --report，打开report.html

![image](http://cimg.xuezsl.com/image/vue2.png)




可以看到elementui、echarts、bluebird等第三方插件webpack打包时候已经排除了出去



3.在index.html引入cdn



[第三方cdn服务](https://www.bootcdn.cn/)



4. 在network查看js资源请求
![image](http://cimg.xuezsl.com/image/vue1.png)






