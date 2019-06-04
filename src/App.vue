<template>
<v-app id="inspire">
    <v-toolbar color="blue lighten-1" fixed app>
      <v-toolbar-title>Pos</v-toolbar-title>
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
  data: () => ({

    })
};
</script>
