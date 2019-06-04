<template>
  <v-container fluid grid-list-sm>
    <v-layout row wrap>
      <v-flex xs6>
        <Customer :customers="customers"/>
      </v-flex>
      <v-flex xs6>
        <v-card>
          <v-card-text>

          </v-card-text>
        </v-card>
      </v-flex>
        <v-flex sm9 md9 lg9>
          <v-card>
            <DataTable></DataTable>
          </v-card>
        </v-flex>
        <v-flex sm3 md3 lg3>
          <ProductList v-bind:productsList="products"></ProductList>
        </v-flex>


    </v-layout>
  </v-container>
</template>

<script>
import DataTable from "../components/DataTable";
import ProductList from "../components/ProductList";
import Customer from "../components/Customer";
import axios from "axios";

export default {
  name: "Posmain",
  components: {
    DataTable,
    ProductList,
    Customer
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
