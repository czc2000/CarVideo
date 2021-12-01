<template>
    <video id="video" autoplay muted></video>
</template>
<script>

export default {
  data() {
    return {
      webRtcServer:null
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
    // eslint-disable-next-line no-undef
    let webRtcServer = new WebRtcStreamer("video","http://0.0.0.0:8000");
    this.webRtcServer=webRtcServer;
    webRtcServer.connect("rtsp://10.0.0.1:8554/stream1");
    },
  methods: {
  }
  }
</script>
<style scoped>
#video{
  height: 540px;
  width: 720px;
  border-radius: 20px;
  background-color: lightblue;
}
</style>
