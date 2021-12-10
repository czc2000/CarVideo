<template>
  <v-card
      elevation="15"
      outlined
      color="#1e1e1e"
  >
  <div id="control">
    <v-row no-gutters>
      <v-col
          cols="6"
          md="6"
      >
        <v-card
            class="ma-2"
            :height="620"
            elevation="15"
        >
          <v-toolbar
              elevation="4"
              rounded
              color="purple darken-4"
          >
            <v-progress-linear
                :active="loading"
                :indeterminate="loading"
                absolute
                top
                color="purple accent-4"
                height="6"
            ></v-progress-linear>
            <h3>当前状态</h3>
          </v-toolbar>
          <div ref="speedChart" class="speedChart">
          </div>
        </v-card>
      </v-col>
      <v-col
          cols="12"
          md="6"
      >
        <v-card
            class="ma-2"
            :height="620"
            elevation="15"
        >
          <v-toolbar
              elevation="4"
              rounded
              color="indigo darken-4"
          >
            <v-progress-linear
                :active="loading"
                :indeterminate="loading"
                absolute
                top
                color="indigo accent-4"
                height="6"
            ></v-progress-linear>
            <h3>控制小车</h3>
          </v-toolbar>
          <div class="buttonControl">
            <v-tooltip bottom>
              <template v-slot:activator="{ on, attrs }">
            <v-btn
                class="ma-2 right"
                x-large
                fab
                tile
                color="indigo darken-1"
                v-bind="attrs"
                v-on="on"
                @click="TurnRight"
                :disabled="loading"
            >
              <v-icon x-large>mdi-arrow-right-bold-hexagon-outline</v-icon>
            </v-btn>
              </template>
            <span>向右转向</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ on, attrs }">
            <v-btn
                class="ma-2 left"
                x-large
                fab
                tile
                color="indigo darken-1"
                v-bind="attrs"
                v-on="on"
                @click="TurnLeft"
                :disabled="loading"

            >
              <v-icon x-large>mdi-arrow-left-bold-hexagon-outline</v-icon>
            </v-btn>
              </template>
              <span>向左转向</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ on, attrs }">
            <v-btn
                class="ma-2 down"
                x-large
                fab
                tile
                color="indigo darken-1"
                v-bind="attrs"
                v-on="on"
                @click="GoBack"
                :disabled="loading"
            >
              <v-icon x-large>mdi-arrow-down-bold-hexagon-outline</v-icon>
            </v-btn>
              </template>
              <span>倒退一段距离</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ on, attrs }">
            <v-btn
                class="ma-2 up"
                x-large
                fab
                tile
                color="indigo darken-1"
                v-bind="attrs"
                v-on="on"
                @click="Speedup"
                :disabled="loading"
            >
              <v-icon x-large>mdi-arrow-up-bold-hexagon-outline</v-icon>
            </v-btn>
              </template>
              <span>加速</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ on, attrs }">
            <v-btn
                class="ma-2 stop"
                x-large
                fab
                color="deep-orange darken-1"
                v-bind="attrs"
                v-on="on"
                @click="Brake"
                :disabled="loading"
            >
              <v-icon x-large>mdi-pause-octagon-outline</v-icon>
            </v-btn>
              </template>
              <span>刹车</span>
            </v-tooltip>
          </div>
        </v-card>
      </v-col>
    </v-row>
  </div>
  </v-card>
</template>

