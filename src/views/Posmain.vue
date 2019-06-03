<template>
  <v-container fluid>
    <v-layout wrap row>
      <v-flex xs12>
        <v-card>
          <h4 class="customer">Customer Details</h4>
          <v-card-text>
            <div>
              <Customer :customers="customers"/>
            </div>
          </v-card-text>
        </v-card>
      </v-flex>
      <v-flex grow pa-1>
          <DataTable></DataTable>
      </v-flex>
      <v-flex shrink pa-1>
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
