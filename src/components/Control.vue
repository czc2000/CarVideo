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
            class="ma-2"
            :height="535"
            elevation="15"
        >
          <v-toolbar
              elevation="4"
              rounded
              color="purple darken-4"
          >
            <h3>当前状态</h3>
          </v-toolbar>
        </v-card>
      </v-col>
      <v-col
          cols="12"
          md="8"
      >
        <v-card
            class="ma-2"
            :height="535"
            elevation="15"
        >
          <v-toolbar
              elevation="4"
              rounded
              color="indigo darken-4"
          >
            <h3>控制小车</h3>
          </v-toolbar>
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
  width: 1100px;
}
</style>