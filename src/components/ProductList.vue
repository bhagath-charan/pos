<template>
  <v-layout row>
    <v-flex>
      <v-card>
        <v-toolbar>
          <v-toolbar-title>Products List</v-toolbar-title>

          <v-spacer></v-spacer>

          <v-btn icon>
            <v-icon>search</v-icon>
          </v-btn>
        </v-toolbar>

        <v-list two-line subheader>
          <v-list-tile
            v-for="product in prodcutsList"
            :key="product.name"
            avatar
            @click="selectedProduct(product)"
          >
            <v-list-tile-content>
              <v-list-tile-title>{{ product.name }}</v-list-tile-title>
              <v-list-tile-sub-title>{{ product.gst }}</v-list-tile-sub-title>
            </v-list-tile-content>

            <v-list-tile-action>
              <v-btn icon ripple>
                <v-icon color="grey lighten-1">info</v-icon>
              </v-btn>
            </v-list-tile-action>
          </v-list-tile>
        </v-list>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import { posEventBus } from "../main";

export default {
  name: "ProductsList",
  props: ["prodcutsList"],
  methods: {
    selectedProduct(product) {
      posEventBus.$emit("selectedProduct", product);
      alert(product.name + " is selected ");
    }
  }
};
</script>