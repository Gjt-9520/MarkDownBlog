---
title: "Apache ECharts入门"
date: 2024-07-11
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage61.jpg?raw=true
tags: ["ECharts"]
category: "笔记"
updated: 2024-07-12
  
top_group_index: 
---

# Apache ECharts

Apache ECharts是一款基于Javascript的数据可视化图表库,提供直观,生动,可交互,可个性化定制的数据可视化图表

[Apache ECharts官网](https://echarts.apache.org/zh/index.html)

# 入门案例

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>ECharts</title>
    <!-- 引入刚刚下载的 ECharts 文件 -->
    <script src="echarts.js"></script>
  </head>
  <body>
    <!-- 为 ECharts 准备一个定义了宽高的 DOM -->
    <div id="main" style="width: 600px;height:400px;"></div>
    <script type="text/javascript">
      // 基于准备好的dom,初始化echarts实例
      var myChart = echarts.init(document.getElementById('main'));

      // 指定图表的配置项和数据
      var option = {
        title: {
          text: 'ECharts 入门示例'
        },
        tooltip: {},
        legend: {
          data: ['销量']
        },
        xAxis: {
          data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
        },
        yAxis: {},
        series: [
          {
            name: '销量',
            type: 'bar',
            data: [5, 20, 36, 10, 10, 20]
          }
        ]
      };

      // 使用刚指定的配置项和数据显示图表,
      myChart.setOption(option);
    </script>
  </body>
</html>
```
