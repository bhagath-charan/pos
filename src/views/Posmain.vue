<template>
  <v-container fluid>
    <v-layout row wrap>
      <v-flex>
        <v-card>
          <h2>Customer details</h2>
          <Customer :customers="customers"/>
        </v-card>
      </v-flex>
      <v-flex pa-1>
        <v-card height="100%">
          <DataTable></DataTable>
        </v-card>
      </v-flex>
      <v-flex shrink pa-1>
        <v-card height="450px">
          <v-card-text>
            <div>
              <ProductList v-bind:productsList="products"></ProductList>
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
        .get("http://localhost:8081/products.json")
        .then(response => {
          this.products = response.data.products;
        })
        .catch(error => alert("Error while fetching" + error));
    },
    getCustomers() {
      axios
        .get("http://localhost:8081/customers.json")
        .then(response => {
          this.customers = response.data.customers;
        })
        .catch(error => alert("Error while fetching customers" + error));
    }
  },
  created() {
    this.getProducts();
  }
};
</script>
