<template>
  <v-container fluid>
    <v-layout row>
      <v-flex grow pa-1>
        <v-card>
          <v-card-title secondary-title>
            <h3 class="headline">CART</h3>
          </v-card-title>
          <v-card-text>
            <div>
              <DataTable></DataTable>
            </div>
          </v-card-text>
        </v-card>
      </v-flex>
      <v-flex shrink pa-1>
        <v-card>
          <v-card-text>
            <div>
              <ProductList v-bind:prodcutsList="products"></ProductList>
            </div>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import DataTable from "../components/DataTable";
import ProductList from "../components/ProductList";
import axios from "axios";
export default {
  name: "Posmain",
  components: {
    DataTable,
    ProductList
  },
  data() {
    return {
      products: {}
    };
  },
  methods: {
    getProducts() {
      axios
        .get("http://localhost:8081/products.json")
        .then(response => {
          this.products = response.data.products;
        })
        .catch(error => alert("Error while fetching"));
    }
  },
  created() {
    this.getProducts();
  }
};
</script>
