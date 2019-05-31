<template>
  <v-form>
    <v-container>
      <v-layout row wrap>

        <v-flex xs12 sm6 md3>
          <v-text-field
            label="Name"
            outline
          ></v-text-field>
        </v-flex>
        <v-flex xs12 sm6 md3>
          <v-text-field
            label="Mobile"
            outline
          ></v-text-field>
        </v-flex>
        <v-dialog v-model="dialog" persistent max-width="600px">  
          <template v-slot:activator="{ on }">         
            <v-btn title="Create Customer" class="addButton"
              color="#341552"
              dark
              small
              fab
            >
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
                  <v-text-field v-model="newCustomer.name" label="Name*" required></v-text-field>
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
        </v-layout>
    </v-container>
  </v-form>
</template>

<script>
import axios from "axios";
export default {
    name: "Customer",
    props:["customers"],
    data(){
      return {
        newCustomer:{
          name:"",
          mobile:""
        },
        dialog:false
      }
    },
    methods:{
        createCustomer(){
          axios
              .get("https://yesno.wtf/api")
              .then(response => {
                console.log(response.data.answer);
              })
              .catch(error=>alert("Error while creating the customer"));
              this.newCustomer.name=null;
              this.newCustomer.mobile=null;
              this.dialog=false;
        }
    }
}
</script>

<style>
.addButton{
  top: 9px;
}
</style>

