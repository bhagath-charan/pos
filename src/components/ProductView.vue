<template>
<v-card >
 <v-layout column>
   <v-flex >
     <v-toolbar dense flat>
       <v-toolbar-title class="details">Selected Product Details</v-toolbar-title>
       <v-spacer></v-spacer>
     </v-toolbar>
   </v-flex>
   <v-flex xs5>
     <v-card elevation="0">
       <v-layout row>
         <v-card-text>
           <h3 class="font-weight-regular">Name : &nbsp; {{ this.product.name }}</h3>
           <h3 class="font-weight-regular">Brand : &nbsp; {{ this.product.brand }}</h3>
         </v-card-text>
         <v-card-text>
          <h3 class="font-weight-regular">Category : &nbsp; {{ this.product.category }} </h3>
          <h3 class="font-weight-regular">Molecule : &nbsp; {{ this.product.activrPharmaIngrediant }} </h3>
         </v-card-text>
       </v-layout>
     </v-card>
   </v-flex>
 </v-layout>
</v-card>
</template>
<script>
import { posEventBus } from "../main";
export default {
  data: () => ({
   product: {}
  }),
  created() {
    posEventBus.$on("selectedProduct", product => {
        this.product = product;
        console.log(product);
    });
  }
}
</script>
