<template>
    <v-app>
      <v-main>
        <v-card class="d-flex justify-space-around pa-5">
          <div class="video">
            <Video/>
          </div>
          <div class="control">
            <Control/>
          </div>
        </v-card>
      </v-main>
    </v-app>
</template>

<script>
import Video from './components/Video';
import Control from "./components/Control";

export default {
  name: 'App',
  components: {
    Video,
    Control,
  },

};
</script>
<style>
/*.video{*/
/*  float: left;*/
/*  margin: 8px;*/
/*}*/
/*.control{*/
/*  float: left;*/
/*  margin: 8px;*/
/*}*/
</style>
