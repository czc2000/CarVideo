<template>
  <v-card
      class="d-flex justify-center align-center" elevation="15" outlined color="#1e1e1e" :width="720" :height="547"
  >
      <video v-show="!connecting&&success" id="video" autoplay muted></video>
    <div class="loading" v-show="connecting">
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
          v-show="!connecting&&!success"
          :width="540"
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
  </v-card>
</template>
<script>

export default {
  data() {
    return {
      webRtcServer:null,
      connecting:false,
      success:false,
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
      let webRtcServer = new WebRtcStreamer("video","http://0.0.0.0:8000");
      this.webRtcServer=webRtcServer;
      webRtcServer.connect("rtsp://10.0.0.1:8554/stream1");
      var that=this;
      setTimeout(function (){
        console.log(webRtcServer)
        if(webRtcServer.pc!==null){
          that.success=true
        }
        that.connecting=false
      },2000)
    }
  }
  }
</script>
<style scoped>
#video{
  height: 540px;
  width: 720px;
}
</style>
