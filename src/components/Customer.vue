<template>
  <v-layout>

    <v-flex>
      <div class="searchBox">
      <v-autocomplete
        v-model="select"
        background-color="#afabab"
        color="black"
        :loading="loading"
        :items="items"
        return-object
        item-text="mobile"
        :search-input.sync="search"
        cache-items
        flat
        no-data-text="No options"
        hide-details
        label="Search customer by mobile number"
        solo-inverted
      ></v-autocomplete>
      <v-dialog v-model="dialog" persistent max-width="600px">
        <template v-slot:activator="{ on }">
          <v-btn title="Create Customer" class="addButton" color="#341552" dark small fab>
            <v-icon v-on="on">add</v-icon>
          </v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="headline">Customer Profile</span>
          </v-card-title>
          <v-card-text>
            <v-container grid-list-md>
              <v-layout wrap>
                <v-flex xs12 sm6 md4>
                  <v-select
                    v-model="newCustomer.title"
                    :items="titles"
                    label="title"
                  ></v-select>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field v-model="newCustomer.firstName" label="First Name*" required></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field v-model="newCustomer.lastName" label="Last Name"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field v-model="newCustomer.dob" label="Date of Birth"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field v-model="newCustomer.email" label="E-mail"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field v-model="newCustomer.mobile" label="Mobile*" required></v-text-field>
                </v-flex>
              </v-layout>
            </v-container>
            <small>*indicates required field</small>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" flat @click="dialog = false">Close</v-btn>
            <v-btn color="blue darken-1" flat @click="createCustomer">Save</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      </div>

      <v-card class="data" elevation="0" max-width="50%">
        <v-layout row wrap>
          <v-flex xs4><v-card-text> Name : {{select.firstName}}</v-card-text></v-flex>
          <v-flex xs4><v-card-text> Mobile :{{select.mobile }} </v-card-text></v-flex>
          <v-flex xs4><v-card-text> E-mail  : {{select.email}}</v-card-text>  </v-flex>
        </v-layout>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from "axios";
export default {
  name: "Customer",
  props: ["customers"],
  data() {
    return {
      titles:['Mr','Ms',"Mrs"],
      newCustomer: {
        firstName: "",
        lastName: "",
        title: "",
        dob: "",
        email: "",
        mobile: ""
      },
      dialog: false,
      loading: false,
      items: [],
      search: null,
      select: {}
    };
  },
  watch: {
    search(val) {
      val && val !== this.select && this.filterCustomers(val);
    }
  },
  methods: {
    createCustomer() {
      axios
        .get("https://yesno.wtf/api")
        .then(response => {
          console.log(response.data.answer);
        })
        .catch(error => alert("Error while creating the customer"));
      this.newCustomer.name = null;
      this.newCustomer.mobile = null;
      this.dialog = false;
    },
    filterCustomers(v) {
      this.loading = true;
      setTimeout(() => {
        this.items = this.customers.filter(customer => {
          return customer.mobile.toString().indexOf(v) > -1;
        });
        this.loading = false;
      }, 500);
    },selectedCustomer() {
      for (var i = 0; i < this.items.length; i++) {
        console.log(this.items[i].mobile === this.select ? this.items[i] : {});
        this.selectedCustomer1 =  this.items[i] && this.items[i].mobile === this.select ? this.items[i] : {};
      }
    }
  },
};
</script>

<style>
.data{
  padding-top: 0px;
}
.searchBox {
  width: 30%;
  display: flex;
}

</style>
