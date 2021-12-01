<template>
    <v-app>
      <v-main>
        <v-card class="d-flex justify-space-around pa-5" color="#121212" :elevation="0">
          <div class="video">
            <Video/>
          </div>
          <div class="control">
            <Control/>
          </div>
        </v-card>
        <v-card class="ma-15" color="#121212" :elevation="0">
          <Chart/>
        </v-card>
      </v-main>
    </v-app>
</template>

<script>
import Video from './components/Video';
import Control from "./components/Control";
import Chart from "./components/Chart";

export default {
  name: 'App',
  components: {
    Video,
    Control,
    Chart,
  },

};
</script>
<style>
</style>
