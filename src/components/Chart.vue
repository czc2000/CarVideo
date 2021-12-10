<template>
  <div class="d-flex flex-wrap justify-center">
  <v-card class="ma-10 pa-5" :max-width="1600" elevation="15">
    <div id="chart">
    </div>
    <v-btn-toggle
        borderless
        color="light-blue darken-4"
    >
    <v-btn x-large @click="StartRunning" :disabled="loading" >
      <span class="hidden-sm-and-down" >Go</span>
      <v-icon right>
        mdi-restart
      </v-icon>
    </v-btn>
    <v-btn x-large @click="StopRunning">
      <span class=" hidden-sm-and-down">Stop</span>
      <v-icon right>
        mdi-pause-circle-outline
      </v-icon>
    </v-btn>
      <div class="pa-2">
        <v-progress-circular
            indeterminate
            color="green accent-3"
            v-if="loading"
        ></v-progress-circular>
        <v-progress-circular
            :value="100"
            color="red accent-3"
            v-if="!loading"
        ></v-progress-circular>
      </div>
    </v-btn-toggle>
    <v-snackbar
        v-model="snackbar"
    >
      网络连接出错——无法获取加速度信息
      <template v-slot:action="{ attrs }">
        <v-btn
            color="red accent-3"
            text
            v-bind="attrs"
            @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-card>
  </div>

</template>

<script>
import * as echarts from 'echarts'
// eslint-disable-next-line no-unused-vars
import dark from 'echarts/theme/dark'
export default {
  name: "Chart",
  data(){
    return{
      loading:false,
      option: {
        title: {
          text: '小车加速度'
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          data: ['X轴方向', 'Y轴方向', 'Z轴方向']
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        },
        toolbox: {
          feature: {
            saveAsImage: {}
          }
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri']
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            name: 'X轴方向',
            type: 'line',
            data: [0, 0, 0, 0, 0]
          },
          {
            name: 'Y轴方向',
            type: 'line',
            data: [0, 0, 0, 0, 0],
          },
          {
            name: 'Z轴方向',
            type: 'line',
            data: [0, 0, 0, 0, 0]
          },
        ]
      },
      times:{
        x:[],
        y:[],
        z:[],
        record_time:[],
      },
      chart:null,
      stop:false,
      snackbar:false,
    }
  },
  created() {

  },
  mounted() {
    this.init();
    this.Acc();
  },
  methods:{
    init() {
      const chartDom = document.querySelector("#chart")
      const chart = echarts.init(chartDom, 'dark', {renderer: 'canvas'})
      this.chart=chart;
      chart.setOption(this.option)
    },
    updateChart(data){
      console.log(data);
      for(var i=0;i<data.length;i++){
        if(this.times.x.length>=300){
          this.times.x.shift();
          this.times.y.shift();
          this.times.z.shift();
          this.times.record_time.shift();
        }
        this.times.x.push(data[i].x_acc);
        this.times.y.push(data[i].y_acc);
        this.times.z.push(data[i].z_acc);
        this.times.record_time.push(data[i].record_time);
      }
      this.option.xAxis.data=this.times.record_time;
      this.option.series[0].data=this.times.x;
      this.option.series[1].data=this.times.y;
      this.option.series[2].data=this.times.z;
      this.chart.setOption(this.option);
    },
    Acc(){
      if(this.stop){
        this.loading=false;
        return;
      }
      this.loading=true
      var that=this;
      this.axios.get('acc/').then(res=>{
          this.updateChart(res.data.data)
          setTimeout(function (){
            that.Acc();
          },5000);
        }).catch(err=>{
          console.log(err);
          this.loading=false;
          this.snackbar=true;
        })
      },
    StartRunning(){
      this.stop=false;
      this.Acc();
    },
    StopRunning(){
      this.snackbar=false;
      this.stop=true;
    },
    },


}
</script>

<style scoped>
#chart {
  width: 1560px;
  height: 720px;
}
</style>