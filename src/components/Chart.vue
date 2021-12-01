<template>
  <v-card class="pa-5" :max-width="750" elevation="15">
    <div id="chart">
    </div>
  </v-card>
</template>

<script>
import * as echarts from 'echarts'
// eslint-disable-next-line no-unused-vars
import dark from 'echarts/theme/dark'
export default {
  name: "Chart",
  data(){
    return{
    }
  },
  mounted() {
    this.init()
  },
  methods:{
    init() {
      console.log(this.vintageTheme)
      /**
       * 自定图表主题
       * @type {Element}
       *  1. https://echarts.apache.org/zh/theme-builder.html 使用主题构建工具自定义主题
       */
      /**
       * 当前图标默认的渲染方式是canvas
       * 修改渲染方式为SVG
       * 在第三个参数上传递 {renderer:svg}
       * @type {Element}
       * 频繁的进行更新动画建议使用canvas
       * 如果是大屏展示需要使用svg
       */
      // 1. 获取需要渲染的DOM
      const chartDom = document.querySelector("#chart")
      // 2. 初始化
      const chart = echarts.init(chartDom, 'dark', {renderer: 'canvas'})
      // 3. 设置 option
      chart.setOption({
            xAxis: {
              data: ['A', 'B', 'C', 'D', 'E']
            },
            yAxis: {},
            series: [
              {
                data: [10, 22, 28, 23, 19],
                type: 'line',
                smooth: true,
                label: {
                  show: true,
                  position: 'bottom',
                  textStyle: {
                    fontSize: 16
                  }
                }
              }
            ]}
        )
    }
  }
}
</script>

<style scoped>
#chart {
  width: 720px;
  height: 720px;
}
</style>