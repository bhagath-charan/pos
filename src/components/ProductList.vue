<template>
  <v-layout column justify-center align-center row>
    <v-flex>
      <v-text-field v-model="search" @input="onChange" label="Search Product" solo></v-text-field>
      <v-list style="max-height: 350px" class="scroll-y" two-line subheader>
        <v-list-tile v-for="product in results" :key="product.name">
          <v-list-tile-content @click="selectedProduct(product)">
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
    </v-flex>
  </v-layout>
</template>

<script>
import { posEventBus } from "../main";

export default {
  name: "ProductsList",
  props: ["productsList"],
  data() {
    return {
      search: "",
      results: [],
      isOpen: false
    };
  },

  methods: {
    selectedProduct(product) {
      posEventBus.$emit("selectedProduct", product);
    },
    filterResults() {
      this.results = this.productsList.filter(
        product =>
          product.name.toLowerCase().indexOf(this.search.toLowerCase()) > -1
      );
    },
    onChange() {
      this.isOpen = true;
      this.filterResults();
    }
  }
};
</script>
