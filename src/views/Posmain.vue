<template>
  <v-container fluid grid-list-sm>
    <v-layout row wrap>
      <v-flex xs6>
        <Customer :customers="customers"/>
      </v-flex>
      <v-flex xs6>
        <ProductView></ProductView>
      </v-flex>
          <v-card style="width:75%">
            <DataTable></DataTable>
          </v-card>

        <v-card style="width:25%">
          <ProductList v-bind:productsList="products"></ProductList>
        </v-card>


    </v-layout>
  </v-container>
</template>

<script>
import DataTable from "../components/DataTable";
import ProductList from "../components/ProductList";
import Customer from "../components/Customer";
import ProductView from "../components/ProductView";
import axios from "axios";

export default {
  name: "Posmain",
  components: {
    DataTable,
    ProductList,
    Customer,
    ProductView
  },
  data() {
    return {
      products: {},
      customers: {}
    };
  },
  methods: {
    getProducts() {
      axios
        .get("http://localhost:8080/products.json")
        .then(response => {
          this.products = response.data.products;
        })
        .catch(error => alert("Error while fetching" + error));
    },
    getCustomers() {
      axios
        .get("http://localhost:8080/customers.json")
        .then(response => {
          this.customers = response.data.customers;
        })
        .catch(error => alert("Error while fetching customers" + error));
    }
  },
  created() {
    this.getProducts();
    this.getCustomers();
  }
};
</script>

<style>
.customer{
    font-size: 24px!important;
    line-height: 32px!important;
    padding-left: 15px;
}
</style>
