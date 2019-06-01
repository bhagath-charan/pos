<template>
  <v-container fluid>
<<<<<<< Updated upstream
<<<<<<< Updated upstream
<<<<<<< Updated upstream
    <v-layout row wrap>
      <v-flex>
=======
    
    <v-layout wrap row>
      <v-flex xs12>
=======
=======
>>>>>>> Stashed changes
    
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
<<<<<<< Updated upstream
>>>>>>> Stashed changes
=======
>>>>>>> Stashed changes
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
>>>>>>> Stashed changes
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

