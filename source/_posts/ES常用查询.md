---
title: ES常用查询
date: 2021-01-06 15:13:35
comments: true
tags:
	- 常见问题
---

随着大数据、日志埋点等的兴起，我们常常听到ELK这个词语


#### 什么是ELK?
> ELK是三个开源软件的缩写，分别表示：Elasticsearch , Logstash, Kibana

在工作中，kibana的使用必然是不可缺少的，kibana常用的的模块主要集中在Discover/Visualize/Dashboard


分别理解一下这三个软件

##### Elasticsearch

> Elasticsearch 是一个分布式、高扩展、高实时的搜索与数据分析引擎。Elasticsearch是与名为Logstash的数据收集和日志解析引擎以及名为Kibana的分析和可视化平台一起开发的。这三个产品被设计成一个集成解决方案，称为“Elastic Stack”（以前称为“ELK stack”）。


##### Logstash
>  Logstash最常用于ELK（elasticsearch + logstash + kibane）中作为日志收集器使用

##### Kibana

> Kibana 是一款开源的数据分析和可视化平台.Kibana 可以使大数据通俗易懂


三者相辅相成，是一个集成的解决方案



在实际应用中，我们可能会用es取一些特定的数据做数据分析，这时候就有必要深入了解一下es查询语法


## kibana中常用查询语法
1. 查询某个字段中包含某个单词：

```
filedname:"新东方"
```

2. 字段为空

```
_exists_:title
```

3. 范围查询

```
filename :{0 TO 3600000} OR visibleTime : {0 TO 3600000}

其中[]表示闭区间，{}表示开区间
```
4. NOT/OR/AND 逻辑符

```
NOT title:"weixin*" AND NOT href:"*\/show*"
```
5. 全文检索

```
在搜索栏直接搜索单词，会返回所有字段中包含所搜索单词的文档

使用双引号，作为短语搜索

"hello world"
```

6. 字段是否存在

```
exists:http：  返回结果中需要有http字段
missing:http： 不能含有http字段
```

## es查询语法

1. term 过滤


title 字段完全匹配成 新东方 的数据：

``` javascript
{
  "query": {
    "term": {
      "title": "新东方"
    }
  }
}
```

2. terms 过滤

同样是过滤，和term的区别在于这有个s，是复数，准许多个匹配条件

匹配状态码为200、300、400的数据
```
{
  "query": {
    "terms": {
      "ajax.status": [
        200,
        300,
        400
      ]
    }
  }
}
```

3. range 过滤

获取指定条件下的数据内容
```

    "range": {
        "@timestamp": {
            "gte":  1609487165000,
            "lt":   1609919165000
        }
    }
}
```
gt :: 大于
gte:: 大于等于
lt :: 小于
lte:: 小于等于


4. exists 和 missing 过滤

exists 和 missing 过滤可以用于查找文档中是否包含指定字段或没有某个字段

```
{
    "exists":   {
        "field":  "title"
    }
}
```

5. bool过滤

bool过滤包含以下几个操作符:

must: []   // 相当于and
must_not:[]   // 相当于not
should:[]     // 相当于or

```
{
    "bool": {
        "must": [
             {
                 "match_phrase": {
                     "feHost": feHost
                 }
             },
             {
                 "match_phrase": {
                     "logType": {
                         "query": "LOG_PATH"
                     }
                 }
             },
             {
                 "range": {
                     "@timestamp": {
                         "gte": timegte,
                         "lte": timelte,
                         "format": "epoch_millis"
                     }
                 }
             }
         ]
    }
}
```