<script>
import * as echarts from 'echarts'
// eslint-disable-next-line no-unused-vars
import dark from 'echarts/theme/dark'
export default {
  name: "Control",
  data(){
    return{
      loading:false,
      speedChart:null,
      speedChartOption:null,
    }
  },
  mounted() {
    this.speedChartOption={
      series: [
        {
          type: 'gauge',
          startAngle: 225,
          endAngle: -45,
          min: 0,
          max: 1,
          splitNumber: 6,
          axisLine: {
            lineStyle: {
              width: 8,
              color: [
                [0.33, '#4DB6AC'],
                [0.66, '#FDDD60'],
                [1, '#E57373']
              ]
            }
          },
          pointer: {
            icon: 'path://M12.8,0.7l12,40.1H0.7L12.8,0.7z',
            length: '12%',
            width: 20,
            offsetCenter: [0, '-60%'],
            itemStyle: {
              color: 'auto'
            }
          },
          axisTick: {
            length: 12,
            lineStyle: {
              color: 'auto',
              width: 2
            }
          },
          splitLine: {
            length: 20,
            lineStyle: {
              color: 'auto',
              width: 5
            }
          },
          axisLabel: {
            color: '#464646',
            fontSize: 20,
            distance: -60,
            formatter: function () {
              return '';
            }
          },
          title: {
            offsetCenter: [0, '-25%'],
            fontSize: 30
          },
          detail: {
            fontSize: 35,
            offsetCenter: [0, '0%'],
            valueAnimation: true,
            formatter: function (value) {
              if(value==0){
                return "停止"
              }
              return Math.round(value * 3) + '档';
            },
            color: 'auto'
          },
          data: [
            {
              value: 0,
              name: '速度'
            }
          ]
        }
      ]
    }
    this.speedChartInit();
  },
  methods:{
    Speedup(){
      this.loading=true;
      if(this.speedChartOption["series"][0].data[0].value>=0.99){
        this.loading=false;
        return;
      }
      var that=this;
      this.axios.get('speedup/').then(res=>{
        console.log(res);
        this.speedChartOption["series"][0].data[0].value+=1.0/3;
        this.speedChart.setOption(this.speedChartOption)
        setTimeout(function () {
          that.loading=false;
        },500)
      }).catch(err=>{
        console.log(err);
        setTimeout(function () {
          that.loading=false;
        },500)
      })
    },
    Brake(){
      this.loading=true;
      var that=this;
      this.axios.get('brake/').then(res=>{
        console.log(res);
        this.speedChartOption["series"][0].data[0].value=0;
        this.speedChart.setOption(this.speedChartOption)
        setTimeout(function () {
          that.loading=false;
        },500)
      }).catch(err=>{
        console.log(err);
        setTimeout(function () {
          that.loading=false;
        },500)
      })
    },
    TurnLeft(){
      this.loading=true;
      var that=this;
      this.axios.get('left/').then(res=>{
        console.log(res);
        setTimeout(function () {
          that.loading=false;
        },500)
      }).catch(err=>{
        console.log(err);
        setTimeout(function () {
          that.loading=false;
        },500)
      })
    },
    TurnRight(){
      this.loading=true;
      var that=this;
      this.axios.get('right/').then(res=>{
        console.log(res);
        setTimeout(function () {
          that.loading=false;
        },500)
      }).catch(err=>{
        console.log(err);
        setTimeout(function () {
          that.loading=false;
        },500)
      })
    },
    GoBack(){
      this.loading=true;
      var that=this;
      this.axios.get('back/').then(res=>{
        console.log(res);
        setTimeout(function () {
          that.loading=false;
        },500)
      }).catch(err=>{
        console.log(err);
        setTimeout(function () {
          that.loading=false;
        },500)
      })
    },
    speedChartInit(){
      this.speedChart=echarts.init(this.$refs.speedChart);
      this.speedChart.setOption(this.speedChartOption)
    }

  }
}
</script>

<style scoped>
#control{
  height: 640px;
  width: 1100px;
}
.buttonControl{
  width: 440px;
  height: 540px;
  margin: 0 auto;
  position: relative;
}
.buttonControl .left{
  border-radius: 10px;
  position: absolute;


  bottom: 40%;
  left: 10%;
}
.buttonControl .right{
  border-radius: 10px;
  position: absolute;
  bottom: 40%;
  right: 10%;
}
.buttonControl .up{
  border-radius: 10px;
  position: absolute;
  top: 20%;
  left: calc(50% - 40px);
}
.buttonControl .down{
  border-radius: 10px;
  position: absolute;
  bottom: 17%;
  left: calc(50% - 40px);
}
.buttonControl .stop{
  position: absolute;
  bottom: 40%;
  left: calc(50% - 40px);
}
.speedChart{
  width: 500px;
  height: 500px;
  margin:20px auto;
}
</style>