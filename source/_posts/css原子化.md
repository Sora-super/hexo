---
title: css原子化
date: 2021-01-13 14:09:35
comments: true
tags:
	- 基础搭建
---


今天刷掘金，又一次看到了css原子化这个名词，并且扯到了facebook重构这款大皮

先不说css原子化好或者不好，每个人都有各自的评判标准，先来了解下，什么是原子化，什么是组件化

在日常的代码编写中，我们一定写过这种代码

``` javascript
...
<div class="fr"></div>
<div class="fl"></div>

...

<style>
    .fr{
        float: right;
    }
    .fl{
        float: left;
    }
</style>
```
<!-- more -->

很明显，所有的css类，都只有唯一的css规则，这就是css原子化

在看下面这些代码

``` javascript
<div class="box">
</div>

<style>
    .box{
        width: 100px;
        height: 100px;
        left: 0;
        right: 0;
        bottom: 0;
        top: 0;
        margin: auto;
        background: pink;
    }
</style>
```

.box这个类名下的所有规则，构成了这个元素的的呈现方式，这就是css组件化

---

是不是感觉像js中面向对象和面向过程的思想？

关于css原子化，雅虎[Atomic css](https://acss.io/)一直在路上


---

这么说可能有点过于笼统，让我们跳出程序：

```
“汉字的数量并没有准确数字,大约将近十万个,日常所使用的汉字只有几千字。
据统计,1000个常用字能覆盖约92%的书面资料,2000字可覆盖98%以上,3000字则已到99%“


每一个汉字，就是原子化


一个个汉字组成了不同优美的文章，就是组件化

```


汉字尚且如此，英文更不用说了

---



日常生活中这些思想也是比比皆是


组件化和原子化没有好或者不好，这和具体的业务还是挂钩的
哪一种模式更适合自己业务，更利于项目的长久可持续发展，就是好模式