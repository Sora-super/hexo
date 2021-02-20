---
title: antv词云
date: 2021-02-20 14:58:44
tags:
	- 常见问题
---


![image](http://cimg.xuezsl.com/image/987.png)


[官网词云地址](https://antv-g2.gitee.io/zh/examples/other/other#word-cloud)


- 在vue中的使用

新建word.js

```js
import DataSet from '@antv/data-set';
import { Chart, registerShape, Util } from '@antv/g2';

function getTextAttrs(cfg) {
  return {
    ...cfg.defaultStyle,
    ...cfg.style,
    fontSize: cfg.data.size,
    text: cfg.data.text,
    textAlign: 'center',
    fontFamily: cfg.data.font,
    fill: cfg.color || cfg.defaultStyle.stroke,
    textBaseline: 'Alphabetic'
  };
}

// 给point注册一个词云的shape
registerShape('point', 'cloud', {
  draw(cfg, container) {
    const attrs = getTextAttrs(cfg);
    const textShape = container.addShape('text', {
      attrs: {
        ...attrs,
        x: cfg.x,
        y: cfg.y
      }
    });
    if (cfg.data.rotate) {
      Util.rotate(textShape, cfg.data.rotate * Math.PI / 180);
    }
    return textShape;
  }
});

function wordChart(id, data){

    const dv = new DataSet.View().source(data);
    const range = dv.range('count');
    const min = range[0];
    const max = range[1];
    const MAX_FONTSIZE = 36; // 最大的字体
    const MIN_FONTSIZE = 12; // 最小的字体
    dv.transform({
      type: 'tag-cloud',
      fields: ['key', 'count'],
      size: [600, 500],
      font: 'Verdana',
      padding: 0,
      color: ['red','pink'],
      timeInterval: 5000, // max execute time
      rotate() {
        let random = ~~(Math.random() * 4) % 4;
        if (random === 2) {
          random = 0;
        }
        return random * 90; // 0, 90, 270
      },
      fontSize: function fontSize(d) {
        if (d.value) {
          return (d.value - min) / (max - min) * (MAX_FONTSIZE - MIN_FONTSIZE) + MIN_FONTSIZE;
        }
        return 0;
      }
    });

    const chart = new Chart({
      container: id,
      autoFit: true,
      height: 400,
      padding: 0
    });
    chart.data(dv.rows);
    chart.scale({
      x: { nice: false },
      y: { nice: false }
    });
    chart.legend(false);
    chart.axis(false);
    chart.tooltip({
      showTitle: false,
      showMarkers: false
    });
    chart.coordinate().reflect();
    chart.point()
      .position('x*y')
      .color('#c0c6cf')
      .shape('cloud')
      .tooltip('key*count');
    chart.interaction('element-active');
    chart.render()

}



export default wordChart;
```
<!-- more -->

index.vue
```vue
<template>
    <div>
         <div id = "main" ref='wordCanvas'  v-show="tabType === '1'" ></div>
    </div>
</template>

<script>
import worchart from './word.js'

export default {

    methods: {

        async getData(){

            const data = await axios.get......
            /**
                main 为id
                data为数据
            */
            worchart('main',data)

        }
    }
}
</script>
```


大功告成！


表面上是大功告成，其实并没有，当你重复赋值的时候，会发现之前的数据并没有清空

造成了重复绘制的问题

下面提供一个最简单的解决方案

修改methods下的getData方法

```js
       async getData(){

            const data = await axios.get......
            // 增加这么一行，在绘制之前清空dom元素
            this.$refs.wordCanvas.innerHTML = ''

            worchart('main',data)

        }
```

