<template>
<v-card>
  <v-layout column>
    <v-flex md3>
        <v-toolbar dense flat>
            <v-toolbar-title class="details">Customer</v-toolbar-title>
            <v-spacer></v-spacer>
              <v-flex md5>
                <v-autocomplete
                  v-model="select"
                  append-icon="search"
                  background-color="#dadbde"
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
                  label="Mobile number"
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
                  <v-card>
                    <v-form ref="form"
                      v-model="valid"
                      >
                    <v-card-title>
                      <span class="headline"> New Customer</span>
                    </v-card-title>
                    <v-card-text>
                      <v-container class="" grid-list-md>
                        <v-layout wrap>
                          <v-flex xs12 md4>
                            <v-select
                              v-model="newCustomer.title"
                              :items="titles"
                              label="title"
                            
                            ></v-select>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field v-model="newCustomer.firstName" :rules="formRules.firstNameRules" label="First Name*" required></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field v-model="newCustomer.lastName" label="Last Name"></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field mask="##/##/####"  v-model="newCustomer.dob" label="Date of Birth" placeholder="dd/mm/yyyy"></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field :rules="formRules.emailRules" v-model="newCustomer.email" label="E-mail*" required></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field type="number" :rules="formRules.mobileRules" v-model="newCustomer.mobile" label="Mobile*" required></v-text-field>
                          </v-flex><br>
                        </v-layout>
                        
                          <h3>Address</h3>
                          <!-- <v-divider></v-divider><br> -->
                            <v-layout wrap>
                          <v-flex xs12 sm6 md4>
                            <v-text-field v-model="newCustomer.address.addressLine1" label="Line 1"></v-text-field>
                          </v-flex>
                           <v-flex xs12 sm6 md4>
                            <v-text-field v-model="newCustomer.address.addressLine2" label="Line 2"></v-text-field>
                          </v-flex>
                          <v-flex xs12 md4>
                            <v-select
                              v-model="newCustomer.address.type"
                              :items="addressTypes"
                              label="Type"
                          
                            ></v-select>
                          </v-flex>
<<<<<<< Updated upstream

=======
                          <v-flex xs12 sm6 md4>
                            <v-text-field v-model="newCustomer.address.cityOrTown" label="City/Town"></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field v-model="newCustomer.address.district" label="District"></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field v-model="newCustomer.address.state" label="State"></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field v-model="newCustomer.address.country" label="Country"></v-text-field>
                          </v-flex>
                          <v-flex xs12 sm6 md4>
                            <v-text-field v-model="newCustomer.address.pincode" label="Pincode"></v-text-field>
                          </v-flex>                          
>>>>>>> Stashed changes
                        </v-layout>
                      </v-container>
                      <small>*indicates required field</small>
                    </v-card-text>
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn color="blue darken-1" flat @click="closeDialog">Close</v-btn>
                      <v-btn :disabled="!valid" color="blue darken-1" flat @click="createCustomer">Save</v-btn>
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
      
            <h3 class="font-weight-regular">Name : &nbsp; {{select.firstName}}</h3>
            
            <h3 class="font-weight-regular">Mobile : &nbsp; {{select.mobile }}</h3>
            <h3 class="font-weight-regular">E-mail  : &nbsp; {{select.email}} </h3>
          
        </v-card-text>
        <v-card-text>
            <h3 class="font-weight-regular">Address : &nbsp;</h3>
            <span> {{select.hasOwnProperty('address') ? select.address.addressLine1 : ''}}&nbsp;</span><br>
            <span>{{select.hasOwnProperty('address') ? select.address.addressLine2 : ''}}&nbsp;</span><br>
            <span>{{select.hasOwnProperty('address') ? select.address.cityOrTown : ''}}&nbsp;</span>
            <span>{{select.hasOwnProperty('address') ? select.address.state : ''}}&nbsp;</span>

        </v-card-text>
        </v-layout>
      </v-card>
    </v-flex>
  </v-layout>
</v-card>
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
      addressTypes:[
        'BILLING',
        'SHIPPING',
        'OFFICE',
        'PERSONAL',
        'SHOP',
        'WAREHOUSE'
      ],
      newCustomer: {
        firstName: "",
        lastName: "",
        title: "",
        dob: "",
        email: "",
        mobile: "",
        address:{
            addressLine1:"",
            addressLine2: "",
            type:"",
            cityOrTown:"",
            district:"",
            state:"",
            country:"",
            pincode:''
        }
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
      console.log("&&&&&&&7")
      axios
        .get("https://yesno.wtf/api")
        .then(response => {
          console.log(this.newCustomer);
          console.log(response.data.answer);
        })
        .catch(error => alert("Error while creating the customer"));

      this.dialog = false;
      this.$refs.form.reset()
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
      this.$refs.form.reset()
    }
  },
};
</script>

<style>
.data{
  margin-top: 3px;
}
.details{
  padding-left: 0px;
}

</style>
