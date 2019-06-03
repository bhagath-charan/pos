<template>
  <div
    id="e3"
    style="width: 300px; bottom-margin: 2px;"
    class="grey lighten-3"
  >
    <v-card>
      <v-container padding="10px">
        <v-layout column>
          <v-text-field v-model="search" @input="onChange" label="Search Product" solo></v-text-field>
          <v-list style="max-height: 600px" class="scroll-y" two-line subheader>
            <template v-for="(product, index) in results">
              <v-list-tile :key="index" avatar  ripple @click="">

                <v-list-tile-content avatar @click="selectedProduct(product)">
                  <div>
                    <h5>{{ product.name }}</h5>
                  </div>
                  <v-list-tile-sub-title>{{ product.gst }}</v-list-tile-sub-title>
                </v-list-tile-content>

                <v-list-tile-action>
                  <v-dialog v-model="dialog" hide-overlay max-width="40%">
                    <template v-slot:activator="{ on }">
                      <v-btn icon>
                        <v-icon color="grey lighten-1" v-on="on">info</v-icon>
                      </v-btn>
                    </template>

                    <v-card>
                      <v-card-title class="headline grey lighten-2">
                        {{ product.name }}
                      </v-card-title>
                      <v-card-text>
                        {{ product.name }}
                      </v-card-text>
                    </v-card>
                  </v-dialog>
                </v-list-tile-action>
              </v-list-tile>
              <v-divider v-if="index + 1 < results.length" :key="`divider-${index}`"></v-divider>
            </template>
          </v-list>
        </v-layout>
      </v-container>
    </v-card>
  </div>
</template>

<script>
import { posEventBus } from "../main";

export default {
  name: "ProductsList",
  props: ["productsList"],
  data() {
    return {
      dialog : false,
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
