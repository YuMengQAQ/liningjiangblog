---
date: 2021-5-31
tag:
  - Echarts
author: liningjiang
location: Chengdu
---

# Echarts 使用套路总结

- 1、为 Echarts 准备一个具备大小（宽高）的画布容器 DOM
- 2、加载 echarts
- 3、初始化图表
- 4、指定图表的配置项和数据
- 5、使用刚指定的配置项和数据显示图表

- 如果使用以来百度地图的图表，则必须加载 echarts 百度地图的扩展

## Vue 中使用 Echarts5 版本

- 先 npm 安装

```
npm install echarts --save
```

- 然后封装 echatsUi.js 工具函数

```
// 引入 echarts 核心模块，核心模块提供了 echarts 使用必须要的接口。
import * as echarts from "echarts/core";
// 引入各种图表，图表后缀都为 Chart
import { BarChart, PieChart } from "echarts/charts"; //这里我引用两个类型的图表
// 引入提示框，标题，直角坐标系等组件，组件后缀都为 Component
import {
  TitleComponent,
  TooltipComponent,
  GridComponent,
  LegendComponent,
  // GeoCoComponent
} from "echarts/components";
// 引入 Canvas 渲染器，注意引入 CanvasRenderer 或者 SVGRenderer 是必须的一步
import { CanvasRenderer } from "echarts/renderers";

// 注册必须的组件
echarts.use([
  TitleComponent,
  TooltipComponent,
  GridComponent,
  LegendComponent,
  BarChart,
  PieChart,
  CanvasRenderer,
]);

export default echarts;
```

- 在 main.js 中挂载到 Vue 原型中

```
Vue.prototype.$echarts = echarts;
```

- 为 ECharts 准备一个具备大小（宽高）的 Dom

```
<div ref="main" style="width: 600px;height:400px"></div>
```

- 基于准备好的 dom，初始化 echarts 实例

```
 let myChart = this.$echarts.init(this.$refs["main"]);
```

- 指定图表的配置项和数据

```
 let option = {
      title: {
        text: "ECharts 入门示例",
      },
      tooltip: {},
      legend: {
        data: ["销量"],
      },
      xAxis: {
        data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"],
      },
      yAxis: {},
      series: [
        {
          name: "销量",
          type: "bar",
          data: [5, 20, 36, 10, 10, 20],
        },
      ],
    };

```

- 使用刚指定的配置项和数据显示图表。

```
myChart.setOption(option);
```
