<template>
  <v-card
      elevation="15" outlined color="#1e1e1e"
      :width="722" :height="640"
      style="position: relative"
  >
    <v-progress-linear
        indeterminate
        color="light-blue accent-4"
        :active="loading"
    ></v-progress-linear>
    <video v-show="!connecting&&success" id="video" autoplay muted controls></video>
    <div class="loading" v-if="connecting" >
      <v-progress-circular
          :size="180"
          :width="15"
          color="indigo darken-4"
          indeterminate
      ></v-progress-circular>
    </div>
    <div class="failure">
      <v-alert
          prominent
          dismissible
          v-if="!connecting&&!success"
          :width="720"
          border="left"
          icon="mdi-alert-rhombus"
          color="red accent-3"
          text
          rounded
      >
        <v-row align="center">
          <v-col class="grow">
            连接似乎出现了一些问题
          </v-col>
          <v-col class="shrink">
            <v-btn @click="connectWebRtc">再次连接</v-btn>
          </v-col>
        </v-row>
      </v-alert>
    </div>

    <div class="CamControl d-flex justify-center" v-if="success">
      <v-btn-toggle
          borderless
          color="light-blue darken-4"
      >

        <v-btn x-large value="left" @click="Camleft">
          <span class="hidden-sm-and-down">LEFT</span>
          <v-icon right>
            mdi-arrow-left-bold-box
          </v-icon>
        </v-btn>
        <v-btn x-large value="center" @click="Camright">
          <span class="hidden-sm-and-down">RIGHT</span>
          <v-icon right>
            mdi-arrow-right-bold-box
          </v-icon>
        </v-btn>
        <v-btn x-large value="right" @click="Camup">
          <span class="hidden-sm-and-down">UP</span>
          <v-icon right>
            mdi-arrow-up-bold-box
          </v-icon>
        </v-btn>
        <v-btn x-large>
          <span class="hidden-sm-and-down" @click="Camdown">DOWN</span>
          <v-icon right>
            mdi-arrow-down-bold-box
          </v-icon>
        </v-btn>
      </v-btn-toggle>
    </div>
  </v-card>
</template>
<script>

export default {
  data() {
    return {
      webRtcServer:null,
      connecting:false,
      success:false,
      loading:false,
    }
  },
  components:{
  },
  computed: {
  },
  destroyed() {
    this.webRtcServer.disconnect()
  },
  mounted() {
    this.connectWebRtc();
  },
  methods: {
    connectWebRtc(){
      this.connecting=true;
      // eslint-disable-next-line no-undef
      let webRtcServer = new WebRtcStreamer("video",location.protocol+"//"+window.location.hostname+":8000");
      this.webRtcServer=webRtcServer;
      webRtcServer.connect("rtsp://10.0.0.1:8554/stream1","","rtptransport=tcp&timeout=60");
      var that=this;
      setTimeout(function (){
        console.log(webRtcServer)
        if(webRtcServer.pc!==null){
          that.success=true
        }
        that.connecting=false
      },2000)
    },
    Camup(){
      this.loading=true;
      var that=this;
      this.axios.get('camup/').then(res=>{
        console.log(res);
        setTimeout(function () {
          that.loading=false;
        },500)
      }).catch(err=>{
        console.log(err);
        this.loading=false;
        setTimeout(function () {
          that.loading=false;
        },500)
      })
    },
    Camdown(){
      this.loading=true;
      var that=this;
      this.axios.get('camdown/').then(res=>{
        console.log(res);
        setTimeout(function () {
          that.loading=false;
        },500)
      }).catch(err=>{
        console.log(err);
        this.loading=false;
        setTimeout(function () {
          that.loading=false;
        },500)
      })
    },
    Camleft(){
      this.loading=true;
      var that=this;
      this.axios.get('camleft/').then(res=>{
        console.log(res);
        setTimeout(function () {
          that.loading=false;
        },500)
      }).catch(err=>{
        console.log(err);
        this.loading=false;
        setTimeout(function () {
          that.loading=false;
        },500)
      })
    },
    Camright(){
      this.loading=true;
      var that=this;
      this.axios.get('camright/').then(res=>{
        console.log(res);
        setTimeout(function () {
          that.loading=false;
        },500)
      }).catch(err=>{
        console.log(err);
        this.loading=false;
        setTimeout(function () {
          that.loading=false;
        },500)
      })
    },
  }
}
</script>
<style scoped>
#video{
  border-radius: 3px;
  height: 540px;
  width: 720px;
}
.loading{
  position: absolute;
  top:calc(50% - 90px);
  left: calc(50% - 90px);
}
.failure{
  position: absolute;
  top:calc(50% - 30px);
}
.CamControl{
  width: 100%;
}
</style>
