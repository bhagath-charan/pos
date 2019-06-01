<template>
  <div>
    <v-toolbar flat color="white">
      <v-toolbar-title>CART</v-toolbar-title>
      <v-divider class="mx-2" inset vertical></v-divider>
    </v-toolbar>

    <v-data-table :headers="headers" :items="pos.lineItems" :pagination.sync="pagination">
      <template v-slot:items="props">
        <td>{{ props.item.product.name }}</td>
        <td>{{ }}</td>
        <td>{{ }}</td>
        <td>
          <v-edit-dialog :return-value.sync="props.item.quantity">
            {{props.item.quantity}}
            <template v-slot:input>
              <v-text-field
                v-model="props.item.quantity"
                label="Edit"
                single-line
                counter
                :on="calculateAmount(props.item, props.item.quantity)"
              ></v-text-field>
            </template>
          </v-edit-dialog>
        </td>
        <td>{{ props.item.product.mrp }}</td>
        <td>
          <v-edit-dialog :return-value.sync="props.item.discount">
            {{props.item.discount}}
            <template v-slot:input>
              <v-text-field v-model="props.item.discount" label="Edit" single-line counter></v-text-field>
            </template>
          </v-edit-dialog>
        </td>
        <td>{{ props.item.amount }}</td>
        <td>
          <v-icon small @click="deleteItem(props.item)">delete</v-icon>
        </td>
      </template>
    </v-data-table>
    <v-toolbar flat color="white">
      <v-toolbar-title>Total Amount : {{ pos.totalAmount }}</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn color="secondary" class="mb-1" flat>Cancel</v-btn>
      <v-btn color="secondary" class="mb-1" flat>Procced</v-btn>
    </v-toolbar>
  </div>
</template>

<script>
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
    { text: "discount %", value: "discount" },
    { text: "amount", value: "amount" },
    { text: "Actions", value: "name", sortable: false }
  ],
    pos: {
      customer: {},
      lineItems: [],
      totalAmount: 0
    },
    lineItem: {
      product: {},
      quantity: 0,
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
        quantity: 0,
        discount: 0,
        amount: 0
      };
    });
  },

  methods: {
    deleteItem(item) {
      const index = this.pos.lineItems.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        this.pos.lineItems.splice(index, 1);
    },

    close() {
      this.dialog = false;
      setTimeout(() => {
        this.lineItem = Object.assign({}, this.lineItem);
        this.editedIndex = -1;
      }, 300);
    },

    calculateAmount(item, value) {
      const index = this.pos.lineItems.indexOf(item);

      let amount = value * this.pos.lineItems[index].product.mrp;

      let discountAmount = (amount * this.pos.lineItems[index].discount) / 100;

      this.pos.lineItems[index].amount = amount - discountAmount;
    }
  }
};
</script>
