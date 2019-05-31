<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>POS</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn flat href target="_blank">
        <span class="mr-2">Latest Release</span>
      </v-btn>
    </v-toolbar>

    <v-content>
      <PosMain/>
    </v-content>
  </v-app>
</template>

<script>
import PosMain from "./views/Posmain";

export default {
  name: "App",
  components: {
    PosMain
  },
  data() {
    return {
      //
    };
  }
};
</script>
