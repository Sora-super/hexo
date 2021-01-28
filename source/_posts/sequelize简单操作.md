---
title: sequelize简单操作
date: 2021-01-05 17:15:30
comments: true
tags:
	- 常见问题
---

使用sequelize，进行对数据库的简单操作，[点击查看中文文档](https://www.sequelize.com.cn/)


## 简单查询

``` javascript
  let responses = await DATABASE.findAll({
    attributes: [
      ['host', 'log_host'],
      ['date','log_date'],
      [sequelize.fn('COUNT', sequelize.col('*')), 'log_count'],
      [sequelize.fn('SUM', sequelize.col('pv')), 'log_pv'],
      [sequelize.fn('AVG', sequelize.col('usetime')), 'log_avgtime']
    ],
    where:{
      host: 'xxxxx',
      date:{
        [Op.between]: ['2020-01-02', '2020-01-10']
      }
    },
    group: 'date',
    order:[
      ['date','DESC']
    ]
  })
```
<!-- more -->

#### 有时候会用到distinct函数去重，找了半天文档也没有找到合适的解决方案，最后只能直接写sql语句

``` javascript
  let sql = `select date as log_date,avg(usetime) as log_avgtime,count(distinct uuid) as log_count,sum(pv) as log_pv from esuselog
  where date between '2020-01-02' and '2020-01-10' group by date order by date desc `
  const responses = await sequelize.query(sql, {
    type: sequelize.QueryTypes.SELECT,
  })
```


#### 拓展


众所周知，sequelize是一个node中的的orm框架，那么，ORM是什么?


ORM  [对象-关系映射]
> ORM（Object Relational Mapping）框架采用元数据来描述对象与关系映射的细节，元数据一般采用XML格式，并且存放在专门的对象一映射文件中。简单理解为一种框架的格式
