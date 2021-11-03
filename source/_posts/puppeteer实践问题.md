---
title: puppeteer实践问题
date: 2021-08-18 14:20:30
tags:
    -   页面性能
---

Q: puppeteer 打开的界面不是全屏

``` js
    const browser = await puppeteer.launch({
        headless: false,
        defaultViewport: null,
    });
```
