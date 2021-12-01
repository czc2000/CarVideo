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
          md="4"
      >
        <v-card
            class="pa-2 ma-2"
            :height="535"
            elevation="15"
        >
          当前状态
        </v-card>
      </v-col>
      <v-col
          cols="12"
          md="8"
      >
        <v-card
            class="pa-2 ma-2"
            :height="535"
            elevation="15"
        >
          控制
        </v-card>
      </v-col>
    </v-row>
  </div>
  </v-card>
</template>

<script>
export default {
  name: "Control"
}
</script>

<style scoped>
#control{
  height: 547px;
  width: 900px;
}
</style>