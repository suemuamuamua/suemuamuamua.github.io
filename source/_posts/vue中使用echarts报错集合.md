---
title: vue中使用echarts报错集合
date: 2018-04-24 11:45:52
tags: 
      --vue
categories: JavaScript
---
第一次着手系统项目，遇到很多问题，尤其是关于vue中使用echarts报错Uncaught TypeError: Cannot read property 'getAttribute' of null
<!-- more -->
1. Uncaught TypeError: Cannot read property 'getAttribute' of null
  在vue中使用echarts时报错Uncaught TypeError: Cannot read property “getAttribute” of null。问题出在echarts.init(document.getElementById('idName')),获取该元素时为空，应该在页面加载完成后再获取。
  即绘制图标的函数应该在mouted()钩子函数中，而不是created()中。

  另外，若绘制echarts为动态数据时，应保证在其canvas容器加载完成之后初始化echarts.init，比如本次项目中，echarts图表不确定数量，使用v-for遍历请求到的数据对应数组构建dom，此时应监听v-for是否加载完成，加载完成再继续进行下一步操作。监听v-for的方法如下
~~~js
axios.post(url, params)
  .then((response) => {
    this.chartData = response.data // 赋值遍历数组
    this.$nextTick(() => { // dom更新完成后绘制图表
      this.drawEcharts()
      })
    })
~~~



