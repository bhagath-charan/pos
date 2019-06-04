<template>
  <div>
    <v-toolbar flat color="white">
      <v-toolbar-title>CART</v-toolbar-title>
      <v-divider class="mx-2" inset vertical></v-divider>
    </v-toolbar>

    <v-data-table
      :headers="headers"
      :items="pos.lineItems"
      hide-actions
      expand
    >
      <template v-slot:items="props">
        <td>{{ props.item.product.name }}</td>
        <td>
            <v-text-field  v-model="props.item.product.batch" ></v-text-field>
        </td>
        <td>{{ }}</td>

           <td>
              <v-text-field
                type="number"
                v-model="props.item.quantity"
                :on="calculateAmount(props.item, props.item.quantity)"
              >
              </v-text-field>
        </td>
        <td>{{ props.item.product.mrp }}</td>
        <td>{{props.item.product.gst}}</td>
        <td>

              <v-text-field
               type="number"
               v-model="props.item.discount"
              ></v-text-field>

        </td>
        <td>{{( props.item.product.mrp * props.item.quantity * props.item.discount)/100}}</td>
        <td>{{ props.item.amount}}</td>
        <td>
          <v-icon small @click="deleteItem(props.item)">delete</v-icon>
        </td>
      </template>
    </v-data-table>

<v-flex xs11>
    <v-list>
    <v-list-tile-content>
      <v-list-tile-title class="text-lg-right">Gross Amount :{{calculated.GrossAmount}}</v-list-tile-title>
    </v-list-tile-content>
    <v-list-tile-content>
      <v-list-tile-title class="text-lg-right">Discount :{{calculated.totalDiscount}}</v-list-tile-title>
    </v-list-tile-content>
    <v-list-tile-content>
      <v-list-tile-title class="text-lg-right">SGST :{{calculated.taxTotal/2}}</v-list-tile-title>
    </v-list-tile-content>
    <v-list-tile-content>
      <v-list-tile-title class="text-lg-right">CGST :{{calculated.taxTotal/2}}</v-list-tile-title>
    </v-list-tile-content>
    <v-list-tile-content>
      <v-list-tile-title class="text-lg-right">Net Payable :{{ pos.totalAmount }}</v-list-tile-title>
    </v-list-tile-content>
    <v-spacer></v-spacer>
    </v-list>
    </v-flex>
      <v-toolbar flat color="white">
      <v-spacer></v-spacer>
      <v-btn color="secondary" class="mb-1" flat @click="clearPos()">Clear Cart</v-btn>
      <v-btn color="secondary" class="mb-1" flat @click="proceed(pos)">Procced</v-btn>
    </v-toolbar>
  </div>
</template>

<script>
import axios from "axios";
import { posEventBus } from "../main";
export default {
  name: "DataTable",
  data: () => ({
    pagination: { rowsPerPage: 10 },
    dialog: false,
    headers: [
      {
        text: "Product Name",
        align: "left",
        sortable: false,
        value: "name"
      },
      { text: "Batch no.", value: "batch" },
      { text: "Exp Date", value: "expDate" },
      { text: "Quantity", value: "quantity" },
      { text: "MRP", value: "mrp" },
      {text: "GST%", value:"gst" },
      { text: "Disc%", value: "discount" },
      {text: "DiscAmount", value:"discountAmount" },
      { text: "Amount", value: "amount"},
      { text: "Actions", value: "name", sortable: false }
    ],
    calculated:{
      GrossAmount:0,
      totalDiscount:0,
      discountAmount:0,
      taxTotal:0
    },
    pos: {
      customer: {},
      lineItems: [],
      totalAmount: 0
    },
    lineItem: {
      product: {},
      quantity: 1,
      discount: 0,
      amount: 0
    },
    editedIndex: -1
  }),
  watch: {
    dialog(val) {
      val || this.close();
    }
  },
  created() {
    posEventBus.$on("selectedProduct", product => {
      this.lineItem.product = product;
      this.pos.lineItems.push(this.lineItem);
      this.lineItem = {
        product: {},
        quantity: 1,
        discount: 0,
        amount: 0
      };
    });
    posEventBus.$on("add-customer",customer => {
      this.pos.customer = customer
    })
  },
  methods: {
    deleteItem(item) {
      const index = this.pos.lineItems.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        this.pos.lineItems.splice(index, 1);
    },
    increment(item) {
      item.quantity++;
    },
    decrement(item) {
      if (item.quantity === 1) {
        alert("Negative quantity not allowed");
      } else {
        item.quantity--;
      }
    },
    close() {
      this.dialog = false;
      setTimeout(() => {
        this.lineItem = Object.assign({}, this.lineItem);
        this.editedIndex = -1;
      }, 300);
    },
    clearPos() {
      this.dialog = false;
      setTimeout(() => {
        this.pos = Object.assign(
          {},
          {
            customer: {},
            lineItems: [],
            totalAmount: 0
          }
        );
      }, 300);
    },
    netTotal() {
      this.pos.totalAmount = this.pos.lineItems
        .map(e => e.amount)
        .reduce((prev, next) => prev + next);
    },
    tax(){
    this.calculated.taxTotal = this.pos.lineItems
      .map(e => e.product.mrp * e.quantity * e.product.gst/100)
      .reduce((prev, next) => prev + next);
    },
    grossAmount(){
    this.calculated.GrossAmount = this.pos.lineItems
      .map(e => e.product.mrp * e.quantity)
      .reduce((prev,next) => prev + next);
    },
    totalDiscount(){
    this.calculated.totalDiscount = this.pos.lineItems
        .map(e => e.product.mrp * e.quantity * e.discount/100)
        .reduce((prev,next) => prev + next);
    },
    calculateAmount(item, value) {
      const index = this.pos.lineItems.indexOf(item);
      let amount = value * this.pos.lineItems[index].product.mrp;
      let tax = amount * this.pos.lineItems[index].product.gst/100;
      let discountAmount = (amount * this.pos.lineItems[index].discount) / 100;
      this.pos.lineItems[index].amount = (amount + tax) - discountAmount;

      this.netTotal();
      this.tax();
      this.grossAmount();
      this.totalDiscount();
    },
    proceed(data) {
      //change the get to post
      axios
        .get("https://yesno.wtf/api")
        .then(this.clearPos())
        .catch(error => alert("Error ocurred" + error));
    }
  }
};
</script>
