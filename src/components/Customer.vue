<template>
  <v-layout row wrap>
    <v-flex xs12>
        <v-toolbar flat>
            <v-toolbar-title class="headline">Customer Details</v-toolbar-title>
            <v-spacer></v-spacer>           
              <v-flex md3>
                <v-autocomplete
                  v-model="select"
                  append-icon="search"
                  background-color="#ffffff"
                  :loading="loading"
                  :items="items"
                  return-object
                  item-text="mobile"
                  item-value="firstName"
                  :search-input.sync="search"
                  cache-items
                  flat
                  no-data-text="No options"
                  hide-details
                  label="Search customer by mobile number"
                  solo
                >
                  <template v-slot:item="data">
                    <template v-if="typeof data.item !== 'object'">
                      <v-list-tile-content v-text="data.item"></v-list-tile-content>
                    </template>
                    <template v-else>
                      <v-list-tile-content>
                        <v-list-tile-title v-html="data.item.firstName"></v-list-tile-title>
                        <v-list-tile-sub-title v-html="data.item.mobile"></v-list-tile-sub-title>
                      </v-list-tile-content>
                    </template>
                  </template>
                </v-autocomplete>
              </v-flex>
                <v-dialog v-model="dialog" persistent max-width="600px">
                  <template v-slot:activator="{ on }">
                    <v-btn title="Create Customer" color="#42a5f5" small fab>
                      <v-icon v-on="on">person_add</v-icon>
                    </v-btn>
                  </template>
                  <v-card ref="form"
                      v-model="valid"
                      lazy-validation>
                    <v-form>
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
                              outline
                            ></v-select>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field outline v-model="newCustomer.firstName" :rules="formRules.firstNameRules" label="First Name*" required></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field outline v-model="newCustomer.lastName" label="Last Name"></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field outline v-model="newCustomer.dob" label="Date of Birth"></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field outline type="email" :rules="formRules.emailRules" v-model="newCustomer.email" label="E-mail"></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field outline type="number" :rules="formRules.mobileRules" v-model="newCustomer.mobile" label="Mobile*" required></v-text-field>
                          </v-flex>
                        </v-layout>
                      </v-container>
                      <small>*indicates required field</small>
                    </v-card-text>
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn color="blue darken-1" flat @click="closeDialog">Close</v-btn>
                      <v-btn color="blue darken-1" flat @click="createCustomer">Save</v-btn>
                    </v-card-actions>
                    </v-form>
                  </v-card>
              </v-dialog> 
        </v-toolbar>
    </v-flex>
      
    <v-flex xs5>
      <v-card elevation="0">
        <v-layout row>
        <v-card-text> 
      
            <h3 class="font-weight-regular"><b>Name :</b> &nbsp; {{select.firstName}}</h3>
            
            <h3 class="font-weight-regular"><b>Mobile :</b> &nbsp; {{select.mobile }}</h3>
            <h3 class="font-weight-regular"><b>E-mail  :</b> &nbsp; {{select.email}} </h3>
          
        </v-card-text>
        <v-card-text>
            <h3 class="font-weight-regular"><b>Address : </b>&nbsp;</h3>
            <span> {{select.hasOwnProperty('address') ? select.address.addressLine1 : ''}}&nbsp;</span><br>
            <span>{{select.hasOwnProperty('address') ? select.address.addressLine2 : ''}}&nbsp;</span><br>
            <span>{{select.hasOwnProperty('address') ? select.address.cityOrTown : ''}}&nbsp;</span>
            <span>{{select.hasOwnProperty('address') ? select.address.state : ''}}&nbsp;</span>
          
        </v-card-text>
        </v-layout>
      </v-card>
    </v-flex> 
  </v-layout>

</template>

<script>
import axios from "axios";
import { posEventBus } from "../main";
export default {
  name: "Customer",
  props: ["customers"],
  data() {
    return {
      valid:true,
      titles:['Mr','Ms',"Mrs"],
      newCustomer: {
        firstName: "",
        lastName: "",
        title: "",
        dob: "",
        email: "",
        mobile: ""
      },
      formRules:{
        firstNameRules:[
          v => !!v || 'First Name is required'
        ],
        emailRules:[
          v => /.+@.+/.test(v) || 'E-mail must be valid'
        ],
        mobileRules:[
          v => !!v || 'Mobile number is required'
        ]
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
      this.items = []
    },
    select(){
      posEventBus.$emit('add-customer',this.select)
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
        //this.$refs.form.reset()
      this.newCustomer={
        firstName: "",
        lastName: "",
        title: "",
        dob: "",
        email: "",
        mobile: ""
      }
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
    },
    closeDialog(){
      this.dialog = false,
      //this.$refs.form.reset()
      this.newCustomer={
        firstName: "",
        lastName: "",
        title: "",
        dob: "",
        email: "",
        mobile: ""
      }
    }
  },
};
</script>

<style>
.data{
  margin-top: 3px;
}
.details{
  padding: 0px;
}

</style>
